---

copyright:
  years: 2025
lastupdated: "2025-06-05"

keywords: shared responsibility model, load balancing, global load balancing, HA, DR, high availability, disaster recovery, HA for the platform, high availability for platform, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency and {{site.data.keyword.cloud_notm}}'s Shared Responsibility Model
{: #resiliency-and-shared-responsibility}

{{site.data.keyword.cloud_notm}} operates a [shared responsibility model](/docs/overview?topic=overview-shared-responsibilities). This model covers the different resources that are available to use within {{site.data.keyword.cloud_notm}} and various aspects of those services, namely:

* Incident and Operations Management
* Change Management
* Identity and Access Management
* Security and Regulation Compliance
* Disaster Recovery

Depending on the resource type, responsibility for each of the aspects listed above will fall to either the customer, IBM, or be shared. Where there are shared responsibiities, the customer is responsible for the configuration, and IBM is responsibe for the underlying management. For disaster recovery, the customer is responsible for creating resources in a secondary region and managing the application and data recovery.

For example, IBM has responsibility for the recovery of {{site.data.keyword.cloud_notm}} services in the event of an incident or disaster. This means ensuring that the service is restored, available and working correctly. In many cases, this does not include the restoration of individual service insatances. To illustrate, if IBM Cloud Databases suffers a disaster which requires the restoration of the service in a region, {{site.data.keyword.cloud_notm}} will not recover customer instances (i.e. individual databases). This is a good example of shared responsibility - {{site.data.keyword.cloud_notm}} is responsible for recovering the service, whereas customers are responsible for recovering their instance of that service (including data). Remember, {{site.data.keyword.cloud_notm}} has no access to customer data, including backups.

It's important to be familiar with and to understand your responsibilites when using {{site.data.keyword.cloud_notm}}.

## {{site.data.keyword.cloud_notm}}'s resiliency and disaster recovery responsibilities

In general terms, {{site.data.keyword.cloud_notm}} is responsible for providing a cloud platform and services that are highly available and resilient. This is acheived through the way in which {{site.data.keyword.cloud_notm}} and its services have been architected and deployed. In addition, {{site.data.keyword.cloud_notm}} is responsible for repairing and restoring the platform and cloud services in the event of failures or disaster.

The following table provides some examples of the responibilities that {{site.data.keyword.cloud_notm}} has for resilience and disaster recovery.

| Responsibility Area | Examples |
| - | - |
| Regions and Zones | Providing and maintaining highly redundant, multi-zone regions, including power, cooling and security. Restoration and / or repair of the facilities in the event of an incident or disaster  |
| Cloud Fabric | Providing and maintaining redundant physical hardware, storage, networking and control plane. Repair and / or replacement in the event of an incident or disaster |
| {{site.data.keyword.cloud_notm}} services | Providing and maintaining resilient services such as Virtual Private Cloud, IBM Cloud Databases, IBM Kubernetes Services etc. Recovery of the {{site.data.keyword.cloud_notm}} service in the event of an incident or disaster |
| Monitoring of {{site.data.keyword.cloud_notm}} | Running internal incident and operations management to ensure that {{site.data.keyword.cloud_notm}} is functioning correctly, notifying customers if there are incidents that impact their operations |
| {{site.data.keyword.cloud_notm}} resilience and recovery testing | Design and management of internal testing, run at suitable intervals, that demonstrates the resilience and recoverability of {{site.data.keyword.cloud_notm}} facilities, including regions, zones and services |
{: caption="Examples of {{site.data.keyword.cloud_notm}} resiliency and disaster recovery responsibilities" caption-side="top"}


## Customer's resiliency and disaster recovery responsibilities

In contrast, customers are responsible for deploying their workloads using {{site.data.keyword.cloud_notm}} services in such a way that their business requirements around resiliency and availability can be met. This includes the deployment archtecture used to achieve your availability targets, as well as the backup of customer data (i.e. the data that your application generates, uses and stores in {{site.data.keyword.cloud_notm}}). If something with your workload fails that does not fall within {{site.data.keyword.cloud_notm}}'s stated responsibility, then you are responsible for it's repair, including data recovery.

The following table provides some examples of the responibilities that customers have for resilience and disaster recovery when using {{site.data.keyword.cloud_notm}}.

| Responsibility Area | Examples |
| - | - |
| Workload architecture | Ensuring that workloads have been deployed, configured and use {{site.data.keyword.cloud_notm}} services in such a way that customer resilience goals can be met. For example, deploying a load-balanced workload across multiple zones, or multiple regions |
| Application data | Ensuring that application data, including backups, is stored with appropriate redundancy and replication. For example, the use of read-replica databases or cross-region snapshots. Customers are responsible for the recovery of application data |
| Workload monitoring | Ensuring that workloads are running and performing as expected, using monitoring metrics and logs provided by the {{site.data.keyword.cloud_notm}} platform and / or by customer applications |
| Workload resilience and recovery testing | Ensuring that you have a tested plan for the recovery of workloads in the event of an outage or disaster. This includes aspects such as data recovery, service deployment and configuration and internal communications |
{: caption="Examples of customer resiliency and disaster recovery responsibilities when using {{site.data.keyword.cloud_notm}}" caption-side="top"}

The most important aspects to note from the above table are that:

* Customers are responsible for their own data. At an absolute minimum, regularly ensure that your data is backed up and you know how to restore it, especially in the event of a disaster.
* Customers are responsible for ensuring that thier workloads are resilient to failures. Make sure you review and understand the {{site.data.keyword.cloud_notm}} [Service Level Agreements (SLAs)](/docs/overview?topic=overview-slas) and deploy your workloads using an architecture (zonal, multi-zone, cross region) that meets your resilience targets.
* Customers are responsible for service instance configuration. In the event of a disaster, you may need to reprovison and reconfigure the service instances you use. Ensure that you have documentation that describes how your services are configured and test regularly. Storing as infrastructure as code (e.g. Terraform) is a good practice.
