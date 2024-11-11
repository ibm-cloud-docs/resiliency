---

copyright:
  years: 2024
lastupdated: "2024-11-11"

keywords: high availability, ha, understanding ha

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability
{: #understanding-ha}

{{site.data.keyword.cloud_notm}} implements architecture patterns that help guide the design of {{site.data.keyword.cloud_notm}} services to achieve [high availability](#x2284708){: term} (HA) to different fault types that might impact the distributed {{site.data.keyword.cloud_notm}} infrastructure.

## What is high availability?
{: #what-is-ha}

When you run enterprise workloads in a cloud environment, its system components and services remain operational and accessible for an extended period without any interruption during unexpected failures or events. This level of resiliency is known as [high availability](#x2284708){: term}. The two main factors for HA are the downtime that the whole system endures and how long the system runs without any interruption. HA requires redundancy by cloning the system components, services, and by distributing across many environments.
{: shortdesc}

For HA in the cloud, it's important to understand the high-level concepts that you need to address if you're building or running a highly available cloud system.

## Formula for high availability
{: #ha-formula}

A useful formula for [high availability](#x2284708){: term} is MTBF/(MTBF+MTTR), where MTBF is Mean Time Between Failures and MTTR is Mean Time To Repair. Since MTBF is the time when the system is up, and MTTR is the time that the system is down, this formula can be summarized as Availability = (The time the system is up)/(The time the system is up + The time the system is down).

The formula is useful because it shows you how to improve availability. You can increase MTBF, decrease MTTR, or both. This means that availability can be improved by increasing reliability or fixing issues when they break more efficiently. Both approaches have been used with computer systems over the years. The mainframe, which uses high reliability parts, redundant parts, and modularity to allow quicker repairs, shows that significant levels of availability are possible with this approach.

![Availability formula.](images/availability-formula.svg "Availability formula"){: caption="Summarized availability formula" caption-side="bottom"}

## Why do you need high availability?
{: #why-ha}

With {{site.data.keyword.cloud_notm}}, you can protect your critical workloads by building resilient, highly available infrastructure. IBM Cloud supports high availability through its multizone region (MZR) architecture. This is added into many IBM Cloud services and it allows customers to easily deploy workloads across multiple zones in a region, where even the complete failure of a single zone won’t affect the availability of a workload - assuming that the workload is correctly deployed to take full advantage of the MZR.By using the backup, disaster recovery, and high availability features, you can minimize your downtime if a severe failure condition happens. You can safeguard your classic or VPC application with multiple availability zones or regions. By creating cloud infrastructures that are protected against a single point of failure, and backing up your data, you keep your workloads and web applications highly available.

A resilient infrastructure keeps your cloud solution running even if one or more areas of the infrastructure experiences a minor or major problem:

- Power loss
- Planned or unplanned maintenance
- Hardware failure
- Network failure
- Natural disaster
- Ransomware
