---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-12"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation, databases

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Designing DR for {{site.data.keyword.cloud_notm}} Databases
{: #basic-icd-dr-design}

Protecting your {{site.data.keyword.cloud_notm}} Databases via a disaster recovery solution can be achieved in any number of ways. However, to help you create a plan, consider starting with one of the following design options for a cross-region solution. Then, you can modify and test the options to meet your specific disaster recovery goals.

## Option 1: Recovery from backup
{: icd-dr-option-one}

In the event of an outage, the recovery from backup option can be used for all {{site.data.keyword.cloud_notm}} Databases.

{{site.data.keyword.cloud_notm}} Databases automatically back up your instances every day. The backups are placed in a cross-regional Cloud Object Storage bucket that enables the backup to be access in at least two other regions. However, if your solution requires a more aggressive recovery point objective than 24 hours, you can choose to back up your data more often. In the event of a disaster, you choose your recovery region based on the locations in which your backup is stored. To learn more about how backup locations are managed, see the [Cloud Databases docs](/docs/cloud-databases?topic=cloud-databases-dashboard-backups#backup-locations).

{{site.data.keyword.cloud_notm}} Databases for PostgreSQL, MongoDB, and MySQL all provide Point-In-Time Recovery that works by having their transaction logs continuously backed-up. If you're working with these database types, it is possible to restore the last daily backup and then apply transaction logs to restore to a specific point in time. {{site.data.keyword.cloud_notm}} Databases for Elasticsearch does not feature point-in-time recovery.


### Understanding the architecture
{: #icd-dr-architecture-option-one}

The following diagram shows the basic architecture that is used in this design. It uses components that are a default parts of the {{site.data.keyword.cloud_notm}} Databases service, namely:

* Automatically scheduled database backups
* {{site.data.keyword.cloud_notm}} Object Storage

![Diagram showing the basic ICD DR architecture](images/simple-icd-dr-arch2.svg "Diagram depicting a basic view of {{site.data.keyword.cloud_notm}} Database DR architecture"){: caption=" A basic view of {{site.data.keyword.cloud_notm}} Database DR architecture" caption-side="bottom"}

### Enacting a recovery
{: #icd-enact-recovery-option-one}

After you declare a disaster, recovering your database is done from the {{site.data.keyword.cloud_notm}} console, by using the affected instance's dashboard. As part of the recovery process, you can choose the region where the restored instance is located but it should always be done into a new instance. After the restore is complete, reconfigure your workloads so that they connect to the new instance.

### Failing back
{: #icd-failback-option-one}

After the region that suffered a disaster is declared fully operational, failing back is a similar process to recovery. The recovered instance is automatically backed up, with those backup files placed in a cross-region Cloud Object Storage bucket. To failback, restore those backups to a new instance in the original region and reconfigure your workloads so that they connect to the new instance.

### Encrypting backups with HPCS
{: #icd-dr-hpcs-encryption}

In certain regions, it's possible to encrypt database backups with HPCS. Here, it's important that the HPCS instance is available to decrypt the backup files and restore the database. If a disaster occurs that requires recovery of the database and affects the availability of your HPCS instance, then HPCS must be recovered too.

## Option 2: Read-replicas
{:icd-dr-option-two}

An alternative to recovery from a backup is to use read-replica databases, also known as read-only replicas, to provide a recovery solution. This option can be used only where read-replica databases are supported such as MySQL and PostgreSQL Databases.

While the configurations might differ slightly between database types the overall functionality is similar. When transactions are sent to the database to process, they are stored in a transactional log file in a sequential order before they are written to the database. Whether the change is committed and structional changes such as the creation or removal of objects are also logged. If a crash occurs, the database recovers itself and replays any committed transactions it finds in the transaction log up to the point of the crash. 

When a read-replica is created, it's a point-in-time copy of the originating database that is built on the transaction log functionality. The originating database is aware of its replicas and either whole or partial transaction logs are copied to the replicas. As a result, a remote copy of your database if maintained in close to real-time. How close depends on several factors, including network latency and the throughput of transactions on the primary. It's not unusual for a read-eplica to be several minutes behind the primary while it ingests the logs it receives.

Read-replicas have several advantages including the fact that multiple read-replicas can be created simultaniously and fed by a primary instance which broadens your potential for multiple disaster recovery sites and gives you the ability to conduct recovery testing while still maintaining other standby instances. A read-replica can also help if your workload creates reports or conducts other read-intensive activities which can alleviate the primary from conducting the work. However, one major disadvantage with relying solely on read-replicas for your disaster recovery provision is that they replicate data - and that includes corrupted data. If the cause of your disaster is data corruption of any form, then the read-replica is corrupted too and you will need to recover from backup, as per the procedure decribed in the previous option.

### Understanding the architecture
{: #icd-dr-architecture-option-two}

The following diagram shows the basic architecture that is used in this design. It uses functionality that is natively built into the database software.

![Diagram showing the basic ICD DR architecture](images/icd-dr-read-replica.svg "Diagram depicting a basic view of {{site.data.keyword.cloud_notm}} Database read-replica architecture"){: caption=" A basic view of {{site.data.keyword.cloud_notm}} Database read-replica architecture" caption-side="bottom"}


### Enacting a recovery
{: #icd-enact-recovery-option-two}

After a disaster is declared, the read-replica can be promoted into a fully functional read/write database either through the UI, {{site.data.keyword.cloud_notm}} databases CLI, or through the {{site.data.keyword.cloud_notm}} databases API. It takes approximately 10 minutes for read/write availability to be completed, though it takes longer before the database is highly available, with a full complement of cluster members. After a read-replica is promoted, it cannot be returned to a read-replica state, but it can then be used to create other read-replica instances.

You must redirect application instances to the promoted read-replica's endpoints.

### Failing back
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
* [{{site.data.keyword.cloud_notm}} Databases - Hyper Protect Crypto Services Integration](/docs/cloud-databases?topic=cloud-databases-hpcs#use-hpcs-backups).
* [Configuring read replicas (MySQL)](/docs/databases-for-mysql?topic=databases-for-mysql-read-replicas)
* [The read-only replica (PostgreSQL)](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas&interface=ui)
