---

copyright:
  years: 2024
lastupdated: "2024-11-08"

keywords: DR for IBM Cloud, disaster recovery, common mistakes for disaster recovery, plan a disaster recovery strategy

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Planning for Disaster Recovery
{: #PlanningforDR}

To be effective, a disaster recovery (DR) solution must be planned and designed in such a way that it meets both business and technical requirements, striking a balance between the two. For example, a certain business requirement is technically impossible to implement, or a technical implementation becomes cost prohibitive for the business. The starting point for any disaster recovery solution is a disaster recovery plan.

## What is in a DR Plan?
{: #DRPlan}

A disaster recovery plan is a set of procedures and strategies that an organization follows to recover from a disaster or unexpected event. It is a critical document that outlines the steps that an organization takes to minimize the impact of a disaster. It also outlines the steps to get back to normal operations. A disaster recovery plan is primarily a business plan and requires input from stakeholders across the business organization, not just the IT department. It also requires appropriate funding and resourcing to help ensure that it remains a living, rather than a one-off, document.

Under the following headings, a DR plan:

1. Risk Assessment
* Identifies potential disasters and the likelihood of them occurring.
* Evaluates the impact that each disaster would have on the organization.

2. Emergency Response
* Designates a disaster management team.
* Established communication protocols.
* Identifies emergency response procedures.

3. Business Continuity
* Develops a business continuity plan that includes procedures for keeping the organization running during a disaster.
* Identifies key personnel and their roles in the recovery process.
* Establishes procedures for communication with customers, vendors, and other stakeholders.

4. Cloud Environment Recovery
* Establishes procedures for the provision of replacement services.
* Establishes procedures for the provision of standby services.
* Establishes procedures for the scaling of standby services.

5. Data Backup and Recovery
* Establishes backup procedures for critical data and systems.
* Validates that backup systems are regularly tested and updated.
* Develops procedures for recovering lost data and systems.

6. Training and Testing
* Provides regular training to employees on disaster recovery procedures.
* Conducts regular testing of the disaster recovery plan.
* Adds updates to the disaster recovery plan, based on testing and new risks identified.

By following these steps and regularly reviewing and updating the disaster recovery plan, an organization can be better prepared to respond to and recover from disasters. Testing under a range of scenarios identifies common pitfalls that can render plans unusable under real conditions.

## Avoiding common disaster recovery pitfalls
{: #common-pitfalls}

To help ensure that your plans provide the results that you require if a disaster strikes, be aware of the following common disaster recovery pitfalls.


### Planning and designing something that is not production-ready
{: #best-conditions}

The DR infrastructure capacity must be able to manage production-size workloads, unless your plan dictates otherwise. Implementing DR with resources that do not match production might seem attractive from a cost perspective, but it can make the effects of a disaster much more severe.

As you plan and implement your disaster recovery environment, consider its own resiliency requirements. Consider carefully the temptation to reduce costs by reducing the high availability requirements of the DR implementation. If a disaster is called, production workloads need a resilient environment. Services in the cloud are scalable, so you can add capacity later, but increased regional demand the disaster causes can result in short-term capacity shortages.


### Not considering nontechnical single-points-of-failure (SPOF)
{: #spof}

SPOF can be anywhere in a solution, not just in the technology. Your solution might depend on people, vendors, providers, and a range of other external dependencies. Know and identify clearly in advance your SPOFs and mitigate your dependencies. Be prepared to discover SPOFs during the first sessions of your [disaster recovery test](/docs/overview?topic=overview-dr-testing).

Among SPOFs, provider risk is a condition to consider in your DR plan. When you have both production and DR on the same provider, your risk condition is increased and must be carefully considered.

### Having only a plan A
{: #plan-a}

Disasters can take many forms, so planning for one specific disaster scenario leaves you vulnerable to others. In conceiving the DR plan, consider several different disaster scenarios and demonstrate how the plan copes.

### Preparing a poor DR testing plan
{: #poor-dr-testing}

An untested DR solution is like running blindfolded in the woods. Chances are high that you encounter an obstacle that stops you in your tracks. Moreover, you make the discovery during the most important run of your life.

Testing is essential to validate that you have a working solution. By extension, the conditions you are going to test are also important and require you to create several testing scenarios.

Conducting a DR test by doing a planned close of operation in one location and a well-organized restart in the other, means are sure your DR test works. Is a graceful full shutdown what happens in a real emergency, though?

Design your tests to mimic the potential emergency conditions as closely as possible by simulating a so-called “rolling disaster condition”, where your workload is progressively impacted as a result of the emergency. Progressive impacts tests the resilience of your solution and provides an insight to its ability to resist stress conditions.

## Disaster Recovery Solution Considerations
{: #dr-considerations}

In the design of the technical aspects of a disaster recovery solution, various factors are considered to help ensure that it is fit for purpose.

### High availability
{: #HighAvailability}

As mentioned in a previous section, high availability does not equate to disaster recovery. {{site.data.keyword.cloud_notm}} recommends, as a best practice, that customers verify that their deployments are highly available, taking advantage of {{site.data.keyword.cloud_notm}}’s Multizone Regions (MZRs). Each MZR has a minimum of three zones, which are highly interconnected, yet operationally separate data centers. Often, issues and outages affect only a single zone, not the whole region. Deploying workloads by default across all zones in an MZR can diminish downtime and the need to call a disaster at all. Failover to a second region can be a major process, as is failing back, so it's wise to take as many steps as possible to prevent needing to do it. Remember though, MZRs don't prevent disasters such as data corruption or malicious damage.

### Regulation and Compliance
{: #RegulationAndCompliance}

Some workloads and data are subject to strict industry regulation and compliance standards, which can affect the physical location in which they can be run or stored. When you choose a location for a disaster recovery site, verify that the chosen region meets any necessary regulation and compliance. Locations to which you choose to replicate data must be verified too.

Cross-regional Object Storage buckets offer a way to replicate data to different regions, but use with care if your data is subject to territorial restrictions. Verify that your data is not transferred to a region in a country or territory that breaches organizational or industry compliance rules.


### Capacity in the DR Region
{: #CapacityinDRRegion}

If a disaster strikes, many customers can be affected and enacting their disaster recovery plans. The additional demand might put a severe strain on the capacity of one or more nearest regions to the region that failed. For example, if us-south were to fail, then it’s likely many customers recover in the us-east region as a first choice. If eu-gb fails, then extra demand is expected to fall on eu-de.

This increase in local demand might hamper recovery efforts and increase recovery times for a period.

To avoid capacity problems, consider pre-building out as much capacity as needed in the DR region of choice or consider other locations that are further afield. Verify that your backups can be restored in your chosen region. For example, databases cannot be restored in regions that cross compliance boundaries.

### Connectivity considerations
{: #ConnectivityConsiderations}

Certain services, such as Direct Link and VPNs can take some time to provision as they can require third-party activity or further installation of physical infrastructure. As you create a DR plan, verify that the lead times for such services are considered. Understand that it is optimal to have these services pre-provisioned and ready.

### All, some or nothing?
{: #AllorNothing}

The cloud is composed of different services, and an application can use multiple services. DR plans can focus on the catastrophic failure of the whole region and implies that everything in that region is lost, but that's not always the case. Individual services can suffer a catastrophic outage in isolation to others. In these cases, it’s important to understand whether a need exists to failover to another region. If so, does the whole workload need to fail over or just the component that failed?

For example, suppose that the database service suffers a catastrophic failure. Does that require failing over the whole workload, web services and all? Can the remaining services be easily switched to point at an enlivened read-replica database that was on standby in another region? The ease of answering such questions comes down to how the workload was developed and configured. Remember not to hardcode service names and consider network latency.

### Failback
{: #Failback}

An extra consideration is the failback position after the dust settles on a disaster. The failback procedure is documented in the DR plan. Failback considerations include:

* Do you fail back or continue to run as normal at the recovery location?
* If failback is mandatory, when is it attempted?
* How will failback work to create as little further disruption as possible?

Failback can be as complicated and disruptive as a disaster, so consider whether failback is needed at all. Instead, can services continue to run in the second location? The timing of failback is important too. You need to be sure that the circumstances that caused the disaster are resolved and that the failback doesn't cause more prolonged disruption.

Whether failback is enacted or not, the DR provision needs to be re-created or reset.
