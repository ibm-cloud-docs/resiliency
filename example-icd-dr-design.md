---

copyright:
  years: 2021, 2024
lastupdated: "2025-06-20"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation, databases

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Example DR design for {{site.data.keyword.cloud_notm}} Databases
{: #example-icd-dr-design}

This page describes an example design that can be used as a low-cost, cross-region, {{site.data.keyword.cloud_notm}} Database disaster recovery solution.

## Architecture
{: #icd-dr-architecture}

The following diagram shows the basic architecture used in this design. It uses components that are a default parts of the {{site.data.keyword.cloud_notm}} Databases service, namely:

* Automatically scheduled database backups
* {{site.data.keyword.cloud_notm}} Object Storage (COS)

![Diagram showing the basic ICD DR architecture](images/simple-icd-dr-arch2.svg "Diagram depicting a basic view of IBM Cloud Database DR architecture"){: caption=" A basic view of IBM Cloud Database DR architecture" caption-side="bottom"}


## How it works
{: #icd-dr-how-it-works}

The mechanics of this design is simple. {{site.data.keyword.cloud_notm}} Databases automatically takes a backup of your instances every day. These backups are placed into a cross-regional COS bucket, enabling the backup to be accessed in at least two other regions. You can choose the region that you wish to recover your database instance in (i.e. your recovery region), based on the locations that the backup is ultimately stored in.

To understand the location of stored backups that are created for instances in different regions, see [Managing Cloud Database backups - Backup Locations](/docs/cloud-databases?topic=cloud-databases-dashboard-backups#backup-locations).

This provides a daily backup, which is restorable in a second region in the event of a disaster, providing a restore point to within 24 hours of the disaster. If a lower restore point is required, then optionally, you can take on demand backups of your instances.

The following databases provide Point-In-Time Recovery, by having their transaction logs continuously backed up:

* {{site.data.keyword.cloud_notm}} Databases for PostgreSQL
* {{site.data.keyword.cloud_notm}} Databases for MongoDB
* {{site.data.keyword.cloud_notm}} Databases for MySQL

For these database types, it is possible to restore the last daily backup and then roll forward by applying transaction logs to a particular point in time.

{{site.data.keyword.cloud_notm}} Databases for Elasticsearch does not feature point-in-time recovery.

## How to enact a recovery
{: #icd-enact-recovery}

Once you have declared a disaster, recovering your database is performed from the {{site.data.keyword.cloud_notm}} console, via the affected instance's dashboard. Recovery is always into a new database instance. As part of the recovery process, you can choose the region where the restored instance will reside. Once the restore is complete, ensure your workloads are reconfigured so that they can connect to the new instance. The restored instance will behave like any other {{site.data.keyword.cloud_notm}} Database instance.

## Failback
{: #icd-failback}

Once the region that has suffered a disaster is declared fully operational, failing back is a similar process to recovery. The recovered instance will have been automatically backed up, with those backup files placed in a cross-region COS bucket. To failback, simply restore those backups to a new instance in the original region and ensure your workloads are reconfigured so that they can connect to the new instance.

## Testing your DR
{: #icd-dr-testing}

It is important that you regularly test your ability to recover your backups. This will provide assurance that you understand the process and can quickly enact a recovery. Most importantly, it will provide you with some indication as to how long it might take to restore your database in the event of a real disaster. Understanding how long a recovery takes, will help you to understand your Recovery Time Objective (RTO) options.

## Other considerations
{: #icd-dr-other-considerations}

These are other considerations when creating a simple DR strategy for {{site.data.keyword.cloud_notm}} Databases.

### Read-replica databases
{: #icd-dr-read-replicas}

Where available, you may choose to deploy a read replica database. This is a copy of your database instance that can be deployed in a different region of choice. The read replica is asynchronously kept in step with the primary instance.

Read replica databases can provide much faster recovery times in the event of a disaster, since no data recovery is required. However, if a disaster is caused by data corruption, remember that the read-replica is likely to be corrupted too, through synchronization from the primary instance.

Recovery from a read-replica should also be practiced, but note, once a read-replica becomes a primary instance, it cannot transition back to a read-replica state. Instead, the read-replica must be recreated.

### HPCS backup encryption
{: #icd-dr-hpcs-encryption}

In certain regions, it it possible to encrypt database backups using HPCS. When using HPCS encryption to protect database instance backups, it's important to remember that the HPCS instance must be available in order to decrypt the backup files and restore the database. If a disaster occurs that invokes recovery of the database and affects the availablity of your HPCS instance, then HPCS must be recovered too.

For more information see:

* [IBM Cloud Databases - Hyper Protect Crypto Services Integration](/docs/cloud-databases?topic=cloud-databases-hpcs#use-hpcs-backups).
* [Hyper Protect Crypto Services - Restoring your data from another region](/docs/hs-crypto?topic=hs-crypto-restore-data).

## Further reading
{: #icd-dr-further-reading}

For additional information on backup, restore and DR for {{site.data.keyword.cloud_notm}} Databases, refer to the following documentation pages:

* [Managing Cloud Database backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui)
* [Understanding business continuity and disaster recovery for Cloud Databases](/docs/cloud-databases?topic=cloud-databases-bc-dr)
