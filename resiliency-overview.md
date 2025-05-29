---

copyright:
  years: 2024, 2025
lastupdated: "2025-05-29"

keywords: load balancing, global load balancing, HA, DR, high availability, disaster recovery, HA for the platform, high availability for platform, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency in {{site.data.keyword.cloud_notm}}
{: #resiliency-overview}

In information technology (IT), resiliency is broadly defined as the ability of an organization or solution to maintain essential systems and applications and to recover from disruptions. Resiliency in {{site.data.keyword.cloud_notm}} focuses on the perspective of {{site.data.keyword.IBM_notm}} clients, their solution planners, architects, and builders and the resilient solutions that they create on the {{site.data.keyword.cloud_notm}} platform.

Resiliency and availability often go hand in hand. The more resilient a service or workload is, the more available it will typically be. For example, assuming an identical operating environment, a workload that demonstrates 99.9% availability in a given month, might be considered to have a lower resiliency when compared to a workload that demonstrates 99.99% availability.

To achieve higher resiliency - and by extension, higher availability - requires a higher redundancy of components. Consider a scenario where you take a long ride on a bicycle. What happens if you get a puncture?
To make sure you don't have a long walk home, you need to be resilient to punctures. First, you can make sure you have some decent tyres on your bike. Spend a little more on tyres and they may have better puncture resistance built in through a layer of Kevlar. To be more resilient, you can take some tyre levers, a pump and a puncture repair kit with you on your ride. Repairing a puncture with a kit on the roadside can take some time though. So to have less downtime and get back on the bike faster, you might take a spare inner tube instead. This solution costs more in the long run but increases your bike's availability through less downtime by replacing the tube rather than fixing the old one. You could go a step further and invest in 'tubeless' tyres and compatible wheels, which use a silicon sealant to 'self-heal' most punctures on the go, without rider intervention. This typically more expensive solution provides much higher resilience to punctures and zero downtime, assuming the puncture does not cause an abnormal level of tyre damage. To be resilient to massive tyre damage, you need to take a spare tyre with you.... and so on.

Notice in this scenario that the more resilient to failure (in this case, a puncture) you want to be, you need a slighty different solution. As you become more resilient or experience less downtime (i.e. the repair is faster), the more the solution costs.

Taking the cycling example to an extreme, you might decide that to be really resilient to all failures that could occur to your bike, and to experience as little disruption to your ride as possible, you have a support car follow you. The car would have a complete set of spare parts, spare bikes and a mechanic. This is the solution employed by professional bike racers. However, this is a very expensive solution and way out of proportion to the problems that most of us out on our bikes will ever encounter. In other words, there comes a point at which resilience, downtime and cost meet an equalibrium and the solution meets our need.

Just as in these cycling scenarios, resiliency requirements and capabilities for IT workloads must be considered according to how crital the application or solution is for an organization, balanced against the cost of the resiliency solution and the financial or reputational loss that could incur through downtime.

To help customers design resilience into their workloads, {{site.data.keyword.cloud_notm}} publishes  service level objectives (SLOs) for each service at [{{site.data.keyword.cloud_notm}} service level objectives](/docs/resiliency?topic=resiliency-slo). This provides the target SLO for each service as an availability target in a highly available configuration. The underlying architecture design of {{site.data.keyword.cloud_notm}} and its services, provides a highly available platform for your workloads, though it is important that workloads are deployed accordingly, across multiple zones in a region. For example, the availability target for IBM Cloud VPC is listed as 99.999% in the compute services section of our SLO. However, for a workload to take full advantage of that SLO, it must be deployed in a highly available fashion in each of the three zones in a given multi-zone region.

Using that VPC example, to take full advantage of the 99.999% SLO VPC has for your workload, you minimally need three virtual server instances (one in each zone) and a load balancer. Perhaps the cost of that is higher than your budget. To reduce the cost, you could just go for two virtual server instances in two zones and a load balancer - but this reduces the resiliency too. Less cost still would be one virtual server instance and no load balancer but the SLO will be reduced further still.

This also helps illustrate that resiliency is a shared responsibility between {{site.data.keyword.cloud_notm}} and clients that build solutions with {{site.data.keyword.cloud_notm}} services. {{site.data.keyword.cloud_notm}} services provide the component architecture of the solution to provide a high level of resiliency - in the VPC example, the ability to run a workload across three zones with virtual server instances and a load balancer. However, the level of resiliency that your workload can achieve using that service depends on how you architect and deploy your workload. If you deploy your workload on just a single virtual server in a single zone, you cannot expect 99.999% uptime.

It's important to remember that SLOs are objectives and not guarantees. In practice, this means that while our underlying architecture has been built with the objective of 99.999% availability, there are still events which could derail this, however small their liklihood of coming to pass.

Since SLOs do not offer guarantees, they are also not a contractural device. However, IBM Cloud recognises that customers pay for a service and expect that service to be available. Just like other cloud providers, we publish another set of availability figures which are known as [Service Level Agreements (SLAs)](/docs/overview?topic=overview-slas). When using an IBM Cloud service, the SLA describes the availability that you can expect from that service as a minimum. If that minimum is breached, then customers become entitled to service credits, as set out in the SLA. Also set out in the SLA are certain terms that may include the deployment model for a workload and the subsequent credit entitlement based on how the workload has been deployed. Again, it is important that you are familiar with the SLA for the IBM Cloud components that you use and how the SLA might apply in different workload deployment scenarios.

The key message here is that IBM Cloud operates a [shared responsibility model](/docs/overview?topic=overview-shared-responsibilities), where in general terms, IBM Cloud is responsible for the resilience and recovery of the cloud, whereas customers are responsible for the resiliency and recovery of their workloads. This solution guide also includes an [introduction to shared responsibilities](/docs/resiliency?topic=resiliency-resiliency-and-shared-responsibility)

Ensuring that your workloads are resilent and recoverable in {{site.data.keyword.cloud_notm}} may sound daunting. However, the this guide provides an overview of basic concepts and capabilities of IBM Cloud that you need to understand to be successful. This includes includes high availability, disaster recovery, cyber resiliency, and the {{site.data.keyword.cloud_notm}} assurances in these areas. In this guide,you can find everything that you need to help you design, plan, test, and support operational resiliency regulations for your {{site.data.keyword.cloud_notm}} solutions. As you read, you will get a general view of {{site.data.keyword.cloud_notm}} resiliency capabilities and best practices. You can use this knowledge as you design, build and deploy your solutions. In addition, {{site.data.keyword.cloud_notm}} docs provides pages for each service, where you can learn more about service-specific capabilities.

For a broader view of {{site.data.keyword.IBM_notm}} resiliency, see the [{{site.data.keyword.IBM_notm}} Well-Architected Framework](https://www.ibm.com/architectures/well-architected/resiliency){: external}.
