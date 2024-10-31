---

copyright:
  years: 2024, 2024
lastupdated: "2024-10-31"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.databases-for-postgresql}} - Understanding high availability and disaster recovery

## Service High Availability
{: #postgresql-service-high-availability}

This service is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **Standard** plan.

{{site.data.keyword.databases-for-postgresql}} provides replication, failover, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and some failures. Deployments contain a cluster with two data **members** - leader and replica. The replica is kept up to date using asynchronous replication. A distributed consensus mechanism is used to maintain cluster state and handle failovers. If the leader becomes unreachable, the cluster initiates a failover, and the replica is promoted to leader and a new replica rejoins the cluster as a replica. The leader and replica will always be in different zones of an MZR. If the replica fails, a new replica created. If a zone failure results in a member failing the new replica will be created in a surviving zone.

You can extend high-availability further by adding [PostgreSQL members](/docs/databases-for-postgresql?topic=databases-for-postgresql-horizontal-scaling) to the instance, for greater in-region redundancy, or by provisioning [read-only replicas](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas) for cross-regional failover or read offloading. 

Review the PostgreSQL documentation on [replication techniques](https://www.postgresql.org/docs/current/wal-async-commit.html){: .external} to understand the constraints and tradeoffs that are associated with the asynchronous replication strategy that is deployed by default.

In scenarios where a database becomes critically unhealthy, such as a server crash on the leader, {{site.data.keyword.databases-for-postgresql}} attempts a failover. This auto failover capability is capped at 16 MB of data lag from leader to replica (a few rows of data once accounting for more PostgreSQL data overhead) and is not performed if the lag threshold is exceeded. If the potential for 16 MB of data loss is intolerable for the application, see [synchronous replication](#postgresql-sync-repl) below.

Workloads that programmatically access the service instance should follow the [client availability retry logic](/docs/doesnotexist) to maintain availability.

The service will, at times, do controlled failovers under normal operation. These failovers are no-data-loss events but result in resets of active connections. There is a period of up to 15 seconds where reconnections can fail. At times, unplanned failovers might occur due to unforeseen events on the operating environment. These can take up to 45 seconds, but generally less than 30. [Service maintenance](#postgresql-ibm-service-maintenance), for example, trigger a controlled failover.

A two member instance will automatically recover from a single zone failure (with data loss up to the lag threshold). During data synchronization for a new replica the instance has exposure to second failure causing data loss. A three member, see [adding PostgreSQL members](/docs/databases-for-postgresql?topic=databases-for-postgresql-horizontal-scaling), is resilient to the failure of two members during that time.

TODO clarify
- Loss of leader may result in data loss:
   - How does a customer get notified if less than lag threshold with associated fail over occurred?
   - When the lag threshold of data is lost - how does the customer recover that? Is it possible that a support ticket would help get the WAL file...
   - How does a customer get notified if larger than the lag threshold?


### Synchronous replication
{: #postgresql-sync-repl}
By default, streaming replication is asynchronous. If the leader crashes, some transactions that were committed might not have synced to the replica, causing data loss. {{site.data.keyword.databases-for}} ensures that data loss is kept to a minimum substantial data loss; however, synchronous replication offers the ability to confirm that all changes were made by a transaction have been synced to a replica. This insures consistency across a cluster. This consistency come from confirming that writes are written to a secondary before returning to the connecting client with a success. For variables regarding synchronous replication, see [`synchronous_commit`](/docs/databases-for-postgresql?topic=databases-for-postgresql-changing-configuration#gen-settings) on the Changing Configuration page. 

Synchronous replication brings replica availability into the primary write path. If there is no replica to acknowledge a write it will hang until a replica is available. This requires at least three members to function reliably, as synchronous replication is not supported on two-member deployments. You _must_ horizontally scale to at least three members before synchronous replication can be enabled.  See [adding PostgreSQL members](/docs/databases-for-postgresql?topic=databases-for-postgresql-horizontal-scaling)

While unlikely, it is possible that more than one replica might become unavailable simultaneously. If this happens, the primary database will not be able to complete any writes until a replica comes back online, effectively blocking all write traffic to your database. When you are deciding to use synchronous replication, weigh the relative costs and benefits of higher data durability versus potential availability issues.

Employing synchronous replication negatively impacts the performance of the database. Typically, a performant and effective way to employ this feature is by using it only on specific databases or workloads that require the highest degree of data durability available.
{: .note}

## Customer Disaster Recovery - DR
{: #postgresql-customer-disaster-recovery-dr}

Things to cover in the sections below
- Customer recover from zonal failure. could be detailed like VPC block storage, or could be just a note like ICD - see HA above
- Customer recover from regional failure
- Customer recover from service failure may be same as regional failure
- Customer recover from data corruption
- Customer recovery from BYOK loss this was covered in Secrets Manager is it a general problem


### Customer disaster definition
{: #postgresql-customer-disaster-definition}

A disaster of an instance can be due to:
- Data corruption.
- Service becomes unavailable.
- Regional disaster.

### Customer disaster planning
{: #postgresql-customer-disaster-planning}

To recover from a disaster one of the following mechanisms will be used:
- Create a new instance from a backup.
- Create a new instance from a point-in-time of a working database. 
- Promote a read-only replica to a read/write database.

The recovery instance should align with the workload [disaster recovery approaches within IBM Cloud](https://test.cloud.ibm.com/docs/resiliency?topic=resiliency-dr-approaches)

**Zero Footprint** - Restore from backups is available as described in [Managing Cloud Databases backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups). The restore is to a new database instance with a new connection strings used by clients.
- RTO will be partially dependent on the size of the database. Large databases may take hours or days to restore.
- RPO will be dependent on the frequency of backups - 24 hours is the default. Consider a script using [IBM Cloud® Code Engine - Working with the Periodic timer (cron) event producer](/docs/codeengine?topic=codeengine-subscribe-cron) to produce on demand backups to improve RPO.

**Basic Standby** - It is not possible to restore to an existing instance.

**Minimal Operation** or **Active/Active** - Create a [read-only replicas](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas) for cross-regional failover. [Promote the read-only replica](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas&interface=ui#promoting-read-only-replica) to recover from a disaster.
- RTO - few minutes
- RPO - few minutes


### Customer disaster recovery
{: #postgresql-customer-disaster-recovery}

Data in postgreSQL can be lost due to bugs in software, accidental or malicious operations. See **Active/Nothing** above for recovery. If only a portion of the database has been corrupted consider using the newly created database instance to harvest the corrupted data to apply to the production database.

Creating a new database from the live production using [point-in-time recovery](/docs/databases-for-postgresql?topic=databases-for-postgresql-pitr) is possible when the disaster database is available and the RPO (disaster) falls within the supported window.

A new recovered database may also need the same customer created dependencies of the disaster database - make sure these exist in the recovered region:
- {{site.data.keyword.keymanagementservicefull}}
- {{site.data.keyword.hscrypto}}

Follow the recovery plan created in the previous section to create a new service. The new, recovered instance, will have a different connection strings. Redirect your workload components to the recovered instance.

## IBM disaster recovery
{: #postgresql-ibm-disaster-recovery}

### IBM recovery from zone failure 
{: #postgresql-ibm-recovery-from-zone-failure}

The database is resilient from a single zone failure.  See [service availability](#postgresql-service-high-availability)

### IBM recovery from regional failure
{: #postgresql-ibm-recovery-from-regional-failure}

After a regional failure IBM will attempt to restore the service instance with the same connection strings from the last state in internal persistent storage.
- RTO - TODO
- RPO - TODO

It may not be possible for IBM to restore the database, in which case it will be required for the customer restore the database - see [customer disaster recovery](#postgresql-customer-disaster-recovery)

## IBM Service Maintenance
{: #postgresql-ibm-service-maintenance}

All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by [client availability retry logic](/docs/doesnotexist) within client applications. Changes are initially rolled out sequentially on a region-by-region basis.

Complex changes are enabled/disabled with feature flags to control exposure.
- TODO is this true?  Do a number of changes get dropped in from the postgresql open source?

Changes that impact customer workloads will be described by notifications. See [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.

## Links
- [Understanding high availability for Cloud Databases](/docs/cloud-databases?topic=cloud-databases-ha-dr)
- [Understanding business continuity and disaster recovery for Cloud Databases](/docs/cloud-databases?topic=cloud-databases-bc-dr)
- [Shared responsibilities for Cloud Databases](/docs/cloud-databases?topic=cloud-databases-responsibilities-cloud-databases)
- [High availability - Databases for PostgreSQL](/docs/databases-for-postgresql?topic=databases-for-postgresql-high-availability)
- [The high-availability read-only replica - Databases for PostgreSQL](/docs/databases-for-postgresql?topic=databases-for-postgresql-the-ha-read-only-replica)

## Author section TODO Remove
## Author Links
- [caixa ppt](https://ibm.ent.box.com/file/1475809619207)
- [caixa video](https://ibm.ent.box.com/file/1381668930397?s=1ivunygo476n389ocva8l3dr7kt14mu2)
- [caixa folder](https://ibm.ent.box.com/folder/235605829430)

## Author Questions
- [ ] Why is SC-MZR backups stored in SC-MZR buckets? How do I move these to a different region for restore in case of the SC-MZR failure
- [ ] Not following the logic of the following statement:
> If a single-campus multizone region failure in an MZR or a hardware failure in any region occurs, your data is still accessible as it is replicated onto other fully functioning database servers. Such issues are addressed by IBM Cloud® Specialists in place.
- [ ] `deployment` - service instance
- [ ] only if I have more than one region?  I have to deploy monitoring or what - no customer support?
> If you have deployments in more than one region, you must provision IBM Cloud® Monitoring and enable platform metrics in each region. For more information, see IBM Cloud Monitoring integration.

---
Deleted content
---

## Connection limits
{: #postgresql-connection-limits-ha}

{{site.data.keyword.databases-for-postgresql}} sets the maximum number of connections to your PostgreSQL database to **115**. 15 connections are reserved for the superuser to maintain the state and integrity of your database, and 100 connections are available for you and your applications. After the connection limit is reached, any attempts at starting a new connection results in an error. To prevent overwhelming your deployment with connections, use connection pooling, or scale your deployment and increase its connection limit. For more information, see the [Managing PostgreSQL connections](/docs/databases-for-postgresql?topic=databases-for-postgresql-managing-connections) page.


There is a period of up to 15 seconds where reconnections can fail. At times, unplanned failovers might occur due to unforeseen events on the operating environment. These can take up to 45 seconds, but can be less. In both cases, potential exists for the downtime to be longer.

## High availability, disaster recovery, and SLA resources
{: #ha-recovery-sla}

{{site.data.keyword.databases-for-postgresql}} deployments conform to the {{site.data.keyword.cloud_notm}} Databases [High availability, Disaster recovery, and Service Level Agreement (SLA)](/docs/cloud-databases?topic=cloud-databases-ha-dr) information and terms.
