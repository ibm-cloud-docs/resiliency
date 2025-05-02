---

copyright:
  years: 2024
lastupdated: "2025-05-02"

keywords: DR for IBM Cloud, disaster recovery, common mistakes for disaster recovery, plan a disaster recovery strategy

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Planning for disaster recovery
{: #PlanningforDR}

An effective disaster recovery (DR) solution is planned and designed to meet both business and technical requirements. For example, a certain business requirement might be technically impossible to implement, or a technical implementation can become cost prohibitive for your business. The starting point for any disaster recovery solution is a disaster recovery plan.

## What's in a disaster recovery plan?
{: #DRPlan}

A disaster recovery plan is a set of procedures and strategies that an organization follows to recover from a disaster or an event that impacts availability. A DR plan is a critical document that outlines the steps that an organization takes to minimize the impact of a disaster. The plan also outlines the steps to get back to normal operations. A disaster recovery plan is primarily a business plan and requires input from stakeholders across the business organization, not just the IT department. It also requires appropriate funding and resources to help ensure that it remains a living document.

A disaster recovery plan includes the following elements:

1. Risk assessment
   * Identify potential disasters and the likelihood of them occurring.
   * Evaluate the impact that each disaster might have on the organization.
1. Emergency response
   * Designate a disaster management team.
   * Establish communication protocols.
   * Identify emergency response procedures.
1. Business continuity
   * Develop a business continuity plan that includes procedures to maintain organizational operations during a disaster.
   * Identify key personnel and their role in the recovery process.
   * Establish procedures for communication with customers, vendors, and other stakeholders.
1. Cloud environment recovery
   * Establish procedures for the provision of replacement services.
   * Establish procedures for the provision of standby services.
   * Establish procedures for the scaling of standby services.
1. Data backup and recovery
   * Establish backup procedures for critical data and systems.
   * Validate that backup systems are regularly tested and updated.
   * Develop procedures for recovering lost data and systems.
1. Training and testing
   * Provide regular training for employees on disaster recovery procedures.
   * Conduct regular testing of the disaster recovery plan.
   * Add updates to the disaster recovery plan, based on testing and new risks identified.

Testing under a range of scenarios identifies common pitfalls that can render plans unusable under real conditions. By following these steps and regularly reviewing and updating the disaster recovery plan, your organization can better prepare to respond to and recover from disasters.

## Avoiding common disaster recovery pitfalls
{: #common-pitfalls}

To help ensure that your plans provide the results that you require if a disaster strikes, be aware of the following common disaster recovery pitfalls.

### Planning and designing a plan that isn't production-ready
{: #best-conditions}

The DR infrastructure capacity must be able to manage production-size workloads, unless your plan dictates otherwise. Implementing DR with resources that don't match your production environment might seem attractive from a cost perspective, but it can make the effects of a disaster much more severe.

As you plan and implement your disaster recovery environment, consider its own resiliency requirements. Don't try to cut costs by reducing the high availability requirements of the DR implementation. If a disaster happens, production workloads need a resilient environment. Services in the cloud are scalable, so you can add capacity later, but the increased regional demand that the disaster causes can result in short-term capacity shortages.

### Forgetting nontechnical single-points-of-failure
{: #spof}

Single-points-of-failure (SPOF) can be anywhere in a solution, not just in the technology. Your solution might depend on people, vendors, providers, and other external dependencies. Identify clearly your SPOFs and mitigate your dependencies. Be prepared to discover SPOFs during the first sessions of your [disaster recovery test](/docs/overview?topic=overview-dr-testing).

Among SPOFs, provider risk is a condition to consider in your DR plan. When you have both production and DR on the same provider, your risk condition is increased and must be carefully considered.

### Having only a plan A
{: #plan-a}

Disasters can take many forms, so planning for one specific disaster scenario leaves you vulnerable to others. In conceiving the DR plan, consider several different disaster scenarios and demonstrate how the plan is flexible.

### Testing your plan poorly
{: #poor-dr-testing}

An untested DR solution increases the chances that you encounter an obstacle when success is the most critical. Testing is essential to validate that you have a working solution. The conditions that you test are also important and require you to create several testing scenarios.

Conducting a DR test by doing a planned close of operation in one location and a well-organized restart in the other helps you ensure that your DR test works. However, a simple full shutdown doesn't always happen in a real emergency.

Design your tests to mimic the potential emergency conditions as closely as possible by simulating a “rolling disaster condition”, where your workload is progressively impacted as a result of the emergency. The progressive impact tests the resilience of your solution and provides insight into your ability to resist stress conditions.

## Considerations for your disaster recovery solution
{: #dr-considerations}

The technical design of a disaster recovery solution has various factors to consider to help ensure that it's fit for purpose.

### High availability
{: #HighAvailability}

High availability does not equate to disaster recovery. {{site.data.keyword.cloud_notm}} recommends that customers verify that their deployments are highly available, taking advantage of {{site.data.keyword.cloud_notm}}’s Multizone Regions (MZRs). Each MZR has a minimum of three zones, which are highly interconnected, yet operationally separate data centers. Often, issues and outages affect only a single zone, not the whole region. Deploying workloads by default across all zones in an MZR can diminish downtime and the need to call a disaster at all. Failover to a second region can be a major process, as is failing back, so take as many steps as possible to prevent needing to do so. However, MZRs don't prevent disasters such as data corruption or malicious damage.

### Regulation and compliance
{: #RegulationAndCompliance}

Some workloads and data are subject to strict industry regulation and compliance standards, which can affect the physical location in which they can be run or stored. When you choose a location for a disaster recovery site, verify that the chosen region meets any necessary regulation and compliance. Locations where you choose to replicate data must be verified, too.

Cross-regional {{site.data.keyword.cos_short}} buckets offer a way to replicate data to different regions, but use with care if your data is subject to territorial restrictions. Verify that your data is not transferred to a region in a country or territory that breaches organizational or industry compliance rules.

### Capacity in the disaster recovery region
{: #CapacityinDRRegion}

Disasters can affect many customers, who then enact their disaster recovery plans. The additional demand might put a severe strain on the capacity of one or more of the regions nearest to the region that failed. This could result in a shortage of resources that are available. For example, if `us-south` fails, it’s likely many customers will choose to recover in the `us-east` region first. If `eu-gb` fails, then extra demand is expected to fall on `eu-de`.

Be aware that due to high demand caused by a disaster, your first choice of infrastructure may not be available to provision at your recovery region. This includes popular VPC VSI profiles. Consider various disaster recovery architectures for your applications that take into consideration a potential need to run on alternate VSI profiles due to capacity issues.

If your application cannot tolerate alternate infrastructure, then to avoid capacity problems, consider building out or reserving as much capacity as you need in the DR region of choice in anticipation of a disaster, or consider other locations that are further away.

It is good practice to test data restoratation, and this can provide an indication of restore timing of your data.  However, understand that these times may be extended in the event of a major disaster affecting a full region, where many customers may also be restoring data.

Verify that your backups can be restored in your chosen region. For example, databases cannot be restored in regions that cross compliance boundaries.

### Connectivity
{: #ConnectivityConsiderations}

Certain services, such as {{site.data.keyword.dl_short}} and VPNs, can take some time to provision as they can require third-party activity or further installation of physical infrastructure. As you create a DR plan, consider the lead times for such services. It's optimal to provision these services ahead of time.

### Partial or full failover
{: #partial-full-failover}

The cloud is composed of different services, and applications can rely on multiple components that work together. Disaster recovery plans typically prepare for catastrophic failures at the regional level and assume that all services in the region are lost. However, isolated service outages can occur without affecting other services in the region.

In such cases, it’s crucial to assess whether a full failover to another region is necessary or if only the failed component needs to be addressed.

For example, if a database service experiences a catastrophic failure, do you need to fail over the entire workload, including web services? Or can the other components easily switch to a standby read-replica database in another region?

The ease of handling such scenarios depends on how your workload is designed and configured. Avoid hardcoding service names and ensure that your architecture considers factors like network latency to enable smooth transitions during partial outages.

### Failback
{: #Failback}

An extra consideration is the failback position after the dust settles on a disaster. The failback procedure is documented in the DR plan. Failback considerations include:

* Do you fail back or continue to run as normal at the recovery location?
* If failback is mandatory, when do you attempt it?
* How does failback work to create as little further disruption as possible?

Failback can be as complicated and disruptive as a disaster, so consider whether failback is needed at all. Instead, can services continue to run in the second location? The timing of failback is also important. You need to make sure that the circumstances that caused the disaster are resolved and that the failback doesn't cause prolonged disruption.

Whether you enact a failback or not, the DR provision needs to be re-created or reset.
