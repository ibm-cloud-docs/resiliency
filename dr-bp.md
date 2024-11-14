---

copyright:
  years: 2018, 2024
lastupdated: "2024-11-14"

keywords: resiliency, DR, high availability, disaster recovery, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}


# Best practices for {{site.data.keyword.cloud_notm}} resiliency
{: #resiliency}

This page provides some recommended best practices for resiliency for your {{site.data.keyword.cloud_notm}}-deployed workloads.

## Always have a plan
{: #have-a-plan}

Working through any crisis is stressful and disasters are no different. You are likely trying to bring business critical services up and may be under pressure to do so quickly, which could even lead to mistakes. In the unlikely event of a disaster, having a well defined, pre-determined plan will help you and your business recover in an orderly and predicatble way, which will help alleviate stress and execute your recovery without mistakes. You can read more about creating a disaster recovery plan in the [Planning for Disaster Recovery](bcdr-pitfalls.md) section of this guide.

## Work with the business to determine priorites
{: #work-with-business}

When creating a disaster recovery plan, ensure that the plan is endorsed and understood and funded by the wider business. This will help capture business priorites in the event of a disaster, so that the plan is both comprehensive and proportionate to business expectations. Review the plan with the business on a regular basis to capture changes in priorities, making changes to the plan as required.

## Be clear about what constitutes 'a disaster'
{: #be-clear-what-constitues-a-disaster}

To enact the disaster recovery plan, you need to be sure that what you are experiencing actually consitutes a disaster. What consititues a disaster should be clearly expressed in the disaster recovery plan, since otherwise you risk false alarms or perhaps worse, inactivity when things actually do go wrong. It's best practice to think about a number of different scenarios that would constitute a disaster in relation to your workloads and how the business should react to them, including how quickly. Also consider scope - if one component of a workload suffers a disaster, is the response different to a situation where everything fails?

## Communication is key
{: #communication-is-key}

In a disaster situation, strong and effective communication between teams is going to be important. Your plan should clearly state which individuls can call a disaster and how they communicate they have done so, in particular to the individuals who are required to enact the disaster recovery plan.  The plan should clearly state channels of communication, be that phone, email or some other means, to ensure that important messages are not missed. Think about backup channels of communication too, in case primary channels are affected. The plan may also prescribe certain meetings that must take place to capture situation updates and so forth.

## Test your plan (then test it again)
{: #test-the-plan}

Reacting to a real disaster probably isn't the ideal time to try your plan for the first time. Therefore, it's best practice to regularly test your plan. This ensures that the plan fundamentally works, you understand how long it might take to enact the plan in practice and where other personnel are involved, it allows them to better understand their roles. Following the test, ensure that you incorporate any 'lessons learnt' into the plan, to make it better next time. For more information about testing your disaster recovery plan, see [Disaster Recovery Testing](bcdr-testing.md).


## Understand your responsibilities
{: #understand-responsibilites}

Each {{site.data.keyword.cloud_notm}} service has a 'roles and responsibilties' matrix. This clearly deliniates the responsibilites that sit with IBM, those that sit with customers, and those which are shared, including those around backup, recovery and disasters. Ensure that you fully understand these for each of the services which you use, since they may determine actions you need to carry out to successfully recover your service instances and will help planning acordingly.

## Design HA and DR into your workloads
{: #design-in-dr}

When designing cloud-deployed workloads, think about high availabilty and disaster recovery as part of the requirement-gathering stage. By understanding early in the design process what the resiliency qualities of the workload need to be, descisions can be made that influence the architure and in turn, make recovery easier. For example, by having an understanding of the recovery time objective for a workload, you can make descisions about how to deploy a workload to meet that objective, using the features of avalable services. Similarly, understanding the recovery point objective allows you to better understand descisions you need to make about data and how to back it up or replicate it. Designing this in at an early stage also allow the business to better understand running costs for the workload.

Additionally, consider how application code can be developed to make a switch over to a disaster recover site easier. For example, it is best practce to avoid hard-coding connection strings or other config that could change as a result of connecting to alternate resources.

## Work with the cloud, not against it
{: #work-with-the-cloud}

Think of {{site.data.keyword.cloud_notm}} as a toolbox with a set of tools or services that enable you to deploy and run workloads. To use any tool properly, you need to understand what it can and cannot do, and use the right one. Often, if you try and use a tool to do a job that it wasn't designed for, something will go wrong. When designing your resiliency provision, as a best practice, understand in as much detail as possible how the services that your workload uses operate and what their capabilities and limits are. If the service makes it difficult or impossible to meet your RTO or RPO objectives, then consider other services or tools that may help plug the gap. Also consider whether the objectives that have been set are realistic, or whether shaving off a few extra minutes or seconds for recovery is introducing undue complexity and cost into your solution for little real gain.

## Take backups before making changes
{: #backup-before-changes}

Running IT systems is never risk free. Introducing change into your workload environment is a point when risk increases. It's a good practice to have a change release plan, as well as a backout plan to manage changes that you make to your environment. Include taking backups of data and configuration as one of the first steps of any change release plan. If something goes wrong during or shortly after the release, you can recover from your backups.

## Keep up to date with notificatins from {{site.data.keyword.cloud_notm}}
{: #IBM-cloud-notifications}

Of course, should the worst happen and there is a disaster which affects an {{site.data.keyword.cloud_notm}} service or region, {{site.data.keyword.cloud_notm}} will seek to keep customers up-to-date using our standard notification channels. Ensure that you sign up for notifications on your account and also refer to the {{site.data.keyword.cloud_notm}} Status section of the {{site.data.keyword.cloud_notm}} dashboard at https://cloud.ibm.com/status

## Use {{site.data.keyword.cloud_notm}} services that help facilitate DR
{: #KeyServicesTitle}

There are a number of {{site.data.keyword.cloud_notm}} services which can be used to facilitate a disaster recovery provision and to enact the plan, if needed. Here, we provide a brief overview for awareness but it's recommended that you refer to each service's documentation set for further details.

### {{site.data.keyword.cos_full_notm}}
{: #KeyServicesCOS}

One obvious key to DR is the ability to retrieve and recover data in the DR region. For many {{site.data.keyword.cloud_notm}} services, Object Storage is used to make data, particularly backups and snapshots, available in secondary regions, using either a cross-regional bucket or a replicated bucket.

Everything that is stored in {{site.data.keyword.cos_full_notm}} is highly resilient by being stored in three locations, to a degree based on the bucket type and its related data dispersal type. Single site buckets have data dispersed in three locations in a data center. Regional buckets have data dispersed over the region’s three zones. Cross-region buckets have data dispersed over regions in the geo.

Cross-region buckets offer the simplest choice for disaster recovery and are available as follows:

* `ap-geo` – Asia Pacific (Japan, Australia)
* `us-geo` – North America (USA, Canada)
* `eu-geo` – Europe (UK, Germany, Spain)

When addressing a cross-region bucket, a tethered endpoint will keep all data ingress and egress within a specified region, while still distributing the data. However, this does not provide an automated failover if the tethered region fails. Therefore, it’s important to connect services to your buckets using an endpoint that will be available in the region where it’s needed.

Cross Region buckets may not be suitable for all users with regulatory compliance concerns. Take for example a US based organization that cannot store data in Canada, or a European-based one that cannot store data outside of the European Union - in the UK, for example.

In these cases, bucket replication offers a solution. Here, objects from a specified source bucket are replicated on write to a specified target bucket, with eventual consistency (asynchronous replication). Replication must be set up and managed by the user but using this method, users can control the location(s) of the data and its sovereignty.

Versioning is a requirement for bucket replication, so buckets that are configured with an immutable object storage policy cannot be replicated.  Server-side encryption with customer-provided keys is not supported. Where server-side encryption is required, Key Protect must be used.

### {{site.data.keyword.bplong_notm}}
{: #KeyServicesSchematics}

Automation is a best practice since it can play a important role in deploying or recovering an environment quickly and accurately. Customers that automate large parts of their deployments through Infrastructure as Code (IaC), Configuration as Code and deployment Toochains, are likely to recover workloads faster than those that do not becasue they will be more accurate representations of the lost environment. Using automation will also enable you to react faster to unexpected disaster scenarios, especially if you need to create and configure services on-demand, perhaps even in a different recovery region from that at first planned.

{{site.data.keyword.bpshort}} is a service that can play an important part in any disaster recovery strategy. {{site.data.keyword.bpshort}} provides the ability to deploy infrastructure and services quickly and consistently though IaC, via Terraform, Ansible, Helm and OpenShift Operators.

Codifying the workload environment means that the right services can be quickly provisioned and configured in any {{site.data.keyword.cloud_notm}} region. Terraform and Ansible can be used to provision services including servers, storage, networking and databases. They can also be used to configure the services, as can Helm and Operators.

IaC source will need to be maintained and version controlled in the same manner as any other application code and stored in such a way that it is available to multiple regions. It’s recommended that a Git-based code repository is used.

As with any code, it is best practice not to hard-code elements, such as regions or zone names and instead use variables, which can be resolved through the Schematics interface.

Employing DevOps toochains can automate a lot of the environment build too – including configuration - so it is worth investing time in considering how these tools can be used.


### Deployable Architectures
{: #KeyServicesDeployableArchitectures}

{{site.data.keyword.cloud_notm}} now provides a number of deployable architectures. A deployable architecture is a preconfigured set of IaC assets that are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture and so enable customers to follow the best practices of the {{site.data.keyword.cloud_notm}} Framework for Financial Services. Again, using a deployable architecture provides a means to accurately deploy the same architecture into different regions, so may represent a good starting point, especially since they can be customised. When customising,  care must be taken to ensure that any customisations are maintained throughout each deployment.

From a disaster recovery point of view, when using a deployable architecture, the customer is responsible for:

* provisioning disaster recovery environments, including any dependencies
* data and configuration backup
* replicating that data and configuration to the disaster recovery environment
* managing any failover operations

### {{site.data.keyword.codeenginefull_notm}}
{: #KeyServicesCodeEngine}

While Code Engine can be used to deploy containerized workloads quickly – meaning it could be employed to quickly recover certain containerized applications without the need to rebuild an entire OpenShift or {{site.data.keyword.cloud_notm}} Kubernetes cluster - it can also be used to configure and run jobs, such as triggering database or server backups.
