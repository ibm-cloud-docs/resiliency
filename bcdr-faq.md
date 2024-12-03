---

copyright:
  years: 2024
lastupdated: "2024-12-03"

keywords: disaster recovery, DR, what is disaster recovery, DR strategy, disaster recovery options, disaster recovery strategy

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for disaster recovery
{: #dr-faq}

FAQs for disaster recovery might include questions about what qualifies as a disaster and recovering your workloads in another region. To find all FAQs for {{site.data.keyword.cloud_notm}}, see the [FAQ library](/docs/faqs).
{: shortdesc}

## Is high availability the same as disaster recovery? Are both required?
{: #ha-dr-required}

High availability (HA) and disaster recovery (DR) are easily confused, but they are distinctly different.

Designing high availability into a workload is an effort to prevent a workload from failing. For example, spreading data over multiple disks in a RAID array is an effort to prevent an outage caused by disk failure. Multiple power supplies in a server are an effort to prevent a failure caused by an outage in a power circuit. Running applications across multiple servers in different zones is an effort to prevent an outage that is caused by a server failure and zone failure.

Disaster recovery is the practice of recovering a workload after it fails, despite the availability of resources at its disposal. For example, a workload might be highly available and resilient to failure because it runs across multiple hardware devices in multiple data centers, which might afford the system 99.999% availability. However, it’s still susceptible to disasters such as accidental or malicious data erasure, or a natural disaster that takes out a wide area that includes all the data centers.

Usually, the more highly available a workload needs to be, the more valuable and vital it is to the business. Since a workload is vital to the business, there is a greater need for it to have a well-documented, stringent disaster recovery plan.

## What counts as a disaster?
{: #what-counts-disaster}

An IT disaster is an event that severely disrupts an application or environment, making it unable to function as intended. Disaster events can be short-term or long-lasting, but they typically cause significant harm to the business, either financially, reputationally, or both.

Disaster events might be the result of the following scenarios:

* Natural disasters such as floods, fires, earthquakes.
* Infrastructure problems such as power outages or broader network failures.
* Accidental or malicious actions that delete data, services, or configuration.
* Rolling out a software update that contains a bug or other error that was not anticipated.

In each event, it’s essential to have a recovery plan that restores services within a set time frame and to a point in time before the disaster. It's also important to consider the costs, including both the impact of downtime on the business and the expenses of the recovery solution.

## Can DR take place in one region?
{: #dr-same-region}

Yes, DR can take place in the same region, and it might be faster due to lower latency and no need for cross-region data replication. However, using a separate region is recommended for better resilience. If the primary region is severely impacted, recovery in the same region might not be possible.

{{site.data.keyword.cloud_notm}} seeks to recover services within the region, in line with any published [Service Level Objectives (SLOs)](/docs/resiliency?topic=resiliency-slo). Recovery might take minutes or hours but in the worst-case scenarios, perhaps where there has been physical destruction of property, it can take days, weeks, or months to achieve.

In {{site.data.keyword.cloud_notm}}, if there is an outage where {{site.data.keyword.cloud_notm}} calls a disaster, {{site.data.keyword.cloud_notm}} might have to recover services before you can recover your workloads. If so, you need to add the time it takes for {{site.data.keyword.cloud_notm}} to complete its recovery to the time it takes to do yours. Keep this in mind when you decide whether to rely on just a single region.

## Do all disasters require recovery in a second region?
{: #dr-second-region}

Not all disasters require recovery at a second region. A database administrator might accidentally drop a database table, an automated release might go wrong, or an essential managed cloud service can stop functioning correctly. In these cases, data recovery is key. Depending on how widespread the issue is, how prepared the organization is, and how quickly you need to recover, such a disaster might not force a failover to another region. Depending on the scenario, recovery can be achieved in the same region. Consider multiple disaster scenarios in your plan.

## How can I prevent accidental deletion of services?
{: #prevent-accidental-deletion}

Many {{site.data.keyword.cloud_notm}} services have in-built safeguards against accidental deletion, but you can implement extra protections by using accounts that have appropriate access rights. Use {{site.data.keyword.iamshort}} (IAM) to create accounts with different levels of access and permissions. Then use an appropriate account for the operations task at hand and make sure that other users, such as developers, have only the access and permissions they need.

If you use Terraform, use service IDs that have update access but do not have delete access.
