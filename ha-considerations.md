---

copyright:
  years: 2024
lastupdated: "2024-10-23"

keywords: high availability, ha

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability
{: #understanding-ha}

## What is High Availability?
{: #what}

When it comes to running enterprise workloads in a cloud environment, its system components and services remain operational and accessible for an extended periods of time without any interruption during unexpected failures or events. This level of resiliency is known as [high availability](#x2284708){: term}. The two main factors for [High Availability](#x2284708){: term} are, downtime of the whole system endured and how long did the system run without any interruption. [High Availability](#x2284708){: term} requires redundancy by cloning the system components, services and by distributing across many environments.

When it comes to [high availability](#x2284708){: term} in the cloud, it's important to understand the high-level concepts that need to be addressed by anyone who is looking to build or run a highly available cloud system. 


## Formula for high availability
{: #ha-formula}

One useful definition of availability is MTBF/(MTBF+MTTR), where MTBF is Mean Time Between Failures and MTTR is Mean Time To Repair. Since MTBF is the time when the system is up, and MTTR is the time that the system is down, this can be summarized as Availability = (The time the system is up)/(The time the system is up + The time the system is down).

The reason the formula is useful is that it allows us to see how to improve availability. One can increase MTBF, decrease MTTR, or both. Put another way, this means that availability can be improved by increasing reliability, fixing things when they break more quickly, or both. Both approaches have been used with computer systems over the years. The mainframe, which uses high reliability parts, redundant parts, and modularity to allow quicker repairs, shows that significant levels of availability are possible with this approach.

![Availability formula.](images/availability-formula.svg "Availability formula"){: caption="Summarized availability formula" caption-side="bottom"}

## Why do you need high availability?
{: #why}

With IBM Cloud®, you can protect your critical workloads by building resilient highly available infrastructure. Using the backup, disaster recovery, and high availability features, you can minimize your downtime if a severe failure condition happens. You can safeguard your classic or VPC application with multiple availability zones or regions. By creating cloud infrastructures that are protected against a single point of failure, and backing up your data, you keep your workloads and web applications highly available.

A resilient infrastructure keeps your cloud solution running even if one or more areas of the infrastructure experiences a minor or major problem:

- Power loss
- Planned or unplanned maintenance
- Hardware failure
- Network failure
- Natural disaster
- Ransomware
