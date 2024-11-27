---

copyright:
  years: 2018, 2024
lastupdated: "2024-11-27"

keywords: resiliency, DR, high availability, disaster recovery, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}


# Best practices for resiliency
{: #resiliency}

Follow best practices for resiliency on {{site.data.keyword.cloud_notm}} to help ensure that your workloads are highly available and that you can recover from a disaster.

## Have a plan
{: #have-a-plan}

Working through any crisis is stressful and disasters are no different. During a disaster, you are likely trying to bring up business-critical services and might be under pressure to do so quickly, which might lead to mistakes. In the unlikely event of a disaster, having a clearly defined plan helps your business recover in a predictable way, which helps alleviate stress and reduces mistakes. For more information about creating a disaster recovery plan, see [Planning for disaster recovery](/docs/resiliency?topic=resiliency-PlanningforDR).

## Determine priorities
{: #work-with-business}

When you create a disaster recovery plan, make sure that your organization endorses, understands, and can fund the plan. When you get broader stakeholder buy in, you can capture the business's priorities if a disaster occurs. This way, the plan is both comprehensive and proportionate to business expectations. Review the plan with your organization regularly to capture changes in priorities and update the plan as needed.

## Understand what qualifies as a disaster
{: #be-clear-what-constitues-a-disaster}

To enact the disaster recovery plan, you need to be sure that what you are experiencing is a disaster. Clearly express what qualifies as a disaster in the plan, otherwise you risk false alarms or inactivity when a disaster occurs. Define several different scenarios that constitute a disaster, how the business can react to them, and how quickly. Also consider the scope of impact. If one component of a workload suffers a disaster, is the response different than a situation where everything fails?

## Communication is key
{: #communication-is-key}

In a disaster situation, effective communication between teams is important. In your plan, clearly state which individuals can call a disaster. Define how they communicate with the organization and the people who are required to enact the disaster recovery plan. Clearly state channels of communication, such as by phone or email, to help ensure that important messages are not missed. Think about backup channels of communication in case primary channels are affected. The plan might also prescribe certain meetings that must take place to capture situation updates.

## Test your plan
{: #test-the-plan}

Reacting to a real disaster isn't the ideal time to test your plan for the first time. Regularly test your plan to help ensure that it works and you understand how long it might take to enact the plan. Where other personnel are involved, testing your plan allows them to better understand their roles. Following the test, make sure that you incorporate any lessons that are learned into the plan. For more information about testing your disaster recovery plan, see [Disaster Recovery Testing](bcdr-testing.md).

## Understand your responsibilities
{: #understand-responsibilites}

Each {{site.data.keyword.cloud_notm}} service has a roles and responsibilities matrix that defines {{site.data.keyword.IBM}}'s responsibilities, customer responsibilities, and shared responsibilities, including responsibilities related to backup, recovery, and disasters. Make sure that you fully understand the ownership of responsibilities for each of the services that you use, since they determine the actions to successfully recover your service instances and help you plan accordingly.

## Consider data resiliency and data residency requirements
{: #data-resiliency-and-residency}

Data resiliency refers to the ability to access, maintain or quickly recover data in the event of failures or disasters. It is related to the previous discussed concepts of High Availability, Disaster Recovery and Cyber Resiliency. 
For more general info on Data Resiliency, see [IBM Well-Architected Framework](https://www.ibm.com/architectures/well-architected/resiliency#Practices){: external}.

Another aspect that needs to be considered is data residency, that is, where the data will be located (not only for your main production environment, but also for backup or recovery) and any restrictions or requirements on its placement. The next section explains {{site.data.keyword.cloud_notm}} capabilities and provides considerations on this topic.

### Understanding data residency in {{site.data.keyword.cloud_notm}}
{: #data-residency}

{{site.data.keyword.cloud_notm}}'s global network of locations provides you with the flexibility of choosing where you want to run your workloads.

For regional and zonal services, you request instances of the service to be deployed in a specific region in accordance with your specific geographic requirements. {{site.data.keyword.cloud_notm}} ensures that content that is provided by you and your workload (as defined in the [{{site.data.keyword.cloud_notm}} Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304&cc=us&lc=en){: external}) is stored and processes locally in the selected region location. For a complete list of the locations where {{site.data.keyword.cloud_notm}} services are available see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

{{site.data.keyword.cloud_notm}} services provide support for saving encrypted backups of the customer content within the location where the regional or zonal service is located for recovery in case of data corruption or a major data center disaster. 

For client's metatada, including client business contact and account usage information (as defined in the [{{site.data.keyword.cloud_notm}} Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304&cc=us&lc=en){: external}), {{site.data.keyword.cloud_notm}} stores and processes them where the control planes of the regional and global services are located.

- Regional services typically have control planes that are located in the same region where you selected for the service except for the services indicated in [Services with global control planes](/docs/overview?topic=overview-zero-downtime#service-global-control-plane).
- Global services control planes locations are indicated in [Global platform services](/docs/overview?topic=overview-zero-downtime#global-platform).

For a complete list of data attributes that are stored and processed by each single {{site.data.keyword.cloud_notm}} service you can refer to the documentation of the API of each service in the [API and SDK reference library](https://cloud.ibm.com/docs?tab=api-docs).

All data in transit is encrypted. Only TLS 1.2 and 1.3 are supported in {{site.data.keyword.cloud_notm}} with TLS 1.1 and below explicitly disabled to prevent downgrading to a vulnerable version of the protocol.

{{site.data.keyword.cloud_notm}} data privacy processing processes and procedures are documented within the {{site.data.keyword.cloud_notm}} DPA. This Data Processing Addendum (DPA) and its applicable DPA Exhibits apply to the Processing of Personal Data by {{site.data.keyword.cloud_notm}} on behalf of Client (Client Personal Data). The processing of Personal Data is subject to the General Data Protection Regulation 2016/679 (GDPR). It is also subject to any other data protection laws that are identified at [Data Protection Laws](https://www.ibm.com/support/customer/csol/terms/?id=DPA-DPL&lc=en){: external} in order to provide services (Services) according to the Agreement between Client and {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.cloud_notm}} DPA can be found at [Data Processing Addendum](https://www.ibm.com/support/customer/csol/terms/?id=Z126-7870&lc=en){: external}.

In addition to the DPA, the cloud services provide DPA exhibits that can be found on the [{{site.data.keyword.cloud_notm}} Terms site](https://www.ibm.com/support/customer/csol/terms/){: external}.


## Design HA and DR into your workloads
{: #design-in-dr}

When you design cloud-deployed workloads, think about high availability and disaster recovery as part of the requirement-gathering stage. By understanding early in the design process what the resiliency qualities of the workload need to be, you can make decisions that influence the architecture and make recovery easier. For example, if you understand the recovery time objective for a workload, you can decide how to deploy a workload to meet that objective by using the features of available services. Similarly, if you understand the recovery point objective you can make better decisions about data, how to back it up, or how to replicate it. Designing this in at an early stage also allow the business to better understand running costs for the workload.

Consider how you can develop application code to make a switching to a disaster recover site easier. For example, avoid hardcoding connection strings or other configurations that might change as a result of connecting to alternative resources.

## Choose the right tools
{: #work-with-the-cloud}

Think of {{site.data.keyword.cloud_notm}} as a toolbox with a set of tools or services that you can use to deploy and run workloads. To use any tool properly, you need to understand what it can and can't do, and choose the right one. If you try to use a tool for a job that it wasn't designed for, something can go wrong. When you design your resiliency plan, understand in as much detail as possible the services that your workload uses, what their capabilities are, and their limits. If a service makes can't meet your RTO or RPO objectives, consider other services or tools that might help close the gap. Also consider whether the objectives that you set are realistic, or whether you are introducing undue complexity and cost into your solution for little real gain.

## Take backups before you make changes
{: #backup-before-changes}

Running IT systems is never risk-free, and introducing change into your workload environment is a point when risk increases. Have a change release plan and a backout plan to manage changes that you make to your environment. As one of the first steps of any change release plan, include taking backups of data and configurations. If something goes wrong during or shortly after the release, you can recover from your backups.

## Stay updated with {{site.data.keyword.cloud_notm}} notifications
{: #IBM-cloud-notifications}

If a disaster occurs that affects an {{site.data.keyword.cloud_notm}} service or region, you receive notifications from {{site.data.keyword.cloud_notm}} in your account or by email. Sign up for notifications on your account by reviewing [View notifications](/docs/account?topic=account-viewing-notifications#subscribe-email-notifications). You can also view the {{site.data.keyword.cloud_notm}} [Status Overview](/status) page.

## Use {{site.data.keyword.cloud_notm}} services for disaster recovery
{: #KeyServicesTitle}

You can use {{site.data.keyword.cloud_notm}} services to facilitate a disaster recovery provision and to enact your plan. Review the following overview of {{site.data.keyword.cloud_notm}} services for disaster recovery. Refer to each service's documentation set for more information.

### {{site.data.keyword.cos_full_notm}}
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

### {{site.data.keyword.bplong_notm}}
{: #KeyServicesSchematics}

Use automation to deploy or recover an environment quickly and accurately. Automating deployments with IaC, Configuration as Code, and toolchains helps you recover workloads faster and more accurately by re-creating lost environments more precisely than manual methods. Using automation also helps you react faster to unexpected scenarios, especially if you need to create and configure services on-demand in a different recovery region than you first planned.

{{site.data.keyword.bpshort}} is a service that can play an important part in any disaster recovery strategy. Use [IBM Cloud Schematics](https://cloud.ibm.com/schematics/overview) to implement runbooks for recovery processes. Schematics workspaces provide Terraform-as-a-Service and Schematics Actions provides Ansible-as-a-Service. [Schematics workspaces](/docs/schematics?topic=schematics-sc-workspaces) automates the deployment and management of IBM Cloud infrastructure and services. Schematics Actions automates configuration management and runs scripted day-2 operations. See [Schematics Use Cases](/docs/schematics?topic=schematics-how-it-works) for more details.

In short, with {{site.data.keyword.bpshort}}, you can deploy infrastructure and services quickly and consistently though Infrastructure as Code (IaC), Terraform, Ansible, Helm, and Red Hat OpenShift Operators.

Codifying the workload environment means defining and automating the setup of your workload by using code. This way, the right services can be quickly provisioned and configured in any {{site.data.keyword.cloud_notm}} region. You can use Terraform and Ansible to provision services, including servers, storage, networking, and databases. They can also be used to configure the services, as can Helm and Operators.

IaC code should be maintained, version-controlled like application code, and stored for access across multiple regions. Using a Git-based repository is recommended.

As with any code, it is best practice not to hardcode elements, such as regions or zone names. Instead use variables, which can be resolved through the {{site.data.keyword.bpshort}} interface.

Employing DevOps toolchains can automate much of the environment build, including configuration, so invest time in considering how you can use these tools.

### Deployable Architectures
{: #KeyServicesDeployableArchitectures}

A deployable architecture is a preconfigured set of IaC assets that are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture. These deployable architectures enable you to meet {{site.data.keyword.cloud_notm}} Framework for Financial Services best practices. With a deployable architecture, you can accurately deploy the same architecture into different regions. These preconfigured architectures also provide a starting point for customization. Make sure that you maintain any customizations throughout each deployment.

When using a deployable architecture, you are responsible for the following disaster recovery actions:

* Provisioning disaster recovery environments, including any dependencies
* Data and configuration backup
* Replicating data and configuration to the disaster recovery environment
* Managing failover operations

### {{site.data.keyword.codeenginefull_notm}}
{: #KeyServicesCodeEngine}

You can use {{site.data.keyword.codeengineshort}} to deploy containerized workloads quickly by recovering certain containerized applications without the need to rebuild an entire Red Hat OpenShift or {{site.data.keyword.cloud_notm}} Kubernetes cluster. You can also use it to configure and run jobs, such as triggering database or server backups.

### Backup and restore options
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

### Create Custom Images
{: #create-customer-images}

 Create a [custom image from a boot volume](/docs/vpc?topic=vpc-image-from-volume-vpc&interface=ui) and use it as the golden image, with preinstalled applications and configurations, to reduce the provisioning time for VPC virtualserver instances in the DR site. The boot volume must be attached to a stopped Virtual Server Instance (VSI) to create the custom image.

 ### Use hostnames for subnets
 {: #use-hostnames-for-subnets}

 Use hostnames and DNS instead of IP addresses to minimize the number of changes that are required to redeploy an application in the DR site, particulary with VPC instances, including virtual server instances. Subnets are zone-specific and do not extend across zones. New IP addresses are assigned to new service instances, which can break any existing configurations such as security rules, application configuration files.

 ### Configure key management services for disaster recovery
 {: configure-key-management-services-for-dr}

 For Key Protect, configure the service in the primary site with failover in the DR region to enable automatic rerouting of Key Protect requests if a regional service outage occurs. Create scripts to update the Virtual Private Endpoint (VPE) settings to access the Key Protect Service, specifically the Internet Protocol (IP) address, as part of disaster recovery procedures.

 For HPCS, configure a failover crypto unit in the DR region. Initialize and configure failover crypto units the same as the operational crypto units before the disaster happens, so they are available if a regional outage occurs
