---

copyright:
   years: 2020, 2025
lastupdated: "2025-07-28"

keywords: resiliency, DR, high availability, disaster recovery, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Building a cross region disaster recovery solution with {{site.data.keyword.cloud_notm}} services
{: #KeyServicesTitle}

When you deploy a solution on IBM Cloud, it generally consists of one or more {{site.data.keyword.cloud_notm}} services. When the solution is deployed correctly, the services provide a resilient environment for your workload. However, a number of services can also be used in your disaster recovery design, that might not be part of your workload's deployment architecture. Learn more about those services in the following section.

## {{site.data.keyword.cos_full_notm}}
{: #KeyServicesCOS}

One key to DR is the ability to retrieve and recover data in the DR region. For many {{site.data.keyword.cloud_notm}} services, you can use {{site.data.keyword.cos_short}} to make data, like backups and snapshots, available in secondary regions by using a cross-regional bucket or a replicated bucket.

Data that is stored in {{site.data.keyword.cos_short}} is highly resilient and replicated across three locations based on the bucket type and its data dispersal method. Single site buckets have data dispersed in three locations in a data center. Regional buckets have data dispersed over the region’s three zones. Cross-region buckets have data dispersed over regions in the geo.

Cross-region buckets offer the simplest choice for disaster recovery and are available as follows:

* `ap-geo` – Asia Pacific (Japan, Australia)
* `us-geo` – North America (US, Canada)
* `eu-geo` – Europe (UK, Germany, Spain)

For a cross-region bucket, a tethered endpoint keeps all data ingress and egress within a specified region while still distributing the data. However, this mechanism does not provide an automated failover if the tethered region fails. Therefore, it’s important to connect services to your buckets by using an endpoint that is available in the region where you need it.

Cross-region buckets might not be suitable for users with regulatory compliance concerns. For example, a US-based organization might not be able to store data in Canada. Or, a European Union-based company might not be able to store data outside of the European Union, such as in the UK.

In these cases, bucket replication offers a solution. With bucket replication, objects written to a source bucket are asynchronously copied to a target bucket, helping ensure redundancy with eventual consistency. You must set up and manage replication. By using this method, you can control the locations of the data and its sovereignty.

Versioning is a requirement for bucket replication so that buckets that are configured with an immutable {{site.data.keyword.cloud_notm}} policy can't be replicated. Server-side encryption with customer-provided keys is not supported. Where server-side encryption is required, use {{site.data.keyword.keymanagementserviceshort}}.

## {{site.data.keyword.bplong_notm}}
{: #KeyServicesSchematics}

Use automation to deploy or recover an environment quickly and accurately. Automating deployments with Infrastructure as Code (IaC), Configuration as Code, and toolchains helps you recover workloads faster and more accurately by re-creating lost environments more precisely than manual methods. Using automation also helps you react faster to unexpected scenarios, especially if you need to create and configure services on-demand in a different recovery region than you first planned.

{{site.data.keyword.bpshort}} is a service that can play an important part in any disaster recovery strategy. Use [{{site.data.keyword.bplong_notm}}](/schematics/overview) to implement runbooks for recovery processes. [Schematics workspaces provide Terraform-as-a-Service and automate the deployment and management of {{site.data.keyword.cloud_notm}} infrastructure and services. [{{site.data.keyword.bpshort}} actions](/docs/schematics?topic=schematics-sc-actions) provide Ansible-as-a-Service, automate configuration management, and run scripted day-2 operations. For more information, see [Understanding Schematics use cases](/docs/schematics?topic=schematics-how-it-works).

In short, with {{site.data.keyword.bpshort}}, you can deploy infrastructure and services quickly and consistently though IaC, Terraform, Ansible, Helm, and Red Hat OpenShift Operators.

Codifying the workload environment means defining and automating the setup of your workload by using code. This way, the right services can be quickly provisioned and configured in any {{site.data.keyword.cloud_notm}} region. You can use Terraform and Ansible to provision services, including servers, storage, networking, and databases. They can also be used to configure the services, as can Helm and Operators.

IaC code should be maintained, version-controlled like application code, and stored for access across multiple regions. Using a Git-based repository is recommended.

As with any code, it is best practice not to hardcode elements, such as regions or zone names. Instead use variables, which can be resolved through the {{site.data.keyword.bpshort}} interface.

Employing DevOps toolchains can automate much of the environment build, including configuration, so invest time in considering how you can use these tools.

## Deployable Architectures
{: #KeyServicesDeployableArchitectures}

A deployable architecture is a preconfigured set of IaC assets that are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture. These deployable architectures enable you to meet {{site.data.keyword.cloud_notm}} Framework for Financial Services best practices. With a deployable architecture, you can accurately deploy the same architecture into different regions, which is an important consideration for disaster recovery. Since these preconfigured architectures also provide a starting point for customization, ensure that you maintain any customizations throughout each deployment.

When you use a deployable architecture, you are responsible for the following disaster recovery actions:

* Provisioning disaster recovery environments, including any dependencies
* Data and configuration backup
* Replicating data and configuration to the disaster recovery environment
* Managing failover operations

## Backup and restore options
{: #backup-and-restore-options}

[{{site.data.keyword.cloud}} Backup for VPC](/docs/vpc?topic=vpc-backup-service-about&interface=ui) is a cloud service that supports creating and managing boot and data storage volume snapshots. Use {{site.data.keyword.cloud}} Backup for VPC to schedule regular backups and restore applications deployed in {{site.data.keyword.vsi_is_short}} VSIs when application-consistent backups are not required.

- Boot volume backup: Create snapshots of the boot volume for the {{site.data.keyword.vsi_is_short}} hosting the application. [Restore the bootable snapshots](/docs/vpc?topic=vpc-baas-vpc-restore&interface=ui#baas-restore-concept-boot) and use them to re-create the application virtual server instances. Bootable snapshots don’t immediately load or "hydrate" all of their data when they’re created. Performance degradation occurs during the restoration because your data is copied from {{site.data.keyword.cos_full}} to {{site.data.keyword.block_storage_is_short}} in the background. Use the fast restore feature to cache snapshots in a different zone for quick restore of individual volumes.

- Data volume backup: Create snapshots of the data volumes that are attached to the application. The snapshots replicate the data to other availability zones and regions to support the recovery of configuration or any other critical files. For more information, see [Creating {{site.data.keyword.block_storage_is_short}} snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui).

[{{site.data.keyword.IBM}} Storage Protect](/catalog/content/SPonIBMCloud-20c54034-d319-48c0-beb6-0b4adc54265c-global){: external} is an enterprise-level backup solution for virtual, physical, cloud, software-defined environments, and core applications. Use {{site.data.keyword.IBM}} Storage Protect to create application-consistent backups for database applications that are deployed on {{site.data.keyword.vsi_is_short}} and to create file or folder level backups.

| Backup feature            | IBM Storage Protect                      | IBM Cloud Backup for VPC                              |
|-------------------------------|--------------------------------|--------------------------------|
| Backup capabilities       | - Agent-based \n - Scheduled backups \n - Backups management      | - Scheduled backups \n - Backups management \n - Fast restore clone \n - Cross-regional copies                |
| Backup scope            | Selected VSIs, selected volumes, or files in VSIs                        | Selected volumes (boot or data) attached to any VSIs                                         |
| File or folder level support | Yes                                                                     | No                                                                                           |
| Backup storage           | {{site.data.keyword.block_storage_is_short}} or {{site.data.keyword.cos_full_notm}}                                            | {{site.data.keyword.cos_full_notm}}                                                                                      |
| Database protection      | Application-consistent backups (Oracle, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.mongodb}}, MS SQL Server) | Not supported                                                                                |
| Encryption          | In-transit and at rest                                                  | In-transit and at rest                                                                       |
| Recommendation          | Database or folder level backup for multiple virtual servers                             | Complex backup operations for multiple virtual servers that do not require application data consistency |
{: caption="Comparison of backup options for {{site.data.keyword.vsi_is_short}}" caption-side="bottom"}
