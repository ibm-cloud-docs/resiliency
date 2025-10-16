---

copyright:
   years: 2020, 2025
lastupdated: "2025-08-28"

keywords: disaster recovery, dr, disaster recovery strategy

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Disaster recovery approaches
{: #dr-approaches}

Several approaches or strategies to disaster recovery (DR) can support a disaster recovery plan. Depending on your use case, you can mix and match these approaches based on the workloads, environments, Recovery Time Objectives (RTO), and Recovery Point Objectives (RPO) that your organization defines.
{: shortdesc}

Backup and restore are pivotal to any disaster recovery approach. Make sure that you are at least backing up your workload data. In accordance with the {{site.data.keyword.Bluemix_notm}} shared responsibility model, you must have recoverable backups of your data that is available to restore if a disaster occurs. For more information, see [Shared responsibilities for using {{site.data.keyword.cloud_notm}} products](/docs/overview?topic=overview-shared-responsibilities). To make sure that you understand how to access and restore backup files, review each service's documentation.

Waiting for {{site.data.keyword.Bluemix_notm}} to recover an affected region or service is a valid path, but remember it can take many hours or longer. Review the Table 1. to understand DR approaches that offer varying levels of recovery speed, complexity, and cost.

| Approach          | Workload type      | Downtime tolerance      | Cost and complexity |
|-------------------|--------------------|-------------------------|---------------------|
| Zero footprint    | Noncritical        | Extended downtime       | Low                 |
| Basic standby     | Semi-critical      | Several hours           | Medium              |
| Minimal operation | Semi-critical      | A few hours             | Medium              |
| Active/active     | Critical systems   | Zero downtime           | High                |
{: caption="Disaster recovery approaches and possible workload types, downtime tolerence, and cost and complexity to maintain." caption-side="bottom"}

Zero footprint is only suitable for systems that tolerate extended downtime, like noncritical development and test environments. It is a cost-effective option that relies on cloud computing's ability to quickly scale up resources when needed rather than maintain standby systems. Basic standby builds on the zero footprint approach by pre-creating some components which otherwise can take time to provision. Minimal operation is appropriate for workloads where a few hours of downtime can be tolerated, reducing the overall cost. Crucial systems, which tolerate zero downtime, run in an active-active cross-regional active/acitive configuration. Maintaining systems in an active/active configuration involves the most cost and effort. For your DR plan, you can mix and match approaches to suit different workloads, especially where budget is a constraint.

To choose an approach, it’s important to understand if the service automatically takes backups of your data and how backups are taken within the service. Review each service's documentation to understand where the backups are stored, how to restore the backups, and where you can recover them.

If the service automatically takes backups of your data, including snapshots, check that they are stored cross-regionally or in buckets that are replicated to another region. If a service doesn't automatically take backups, you need to configure such backup storage or replication.

Consider the scope of the recovery that you might need for each specific disaster. A disaster might disable an entire region or it might disable a single service instance. Adapt your approach to the scenarios that you plan for and ask yourself the following questions:

* Does my plan allow for the recovery of one or more cloud services in isolation?
* How flexible is my workload with services that are located in different regions?
* If I recover a single service, what is the impact on my workloads, do I need to recover anything else?
* What configuration changes might be needed within my applications to point at different service instances and how simple are is it to make such changes?

Working through different disaster scenarios enriches your planning.

The following DR approaches focus on VPC and database services as examples. The same general principles apply to other {{site.data.keyword.cloud_notm}} services.

## Approach 1: Zero footprint
{: #Approach1-ZeroFootprint}

A zero footprint approach has the longest overall recovery time and the lowest cost profile. For many organizations, zero footprint is not acceptable for production workloads but a good choice for development, system test, or other workloads that are low on the list of recovery priority.

Summary of the zero footprint approach:

* No infrastructure or services are present at a second region. Everything is re-created if a disaster occurs.
* Server-based data is backed up using a cross-region snapshot.
* Database data is recovered by using database backups.
* Use of Infrastructure as Code through IBM Cloud Schematics to deploy your environment.
* Use toolchains to deploy application code.
* Suitable for least stringent RTO and RPO, though some support for point in time recovery is available for certain databases.
* Allows for recovery from data corruption.


To enable an accurate and faster rebuild of the environment, create and use a toolchain that deploys infrastructure services by using {{site.data.keyword.bplong_notm}}. Then, deploy code to those services by using Git-based code repositories.

To backup and replicate data that is written to block storage volumes or file storage, use [Backup for VPC](/docs/vpc?topic=vpc-backups-vpc-best-practices&interface=ui). Verify that the backup is stored as cross-region backup snapshots and file storage replication is active.

An alternative method is to deploy and configure a [Veeam](/docs/vpc?topic=vpc-about-veeam) agent on each server or a central Veeam Backup and Replication server or similar ‘bring your own’ backup software. Depending on the chosen tool, devise a suitable backup or replication schedule. If you decide to use Veeam, write backup files to a {{site.data.keyword.cos_short}} bucket. The bucket must be cross-regional or, where compliance needs dictate, be configured for replication to another specified bucket hosted in a second region of choice.

For VSIs running Linux operating systems, the bucket can be mounted directly by using [`s3fs`](https://github.com/s3fs-fuse/s3fs-fuse), based on FUSE. For VSIs running Microsoft Windows, [Rclone](https://rclone.org) is the preferred tool for direct mounting. Rclone is an open source command prompt tool that is used to manage files in cloud storage, including {{site.data.keyword.cos_short}}. In each case, the bucket is mounted as a network drive and operates in a similar way to a Common Internet File System (CIFS) or Network File System (NFS) shared drive. The installation of the Veeam agent and the bucket mount can be scripted and automated at the VSI provision time, by using the user-data settings. Be sure to use the appropriate endpoint for the bucket.

If you are using {{site.data.keyword.databases-for-mysql}}, {{site.data.keyword.databases-for-postgresql_full_notm}}, or {{site.data.keyword.databases-for-mongodb_full_notm}}, point-in-time recovery from backups is available. Point-in-time recovery works by automatically recording all transactional changes that occur after a full backup in transaction log files or similar. The process starts again when the next full backup is taken. When the database is recovered, the database administrator defines a restore point within the past 7 days. Next, the nearest full backup before that time is restored into a new instance. Finally, the recorded transactional changes are replayed, up to the defined point in time. The last point in time for the recovery is that when the last transactional change was recorded and is available from the source database. Restores of this kind are always rolled forward, not backwards. You cannot restore a backup and roll backwards by undoing transactions. Databases that do not offer point-in-time recovery can be recovered to the point of last backup. Transactional changes are not applied to them, so transactions that take place after the backup are lost on recovery. Backups are automatically taken every 24 hours and backup files are placed in a cross-regional {{site.data.keyword.cos_short}} bucket for resiliency. Recovery is possible to any other multi-zone region, except across compliance zone boundaries.

Database restores can take several hours to complete.

In addition to automatic database backups, customers can take on-demand backups. An automatic backup is taken every 24 hours. Unless point-in-time recovery is available, up to 24 hours-worth of transactions can be lost when a daily backup is restored. On-demand backups that are taken during the 24-hour period reduce transactional data loss. On-demand backups can be completed either through the console, a CLI call or by using an API call. Using CLI or API calls means that backups can be automated through a simple cron job. All on-demand backups are also written to a multi-region {{site.data.keyword.cos_short}} bucket.

If you are using another, bring your own database, refer to product documentation on the most suitable backup method. Generally, back up the data to a cross-region {{site.data.keyword.cos_short}} bucket, including any logs that provide point in time recovery.

![Diagram depicting an example architecture for a zero footprint DR solution](images/DRApproach1.svg "Diagram depicting an example architecture for a zero footprint DR solution"){: caption="Diagram depiting an example architecture for a zero footpring DR solution" caption-side="bottom"}

## Approach 2: Basic standby
{: #DRApproach2-BasicStandby}

The basic standby approach differs from Approach 1 because some elements of the DR environment are built out, particularly networking services that have a longer lead time. The built out elements reduce the overall RTO if DR is called but they attract an ongoing financial cost. The cost of basic standby is reduced compared to maintaining a 'minimal operation' DR environment.

Summary of the basic standby approach:

* Limited infrastructure is provisioned and configured in a second region.
* Networking infrastructure that has longer lead times such as global load balancers, Direct Link, VPNs, or similar is provisioned and configured.
* Limited numbers of VSI images are created and shut down to provide faster recovery yet attract minimal cost.
* Use autoscaling groups to scale quickly.
* Use read-replica databases, where they are supported.
* A faster RTO compared to Approach 1, since some services are pre-created.

Consider implementing Infrastructure as Code (IaC) to deploy both production and DR infrastructure and configuration. Store and maintain IaC source code in a single repository for accurate replication in the DR region. Matching services and configuration in the DR region with the production region avoids problems that are caused through misconfiguration that later take time to identify and fix.

Runtime costs can be reduced for services - in particular, virtual machines - that are provisioned, configured, and then shut down. Use instance templating and autoscaling techniques to quickly scale to support the necessary workload capacity.

To avoid delays associated with lead times, certain networking elements are provisioned. Networking elements include {{site.data.keyword.dl_short}}, which can have significant lead times, VPN connections, and global load balancers. If a disaster is called, configuration can be put in place to quickly reroute traffic to the DR region, without needing to make connection changes at the client end.

As before, to backup and replicate data that is written to block storage volumes or file storage, use Backup for VPC. Verify that the backup is stored as cross-region backup snapshots and file storage replication is active.

An alternative method is to deploy and configure a [Veeam](/docs/vpc?topic=vpc-about-veeam) agent on each server or a central Veeam Backup and Replication server or similar ‘bring your own’ backup software. Depending on the chosen tool, devise a suitable backup or replication schedule. If you decide to use Veeam, write backup files to a {{site.data.keyword.cos_full_notm}} bucket. The bucket must be cross-regional or, where compliance needs dictate, be configured for replication to another specified bucket hosted in a second region of choice.

It’s possible to create a read-replica database in the region chosen for DR for certain IBM Cloud databases. A copy of the database is maintained automatically in the second region, which can be turned into a stand-alone copy if disaster strikes. Read-replicas provide a more stringent RTO, while asynchronous replication from the production database aims to limit any transactional data loss to under 15 minutes.

For other IBM Cloud Database services, the last backup must be restored to a new instance at the DR region, noting that some database types provide point-in-time recovery. For databases that don't provide point-in-time recovery, take on-demand backups in addition to the automatic daily backup to reduce data loss. On-demand backups can be scheduled as a cron job or use IBM Cloud Code Engine. Database restores can take several hours or more, and the time taken increases with the size of the database.

In each of these cases, data loss depends on the time of the last available database backup, which might be up to 24 hours ago.

![Diagram depicting an example architecture for a basic standby DR solution](images/DRApproach2.svg "Diagram depicting an example architecture for a basic standby DR solution"){: caption="Diagram depiting an example architecture for a basic standby DR solution" caption-side="bottom"}

## Approach 3: Minimal operation
{: #Approach3-MinimalOperation}

The minimal operation approach differs from Approach 2 because a minimal service is maintained and running on the DR site, reducing RTO. Included is networking fabric, such as {{site.data.keyword.dl_short}}, VPNs, and global load balancers, which might otherwise have lead times of several days.

Summary of the minimal operation approach:

* Minimal infrastructure is provisioned, configured and active in a second region.
* Autoscaling is used to quickly provision capacity.
* Networking infrastructure such as {{site.data.keyword.dl_short}}, {{site.data.keyword.vpn_vpc_short}}, and global load balancers are in place.
* Data is frequently restored in the DR region.
* Database read replicas are in-place, where available.
* Minimal operation provides a faster RTO, since services are running and data is at least partially restored.

Data backups are applied to the active DR services at a frequency that supports a more aggressive RTO. Backups can be automated through jobs {{site.data.keyword.codeenginefull_notm}} run.

If a disaster is called, the recovery of outstanding, unapplied backups is carried out and services are scaled to an appropriate level. If a read replica IBM Cloud Database is used, the read replica is transformed into a stand-alone copy.

![Diagram depicting an example architecture for a minimal operation DR solution](images/DRApproach3.svg "Diagram depicting an example architecture for a minimal operation DR solution"){: caption="Diagram depicting an example architecture for a minimal operation DR solution" caption-side="bottom"}

## Approach 4: Active/active
{: #Approach4-ActiveActive}

The active/active approach basically means running two deployments, each of which is used in day-to-day operations. Having two production regions eradicates recovery time, other than scaling up resources at the surviving site but is more complex from a data point of view.

Using the active/active approach:

* Two regions are built out and each is actively used.
* Databases maintain two copies, one per region.
* Provides the fastest RTO, since services are running at the second region.
* The approach increases complexity.

Managing changing application data is the most complex consideration with this approach. Where databases are used, a true active/active deployment has databases that are read and writable in two regions. To keep both copies in sync, customer workloads must manage and verify writes are successful to both database instances, by using application code.

Strictly speaking, active/active isn't really an approach to disaster recovery. Instead, it's an approach to help ensure high availability where an IBM Cloud region is removed as a possible single point of failure. If a failure is detected, measures are needed to scale services quickly and for all connection requests to be resolved to the surviving region.

![Diagram depicting an example architecture for an Active/Active solution](images/DRApproach4.svg "Diagram depicting an example architecture for an Active/Active solution"){: caption="Diagram depicting an example architecture for an Active/Active solution" caption-side="bottom"}
