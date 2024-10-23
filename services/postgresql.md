---

copyright:
  years: 2024, 2024
lastupdated: "2024-10-23"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Databases for PostgreSQL - Understanding high availability and disaster recovery

The current [PostgreSQL High availability docs (https://cloud.ibm.com/docs/databases-for-postgresql?topic=databases-for-postgresql-high-availability) are excellent, but missing details below:

### HA
**This service** is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **Standard** plan.

This service is provisioned via the [Resource Controller](/apidocs/resource-controller/resource-controller) see [Resource Controller - Understanding high availability](/docs/doesnotexist).

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


