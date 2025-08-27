---

copyright:
   years: 2020, 2025
lastupdated: "2025-08-27"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation, databases

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Basic DR design for {{site.data.keyword.cloud_notm}} Databases
{: #basic-icd-dr-design}

Here are two disaster recovery design options that can be used for cross-region, {{site.data.keyword.cloud_notm}} Database disaster recovery solutions. You can use these options as a starting point for protecting your {{site.data.keyword.cloud_notm}} Databases against a disaster. Modify and test the options to meet your specific disaster recovery goals.

## Option One - Recovery from Backup
{:icd-dr-option-one}

This option can be used for all {{site.data.keyword.cloud_notm}} databases.

### Architecture
{: #icd-dr-architecture-option-one}

The following diagram shows the basic architecture that is used in this design. It uses components that are a default parts of the {{site.data.keyword.cloud_notm}} Databases service, namely:

* Automatically scheduled database backups
* {{site.data.keyword.cloud_notm}} Object Storage

![Diagram showing the basic ICD DR architecture](images/simple-icd-dr-arch2.svg "Diagram depicting a basic view of IBM Cloud Database DR architecture"){: caption=" A basic view of IBM Cloud Database DR architecture" caption-side="bottom"}


### How it works
{: #icd-dr-how-it-works-option-one}

The mechanics of this design is simple. {{site.data.keyword.cloud_notm}} Databases automatically take a backup of your instances every day. These backups are placed into a cross-regional Cloud Object Storage bucket, enabling the backup to be accessed in at least two other regions. Choose your recovery region based on the locations that the backup is stored in.

To understand the location of stored backups that are created for instances in different regions, see [Managing Cloud Database backups - Backup Locations](/docs/cloud-databases?topic=cloud-databases-dashboard-backups#backup-locations).

The standard automated process provides a daily backup, which is restorable in a second region. It provides a restore point to within 24 hours of the disaster. If a more aggressive restore point is required, then optionally, you can take extra on-demand backups of your instances.

The following databases provide Point-In-Time Recovery (PITR). PITR works by having their transaction logs continuously backed up:

* {{site.data.keyword.cloud_notm}} Databases for PostgreSQL
* {{site.data.keyword.cloud_notm}} Databases for MongoDB
* {{site.data.keyword.cloud_notm}} Databases for MySQL

For these database types, it is possible to restore the last daily backup and then roll forward by applying transaction logs to a particular point in time.

{{site.data.keyword.cloud_notm}} Databases for Elasticsearch does not feature point-in-time recovery.

### How to enact a recovery
{: #icd-enact-recovery-option-one}

After you declare a disaster, recovering your database is done from the {{site.data.keyword.cloud_notm}} console, by using the affected instance's dashboard. Recovery is always into a new database instance. As part of the recovery process, you can choose the region where the restored instance is located. After the restore is complete, reconfigure your workloads so they connect to the new instance. The restored instance behaves like any other {{site.data.keyword.cloud_notm}} Database instance.

### Failback
{: #icd-failback-option-one}

After the region that suffered a disaster is declared fully operational, failing back is a similar process to recovery. The recovered instance is automatically backed up, with those backup files placed in a cross-region Cloud Object Storage bucket. To failback, restore those backups to a new instance in the original region and reconfigure your workloads so that they connect to the new instance.

### HPCS backup encryption
{: #icd-dr-hpcs-encryption}

In certain regions, it is possible to encrypt database backups with HPCS. Here, it's important that the HPCS instance is available to decrypt the backup files and restore the database. If a disaster occurs that requires recovery of the database and affects the availability of your HPCS instance, then HPCS must be recovered too.


## Option Two - Read-replicas
{:icd-dr-option-two}
Option Two uses read-replica databases (also known as 'read-only replicas') to provide a recovery solution. This option can be used only where read-replica databases are supported, for example, with MySQL and PostgreSQL instances. Check the {{site.data.keyword.cloud_notm}} Database Service documentation for details.

### Architecture
{: #icd-dr-architecture-option-two}

The following diagram shows the basic architecture that is used in this design. It uses functionality that is natively built into the database software.

![Diagram showing the basic ICD DR architecture](images/icd-dr-read-replica.svg "Diagram depicting a basic view of IBM Cloud Database read-replica architecture"){: caption=" A basic view of IBM Cloud Database read-replica architecture" caption-side="bottom"}


### How it works
{: #icd-dr-how-it-works-option-two}

The mechanics of this design is relatively simple and uses functionality that is built into the database software. While the actual mechanics differ slightly from database flavor to database flavor, the overall functionality is similar. When transactions such as inserts, updates and deletes are sent to the database to process, they are stored in a transactional log file before they are written to disk. Whether they were committed changes is also logged. The log is sequential. This transactional log file is primarily used for crash recovery. In other words, if a crash occurs, the database recovers itself and replays any committed transactions it finds in the transaction log, up to the point of the crash. Structural changes to the database are recorded in the transaction log too, including the creation or removal of objects such as tables and indexes.

Read-replicas build on this transaction log functionality. When a read-replica is created, it is a point-in-time copy of the originating database. The originating database is aware of its replicas, and either whole or partial transaction logs (depending on the database flavor and how it implements the functionality) are copied to the replicas. The replicas then apply the committed transactions that it reads from the transmitted log file. The result is a remote copy of the database that is maintained in near real-time.

How close to real-time the read-replica is maintained depends on several factors, including network latency and the throughput of transactions on the primary. It is not unusual for a read-replica to be several minutes behind the primary, while it continually ingests the logs it receives.

Read-replicas have two other advantages.

The first is that they are a read-only copy of the primary database, so they can be read from. If you have workloads that create reports or other read-intensive activities, then the read-only replica can serve these requests, alleviating the primary of such duties.

The second is that multiple read replicas can be created simultaneously, each fed by the primary instance. Multiple read-replicas provide the potential for multiple disaster recovery sites or to conduct recovery testing while still maintaining other standby instances.

However, one major disadvantage with relying solely on read-replicas for your disaster recovery provision is that they replicate data - and that includes corrupted data. If the cause of your disaster is data corruption of any form, then the read-replica is corrupted too and you will need to recover from backup, as per the procedure decribed in Option A.

### How to enact a recovery
{: #icd-enact-recovery-option-two}

After a disaster is declared, the read-replica can be promoted into a fully functional read/write database either through the UI, IBM Cloud databases CLI, or through the IBM Cloud databases API. It takes approximately 10 minutes for read/write availability to be completed, though it takes longer before the database is highly available, with a full complement of cluster members. After a read-replica is promoted, it cannot be returned to a read-replica state, but it can then be used to create other read-replica instances.

You must redirect application instances to the promoted read-replica's endpoints.

### Failback
{: #icd-failback-option-two}

Two options are available to failback to the original region after it is operational again. First, by restoring the database from backups or second, by creating a new read-replica from the current production instance and then promoting that read-replica. Again, reconfigure application instances to the correct database endpoints.


## Testing your DR
{: #icd-dr-testing}

Regularly test your ability to recover your backups or fail over to any read-replicas that you create. Doing so provides assurance that you understand the process and can quickly enact a recovery. Most importantly, it provides you with some indication as to how long it might take to recover your database if a real disaster occurs. Understanding how long a recovery takes helps you to understand your Recovery Time Objective (RTO) options.


## Further reading
{: #icd-dr-further-reading}

Additional information on backup, restore, read-replicas, and DR for {{site.data.keyword.cloud_notm}} Databases, refer to the following documentation pages:

* [Hyper Protect Crypto Services - Restoring your data from another region](/docs/hs-crypto?topic=hs-crypto-restore-data).
* [Managing Cloud Database backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui)
* [Understanding business continuity and disaster recovery for Cloud Databases](/docs/cloud-databases?topic=cloud-databases-bc-dr)
* [IBM Cloud Databases - Hyper Protect Crypto Services Integration](/docs/cloud-databases?topic=cloud-databases-hpcs#use-hpcs-backups).
* [Configuring read replicas (MySQL)](/docs/databases-for-mysql?topic=databases-for-mysql-read-replicas)
* [The read-only replica (PostgreSQL)](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas&interface=ui)
