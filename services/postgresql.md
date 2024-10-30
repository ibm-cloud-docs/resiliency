---

copyright:
  years: 2024, 2024
lastupdated: "2024-10-30"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# REMOVE - {{site.data.keyword.databases-for-postgresql}}
# Understanding high availability and disaster recovery

## Service High Availability
{: #service-high-availability}

This service is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **Standard** plan.

{{site.data.keyword.databases-for-postgresql}} provides replication, failover, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. Deployments contain a cluster with two data **members**, a leader and a replica. Both members contain a copy of your data by using asynchronous replication, with a distributed consensus mechanism to maintain cluster state and handle failovers. If the leader becomes unreachable, the cluster initiates a failover, and the replica is promoted to leader and a new replica rejoins the cluster as a replica. The leader and replica will always be in different zones of an MZR. If the replica fails, a new replica created. If a zone failure results in a member failing the new replica will be created in a surviving zone.

You can extend high-availability further by adding [PostgreSQL members](https://cloud.ibm.com/docs/databases-for-postgresql?topic=databases-for-postgresql-horizontal-scaling) to the instance, for greater in-region redundancy, or by provisioning [read-only replicas](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas) for cross-regional failover or read offloading. 

Review the PostgreSQL documentation on [replication techniques](https://www.postgresql.org/docs/current/wal-async-commit.html){: .external} to understand the constraints and tradeoffs that are associated with the asynchronous replication strategy that is deployed by default with {{site.data.keyword.databases-for-postgresql}}.

In scenarios where a database becomes critically unhealthy, such as a server crash on the leader, {{site.data.keyword.databases-for-postgresql}} attempts a failover. This auto failover capability is capped at 16 MB of data lag from leader to replica (a few rows of data once accounting for more PostgreSQL data overhead) and is not performed if the lag threshold is exceeded. If the potential for 16 MB of data loss is intolerable for the application, see [synchronous replication](#sync-repl) below.

Workloads that programmatically access the service instance should follow the [client availability retry logic](/docs/doesnotexist) to maintain availability.

The service will, at times, do controlled failovers under normal operation. These failovers are no-data-loss events that result in resets of active connections. There is a period of up to 15 seconds where reconnections can fail. At times, unplanned failovers might occur due to unforeseen events on the operating environment. These can take up to 45 seconds, but generally less than 30. [Service maintenance](#ibm-service-maintenance) may trigger a controlled failover.

In addition:
- Loss of leader may result in data loss:
   - How does a customer get notified if less than lag threshold with associated fail over occurred?
   - How does a customer get notified if larger than the lag threshold?
- A two member instance will recover from a single zone failure without data loss (other than the lag threshold). During data synchronization for a new replica the instance has exposure to second failure causing data loss. A three member, see [adding PostgreSQL members](/docs/databases-for-postgresql?topic=databases-for-postgresql-horizontal-scaling), is resilient to the failure of two members during that time.


### Synchronous replication
{: #sync-repl}
By default, streaming replication is asynchronous. If the leader crashes, some transactions that were committed might not have been replicated to the replica, causing data loss. {{site.data.keyword.databases-for}} ensures that data loss is kept to a minimum substantial data loss; however, synchronous replication offers the ability to confirm that all changes were made by a transaction have been transferred to a synchronous member, ensuring consistency across a cluster. This consistency come from confirming that writes are written to a secondary before returning to the connecting client with a success. For variables regarding synchronous replication, see [`synchronous_commit`](/docs/databases-for-postgresql?topic=databases-for-postgresql-changing-configuration#gen-settings) on the Changing Configuration page. 

Synchronous replication brings replica availability into the primary write path. If there is no replica to acknowledge a write it will hang until a replica is available. This requires at least three members to function reliably, as synchronous replication is not supported on two-member deployments. You _must_ horizontally scale to at least three members before synchronous replication can be enabled.  See [adding PostgreSQL members](/docs/databases-for-postgresql?topic=databases-for-postgresql-horizontal-scaling)

While unlikely, it is possible that more than one replica might become unavailable simultaneously. If this happens, the primary database will not be able to complete any writes until a replica comes back online, effectively blocking all write traffic to your database. When you are deciding to use synchronous replication, weigh the relative costs and benefits of higher data durability versus potential availability issues.

Employing synchronous replication negatively impacts the performance of the database. Typically, a performant and effective way to employ this feature is by using it only on specific databases or workloads that require the highest degree of data durability available.
{: .note}

## Customer Disaster Recovery - DR
{: #customer-disaster-recovery-dr}

Things to cover in the sections below
- Customer recover from zonal failure. could be detailed like VPC block storage, or could be just a note like ICD - see HA above
- Customer recover from regional failure
- Customer recover from service failure may be same as regional failure
- Customer recover from data corruption
- Customer recovery from BYOK loss this was covered in Secrets Manager is it a general problem


### Customer disaster definition
{: #customer-disaster-definition}

A disaster of an instance can be due to:
- Data corruption.
- Service becomes unavailable.
- Regional disaster.

### Customer disaster planning
{: #customer-disaster-planning}

To recover from a disaster one of the following mechanisms will be used:
- Create a new instance from a backup.
- Create a new instance from a point-in-time of a working database. 
- Promote a read-only replica to a read/write database.

Planning for regional or database resource failure. Consider the framework defined in [Understanding disaster recovery - DR strategy options](/docs/resiliency?topic=resiliency-understanding-dr#dr-categories)

**Active/Nothing** -
Restore from backups is available as described in [Managing Cloud Databases backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups). The restore is to a new database instance with a new connection strings used by clients.
- RTO for this strategy will be partially dependent on the size of the database. Large databases may take hours or days to restore.
- RPO will be dependent on the frequency of backups - 24 hours is the default. Consider a script using [IBM Cloud® Code Engine - Working with the Periodic timer (cron) event producer](/docs/codeengine?topic=codeengine-subscribe-cron) to produce on demand backups to improve RPO.

**Active/Passive** - It is not possible to restore to an existing instance.

**Active/Standby, Active/Active** - Create a [read-only replicas](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas) for cross-regional failover. [Promote the read-only replica](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas&interface=ui#promoting-read-only-replica) to recover from a disaster. New connection strings are required by clients.  RPO times can be just a few seconds.

**Data Disaster**
Data in postgreSQL can be lost due to bugs in software, accidental or malicious operations. See **Active/Nothing** above for recovery. If only a portion of the database has been corrupted consider using the newly created database instance and using 

### Customer disaster recovery
{: #customer-disaster-recovery}

Creating a new database from the live production using [point-in-time recovery](/docs/databases-for-postgresql?topic=databases-for-postgresql-pitr) is possible when the disaster database is available and the RPO (disaster) falls within the supported window.

Following the recovery plan created in the previous section to create a new service instance with a new endpoint.
o recover from a service instance outage a recovery service instance should be created in a recovery region. The recovery service instance is initialized with the data 

should be configured with same data as the source service instance.
Redirect your workload components to the recovered instance or optionally insert into the retry logic to redirect requests to the second instance (Minimal Operation). 


The recovery instance should align with the workload [disaster recovery approaches within IBM Cloud](https://test.cloud.ibm.com/docs/resiliency?topic=resiliency-dr-approaches)


## IBM disaster recovery
{: #ibm-disaster-recovery}

### IBM recovery from zone failure 
{: #ibm-recovery-from-zone-failure}

### IBM recovery from regional failure
{: #ibm-recovery-from-regional-failure}

After a regional failure a service instance is recovered from ...
If dependent storage devices in the region are damaged the service is recovered from backups previously stored in a regional COS bucket - these may be customer visible and managed backups like those in ICD or invisible backups like those kept by Secrets Manager.

## IBM Service Maintenance
{: #ibm-service-maintenance}

All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by [client availability retry logic](/docs/doesnotexist) within client applications. Changes are initially rolled out sequentially on a region-by-region basis.

ADD THIS IF APPROPRIATE:
Complex changes are enabled/disabled with feature flags to control exposure.

Changes that impact customer workloads will be described by notifications. See [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.



## IBM disaster recovery
{: #ibm-disaster-recovery}

### IBM recovery from zone failure 
{: #ibm-recovery-from-zone-failure}

### IBM recovery from regional failure
{: #ibm-recovery-from-regional-failure}

After a regional failure a service instance is recovered from ...
If dependent storage devices in the region are damaged the service is recovered from backups previously stored in a regional COS bucket - these may be customer visible and managed backups like those in ICD or invisible backups like those kept by Secrets Manager.

## IBM Service Maintenance
{: #ibm-service-maintenance}

All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by [client availability retry logic](/docs/doesnotexist) within client applications. Changes are initially rolled out sequentially on a region-by-region basis.

Changes that impact customer workloads will be described by notifications. See [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.





# Deleted content
## Connection limits
{: #connection-limits-ha}

{{site.data.keyword.databases-for-postgresql}} sets the maximum number of connections to your PostgreSQL database to **115**. 15 connections are reserved for the superuser to maintain the state and integrity of your database, and 100 connections are available for you and your applications. After the connection limit is reached, any attempts at starting a new connection results in an error. To prevent overwhelming your deployment with connections, use connection pooling, or scale your deployment and increase its connection limit. For more information, see the [Managing PostgreSQL connections](/docs/databases-for-postgresql?topic=databases-for-postgresql-managing-connections) page.


There is a period of up to 15 seconds where reconnections can fail. At times, unplanned failovers might occur due to unforeseen events on the operating environment. These can take up to 45 seconds, but can be less. In both cases, potential exists for the downtime to be longer.

## High availability, disaster recovery, and SLA resources
{: #ha-recovery-sla}

{{site.data.keyword.databases-for-postgresql}} deployments conform to the {{site.data.keyword.cloud_notm}} Databases [High availability, Disaster recovery, and Service Level Agreement (SLA)](/docs/cloud-databases?topic=cloud-databases-ha-dr) information and terms.
