---

copyright:
   years: 2020, 2026
lastupdated: "2026-07-08"

keywords: load balancing, global load balancing, HA, DR, high availability, disaster recovery, HA for the platform, high availability for platform, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency in {{site.data.keyword.cloud_notm}}
{: #resiliency-overview}

In information technology (IT), resiliency is broadly defined as the ability of an organization or solution to maintain essential systems and applications and to recover from disruptions. Resiliency in {{site.data.keyword.cloud_notm}} focuses on the perspective of {{site.data.keyword.IBM_notm}} clients, their solution planners, architects, and builders and the resilient solutions that they create on the {{site.data.keyword.cloud_notm}} platform.

Resiliency and availability often go hand in hand. The more resilient a service or workload is, the more available it will typically be. For example, assuming an identical operating environment, a workload that demonstrates 99.9% availability in a given month might be considered to have less resiliency when compared to a workload that demonstrates 99.99% availability. To achieve more resiliency — and by extension, higher availability — requires a higher redundancy of components.

## Resiliency scenario
{: #resiliency-scenario}

Consider the following scenario: You are an avid cyclist who frequently takes long bike rides out in the countryside. While the majority of your rides run smoothly and without issue, there is always the possibility for something to go wrong. For example, maybe you run over a sharp object and get a puncture — what happens then? If you're unprepared, you could have a long walk home. But if you've already considered the possibility, you might have fitted tires with higher puncture resistance ahead of time.

To be even more resilient, you might take a puncture repair kit and pump along on your ride. Repairing a puncture on the side of the road could take some time though. So, you decide to take a spare inner tube instead. The solution costs more, but increases your bike's availability by ensuring you are ready to ride again more quickly by replacing the tube instead of fixing the older one. You could always go one step further and invest in tubeless tires that can self-heal some minor punctures on the go without rider intervention. While typically more expensive, the solution provides much higher resilience to punctures and ensures less downtime, assuming there is not an abnormal level of damage.

To be resilient to massive damage, you might decide to take an entire spare tire with you — and so on. Taken to the extreme, you might decide that you need to be fully resilient to any failure that could occur to avoid disruption to your ride, so you employ the solution of professional riders and have a support car follow you. The support car might have a complete set of spare parts, multiple spare bikes, and a mechanic. While this solution ensures near-zero downtime, it's also not proportional to the issues that most of us encounter on an average bike ride. In other words, there comes a point at which resilience, downtime, and cost meet an equilibrium and the solution meets your needs.

Just as in the cycling scenario, resiliency requirements and capabilities for IT workloads must be considered according to how critical the application or solution is for an organization, balanced against the cost of the resiliency solution and the financial or reputational loss that could result from downtime.

## Service level objectives
{: #resiliency-slos}

To help customers design resilience into their workloads, {{site.data.keyword.cloud_notm}} publishes service level objectives (SLOs) for each service at [{{site.data.keyword.cloud_notm}} service level objectives](/docs/resiliency?topic=resiliency-slo). This provides the target SLO for each service as an availability target in a highly available configuration. The underlying architecture design of {{site.data.keyword.cloud_notm}} and its services provides a highly available platform for your workloads, though it is important that workloads are deployed accordingly, across multiple zones in a region.

For example, the availability target for IBM Cloud VPC is listed as 99.999% in the compute services section of the SLO. However, for a workload to take full advantage of that SLO, it must be deployed in a highly available fashion across each of the three zones in a given multi-zone region. Using that VPC example, to take full advantage of the 99.999% SLO for your workload, you minimally need three virtual server instances (one in each zone) and a load balancer. If the cost of that configuration exceeds your budget, you could deploy two virtual server instances in two zones with a load balancer — but this reduces resiliency. Reducing further to one virtual server instance with no load balancer reduces the effective SLO still further.

## Service level agreements
{: #resiliency-slas}

It's important to remember that SLOs are objectives and not guarantees. In practice, this means that while the underlying architecture has been built with the objective of 99.999% availability, there are still events that could affect this, however unlikely they might be. Because SLOs do not offer guarantees, they are also not a contractual device.

However, {{site.data.keyword.cloud_notm}} recognizes that customers pay for a service and expect that service to be available. Like other cloud providers, {{site.data.keyword.cloud_notm}} publishes another set of availability figures known as [Service Level Agreements (SLAs)](/docs/overview?topic=overview-slas). When you use an {{site.data.keyword.cloud_notm}} service, the SLA describes the minimum availability that you can expect from that service. If that minimum is breached, customers become entitled to service credits, as set out in the SLA. The SLA also includes terms that may specify the deployment model for a workload and the credit entitlement based on how the workload has been deployed. It is important that you are familiar with the SLA for the {{site.data.keyword.cloud_notm}} components that you use and how the SLA might apply in different workload deployment scenarios.

## Shared responsibilities
{: #resiliency-shared-responsibilities}

{{site.data.keyword.cloud_notm}} operates a [shared responsibility model](/docs/overview?topic=overview-shared-responsibilities), where in general terms, {{site.data.keyword.cloud_notm}} is responsible for the resilience and recovery of the cloud, whereas customers are responsible for the resiliency and recovery of their workloads. For more information, see [Resiliency and shared responsibilities](/docs/resiliency?topic=resiliency-resiliency-and-shared-responsibility).

## About this guide
{: #resiliency-about-guide}

This guide provides an overview of basic concepts and capabilities that you need to understand to design resilient workloads on {{site.data.keyword.cloud_notm}}. Topics include high availability, disaster recovery, cyber resiliency, and {{site.data.keyword.cloud_notm}} assurances in these areas. You can find everything that you need to help you design, plan, test, and support operational resiliency for your {{site.data.keyword.cloud_notm}} solutions.

The guide covers general {{site.data.keyword.cloud_notm}} resiliency capabilities and best practices. For service-specific capabilities and requirements, refer to each service's own documentation.
