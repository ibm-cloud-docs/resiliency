---

copyright:
  years: 2024
lastupdated: "2024-11-05"

keywords: disaster recovery, dr

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Disaster recovery approaches within {{site.data.keyword.cloud_notm}}
{: #dr-approaches}

This page outlines four general approaches or strategies to disaster recovery (DR) support a disaster recovery plan. Depending on your use case, you can mix and match the these based on the workloads, environments, and Recovery Time Objectives (RTO) and Recovery Point Objectives (RPO) values that the business assigns.

The four DR approaches focus on VPC and database services in particular. Of course, many other services are available from {{site.data.keyword.cloud_notm}}, but the same general principles apply. To choose an approach, it’s important to understand if and how backups of your data - the data you are responsible for restoring - are taken within the service. Make sure that you understand how the backups are available to you, how they can be recovered, and where there can be recovered.

As an example, crucial systems, which tolerate zero downtime should run in an active or active cross-regional configuration, as detailed in Approach 4. Maintaining systems in such a configuration involves most cost and effort, so Approach 3 is appropriate for workloads where a few hours downtime can be tolerated, which in turn reduces the overall cost. Approach 1 is only suitable for systems that can tolerate extended downtime – such as noncritical development and test environments – but again, it’s a means to reduce the DR overhead by using the rapid scaling features of cloud computing. Approach 2 builds on the first approach but has some components that are built out, particularly those that can take time to deliver. So, when considering the DR plan and strategy, consider a mix and match approach, especially where budget is a constraint.

If the service automatically takes backups of your data, including snapshots, help ensure that they are placed in cross-region or buckets that are replicated to another region. This might be done by the service, or you might need to configure such replication. If the service does not automatically take backups of your data, be sure to understand how backups can be taken and stored. Remember, the {{site.data.keyword.Bluemix_notm}} shared responsibility model states that it is the customer's responsibility to help ensure that they have backups of their data, and to help ensure that it is recoverable. Review the {{site.data.keyword.Bluemix_notm}} docs for more service-by-service information.

Another consideration to note is the scope of the recovery that you attempt for any given disaster. While this guide may focus on a complete outage of a region, requiring full recovery elsewhere, outages that invoke disaster recovery plans may only befall one or a small number of cloud services. In these cases, consider your approach and the scenarios that you have planned for. Does your plan allow for the recovery of one or more cloud services in isolation, or does it demand that all services are recovered elsewhere, regardless? If you do just recover a single service, how will your workloads react and perform? What configuration changes might be needed and how easy are they to complete? Key to this is working through different disaster scenarios, that enrich your planning accordingly.

There is, of course, a silent fifth approach: wait for the affected region or service to be recovered by {{site.data.keyword.cloud_notm}}. This is a valid path but rememeber that it may take many hours or longer (depending on the cause of the outage) for recovery to complete in the affected region and you may still need to spend futher time restoring your data and so forth, in accordance with the shared responsibility model.

## Approach 1: Zero Footprint
{: #Approach1-ZeroFootprint}

With a zero DR footprint, this approach has the longest overall recovery time, though it does have the lowest cost profile. For many organizations, this is not an acceptable approach for production workloads, but it might be considered for development, system test workloads, or other workloads that need a disaster recovery provision but are low on the list of recovery priority. That said, unless you adopt a read replica instance, this is effectively the only way to recover an IBM Cloud Database service.

By using this approach:

* No infrastructure or services are present at a second region. Everything is re-created if a disaster occurs.
* Server-based data is backed up using a cross-region snapshot.
* Database data is recovered by using database backups.
* Use of Infrastructure as Code through IBM Cloud Schematics is highly recommended, as is the use of toolchains to deploy application code.
* Suitable for least stringent RTO and RPO, though some support for point in time recovery is available for certain databases.
* Allows for recovery from data corruption
* This is the least cost approach.

To enable an accurate and faster rebuild of the environment, create and use a toolchain that deploys infrastructure services by using {{site.data.keyword.bplong_notm}}. Then, deploy code to those services by using Git-based code repositories.

To backup and replicate data, which is written to block storage volumes or file storage, use Backup for VPC and store as cross-region backup snapshots and file storage replication in the first instance. Optionally, deploy and configure a Veeam agent on each server or a central Veeam Backup and Replication server or similar ‘bring your own’ backup software.  Depending on the chosen tool, devise a suitable backup or replication schedule. If you decide to use Veeam, write backup files to a IBM Cloud Object Storage bucket. The bucket should be cross-regional or, where compliance needs dictate, be configured for replication to another specified bucket hosted in a second region of choice.

For VSIs running Linux operating systems, the bucket can be mounted directly by using `s3fs`, based on FUSE. For VSIs running Windows, rclone is the preferred tool for direct mounting.  In each case, the bucket is mounted as a network drive and operates in a similar way to a CIFS or NFS shared drive. The installation of the Veeam agent and the bucket mount can be scripted and automated at the VSI provision time, by using the user-data settings. Be sure to use the appropriate endpoint for the bucket.

More complex application data should be stored within a database and IBM Cloud offers several that are suitable for most workloads.
{: note}

If you are using IBM Cloud Databases for MySQL, IBM Cloud Databases for PostgreSQL, or IBM Cloud Databases for MongoDB, point in time recovery from backups is available. Simply put, this works by a full backup being taken, with subsequent transactional changes being recorded, until the next full backup is taken. This means that when recovering the database, a date and time within the past 7 days can be chosen and the nearest full backup taken prior to that time is restored into a new instance before recorded transactional changes are replayed, up to the desired point in time. The very latest point in time for the recovery is that when the last transactional change was recorded and is available from the source database. Note that restores of this kind are rolled forward, but not backwards. Other databases are recoverable to the point of last backup, meaning they do not then have additional transactional changes applied to them. Backups are automatically taken every 24 hours and backup files placed in a cross-regional object storage bucket for resiliency. Recovery is possible to any other multi-zone region, except across compliance zone boundaries and note that database restores can take several hours to complete.

As well as the automatic database backups, customers can take on-demand backups too. Since automatic backups are taken once every 24 hours, this can result in the loss of up to 24 hours-worth of transactions when restored, if point-in-time restores are not available. Customers wishing to reduce that up to 24 hour figure, are therefore encouraged to take additional on-demand backups. On-demand backups can either be taken through the console or by using a CLI or API call, which means they can be automated through a job by using, for example, IBM Cloud Code Engine. All on-demand backups are also written to a multi-region object storage bucket.

If you are using another, bring your own database, refer to product documentation on the most suitable backup method. Generally, back up the data to a cross region object storage bucket, including any logs that provide point in time recovery.

![Diagram depiting an example architecture for a zero footpring DR solution](images/DRApproach1.png "Diagram depiting an example architecture for a zero footpring DR solution"){: caption="Diagram depiting an example architecture for a zero footpring DR solution" caption-side="bottom"}

## Approach 2: Basic Standby
{: #DRApproach2-BasicStandby}

Using this approach:

* Limited infrastructure is stood up and ready in a second region.
* Networking infrastructure that has longer lead times such as global load balancers, Direct Link, VPNs or similar, should be stood up and ready.
* Limited VSI images may be created and shut down, attracting minimal cost.
* Use autoscaling groups to bring up services more quickly.
* Use read-replica databases, where they are supported.
* Provides a faster RTO, compared to Approach 1 since some services are pre-created, though incurs a higher cost.

This approach differs from Approach 1 because some elements of the DR environment are built out, particularly networking services that have a longer lead time. This helps reduce the RTO if DR is called. This is a lower cost option compared to maintaining a full active and passive DR environment.

Consider using Infrastructure as Code (IaC) to capture production infrastructure changes in a single source repository for accurate replication in the DR region. Use this code to deply elements such as the VPC into the DR region. This includes the layout of the VPC networks and elements such as access control lists, security groups, and more. This increases overall operational overheads, since changes to the production environment must be rolled out to the DR environment to ensure consistency and problems with mis-configuration are not encountered if DR is called.

While calling for a slightly more complex IaC and scripting approach if fully automated, single servers of each type that are used in the deployment can be provisioned, configured and shut down. Runtime costs are then avoided but there are charges made for any storage that's consumed. This approach can enable a faster RTO for some services, where instances can be switched on and then scaled using instance templating and autoscaling techniques.

To avoid delays associated with lead times, certain networking elements should be created. These include items such as Direct Link, which can have significant lead times, VPN connections, and global load balancers. While this adds to ongoing costs, it does reduce overall recovery times as configurations can be put in place to quickly re-route traffic to the new region, without needing to make connection changes at the client end.

As before, to backup and replicate data which is written to block storage volumes or file storage, use Backup for VPC and store as cross-region backup snapshots and file storage replication in the first instance. Optionally, deploy and configure a Veeam agent on each server or a central Veeam Backup and Replication server or similar ‘bring your own’ backup software.  Depending on the chosen tool, devise a suitable backup or replication schedule. If you decide to use Veeam, write backup files to a IBM Cloud Object Storage bucket. The bucket should be cross-regional or, where compliance needs dictate, be configured for replication to another specified bucket hosted in a second region of choice.

It’s possible to create a read-replica database in the region chosen for DR for certain IBM Cloud databases. A copy of the database is maintained automatically in the second region, which can be turned into a stand-alone copy in the event of a disaster. This provides a lower RTO and asynchronous replication from the production database will typically limit any transactional data loss to under 15 minutes.

For other IBM Cloud Database services, the last backup must be restored to a new instance at the DR region, noting that some database types will allow point-in-time recovery within the past 7 days. For databases that don't allow point-in-time recovery, it’s recommended that on-demand backups are taken in addition to the automatic daily backup and these can be scheduled as a job using, for example, IBM Cloud Code Engine. Database restores can take several hours or more, and the time taken increases with the size of the database.

In each of these cases, data loss will depend on the time of the last available database backup, which could be up to 24 hours ago.

![Diagram depiting an example architecture for a basic standby DR solution](images/DRApproach2.png "Diagram depiting an example architecture for a basic standby DR solution"){: caption="Diagram depiting an example architecture for a basic standby DR solution" caption-side="bottom"}

## Approach 3: Minimal operation
{: #Approach3-MinimalOperation}

Using this approach:

* Minimal infrastructure is stood up and active in a second region, with autoscaling
* Networking infrastructure such as Direct Link, VPN, and global load balancers are in place
* Block storage data is frequently restored in the DR region from backups made using Backup for VPC or Veeam.
* Database read replicas are in-place, where available.
* Provides a faster RTO, since services are running and data is at least partially restored.
* Increases the cost.

This approach differs from approach 2 because a minimal service is maintained and running on the DR site, reducing RTO. This includes required networking fabric, such as Direct Link, VPNs, and global load balancers, which might otherwise have lead times of several days.

Data backups are applied to the active DR services at a frequency which supports a more aggressive RTO. It might be possible to automate this through jobs running on IBM Cloud Code Engine.

In the event of a disaster, the recovery of outstanding, unapplied backups is carried out and services are scaled to an appropriate level. If a read replica IBM Cloud Databases for service is used, the read replica is transitioned into a stand-alone copy.

![Diagram depicting an example architecture for a minimal operation DR solution](images/DRApproach3.png "Diagram depicting an example architecture for a minimal operation DR solution"){: caption="Diagram depicting an example architecture for a minimal operation DR solution" caption-side="bottom"}

## Approach 4:  Active/active
{: #Approach4-ActiveActive}

Using this approach:

* Two regions are built out and each is actively used.
* Databases maintain two copies, one per region.
* Provides a fastest RTO, since services are running.
* Increases complexity
* Most costly option

This approach basically means running two deployments, each of which is active. This affords little recovery time, other than scaling up resources at the surviving site but is more complex from a data point of view.

Managing changing application data is the most complex consideration with this approach. Where databases are used, a true active/active deployment has databases that are read and writable in two regions. In order to keep both copies in sync, the customer application must manage and ensure writes are successfull to both database instances.

Strictly speaking, this isn't really an approach to disaster recovery - rather it's an approach to ensuring high availability where an IBM Cloud region is removed as a possibe single point of failure. However, in the event of a failure, measures are needed to ensure that services scale quicky and workloads recognise that one region is unavialable and switch to use services in the remaining region.

![Diagram depicting an example architecture for an Active/Active solution](images/DRApproach4.png "Diagram depicting an example architecture for an Active/Active solution"){: caption="Diagram depicting an example architecture for an Active/Active solution" caption-side="bottom"}
