---

copyright:
  years: 2021
lastupdated: "2024-10-14"

keywords: DR for IBM Cloud, disaster recovery, common mistakes for disaster recovery, plan a disaster recovery strategy

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Planning for Disaster Recovery
{:PlanningforDR}

To be effective, a disaster recovery solution should be planned and designed in such a way that it meets both business and technical requirements. In doing this, it should be accepted that a balance may need to be struck between the two. For example, a business requirement may be technically unfeasable to implement, or perhaps the technical implementation becomes cost prohibitive for the business. The starting point for any disaster recovery solution should be a disaster recovery plan.

## What do I need to consider for my DR Plan?
{: #DRPlan}

A disaster recovery plan is a set of procedures and strategies that an organization follows to recover from a disaster or unexpected event. It is a critical document that outlines the steps that an organization will take to minimize the impact of a disaster and get back to normal operations as quickly as possible. A disaster recovery plan is primarily a business plan and requires input from stakeholders across the business organisation, not just the IT department. It also requires appropriate funding and resourcing to ensure that it remains a living, rather than a one-off document.

A DR plan will usually cover the following:

1. Risk Assessment
* Identify potential disasters and the likelihood of them occurring.
* Evaluate the impact that each disaster would have on the organization.

2. Emergency Response
* Designate a disaster management team.
* Establish communication protocols.
* Identify emergency response procedures.

3. Business Continuity
* Develop a business continuity plan that includes procedures for keeping the organization running during a disaster.
* Identify key personnel and their roles in the recovery process.
* Establish procedures for communication with customers, vendors, and other stakeholders.

4. Cloud Environment Recovery
* Establish procedures for the provision of replacement services.
* Establish procedures for the provision of standby services.
* Establish procedures for the scaling of standby services.

5. Data Backup and Recovery
* Establish backup procedures for critical data and systems.
* Ensure that backup systems are regularly tested and updated.
* Develop procedures for recovering lost data and systems.

6. Training and Testing
* Provide regular training to employees on disaster recovery procedures.
* Conduct regular testing of the disaster recovery plan.
* Update the disaster recovery plan as necessary based on testing and new risks identified.

By following these steps and regularly reviewing and updating the disaster recovery plan, an organization can be better prepared to respond to, and recover from disasters. Testing under a range of scenarios, will also help avoid the common pitfalls that can render plans unusable under real conditions. Let's look at that next.

# Avoiding common disaster recovery pitfalls
{: #common-pitfalls}

To ensure that your plans provide the results that you require in the event of a disaster, be aware of the following common disaster recovery pitfalls when planning your solution.
{: shortdesc}

## Plan and design for the “best conditions”
{: #best-conditions}

It is important that the solution and the testing methodology are planned and designed to mimic what the real conditions might be.

A DR plan is a set of policies, tools, and procedures that enable organizations to recover from and continue the operation of vital technology systems following a natural or human-induced disaster. The business continuity and disaster recovery (BCDR) infrastructure capacity should be able to manage the workloads of the production sites, unless your plan dictates otherwise. Assuming that this condition will not occur might seem attractive from a cost perspective, but it can make the effects of a disaster much more severe when there is not enough capacity to manage normal business. Of course, services in the cloud are scalable, so you can add capacity later, but also be aware that you may not be the only customer vacating to the region due to the disaster, which may cause short-term capcity shortages.

Also, avoid reusing dismissed production components or lower grade and quality assets to build the DR solution, since it will affect the reliability of the solution. The amount of time that this solution needs to manage production is not predictable, and this variable introduces more risks.


## Avoiding single-point-of-failure (SPOF)
{: #spof}

SPOF can be anywhere in a solution, not only on the technology side. Your solution might depend on people, vendors, providers, and other external dependencies. You must know and identify clearly in advance most of your SPOFs and have a plan to mitigate your dependencies. Be prepared to discover SPOFs during the first sessions of your [disaster recovery test](/docs/overview?topic=overview-dr-testing).

Among SPOFs, provider risk is a condition to consider in your DR plan. When you have both production and DR on the same provider, your risk condition is increased and must be carefully considered.

## Having only a plan A
{: #plan-a}

Risks can't be eliminated; they can be only mitigated. The more risk that you mitigate with a single solution, the more the solution would cost. Thus, any solutions have a residual risk, as budget for disaster recovery is normally a finite or limited number.

Having a plan B with a different [recovery time objective](#x3167918){: term} (RTO) might help to mitigate additional risks, like increased resiliency and not add too much cost to the budget.

Your plan B RTO might not be the one expected in plan A, meaning you will need more time to restart your operations, and thus you will have more economic impact by the emergency. But, restarting later might be far better than not restarting at all.

A possible plan B, in a two sites topology, might include a periodic backup that is secured off-site from the two sites and at a distance to be considered reasonably safe.

## Preparing a poor DR testing methodology
{: #poor-dr-testing}

An untested DR solution is like running blindfolded in the woods. There are high chances that you will encounter an obstacle that stops your running. Moreover, you discover that only during the most important run of your life.

If testing is essential to guarantee that you have a valid solution, then the conditions you are going to test are also important and may require you to create several testing scenarios.

If you only plan to perform a DR test by doing a planned close of operation on one site and a well-organized restart on the other, you are sure your DR tests works, but is this what will happen during an emergency?

You should design your tests to mimic the potential emergency conditions as closely as possible by simulating a so-called “rolling disaster condition”, where your workload is impacted progressively by the emergency. This is the best way to test your solution and have a reasonable understanding if it is resilient and has the ability to resist stress conditions.

# Disaster Recovery Solution Considerations
{: #dr-considerations}

When designing the technical aspects of a disaster recovery solution, there are a number of factors that you should consider to ensure that it is fit for purpose. Here, we discuss some of them.

## High availability
{: #HighAvailability}

As mentioned in a previous section, high availability does not equate to disaster recovery. However, {{site.data.keyword.cloud_notm}} recommends, as a best practice, that customers ensure their deployments are architected to be highly available, taking advantage of {{site.data.keyword.cloud_notm}}’s Multizone Regions (MZRs). Each MZR has a minimum of three zones, which are highly interconnected, yet operationally separate datacenters.  In many cases, issues and outages are likely to affect only a single zone, as opposed the whole region, so deploying as a matter of course across all zones in an MZR can diminish the need to call a disaster at all. Failover to a second region can be a major process, as is failing back, so it's wise take as many steps as possible to prevent needing to do it. Remember though, MZRs won’t necessarily guard against disasters such as data corruption or malicious damage.

When planning and implementing your disaster recovery environment, consider its own resiliency requirements too. It may be tempting to lower costs by reducing the high availability requirements of the DR implementation, but if you do have cause to enact DR it needs to be suitable to run production workloads. Ensure that you don't compromise the ability for the DR environment to meet the high availability requirements of your workloads but remember you can scale up to meet demand.

## Regulation and Compliance
{: #RegulationAndCompliance}

Some workloads and data may be subject to strict industry regulation and compliance standards, which can affect the physical location in which they can be run or stored. When choosing a location for a disaster recovery site, ensure that the chosen region meets any necessary regulation and compliance. This includes locations to which you choose to replicate data.

While cross regional Object Storage buckets offer a way to replicate data to different regions, use these with care if your data is subject to territorial restrictions, to ensure your data is not transferred to a region that is in a country or territory which breaches organizational or industry compliance rules.


## Capacity in the DR Region
{: #CapacityinDRRegion}

If a disaster strikes that causes you to enact your disaster recovery plan, be aware that many other customers may also be affected and may also be enacting their disaster recovery plans. This could put a severe strain on the capacity of the nearest region(s) to the region that has failed. For example, if us-south were to fail, then it’s likely many customers will try and rebuild in the us-east region first. If eu-gb were to fail, then there may be extra pressure on eu-de and so on.

This increase in local demand could hamper recovery efforts and increase recovery times for a period of time.

To avoid such problems, either consider building out as much required capacity as possible in the DR region of choice (though this comes with cost implications) or consider using other locations, such as those which are not near-neighbour regions. Note though, that some services, such as IBM Cloud Databases, backup data to cross-regional Object Storage buckets, which the service 'owns' and cannot be customer configured. This may place a constraint on the DR locations you can choose from.

## Connectivity considerations
{: #ConnectivityConsiderations}

Certain services, such as Direct Link and VPNs may take some time to provision as they can require third party activity or further installation of physical infrastructure. When creating a DR plan, ensure that the lead times for such services are taken into consideration and understand that it may be optimal to have these services pre-provisioned and ready.

## All, some or nothing?
{: #AllorNothing}

The cloud is comprised of different services, and an application may use multiple services. While this guide tends to focus on the catastrophic failure of the whole region and implies that everything in that region is lost, this may not always be the case and individual services may suffer a catastrophic outage in isolation to others. In these cases, it’s still important to understand whether there’s a need to failover to another region and if that’s the case, whether the whole application needs to fail over or just the component that has had the catastrophic failure.

For example, suppose the database service has suffered a catastrophic failure. Does that necessitate failing over the whole application, web services and all, or can the remaining services be easily switched to point at an enlivened read-replica database that was on standby in another region? The ease of this may come down to how the application has been developed and configured, remembering not to hard-code service names and so on, as well as other considerations, such as network latency.

## Failback
{: #Failback}

There’s an additional need to consider the failback position once the dust has settled on a disaster and this should also be documented in the DR plan. This includes:

* whether failback should be performed - could you just continue to run in the second region and rebuild your DR provision in the first?
* how quickly should failback be attempted?
* how should failback be enabled to create as little further disturbance to users as possible?

Whether failback is enacted or not, the DR provision will need to be recreated or reset, so the activities required to do this need to be considered.

# Key {{site.data.keyword.cloud_notm}} services that help facilitate DR
{: #KeyServicesTitle}

There are a number of {{site.data.keyword.cloud_notm}} services which can be used to facilitate a disaster recovery provision and to enact the plan, if needed. In this section, we explore some of them.

## {{site.data.keyword.cos_full_notm}}
{: #KeyServicesCOS}

Key to DR is the ability to recover data and key to recovering data is being able to retrieve it in the DR region. For many {{site.data.keyword.cloud_notm}} services, Object Storage is used to make data, particularly backups and snapshots, available in secondary regions, using either a cross-regional bucket or a replicated bucket.

Everything that is stored in {{site.data.keyword.cos_full_notm}} is highly resilient by being stored in three locations, to a degree based on the bucket type and its related data dispersal type. Single site buckets have data dispersed in three locations in a data center. Regional buckets have data dispersed over the region’s three zones. Cross-region buckets have data dispersed over regions in the geo.

Cross-region buckets offer the simplest choice for disaster recovery and are available as follows:

* ap-geo – Asia Pacific (Japan, Australia)
* us-geo – North America (USA, Canada)
* eu-geo – Europe (UK, Germany, Spain)

When addressing a cross-region bucket, a tethered endpoint will keep all data ingress and egress within a specified region, while still distributing the data. However, this does not provide an automated failover if the tethered region fails. Therefore, it’s important to connect services to your buckets using an endpoint that will be available in the region where it’s needed.

Cross Region buckets may not suitable for all users with regulatory compliance concerns. Take for example a US based organization that cannot store data in Canada, or a European-based one that cannot store data outside of the European Union - in the UK, for example.

In these cases, bucket replication offers a solution. Here, objects from a specified source bucket are replicated on write to a specified target bucket, with eventual consistency (asynchronous replication). Replication must be set up and managed by the user but using this method, users can control the location(s) of the data and its sovereignty.

Versioning is a requirement for bucket replication, so buckets that are configured with an immutable object storage policy cannot be replicated.  Server-side encryption with customer-provided keys is not supported. Where server-side encryption is required, Key Protect must be used.

## {{site.data.keyword.bplong_notm}}
{: #KeyServicesSchematics}

Automation can play a important role in deploying or recovering an environment quickly and accurately. Customers that automate large parts of their deployments through Infrastructure as Code (IaC), Configuration as Code and deployment Toochains, are likely to recover workloads faster than those that do not becasue they will be more accurate representations of the lost environment. Using automation will also enable you to react faster to unexpected disaster scenarios, especially if you need to create and configure services on-demand, perhaps even in a different region from that at first planned.

{{site.data.keyword.bpshort}} is a key service that can play an important part in any disaster recovery strategy. {{site.data.keyword.bpshort}} provides the ability to deploy infrastructure and services quickly and consistently though IaC, via Terraform, Ansible, Helm and OpenShift Operators.

Codifying the environment means that it can be quickly provisioned and configured in any {{site.data.keyword.cloud_notm}} region. Terraform and Ansible can be used to provision services including servers, storage, networking and databases. They can also be used to configure the services, as can Helm and Operators.

IaC source will need to be maintained and version controlled in the same manner as any other application code and stored in such a way that it is available to multiple regions. It’s recommended that a Git-based code repository is used.

As with any code, it is best practice not to hard-code elements, such as regions or zone names and instead use variables, which can be resolved through the Schematics interface.

Employing DevOps toochains can automate a lot of the environment build too – including configuration - so it is worth investing time in considering how these tools can be used.


## Deployable Architectures
{: #KeyServicesDeployableArchitectures}

{{site.data.keyword.cloud_notm}} now provides a number of deployable architectures. A deployable architecture is a preconfigured set of IaC assets that are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture and so enable customers to follow the best practices of the {{site.data.keyword.cloud_notm}} Framework for Financial Services. Again, using a deployable architecture provides a means to accurately deploy the same architecture into different regions, so may represent a good starting point, especially since they can be customised. When customising,  care must be taken to ensure that any customisations are maintained throughout each deployment.

From a disaster recovery point of view, when using a deployable architecture, the customer is responsible for:

* provisioning disaster recovery environments, including any dependencies
* data and configuration backup
* replicating that data and configuration to the disaster recovery environment,
* managing any failover operations.

## {{site.data.keyword.codeenginefull_notm}}
{: #KeyServicesCodeEngine}

While Code Engine can be used to deploy containerised workloads quickly – meaning it could be employed to quickly recover certain containerized applications without the need to rebuild an entire OpenShift or {{site.data.keyword.cloud_notm}} Kubernetes cluster - it can also be used to configure and run jobs, such as triggering database or server backups.
