---

copyright:
   years: 2020, 2025
lastupdated: "2025-07-28"

keywords: high availability, ha, understanding ha

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability
{: #understanding-ha}

{{site.data.keyword.cloud_notm}} implements architecture patterns to design {{site.data.keyword.cloud_notm}} services for high availability (HA), helping ensure resilience against different fault types that might impact the distributed {{site.data.keyword.cloud_notm}} infrastructure.

## What is high availability?
{: #what-is-ha}

If you run enterprise workloads in a cloud environment, system components and services need to remain operational and accessible for extended periods without any interruption during planned or unplanned outages. This level of resiliency is known as [high availability](#x2284708){: term}. High availability is achieved by using redundant system components and services, and by distributing these components across different environments. {{site.data.keyword.cloud_notm}} delivers highly available cloud services by using redundant components and eliminating single points of failure.

At the [multizone regions](#x9774820){: term} (MZR) level, HA means having workloads running in multiple [availability zones](#x7018171){: term} in the MZR, which provide the redundancy for local failures.
{: shortdesc}

For HA in the cloud, it's important to understand the high-level concepts that you need to address if you're building or running a highly available cloud system.

## Formula for high availability
{: #ha-formula}

A standard formula for high availability is `MTBF/(MTBF+MTTR)`, where `MTBF` is "mean time between failures" and `MTTR` is "mean time to repair". Since `MTBF` is the time when the system is up, and `MTTR` is the time that the system is down, this formula can be summarized as follows:

> Availability = (The time the system is up)/(The time the system is up + The time the system is down).

The formula is useful because it shows you how to improve availability. Consider a scenario where you require that the server is always available for a month of 30 days. In that month, let's say the server went down once for a total of 1 hr.

> To get the total time in the month, multiply 30 days x 24 hrs/day = 720 hours
> MTBF = Total operating time / Number of failures = 720 hrs / 1 = 720 hrs
> MTTR = Total downtime / Number of failures = 1 hr / 1 = 1 hr
> Availability = (MTBF / (MTBF + MTTR)) x 100 = (720 / 721) x 100 = 99.86%

You can increase MTBF, decrease MTTR, or both. This means that availability can be improved by increasing reliability or fixing issues when they break more efficiently. Both approaches are common in computer systems. The mainframe, which uses high reliability parts, redundant parts, and modularity to allow quicker repairs, shows that significant levels of availability are possible with this approach. For more information about the availability of specific {{site.data.keyword.cloud_notm}} services, see [Service Level Agreements (SLAs)](https://www.ibm.com/support/customer/csol/terms/?id=i126-9268).

![Availability formula.](images/availability-formula.svg "Availability formula"){: caption="Summarized availability formula" caption-side="bottom"}

## Why do you need high availability?
{: #why-ha}

With {{site.data.keyword.cloud_notm}}, you can protect your critical workloads by deploying them onto a resilient, highly available infrastructure. By using the backup, disaster recovery, and high availability features, you can minimize downtime if a severe failure occurs. By creating cloud infrastructures that are protected against single points of failure, and backing up your data, you keep your workloads and applications highly available.

A resilient infrastructure keeps your cloud solution running, even if one or more infrastructure components experience a problem such as:

- Power loss in a data center
- Hardware failure in one of the data centers
- Network failure due to a virtual router issue
- Planned or unplanned maintenance of a service
- Limited blast radius natural disaster
- Ransomware attack on virtual server instance
