---

copyright:
  years: 2018, 2024
lastupdated: "2024-11-28"

keywords: resiliency, DR, high availability, disaster recovery, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Using {{site.data.keyword.cloud_notm}} services in your disaster recovery
{: #KeyServicesTitle}

{{site.data.keyword.cloud_notm}} has a number of services that help facilitate a disaster recovery provision. It's important that you understand the capabilities of the {{site.data.keyword.cloud_notm}} services highlighted below. This page should be treated as an overview - refer to each service's documentation set for more information.

## {{site.data.keyword.cos_full_notm}}
{: #KeyServicesCOS}

One key to DR is the ability to retrieve and recover data in the DR region. For many {{site.data.keyword.cloud_notm}} services, you can use {{site.data.keyword.cos_short}} to make data, like backups and snapshots, available in secondary regions by using a cross-regional bucket or a replicated bucket.

Data that is stored in {{site.data.keyword.cos_short}} is highly resilient and replicated across three locations based on the bucket type and its data dispersal method. Single site buckets have data dispersed in three locations in a data center. Regional buckets have data dispersed over the region’s three zones. Cross-region buckets have data dispersed over regions in the geo.

Cross-region buckets offer the simplest choice for disaster recovery and are available as follows:

* `ap-geo` – Asia Pacific (Japan, Australia)
* `us-geo` – North America (US, Canada)
* `eu-geo` – Europe (UK, Germany, Spain)

For a cross-region bucket, a tethered endpoint keeps all data ingress and egress within a specified region while still distributing the data. However, this mechanism does not provide an automated failover if the tethered region fails. Therefore, it’s important to connect services to your buckets by using an endpoint that is available in the region where it’s needed.

Cross-region buckets might not be suitable for users with regulatory compliance concerns. For example, a US-based organization might not be able to store data in Canada. Or, a European Union-based company might not be able to store data outside of the European Union, such as the UK.

In these cases, bucket replication offers a solution. With bucket replication, objects written to a source bucket are asynchronously copied to a target bucket, helping ensure redundancy with eventual consistency. You must set up and manage replication. By using this method, you can control the locations of the data and its sovereignty.

Versioning is a requirement for bucket replication so that buckets that are configured with an immutable {{site.data.keyword.cloud_notm}} policy can't be replicated. Server-side encryption with customer-provided keys is not supported. Where server-side encryption is required, use {{site.data.keyword.keymanagementserviceshort}}.

## {{site.data.keyword.bplong_notm}}
{: #KeyServicesSchematics}

Use automation to deploy or recover an environment quickly and accurately. Automating deployments with IaC, Configuration as Code, and toolchains helps you recover workloads faster and more accurately by re-creating lost environments more precisely than manual methods. Using automation also helps you react faster to unexpected scenarios, especially if you need to create and configure services on-demand in a different recovery region than you first planned.

{{site.data.keyword.bpshort}} is a service that can play an important part in any disaster recovery strategy. Use [IBM Cloud Schematics](https://cloud.ibm.com/schematics/overview) to implement runbooks for recovery processes. Schematics workspaces provide Terraform-as-a-Service and Schematics Actions provides Ansible-as-a-Service. [Schematics workspaces](/docs/schematics?topic=schematics-sc-workspaces) automates the deployment and management of IBM Cloud infrastructure and services. Schematics Actions automates configuration management and runs scripted day-2 operations. See [Schematics Use Cases](/docs/schematics?topic=schematics-how-it-works) for more details.

In short, with {{site.data.keyword.bpshort}}, you can deploy infrastructure and services quickly and consistently though Infrastructure as Code (IaC), Terraform, Ansible, Helm, and Red Hat OpenShift Operators.

Codifying the workload environment means defining and automating the setup of your workload by using code. This way, the right services can be quickly provisioned and configured in any {{site.data.keyword.cloud_notm}} region. You can use Terraform and Ansible to provision services, including servers, storage, networking, and databases. They can also be used to configure the services, as can Helm and Operators.

IaC code should be maintained, version-controlled like application code, and stored for access across multiple regions. Using a Git-based repository is recommended.

As with any code, it is best practice not to hardcode elements, such as regions or zone names. Instead use variables, which can be resolved through the {{site.data.keyword.bpshort}} interface.

Employing DevOps toolchains can automate much of the environment build, including configuration, so invest time in considering how you can use these tools.

## Deployable Architectures
{: #KeyServicesDeployableArchitectures}

A deployable architecture is a preconfigured set of IaC assets that are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture. These deployable architectures enable you to meet {{site.data.keyword.cloud_notm}} Framework for Financial Services best practices. With a deployable architecture, you can accurately deploy the same architecture into different regions. These preconfigured architectures also provide a starting point for customization. Make sure that you maintain any customizations throughout each deployment.

When using a deployable architecture, you are responsible for the following disaster recovery actions:

* Provisioning disaster recovery environments, including any dependencies
* Data and configuration backup
* Replicating data and configuration to the disaster recovery environment
* Managing failover operations

## {{site.data.keyword.codeenginefull_notm}}
{: #KeyServicesCodeEngine}

You can use {{site.data.keyword.codeengineshort}} to deploy containerized workloads quickly by recovering certain containerized applications without the need to rebuild an entire Red Hat OpenShift or {{site.data.keyword.cloud_notm}} Kubernetes cluster. You can also use it to configure and run jobs, such as triggering database or server backups.

## Backup and restore options
{: #backup-and-restore-options}

[IBM Cloud Backup for VPC](/docs/vpc?topic=vpc-backup-service-about&interface=ui) is a cloud service that supports the creation and management of boot and data storage volume snapshots. Use IBM Cloud Backup for VPC to schedule regular backups and restore applications deployed in Virtual Servers for VPC (VSIs) when application-consistent backups are not required.

- Boot Volume backup: Create snapshots of the boot volume for the Virtual Servers for VPC hosting the application. [Restore the bootable snapshots](/docs/vpc?topic=vpc-baas-vpc-restore&interface=ui#baas-restore-concept-boot) and use them to re-create the application virtual server instances. Bootable snapshots are not fully hydrated and impact initial performance. Use the fast restore feature to cache snapshots in a different zone for quick restore of individual volumes.

- Data Volume backup: Create snapshots of the data volumes that are attached to the application. The snapshots replicate the data to other availability zones and regions to support the recovery of configuration or any other critical files. See [Creating Snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui) for details.

[IBM Storage Protect](https://cloud.ibm.com/catalog/content/SPonIBMCloud-20c54034-d319-48c0-beb6-0b4adc54265c-global){: external} is an enterprise-level backup solution for virtual, physical, cloud, software-defined environments, and core applications. Use IBM Storage Protect to create application-consistent backups for database applications that are deployed on Virtual Servers for VPC and to create file or folder level backups.

| Backup feature            | IBM Storage Protect                      | IBM Cloud Backup for VPC                              |
|-------------------------------|--------------------------------|--------------------------------|
| Backup capabilities       | - Agent-based \n - Scheduled backups \n - Backups Management      | - Scheduled backups \n - Backups Management \n - Fast restore clone \n - Cross-regional copies                |
| Backup scope            | Selected VSIs, selected volumes or files in VSIs                        | Selected volumes (boot or data) attached to any VSIs                                         |
| File or folder level support | Yes                                                                     | No                                                                                           |
| Backup storage           | Block Storage for VPC or IBM Cloud Object Storage                                            | IBM Cloud Object Storage                                                                                      |
| Database protection      | Application-consistent backups (Oracle, IBM Db2, MongoDB, MS SQL Server) | Not supported                                                                                |
| Encryption          | In-transit and at rest                                                  | In-transit and at rest                                                                       |
| Recommendation          | Database or folder level backup for multiple virtual servers                             | Complex backup operations for multiple virtual servers that do not require application data consistency |
{: caption="Table 1. Comparison of backup options for Virtual Servers for VPC" caption-side="bottom"}
