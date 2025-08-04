---

copyright:
   years: 2020, 2025
lastupdated: "2025-08-04"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation, databases

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Basic DR design for {{site.data.keyword.cloud_notm}} Databases
{: #basic-icd-dr-design}

This page describes two basic design options that can be used for cross-region, {{site.data.keyword.cloud_notm}} Database disaster recovery solutions. You can use these options as a starting point for protecting your {{site.data.keyword.cloud_notm}} Databases against a disaster. Modify and test the options to meet your specific disaster recovery goals.

## Option One
{:icd-dr-option-one}
This option can be used for all {{site.data.keyword.cloud_notm}} databases.

### Architecture
{: #icd-dr-architecture-option-one}

The following diagram shows the basic architecture used in this design. It uses components that are a default parts of the {{site.data.keyword.cloud_notm}} Databases service, namely:

* Automatically scheduled database backups
* {{site.data.keyword.cloud_notm}} Object Storage (COS)

![Diagram showing the basic ICD DR architecture](images/simple-icd-dr-arch2.svg "Diagram depicting a basic view of IBM Cloud Database DR architecture"){: caption=" A basic view of IBM Cloud Database DR architecture" caption-side="bottom"}


### How it works
{: #icd-dr-how-it-works-option-one}

The mechanics of this design is simple. {{site.data.keyword.cloud_notm}} Databases automatically takes a backup of your instances every day. These backups are placed into a cross-regional COS bucket, enabling the backup to be accessed in at least two other regions. You can choose the region that you wish to recover your database instance in (i.e. your recovery region), based on the locations that the backup is ultimately stored in.

To understand the location of stored backups that are created for instances in different regions, see [Managing Cloud Database backups - Backup Locations](/docs/cloud-databases?topic=cloud-databases-dashboard-backups#backup-locations).

This provides a daily backup, which is restorable in a second region in the event of a disaster, providing a restore point to within 24 hours of the disaster. If a lower restore point is required, then optionally, you can take on demand backups of your instances.

The following databases provide Point-In-Time Recovery, by having their transaction logs continuously backed up:

* {{site.data.keyword.cloud_notm}} Databases for PostgreSQL
* {{site.data.keyword.cloud_notm}} Databases for MongoDB
* {{site.data.keyword.cloud_notm}} Databases for MySQL

For these database types, it is possible to restore the last daily backup and then roll forward by applying transaction logs to a particular point in time.

{{site.data.keyword.cloud_notm}} Databases for Elasticsearch does not feature point-in-time recovery.

### How to enact a recovery
{: #icd-enact-recovery-option-one}

Once you have declared a disaster, recovering your database is performed from the {{site.data.keyword.cloud_notm}} console, via the affected instance's dashboard. Recovery is always into a new database instance. As part of the recovery process, you can choose the region where the restored instance will reside. Once the restore is complete, ensure your workloads are reconfigured so that they can connect to the new instance. The restored instance will behave like any other {{site.data.keyword.cloud_notm}} Database instance.

### Failback
{: #icd-failback-option-one}

Once the region that has suffered a disaster is declared fully operational, failing back is a similar process to recovery. The recovered instance will have been automatically backed up, with those backup files placed in a cross-region COS bucket. To failback, simply restore those backups to a new instance in the original region and ensure your workloads are reconfigured so that they can connect to the new instance.

### HPCS backup encryption
{: #icd-dr-hpcs-encryption}

In certain regions, it it possible to encrypt database backups using HPCS. When using HPCS encryption to protect database instance backups, it's important to remember that the HPCS instance must be available in order to decrypt the backup files and restore the database. If a disaster occurs that invokes recovery of the database and affects the availablity of your HPCS instance, then HPCS must be recovered too.


## Option Two
{:icd-dr-option-two}
Option Two uses read-replica databases (also known as 'read-only replicas') to provide a recovery solution. This option can only be used where read-replica databases are supported, for example, with MySQL and PostgreSQL instances. Check the {{site.data.keyword.cloud_notm}} Database Service documentation for details.

### Architecture
{: #icd-dr-architecture-option-two}

The following diagram shows the basic architecture used in this design. It uses functionality that is natively built into the database software.

![Diagram showing the basic ICD DR architecture](images/icd-dr-read-replica.svg "Diagram depicting a basic view of IBM Cloud Database read-replica architecture"){: caption=" A basic view of IBM Cloud Database read-replica architecture" caption-side="bottom"}


### How it works
{: #icd-dr-how-it-works-option-two}

The mechanics of this design is relatively simple and uses functionality that is built into the database software. While the actual mechanics may differ slightly from database flavor to database flavor, the overal functionality is generally similar. When transactions (e.g. record inserts, updates and deletes) are sent to the database to process, they are stored in a transactonal log file before they are written to disk. Whether or not they were committed changes is also logged. The log is sequential. This transactional log file is primarily used for crash recovery - in other words, if a crash occurs, the database will recover itself and replay any committed transactions it finds in the transaction log. Note that structural changes to the database are recorded in the transaction log too - this includes creating new objects such as tables and indexes, as well as dropping or deleting them.

Read-replicas build on this transaction log functionality. When a read-replica is created, it is a point-in-time copy of the originating database. The origninating database is aware of its replicas, and either whole or partial transaction logs (depending on the database flavor and how it implements the funtionality) are copied to the replicas. The replicas then apply the committed transactions it reads from the transmitted log file. The result is a remote copy of the database that is maintained in near real-time.

How close to real-time the read-replica is depends on a number of factors, including network latency and the throughput of transactions on the primary. Normally, a read-replica will be maintained within 15 minutes of an outage.

There are two further advantages to read-replicas.

The first is that they are a read-only copy of the primary database, so they can be read from. If you have workloads which create reports or other read-intensive activities, then the read-ony replica can serve these requests, alleviating the primary of such duties.

The second is that multiple read replicas can be created at once, each fed by the primary instance. This allows you to have multiple disaster recovery sites or to carry out database disaster recovery testing whilst still maintaining other standby instances.

However, there is one major disadvantage with relying solely on read-replicas for your disaster recovery provision and that is that they will replicate any data corruption. If the cause of your disaster is data corruption in any form (e.g. malicious or accidental damage to data, the structure of the database, unwanted encrption and so forth), then the read-replica will be affected too.

### How to enact a recovery
{: #icd-enact-recovery-option-two}

Once you have declared a disaster, the read-replica can be promoted into a fully-funtional read-write database either through the UI, IBM Cloud databases CLI, or through the IBM Cloud databases API. It will normally take around 10 minutes for read-write availability to be completed, though it may take longer before it is highly available, with a full complement of cluster members. Note that once a read-replica is promoted, it cannot be returned to read-replica state. However, once promotion is complete, it can then be used to create other read-replica instances.

Ensure that you point application instances to the promoted read-replica's endpoints.

### Failback
{: #icd-failback-option-two}

Once the region that has suffered a disaster is declared fully operational, failing back can be acheived either by restoring the database from backups or by creating a new read-replica from the current production instance and then promoting that read-replica in the primary region. Again, point application instances to the correct endpoints.


## Testing your DR
{: #icd-dr-testing}

It is important that you regularly test your ability to recover your backups or fail over to any read-replicas that you have created. This will provide assurance that you understand the process and can quickly enact a recovery. Most importantly, it will provide you with some indication as to how long it might take to recover your database in the event of a real disaster. Understanding how long a recovery takes, will help you to understand your Recovery Time Objective (RTO) options.


## Further reading
{: #icd-dr-further-reading}

Additional information on backup, restore, read-replicas and DR for {{site.data.keyword.cloud_notm}} Databases, refer to the following documentation pages:

* [Hyper Protect Crypto Services - Restoring your data from another region](/docs/hs-crypto?topic=hs-crypto-restore-data).
* [Managing Cloud Database backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui)
* [Understanding business continuity and disaster recovery for Cloud Databases](/docs/cloud-databases?topic=cloud-databases-bc-dr)
* [IBM Cloud Databases - Hyper Protect Crypto Services Integration](/docs/cloud-databases?topic=cloud-databases-hpcs#use-hpcs-backups).
* [Configuring read replicas (MySQL)](/docs/databases-for-mysql?topic=databases-for-mysql-read-replicas)
* [The read-ony replica (PostgreSQL)](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas&interface=ui)
