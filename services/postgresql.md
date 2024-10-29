---

copyright:
  years: 2024, 2024
lastupdated: "2024-10-29"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Databases for PostgreSQL - Understanding high availability and disaster recovery

The current [PostgreSQL High availability docs (/docs/databases-for-postgresql?topic=databases-for-postgresql-high-availability) are excellent, but missing details below:

### HA
**This service** is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **Standard** plan.

In addition:
- Leader and follower will always be in different zones.
- Leader fails: If the leader fails the follower becomes a leader, a new follower will be created.
- Follower fails: New follower created.
- Zone fails: similar to leader or follower fails with the new follower created in a surviving zone
- Loss of leader may result in data loss:
  - How does a customer get notified if less than lag threshold?
  - How does a customer get notified if larger than the lag threshold?  I guess the database is unresponsive?
- A two member instance will recover from a single zone failure without data loss (other than the lag threshold). During data synchronization for a new follower the instance has exposure to second failure causing data loss. A three member, see [adding PostgreSQL members](/docs/databases-for-postgresql?topic=databases-for-postgresql-horizontal-scaling), is resilient to the failure of two members during that time.
- Improved RPO can be achieved by enabling `synchronous commit`, but consider trade offs:
  - Cost of a third member, see [pricing](/docs/databases-for-postgresql?topic=databases-for-postgresql-pricing)
  - Performance impact as described.

### Disaster recovery:
Zone disaster - see HA above covering member and zonal failures.

Planning for regional or database resource failure. Consider the framework defined in [Understanding disaster recovery - DR strategy options](/docs/resiliency?topic=resiliency-understanding-dr#dr-categories)

**Active/Nothing** - Restoring a database from a backup using [Point-in-time recovery](/docs/databases-for-postgresql?topic=databases-for-postgresql-pitr) is possible when the disaster database is available and the RPO falls within the supported window. Restore from backups is available as described in [Managing Cloud Databases backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups). In either case the restore is to a new database instance with a new connection strings used by clients. RTO for this strategy will be partially dependent on the size of the database. Large databases may take hours or days to restore. RPO can be dependent on the frequency of backups, consider a script using [IBM Cloud® Code Engine - Working with the Periodic timer (cron) event producer](/docs/codeengine?topic=codeengine-subscribe-cron) to produce on demand backups if required.

**Active/Passive** - It is not possible to restore to an existing instance.

**Active/Standby, Active/Active** - Implemented provisioning [read-only replicas](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas) for cross-regional failover. [Promote a read-only replica](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas&interface=ui#promoting-read-only-replica) to recover from a disaster. New connection strings are required by clients.  RPO times can be just a few seconds.

**Data Disaster**
Data in postgreSQL can be lost due to bugs in software, accidental or malicious operations. See **Active/Nothing** above for recovery.

================================================================================
# High availability
{: #high-availability}


{{site.data.keyword.databases-for-postgresql}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. Deployments contain a cluster with two data members, a leader and a replica. Both members contain a copy of your data by using asynchronous replication, with a distributed consensus mechanism to maintain cluster state and handle failovers. If the leader becomes unreachable, the cluster initiates a failover, and the replica is promoted to leader. The replica rejoins the cluster and your cluster continues to operate normally. 

{{site.data.keyword.databases-for-postgresql}} will, at times, do controlled switchovers under normal operation. These switchovers are no-data-loss events that result in resets of active connections. There is a period of up to 15 seconds where reconnections can fail. At times, unplanned failovers might occur due to unforeseen events on the operating environment. These can take up to 45 seconds, but can be less. In both cases, potential exists for the downtime to be longer.

You can extend high-availability further by adding [PostgreSQL members](https://cloud.ibm.com/docs/databases-for-postgresql?topic=databases-for-postgresql-horizontal-scaling) to the instance, for greater in-region redundancy, or by provisioning [read-only replicas](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas) for cross-regional failover or read offloading. 

{{site.data.keyword.databases-for-postgresql}} is designed and built to provide a robust, resilient, and performant Database as a Service offering. Review the PostgreSQL documentation on [replication techniques](https://www.postgresql.org/docs/current/wal-async-commit.html){: .external} to understand the constraints and tradeoffs that are associated with the asynchronous replication strategy that is deployed by default with {{site.data.keyword.databases-for-postgresql}}.

In scenarios where a database becomes critically unhealthy, such as a server crash on the leader, {{site.data.keyword.databases-for-postgresql}} attempts a failover. This auto failover capability is capped at 16 MB of data lag from leader to follower (a few rows of data once accounting for more PostgreSQL data overhead) and is not performed if the lag threshold is exceeded. If the potential for 16 MB of data loss is intolerable for the application, horizontally scale your {{site.data.keyword.databases-for-postgresql}} instance to three members and configure {{site.data.keyword.databases-for-postgresql}} to use a synchronous replication strategy on a per user or per database basis.

## Synchronous replication
{: #sync-repl}

By default, streaming replication is asynchronous. If the primary server crashes, some transactions that were committed might not have been replicated to the standby server, causing data loss. {{site.data.keyword.databases-for}} ensures that data loss is kept to a minimum substantial data loss; however, synchronous replication offers the ability to confirm that all changes were made by a transaction have been transferred to a synchronous member, ensuring consistency across a cluster. This consistency come from confirming that writes are written to a secondary before returning to the connecting client with a success. For variables regarding synchronous replication, see [`synchronous_commit`](/docs/databases-for-postgresql?topic=databases-for-postgresql-changing-configuration#gen-settings) on the Changing Configuration page. 

Synchronous replication brings replica availability into the primary write path. If there is no replica to acknowledge the write, the write hangs until a replica is available. This requires at least three members to function reliably, as synchronous replication is not supported on two-member deployments. You _must_ horizontally scale to at least three members before synchronous replication can be enabled.

While unlikely, it is possible that more than one replica might become unavailable simultaneously. If this happens, the primary database will not be able to complete any writes until a replica comes back online, effectively blocking all write traffic to your database. When you are deciding to use synchronous replication, weigh the relative costs and benefits of higher data durability versus potential availability issues.

Employing synchronous replication negatively impacts the performance of the database. Typically, a performant and effective way to employ this feature is by using it only on specific databases or users that must have the highest degree of data durability available.
{: .note}

## Application-level high-availability
{: #application-level-ha}

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-postgresql}} is a managed service, regular updates and database maintenance occurs as part of normal operations. This can occasionally cause short intervals where your database is unavailable. It can also cause the database to trigger a graceful fail-over, retry, and reconnect. It takes a short time for the database to determine which member is a replica and which is the leader, so you might also see a short connection interruption. Failovers generally take less than 30 seconds.

Your applications have to be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruption are not expected. Open a [support case](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods longer than a minute with no connectivity so we can investigate.

## Connection limits
{: #connection-limits-ha}

{{site.data.keyword.databases-for-postgresql}} sets the maximum number of connections to your PostgreSQL database to **115**. 15 connections are reserved for the superuser to maintain the state and integrity of your database, and 100 connections are available for you and your applications. After the connection limit is reached, any attempts at starting a new connection results in an error. To prevent overwhelming your deployment with connections, use connection pooling, or scale your deployment and increase its connection limit. For more information, see the [Managing PostgreSQL connections](/docs/databases-for-postgresql?topic=databases-for-postgresql-managing-connections) page.

## High availability, disaster recovery, and SLA resources
{: #ha-recovery-sla}

{{site.data.keyword.databases-for-postgresql}} deployments conform to the {{site.data.keyword.cloud_notm}} Databases [High availability, Disaster recovery, and Service Level Agreement (SLA)](/docs/cloud-databases?topic=cloud-databases-ha-dr) information and terms.


================================================================================
