## Disaster recovery approaches within {{site.data.keyword.cloud_notm}}
{: #dr-approaches}



There are four general approaches to DR in {{site.data.keyword.cloud_notm}} outlined below that will support a disaster recovery plan. These are not intended to be ‘one size fits all’, meaning that a mix and match approach can be taken, based on the workloads, environments and RTO / RPO values the business assigns to them.

As an example, crucial systems which tolerate zero downtime should run in an Active / Active cross-regional configuration, as detailed in Approach 4. Maintaining systems in such a configuration involves most cost and effort though, so Approach 3 will be appropriate for workloads where a few hours downtime can be tolerated, which in turn will reduce the overall cost.  Approach 1 is really only suitable for systems that can tolerate extended downtime – such as non-critical development and test environments – but again, it’s a means to reduce the DR overhead by using the rapid scaling features of cloud computing. So, when considering the DR plan and strategy, consider a mix and match approach, especially where budget is a constraint.

The DR approaches covered focus on VPC and database services in particular. Of course there are many other services available from {{site.data.keyword.cloud_notm}} but the same general principles will apply. When determining an approach, it’s important to understand if and how backups of customer data are taken within the service. Also ensure you understand how those backups are made available to you, how they can be recovered and where there can be recovered.

For example, if the service automatically takes backups of your data, including snapshots, ensure they are placed in cross-region or buckets replicated to another region. Again, this may be done by the service, or you may need to configure such replication. If the service does not automatically take backups of your data, be sure to understand how backups can be taken and stored.  Remember, the IBM Cloud shared responsibility model states that it is the customer's responsiblity to ensure that they have backups of their data, and to ensure that it is recoverable. Refer to appropriate sections in the IBM Cloud Docs for more service-by-service information.

### Approach 1 - Zero Footprint
{: #ZeroFootprint}

Using this approach:

* No infrastructure or services are present at a second region, everything is re-created should a disaster be called
* Server-based data is backed up using a cross-region snapshots
* Database data is recovered through the use of database backups
* Use of Infrastructure as Code via IBM Cloud Schematics is highly recommended, as is the use of Toolchains to deploy application code
* Suitable for least stringent RTO and RPO, though some support for point in time recovery is available for certain databases
* This is the least-cost approach.

With a zero DR footprint, this approach will have the longest overall recovery time, though it does have the lowest cost profile. For many organizations, this will not be an acceptable approach for production workloads, but it may be considered for development, system test workloads or other workloads that need a disaster recovery provision but are low on the list of priorities.

To enable an accurate and faster rebuild of the environment, create and utilise a toolchain that will deploy infrastructure services via {{site.data.keyword.bplong_notm}} and then deploy code to those services, using Git-based code repositories.

To backup and replicate data which is written to block storage volumes or file storage, use Backup for VPC and store as cross-region backup snapshots and/or file storage replication in the first instance, or optionally deploy and configure a Veeam agent on each server or a central Veeam Backup and Replication server (or similar ‘bring your own’ backup software).  Depending on the chosen tool, devise a suitable backup or replication schedule. If using Veeam, write backup files to an Object Storage bucket. The bucket should be cross-regional or, where compliance needs dictate, be configured for replication to another specified bucket hosted in a second region of choice.

For VSIs running Linux operating systems, the bucket can mounted directly using s3fs, based on FUSE. For VSIs running Windows, rclone is the preferred tool.  In each case, the bucket is mounted as a network drive and operates in a similar way to CIFS or NFS shared drive. The installation of the Veeam agent and the bucket mount can be scripted and automated at VSI provision time, using the ‘user-data’ settings. Be sure to use the appropriate endpoint for the bucket.

More complex application data should be stored within a database and IBM Cloud offers several that are suitable for most workloads.

If using IBM Cloud Databases for MySQL, IBM Cloud Databases for PostgreSQL, or IBM Cloud Databases for MongoDB, point in time recovery from backups is available,  Here, the deployment can perform continuous incremental backups and transactions can be replayed to a point in time, within the last 7 days. Other databases will be recoverable to 'point of last backup'. Backups are automatically taken every 24 hours.  Recovery is possible to another MZR in the same geography, so ensure that your chosen DR region is included.

As well as the automatic database backups, customers can take ‘on demand’ backups too. On demand backups can either be taken through the console or via a CLI or API call, which means they can be automated through a job using, for example, IBM Cloud Code Engine. All backups are written to a multi-region object storage bucket.

If using another ‘bring your own’ database, then refer to product documentation on the most suitable backup method. Generally, back up the data to a Cross Region Object Storage bucket, including any logs that provide point in time recovery.

![Diagram depiting an example architecture for a zero footpring DR solution](images/DRApproach1.png "Diagram depiting an example architecture for a zero footpring DR solution"){: caption="Diagram depiting an example architecture for a zero footpring DR solution" caption-side="bottom"}

### Approach 2 - Basic Standby
{: #DRApproach2}

Using this approach:

* Zero-cost Infrastructure is stood up and ready in a second region
* Networking infrastructure that has longer lead times (global load balancers, Direct Link, VPNs or similar) should be stood up and ready
* Limited VSI images may be created and shut down, attracting minimal cost
* Use of autoscaling groups to bring services up more quickly
* Use of read-replica databases, where supported
* Provides a faster RTO, since some services are pre-created but attracts more cost for DR provision

This approach differs to  Approach 1 in that some elements of the DR environment are built out, particularly networking services that have a longer lead time, thereby saving time should DR be called. This is a lower cost option compared to maintaining a full active/passive DR environment.

While still taking an infrastructure as code / IBM Cloud Schematics approach, elements such as the VPC are built out into the DR region. This will include the ‘layout’ of the VPC networks and elements such as access control lists, security groups and so on. This will increase the overall admin overhead since changes to the production environment must be rolled out to the DR environment to ensure consistency.

Optionally, single servers of each type used in the deployment can be provisioned, configured and then shut down. In doing this, runtime costs are avoided, though there will be charges made for any storage consumed. This approach can enable a faster RTO for some services, where instances can be switched on and then scaled using instance templating and autoscaling techniques.

To avoid delays associated with lead times, certain networking elements, where used, should be created. These include items such as Direct Link, which can have significant lead times, VPN connections and global load balancers. While this adds to ongoing costs, it does reduce overall recovery times as configuration can be put in place to quickly re-route traffic to the new region, without needing to make connection changes at the client end.

As before, VSI volumes should either be subject to cross-regional backup snapshots, file storage replication or configured with the Veeam backup agent or similar. Data may then be recovered to the VSIs that are created in the DR region.

If using IBM Cloud Databases for MySQL or IBM Cloud Databases for PostgreSQL, it’s possible to create a read-replica database in the region chosen for DR. Here, a copy of the database is maintained automatically in the second region, which can be turned into a stand-alone copy in the event of a disaster.  This provides a low RTO (minutes) and typically, data loss will be limited to 15 minutes at most, through asynchronous replication.

For other database services, backups must be restored to a new instance at the DR region. Again, it’s recommended that on-demand backups are taken in addition to the automatic daily backup and these can be scheduled as a job using IBM Cloud Code Engine. Note that IBM Cloud Databases for MongoDB can be configured with continuous backup, which is automated.

In each of these cases, data loss will depend on the time of the last available backup.

![Diagram depiting an example architecture for a basic standby DR solution](images/DRApproach2.png "Diagram depiting an example architecture for a basic standby DR solution"){: caption="Diagram depiting an example architecture for a basic standby DR solution" caption-side="bottom"}

### Approach 3 - Minimal Operation
{: #Approach3MinimalOperation}

Using this approach:

* Minimal Infrastructure is stood up and active in a second region, with autoscaling
* Networking infrastructure such as Direct Link, VPN and global load balancers are in place
* Block storage data is frequently restored in the DR region from backups made using Backup for VPC or Veeam
* Database Read replicas are in-place where available.
* Provides a faster RTO, since services are running and data is at least partially restored
* Increased costs

This approach differs to Approach 2 in so far as a minimal service is maintained and running on the DR site, reducing RTO. This includes required networking fabric, such as Direct Link, VPNs and global load balancers, which may otherwise have lead times of several days.

Data backups are applied to the active DR services at a frequency which supports a more aggressive RTO. Again, it may be possible to automate this through jobs running on IBM Cloud Code Engine.

In the event of a disaster, the recovery of outstanding, unapplied backups is carried out and services are scaled to an appropriate level. If using a read replica IBM Cloud Databases for service, then the read replica is transitioned into a stand-alone copy.

![Diagram depicting an example architecture for a minimal operation DR solution](images/DRApproach3.png "Diagram depicting an example architecture for a minimal operation DR solution"){: caption="Diagram depicting an example architecture for a minimal operation DR solution" caption-side="bottom"}

### Approach 4 - Active / Active Cross Regional Services
{: #Approach4-ActiveActive}

Using this approach:

* Two regions are built out and each is actively used
* Databases maintain two copies, one per region
* Provides a fastest RTO, since services are running
* Greatest cost
* Greatest complexity

As implied, this approach basically means running two deployments, each of which is active. This affords little recovery time, other than perhaps scaling up resources at the surviving site but is more complex from a data point of view.

Managing changing application data is the most complex consideration with this approach. Where databases are used, a true active/active deployment will have databases that are read and writable in two regions. In order to keep both copies in synch, the customer application must manage and ensure writes are successfull to both database instances.

![Diagram depicting an example architecture for an Active/Active solution](images/DRApproach4.png "Diagram depicting an example architecture for an Active/Active solution"){: caption="Diagram depicting an example architecture for an Active/Active solution" caption-side="bottom"}

## General DR guidance for common services
{: #general-dr-guidance}

Review the following sections covering general DR guidance for specific {{site.data.keyword.cloud_notm}} products and services.

### IBM Cloud Object Storage
{: #IBMCloudObjectStorage}

Data that is stored in buckets should be periodically backed up to a second bucket location to protect against loss or corruption. This is a different use case to replication, since replication maintains the current state of a bucket, including deletions.

To perform a bucket backup, the current method is to use rclone from a client computer. This can either be a local machine or a VSI based in the cloud. Both the source and the destination bucket must have public HMAC credentials configured.

These backups are performed through the creation of a simple script, typically written in the python language. They can either be run manually or could be set to automatically run through a job scheduler, such as cron.

### Virtual Private Cloud
{: #DRinVPC}

There are three ways to back up data in VPC, depending on storage type. Backup for VPC, file share replication, and Veeam.

#### Backup for VPC
{: #BackupforVPC}

VPC has a built-in service - Backup for VPC - to back up data on attached block storage volumes, which allows the user to create different backup policies based on different frequencies of backup. For example, static data will not need to be backed up at the same frequency to non-static data, which is likely to need several backups per day and so each data type will have a separate backup policy. Up to 10 backup policies can be created per region using Backup for VPC with up to four plans per policy, which can be edited or deleted as necessary.

VPC backups are based on data snapshots. Each is a cumulative snapshot, meaning that a snapshot only contains the changes made to data since the snapshot before it. Therefore, when restoring, multiple snapshots may need to be recovered. Snapshots can be stored as cross-regional copies. When enabling cross-regional copies, it means that a volume can be can be created from the snapshots in a target region, so this can be used in a disaster recovery situation.

Backup snapshots are taken at the volume level and both boot and data volumes can be targeted. A backup policy will have targets added to it and plans within policies can make backups more granular, with different backup points. The most granular backup point that can be defined is once per hour, via a cron expression.

In terms of DR, the backup snapshots can be used to create new VSIs in the DR region (assuming the boot volume has been backed up) and volumes can created / recovered and attached, again using the backup snapshots. If there are multiple block storage volumes attached to the VPC and they all need to be backed up and then recovered to the same point in time, then a consistency group must be used.

In general, backup snapshots offer a good way to capture VSI state for recovery in a second region, in particular boot volume state, but the more backup snapshots that need to be recovered, the slower the process.

#### File Share Replication
{: #FileShareReplication}

While Backup for VPC offers a data replication solution for block storage, file share replication offers a solution for VPC file storage. Once a file share has been created, it's then possible to configure replication for that share, including to another region in the same geography. This replication is asynchronous and the synchronisation frequency is set by the customer using a cron expression, allowing the most frequent replication to be once per hour.

#### Veeam
{: #VPCVeeam}

VPC VSIs can also be backed up using Veeam, which is a third party offering. Veeam is backup and recovery software, which enables users to create much more nuanced backups and recovery, down to the individual file level, compared to snapshots which just create an image of the entire disk, which must be recovered in its entirety. In other words, Veeam allows users to backup and restore single or multiple files, while leaving others alone.

The recommended approach to Veeam on {{site.data.keyword.cloud_notm}} with VPC instances is to install and configure a Veeam agent on each instance. The agent can then be configured to backup the VSI. The backup file that it creates should be written or copied into an Object Storage bucket, which can be attached directly to the VSI.

An alternate recommended approach, where this is a larger deployment of VSIs, is to have a centralized VSI running Microsoft Windows and install Veeam Backup and Replication software to manage backups for multiple server instances, including those running Windows and Linux operating systems.

### VMware Services on IBM Cloud
{: #VMwareonIBMCloud}

IBM Cloud recommends the use of Veeam to back up VMware services on IBM Cloud. Veeam on IBM Cloud is offered as an add-on service for this purpose, although the Veeam on IBM Cloud serivce itself is operated via Veeam – in other words, it is essentially a third party service, supported directly by Veeam.

The service can be deployed on either a Bare Metal Server (the only choice for Regulated Workloads), a public Windows Server-based VSI or a Windows Server VM in the VMware management cluster.

By default, backups are stored on Endurance (file) storage or local disk but optionally, Veeam can be configured to write to an Object Storage Bucket. Here, the user should configure either a cross-region bucket or a bucket with replication to a bucket in an alternate region to provide a capability for DR into another region.

VMware for vSsphere and VMware for vCenter environments can take some time to provision and configure. This should be considered when forming the DR plan and the build out of DR services.

### Container Services
{: #ContainerServices}

The approaches highlighted above largely concentrate on IBM Cloud VPC and IBM Cloud Databases For services. While many environments include both VPC and databases (and by extension, Bare Metal Servers and Classic Infrastructure) many others will be based around container services that include IBM Cloud Kubernetes Service and Red Hat OpenShift on IBM Cloud.

In terms of cluster infrastructure, the approaches above generally hold, from having a zero DR footprint through to having active/active clusters. Care needs to be taken with storage choices though.

Both service types deal with persistent and non-persistent storage. Non-persistent storage is storage that is effectively removed when the container, worker node or cluster is removed. When the storage is removed, the data that goes with it is lost as well, so disaster recovery does not concern itself with the recovery of non-persistent storage and related data.

Persistent storage, on the other hand, is used for data that needs to exist and be kept beyond the lifespan of the container, worker node or cluster that it is attached to and so does come into the scope of disaster recovery.

The means to create DR recoverable copies of persistent data, depends on the type of storage used. The table below provides details.

| Type | Backup Strategy | Replcation Strategy | Suitable RTO/RPO |
| --- | --- | --- | --- |
| Classic File Storage (NFS) | Backup to Object Storage via a backup and restore pod | Bucket replication | RPO to point of last backuo and replication |
| Classic Block Storage | Backup to Object Storage via a backup and restore pod | Bucket replication | RPO to point of last backuo and replication |
| Object Storage |  | Bucket Replication or use a Cross-regional bucket | RPO to point of last replication |
| Block Storage for VPC |	Use kubectl cp to copy files to Object Storage | Bucket Replication or use a Cross-regional bucket |	RPO to point of last backup & replication |
| File Storage for VPC	| Use kubectl cp to copy files to Object Storage	| Bucket Replication or use a Cross-regional bucket	| RPO to point of last backup & replication |
| Portworx (with DR recovery Plan) |	Asynchronous DR	| Clusters deployed in two regions, each with Portworx installation	| RPO up to 15 mins |
| Portworx – PX-Backup |	Backup / recovery software |	Portworx only	 | Point of last backup & replication |
|OADP - OpenShift Operator backup/recovery software |	Write backup to Object Storage bucket	Bucket | Replication or use a Cross-regional bucket |	Point of last backup & replication |
| Valero – Open source tool for backup / recovery / DR for Kubernetes |	Write backup to Object Storage bucket |	Bucket Replication or use a Cross-regional bucket |	Point of last backup & replication |
| Veeam (OpenShift ony)	| Write backup to Object Storage bucket |	Bucket Replication or use a Cross-regional bucket	| Point of last backup & replication |
{: caption="Backup and replication strategy by storage type" caption-side="top"}


For critical container-based applications, Portworx Asynchronous DR is the recommended approach for persistent storage.

Other backups are at the control plane level and the application level.

IBM Cloud takes care of backups at the control plane level – this is essentially a backup of the etcd database in the master cluster, which contains configuration information.

To backup applications, users need to use the ‘backup’ custom resource within OpenShift. IKS users need to bring their own solution such as Kasten or use Portworx

### IBM Cloud Databases
{: #ICD}

Backups are automatically taken of IBM Cloud Databases once per day and are retained and available for 30 days thereafter, unless the database instance itself is deleted, in which case the backups are also removed.

Backups are placed in a cross-regional Object Storage bucket, which enables them to be restored in a second region in the wider geo and even across accounts, if using the API and sufficient account access is granted. Note backups cannot be downloaded - if local copies are needed, then a database flavour specific backup tool is needed. To guard against malicious deletion of a database service, having a local backup is advisable.

Such backups provide ‘at the time of the backup’ restoration points. If a different restoration point is required (i.e. with less than up to 24 hours data loss), then ‘on demand’ backups can be taken either through the console or via CLI or API. Using the CLI or API means they can be scheduled using cron, for example.

A few IBM Cloud Databases (MySQL, PostgreSQL, EnterpriseDB and MongoDB) also provide Point-in-Time recovery, meaning that they can be restored to a particular timestamp within the past 7 days.

Databases for PostgreSQL, EnterpriseDB and MySQL also support Read-only Replicas. These are replicated databases that are set up in a Master / Replica configuration where the master sends changes to the replica and those changes are applied to the replica instance asynchronously. Under normal operation, the replica can only be used for read transactions, so can support read-intensive operations, such as reporting.

From a DR perspective, read-only replicas provide a means to fail over to a database instance at another location relatively quickly, with little or no data restores being necessary. However, if the disaster is caused by data corruption or deletion, then it is likely the replica will also contain the corrupted data or have had the deletion processed, so they should not be wholly relied on for all disaster scenarios.

### IBM Cloudant
{: #IBMCloudant}



While IBM Cloudant instances are highly available, they still require backup to protect against accidental or malicious data loss or corruption. Backup and recovery tooling is not provided as part of the IBM Cloudant dashboard but IBM Cloud recommends the use of CouchBackup, which is an open source, command line tool that is simple to download and use.

CouchBackup can either be used in one-off operations or its use can be scripted and driven by cron or a similar scheduler. Backups are written to files which can be uploaded to an IBM Cloud Object Storage cross-regional bucket or a bucket that is otherwise replicated.

CouchBackup provides recovery to the point of the last backup.

Additionally, IBM Cloudant allows the configuration of replication for cross-regional disaster recovery, where replication can either be one-way or bi-directional. Replication should not be relied on for all disaster scenarios, since data corruption and deletes will be propagated to the replicated version of the database.
