---

copyright:
  years: 2021, 2024
lastupdated: "2024-08-06"

keywords: disaster recovery, DR, what is disaster recovery, DR strategy, disaster recovery options, disaster recovery strategy

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Understanding disaster recovery
{: #understanding-dr}

When dealing with improved resilience, it important to make some distinctions between [high availability](#x2284708){: term} (HA) and [disaster recovery](#x2113280){: term} (DR).
{: shortdesc}

HA is mainly about keeping the service available to the users when ordinary activities are performed on the system like deploying updates, rebooting the hosting virtual machines, applying security patches to the hosting OS, and so on. For our purposes, high availability within a single cloud region ([MZR](#x9774820){: term}) can be achieved by eliminating single points of failure. For more information on HA, see [Considerations for high availability](/docs/overview?topic=overview-ha-considerations).

HA usually doesn’t deal with major unplanned or planned issues, such as complete site loss because of major power outages, earthquakes, severe hardware failures, full site connectivity loss, and more. In such cases, if the service must meet strict Service Level Objectives (SLO), you should make the whole application stack (infrastructure, services, and application components) redundant by deploying it in at least two different cloud regions. This is typically defined as a DR architecture.

The best practices in this section are based on the services and features currently available and might change over time. We recommend that you verify possible changes or additional features that might apply to your environment.
{: note}

## General disaster recovery strategy
{: #bcdr-general}

The approach to defining your DR strategy needs to be systematic and start with the application or service, which is defined as a set of compute resources, for example Kubernetes apps, virtual machines, services, and more, that make up a business application.

While a holistic approach might be wanted, the reality is that each business application (cloud service) is independent, with its own [recovery time objective](#x3167918){: term} (RTO) and [recovery point objective](#x3429911){: term} (RPO) requirements, which for many customers is expressed in the form of a set of service classes.

Because each business application has a unique set of compute, services, and other resources, each one needs to be reviewed to make sure that the strategy and requirements for DR for that app are understood, documented, and implemented before going to production. To build the framework that will drive that analysis and work, we outlined profiles for continuous availability and advanced recovery applications with common sets of resources to create the scaffolding that application teams can use for their own applications. For more information, see [Designing an architecture for your application resiliency objectives](/docs/overview?topic=overview-bcdr-app-recovery).

### DR strategy options
{: #dr-categories}

There are many options to implement DR solutions. For the sake of simplicity, we can group the different options in three major categories:

Active/Passive
:   Active/Passive options are based on keeping the full application stack active in one location, while another application stack is deployed in a different location but kept idle or shut down. In the case of prolonged unavailability of the primary site, the application stack is activated in the backup site. Often that requires the restoring of backups that are taken in the primary site. This approach is not recommended when losing data can be a problem, for example when the RPO is less than a few hours or when the availability of the service is critical and the RTO objective is less than a few hours.

Active/Standby
:   In the Active/Standby case, the full application stack is active in both the primary and backup location. However, user's transactions are served by the primary site only. The backup site takes care of keeping a replica of the status of the main location though data replication, such as database replication or disk replication. In cases of prolonged unavailability of the primary site, all client transactions are routed to the backup site. This approach provides good RPO and RTO, generally measured in minutes; however, it is significantly more expensive than the Active/Passive options because of the double deployment. For example, resources are wasted because the standby assets can't be used to improve scalability and throughput.

Active/Active
:   In the Active/Active case both locations are active, and client transactions are distributed to both regions according to predefined policies, such as round-robin, geographical load balancing, and so on. In the case of the failure of one site, the other site must be able to serve all clients. It's possible to achieve both an RPO and RTO close to zero with this configuration. The drawback is that both regions must be sized to handle the full load, even if they are used at half of their capabilities when both locations are available. In such cases, auto scaling can help in keeping resources allocated according to the needs. Of course, the data across the two sites is continuously synced with some kind of replication mechanism.

### Site strategy
{: #bcdr-site-strategy}

Site strategy is the most predominant aspect of the overall resiliency solution because it determines what classes of physical events the solution is able to address, sets the requirements in terms of distances, and it also sets constraints on the technology side.

{{site.data.keyword.cloud_notm}} offers a redundant infrastructure with several layers of resiliency that can be summarized as:

Local
:   A physical and logical separation zone, for example an availability zone or availability set, within a cloud location, such as a data center, that is independent from other zones for what pertains to power supply, cooling, and networking.

Site
:   Multiple sites in a region. Using different sites within the same region offers a better level of protection in case of a limited natural disaster, compared to two different zones on a single site. Sites in the same region are usually in close proximity (tens of miles).

Region
:   Using two sites in two regions in the same geography represents the highest level of protection against natural disasters as sites are generally over 400 kilometers or 250 miles apart.

Geography
:   Selecting two sites in different geographies extends the level of protection against natural disasters. Geographies are generally over 1500 kilometers or 1000 miles apart, thus representing the optimal option in terms of wider protection requirements.

The analysis of the cloud sites for the DR of your cloud-enabled workloads is relevant to avoid problems because of latency. Multiple sites in the same region help to implement DR with near zero RPO. And, remote secondary sites far from the primary site might require asynchronous techniques to be evaluated in your design.

For more information, see [How {{site.data.keyword.cloud_notm}} ensures high availability and disaster recovery](/docs/overview?topic=overview-zero-downtime).


### Plan and design for the “worst conditions”
{: #worst-conditions}

When designing your resiliency solution, the key is in the word “resiliency". Your solution has to work in the worst possible conditions and provide enough performance, not only to achieve your recovery objectives (RTO and RPO), but to also be able to sustain and run production from an alternative site for a long period. Your design should not only consider a single plan, but many possible alternatives to best adapt and be resilient, for example being able to work in a degraded or impacted ecosystem.

Of course, costs might be prohibitive in this approach. However, you should have a clear view of what the minimal acceptable conditions are, and what other aspects you might eliminate when a solution is cost prohibitive while understanding what the impact would be. In other words, accept residual risks knowing the consequences.

Your design should not be limited to the IT only. Your IT is dependent on several other factors that can be impacted as well. The following is a nonexhaustive list:

* Key personnel availability
* External networks services
* Dependencies on critical providers
* Road conditions (when planning physical transfers of personnel, backup, and more)
* DR resources availability when required

## Planning your application's recovery strategy objectives
{: #plan-objectives}

You should define the recovery objectives for your application and the disaster recovery metric thresholds for your application. In the following example, RTO and RPO requirements are homogeneous. This helps in the design and implementation of the application's DR strategy.

| Recovery classes | RTO | RPO |
|------------------|----------------|----------------|
| Continuous availability | &lt;= 1 hour | &lt;= 1 hour |
| Advanced recovery | &amp;gt; 1 hour - &lt;= 72 hours | &lt;2 hours - &lt;72 hours |
| Standard recovery | &amp;gt; 72 Hr | Last backup |
| No recovery | n/a | n/a |
{: caption="Table 1. Example RTO and RPO requirements" caption-side="top"}

The resiliency options, proposed profiles, and associated tables are presented so that you can define your application's disaster recovery requirement levels. Informaton stated is not a warranty and {{site.data.keyword.IBM}} will not issue credits for failure to meet an objective. These RTO and RPO [examples](/docs/overview?topic=overview-bcdr-app-recovery) are presented as a reference to guide you on additional steps that can be taken to achieve those levels of resiliency. Refer to the Service Level Agreements (SLAs) for any commitments and credits that are issued upon failure to meet any committed SLAs.
{: .note}

When planning for a migration on cloud, it could be the right moment to review the real business needs and the DR requirements (RPO/RTO). Not all of the workloads need the highest service levels. Leveraging the on-demand nature of the cloud, you can fit business requirements of a subset of applications with a lower class.

Because the final solution depends on the combination of cloud services consumed, when selecting services on cloud, you must verify that they are provided with services levels that match your needs. This applies to every aspect of the managed cloud services, such as backup policies and frequency, committed RPO/RTO if you used for example a database as a service option, and so on. This could generate relevant savings in the costs of the DR solution.

## Resources for disaster recovery implementations
{: #docs-dr}

It is important to have a clear view of the ownership or control of the DR implementation. Check [Shared responsibilities](/docs/overview?topic=overview-shared-responsibilities) for a detailed outline for the responsibilities and roles for each service offered by {{site.data.keyword.cloud_notm}}.

The [{{site.data.keyword.cloud_notm}} service level objectives](/docs/overview?topic=overview-slo) documentation provides links to the available high availability, DR, and backup information for the {{site.data.keyword.cloud_notm}} services that can be used as reference documentation in designing and implementing the DR plan.

More documentation resources:

- [Designing an architecture for your application resiliency objectives](/docs/overview?topic=overview-bcdr-app-recovery)
- [Build resilient applications on the cloud](https://www.ibm.com/topics/cyber-resilience){: external}
- [Secure web applications across multiple regions](/docs/solution-tutorials?topic=solution-tutorials-vpc-multi-region)
- [Resilient and secure multi-region Kubernetes clusters with {{site.data.keyword.cloud_notm}} Internet Services](/docs/solution-tutorials?topic=solution-tutorials-multi-region-k8s-cis)
