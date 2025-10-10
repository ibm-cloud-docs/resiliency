---

copyright:
   years: 2020, 2025
lastupdated: "2025-10-10"

keywords: resiliency, DR, high availability, disaster recovery, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}


# Best practices for resiliency
{: #resiliency}

Follow best practices for resiliency on {{site.data.keyword.cloud_notm}} to help ensure that your workloads are highly available and that you can recover from a disaster.

## Have a plan
{: #have-a-plan}

Working through any crisis is stressful and disasters are no different. During a disaster, you are likely trying to restart business-critical services and might be under pressure to do so quickly, which might lead to mistakes. In the unlikely event of a disaster, having a clearly defined plan helps your business recover in a predictable way, which helps alleviate stress and reduces mistakes. For more information about creating a disaster recovery plan, see [Planning for disaster recovery](/docs/resiliency?topic=resiliency-PlanningforDR).

## Determine priorities
{: #work-with-business}

When you create a disaster recovery plan, make sure that your organization endorses, understands, and can fund the plan. When you get broader stakeholder buy in, you can capture the business's priorities if a disaster occurs. This way, the plan is both comprehensive and proportionate to business expectations. Review the plan with your organization regularly to capture changes in priorities and update the plan as needed.

## Understand what qualifies as a disaster
{: #be-clear-what-constitues-a-disaster}

To enact the disaster recovery plan, you need to be sure that what you are experiencing is a disaster. Clearly express what qualifies as a disaster in the plan, otherwise you risk false alarms or inactivity when a disaster occurs. Define several different scenarios that constitute a disaster, how the business can react to them, and how quickly. Also consider the scope of impact. If one component of a workload suffers a disaster, is the response different than a situation where everything fails?

## Communication is key
{: #communication-is-key}

In a disaster situation, effective communication between teams is important. In your plan, clearly state which individuals can call a disaster. Define how they communicate with the organization and the people who are required to enact the disaster recovery plan. Clearly state channels of communication, such as by phone or email, to help ensure that important messages are not missed. Think about backup channels of communication in case primary channels are affected. The plan might also prescribe certain meetings that must take place to capture situation updates.

## Test your plan
{: #test-the-plan}

Reacting to a real disaster isn't the ideal time to test your plan for the first time. Regularly test your plan to help ensure that it works and you understand how long it might take to enact the plan. Where other personnel are involved, testing your plan allows them to better understand their roles. Following the test, make sure that you incorporate any lessons that are learned into the plan. For more information about testing your disaster recovery plan, see [Disaster Recovery Testing](/docs/resiliency?topic=resiliency-dr-testing).

## Understand your responsibilities
{: #understand-responsibilites}

Each {{site.data.keyword.cloud_notm}} service has a roles and responsibilities matrix that defines {{site.data.keyword.IBM}}'s responsibilities, customer responsibilities, and shared responsibilities, including responsibilities related to backup, recovery, and disasters. Make sure that you fully understand the ownership of responsibilities for each of the services that you use, since they determine the actions to successfully recover your service instances and help you plan.

## Consider data resiliency and data residency requirements
{: #data-resiliency-and-residency}

Data resiliency refers to the ability to access, maintain or quickly recover data if failures or disasters occur. It is related to the concepts of high availability, disaster recovery, and cyber resiliency. For more information about data resiliency, see the [{{site.data.keyword.IBM_notm}} Well-Architected Framework](https://www.ibm.com/architectures/well-architected/resiliency#Practices){: external}.

Another important aspect to consider is data residency and any restrictions or requirements on your data's physical location, not just for production environments but also for backup and recovery.

### Understanding data residency in {{site.data.keyword.cloud_notm}}
{: #data-residency}

{{site.data.keyword.cloud_notm}}'s global network of locations provides you with the flexibility of choosing where you want to run your workloads. Review {{site.data.keyword.cloud_notm}}'s [Service rollout policy](/docs/overview?topic=overview-service-rollout) for guidelines on when to expect and how to request that a service is available in a particular region.

When you provision an instance of a regional and zonal service, you select a region to deploy the instance in accordance with your geographic requirements. {{site.data.keyword.cloud_notm}} helps ensures that content that is provided by you and your workload, as defined in the [Cloud Services Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304&cc=us&lc=en){: external}, is stored and processed locally in the selected region location. For a complete list of the locations where {{site.data.keyword.cloud_notm}} services are available see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

{{site.data.keyword.cloud_notm}} services support saving encrypted backups of the customer content within the location where the regional or zonal service is located for recovery if data corruption or a major data center disaster occurs.

For client's metadata, including client business contact and account usage information (as defined in the [{{site.data.keyword.cloud_notm}} Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304&cc=us&lc=en){: external}), {{site.data.keyword.cloud_notm}} stores and processes them where the control planes of the regional and global services are located.

- Regional services typically have control planes that are located in the same region where you selected for the service except for the services indicated in [Services with global control planes](/docs/resiliency?topic=resiliency-ha-redundancy#service-global-control-plane).
- Global services control planes locations are indicated in [Global platform services](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform).

For a complete list of data attributes that each single {{site.data.keyword.cloud_notm}} service processes and stores, see the [API and SDK reference library](https://cloud.ibm.com/docs?tab=api-docs).

All data in transit is encrypted. Only TLS 1.2 and 1.3 are supported in {{site.data.keyword.cloud_notm}} with TLS 1.1 and lower explicitly disabled to prevent rollback to a vulnerable version of the protocol.

Processes and procedures for {{site.data.keyword.cloud_notm}} data privacy processing are documented within the {{site.data.keyword.cloud_notm}} Data Processing Addendum (DPA). This DPA and its applicable DPA Exhibits apply to the Processing of Personal Data by {{site.data.keyword.cloud_notm}} on behalf of Client (Client Personal Data). The processing of Personal Data is subject to the General Data Protection Regulation 2016/679 (GDPR). It is also subject to any other data protection laws that are identified at [Data Protection Laws](https://www.ibm.com/support/customer/csol/terms/?id=DPA-DPL&lc=en){: external} in order to provide services (Services) according to the Agreement between Client and {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.cloud_notm}} DPA can be found at [Data Processing Addendum](https://www.ibm.com/support/customer/csol/terms/?id=Z126-7870&lc=en){: external}.

In addition to the DPA, the cloud services provide DPA exhibits that can be found on the [{{site.data.keyword.cloud_notm}} Terms site](https://www.ibm.com/support/customer/csol/terms/){: external}.

## Design HA and DR into your workloads
{: #design-in-dr}

When you design cloud-deployed workloads, think about high availability and disaster recovery as part of the requirement-gathering stage. By understanding early in the design process what the resiliency qualities of the workload need to be, you can make decisions that influence the architecture and make recovery easier. For example, if you understand the recovery time objective for a workload, you can decide how to deploy a workload to meet that objective by using the features of available services. Similarly, if you understand the recovery point objective you can make better decisions about data, how to back it up, or how to replicate it. Designing this in at an early stage also allow the business to better understand running costs for the workload.

Consider how you can develop application code to make a switching to a disaster recover site easier. For example, avoid hardcoding connection strings or other configurations that might change as a result of connecting to alternative resources.

## Choose the right tools
{: #work-with-the-cloud}

Think of {{site.data.keyword.cloud_notm}} as a toolbox with a set of tools or services that you can use to deploy and run workloads. To use any tool properly, you need to understand what it can and can't do, and choose the right one. If you try to use a tool for a job that it wasn't designed for, something can go wrong. When you design your resiliency plan, understand in as much detail as possible the services that your workload uses, what their capabilities are, and their limits. If a service makes can't meet your RTO or RPO objectives, consider other services or tools that might help close the gap. Also consider whether the objectives that you set are realistic, or whether you are introducing undue complexity and cost into your solution for little real gain.

## Take backups before you make changes
{: #backup-before-changes}

Running IT systems is never risk-free, and introducing change into your workload environment is a point when risk increases. Have a change release plan and a backout plan to manage changes that you make to your environment. As one of the first steps of any change release plan, include taking backups of data and configurations. If something goes wrong during or shortly after the release, you can recover from your backups.


## Create Custom Images
{: #create-customer-images}

Create a [custom image from a boot volume](/docs/vpc?topic=vpc-image-from-volume-vpc&interface=ui) and use it as the golden image, with preinstalled applications and configurations, to reduce the provisioning time for {{site.data.keyword.vsi_is_full}} instances in the DR site. The boot volume must be attached to a stopped virtual server instance (VSI) to create the custom image.

## Use hostnames for subnets
{: #use-hostnames-for-subnets}

Use hostnames and DNS instead of IP addresses to minimize the changes that are required to redeploy an application in the DR site, particularly with VPC instances, including VSIs. Subnets are zone-specific and do not extend across zones. New IP addresses are assigned to new service instances, which can break any existing configurations such as security rules, application configuration files.

## Configure key management services for disaster recovery
{: #configure-key-management-services-for-dr}

 For Key Protect, configure the service in the primary site with failover in the DR region to enable automatic rerouting of Key Protect requests if a regional service outage occurs. Create scripts to update the Virtual Private Endpoint (VPE) settings to access the Key Protect Service, specifically the Internet Protocol (IP) address, as part of disaster recovery procedures.

 For HPCS, configure a failover crypto unit in the DR region. Initialize and configure failover crypto units the same as the operational crypto units before the disaster happens, so they are available if a regional outage occurs

## Invest in Observability
{: #invest-in-observability}

Observability includes platforms and tools like [IBM Cloud Logs]() and [IBM Cloud Monitoring]() which allow you to learn what is happening in the system and, in the event of an outage or degradation of services, help to identify, mitigate and remediate the root cause quickly. This is especially needed for complex distributed systems where failures are unavoidable and its very difficult to track or correlate dependencies between components. Additionally, [Activity Tracker Event Routing](/docs/atracker) service allows you to audit events and record and monitor changes to the system while [Flow Logs for VPC](/docs/vpc?topic=vpc-flow-logs) provides visibility into IP traffic going to and from network interfaces within VPC and allows you to troubleshoot networking and connectivity issues, which are otherwise difficult to observe.

Reslient applications built for cloud and processes supporting them should leverage these capabilities effectively. For instance, applications should expose metrics for health of its components and their subsystems. All IBM Cloud services support collecting [platform metrics](/docs/monitoring?topic=monitoring-platform_metrics_enabling) and expose detailed metrics which are documented individually, like this for [loadbalancer](/docs/loadbalancer-service?topic=loadbalancer-service-monitoring-metrics). Applications and [dashboards]() built to support them should leverage these metrics to improve operational efficiency. 

IBM Cloud Logs provides advanced [features](/docs/cloud-logs?topic=cloud-logs-features) to enrich, query and alert on logs generated by applications while managing the volume of logs and its cost. Different alerting [types](/docs/cloud-logs?topic=cloud-logs-alerts#alert-types) enable more sophisticated capabilities to effectively monitor systems.


## Stay updated with {{site.data.keyword.cloud_notm}} notifications
{: #IBM-cloud-notifications}

If a disaster occurs that affects an {{site.data.keyword.cloud_notm}} service or region, you receive notifications from {{site.data.keyword.cloud_notm}} in your account or by email. Sign up for notifications on your account by reviewing [View notifications](/docs/account?topic=account-viewing-notifications#subscribe-email-notifications). You can also view the {{site.data.keyword.cloud_notm}} [Status Overview](/status) page.
