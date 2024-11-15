---

copyright:
  years: 2024
lastupdated: "2024-11-15"

keywords: high availability, ha, understanding ha

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability
{: #understanding-ha}

{{site.data.keyword.cloud_notm}} implements architecture patterns that help guide the design of {{site.data.keyword.cloud_notm}} services to achieve [high availability](#x2284708){: term} (HA) to different fault types that might impact the distributed {{site.data.keyword.cloud_notm}} infrastructure.

## What is high availability?
{: #what-is-ha}

When running enterprise workloads in a cloud environment, system components and services need to remain operational and accessible for extended periods without any interruption during planned or unplanned outages. This level of resiliency is known as [high availability](#x2284708){: term}. High availability is achieved by using redundant system components and services, and by distributing these across different environments. IBM Cloud delivers highly available cloud services by using redundant components and eliminating single points of failure.

At the Multi Zone Region (MZR) level, this means having workloads running in multiple Availability Zones (AZs) in the MZR, which provide the redundancy for local failures.
{: shortdesc}

For HA in the cloud, it's important to understand the high-level concepts that you need to address if you're building or running a highly available cloud system.

## Formula for high availability
{: #ha-formula}

A useful formula for [high availability](#x2284708){: term} is MTBF/(MTBF+MTTR), where MTBF is Mean Time Between Failures and MTTR is Mean Time To Repair. Since MTBF is the time when the system is up, and MTTR is the time that the system is down, this formula can be summarized as Availability = (The time the system is up)/(The time the system is up + The time the system is down).

The formula is useful because it shows you how to improve availability. You can increase MTBF, decrease MTTR, or both. This means that availability can be improved by increasing reliability or fixing issues when they break more efficiently. Both approaches have been used with computer systems over the years. The mainframe, which uses high reliability parts, redundant parts, and modularity to allow quicker repairs, shows that significant levels of availability are possible with this approach.

![Availability formula.](images/availability-formula.svg "Availability formula"){: caption="Summarized availability formula" caption-side="bottom"}

## Why do you need high availability?
{: #why-ha}

With {{site.data.keyword.cloud_notm}}, you can protect your critical workloads by deploying them onto a resilient, highly available infrastructure. By using the backup, disaster recovery, and high availability features, you can minimize downtime if a severe failure occurs. By creating cloud infrastructures that are protected against single points of failure, and backing up your data, you keep your workloads and applications highly available.

A resilient infrastructure keeps your cloud solution running, even if one or more infrastructure components experiences a problem such as:

- Power loss
- Planned or unplanned maintenance
- Hardware failure
- Network failure
- Natural disaster
- Ransomware
