---

copyright:
  years: 2018, 2024
lastupdated: "2024-11-20"

keywords: resiliency, DR, high availability, disaster recovery, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}


# Best practices for resiliency
{: #resiliency}

Follow best practices for resiliency on {{site.data.keyword.cloud_notm}} to help ensure that your workloads are highly available and that you can recover from a disaster.

## Have a plan
{: #have-a-plan}

Working through any crisis is stressful and disasters are no different. During a disaster, you are likely trying to bring up business-critical services and might be under pressure to do so quickly, which might lead to mistakes. In the unlikely event of a disaster, having a clearly defined plan helps your business recover in a predictable way, which helps alleviate stress and reduces mistakes. For more information about creating a disaster recovery plan, see [Planning for disaster recovery](/docs/resiliency?topic=resiliency-PlanningforDR).

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

Reacting to a real disaster isn't the ideal time to test your plan for the first time. Regularly test your plan to help ensure that it works and you understand how long it might take to enact the plan. Where other personnel are involved, testing your plan allows them to better understand their roles. Following the test, make sure that you incorporate any lessons that are learned into the plan. For more information about testing your disaster recovery plan, see [Disaster Recovery Testing](bcdr-testing.md).

## Understand your responsibilities
{: #understand-responsibilites}

Each {{site.data.keyword.cloud_notm}} service has a roles and responsibilities matrix that defines {{site.data.keyword.IBM}}'s responsibilities, customer responsibilities, and shared responsibilities, including responsibilities related to backup, recovery, and disasters. Make sure that you fully understand the ownership of responsibilities for each of the services that you use, since they determine the actions to successfully recover your service instances and help you plan accordingly.

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

## Stay updated with {{site.data.keyword.cloud_notm}} notifications
{: #IBM-cloud-notifications}

If a disaster occurs that affects an {{site.data.keyword.cloud_notm}} service or region, you receive notifications from {{site.data.keyword.cloud_notm}} in your account or by email. Sign up for notifications on your account by reviewing [View notifications](/docs/account?topic=account-viewing-notifications#subscribe-email-notifications). You can also view the {{site.data.keyword.cloud_notm}} [Status Overview](/status) page.

## Use {{site.data.keyword.cloud_notm}} services for disaster recovery
{: #KeyServicesTitle}

You can use {{site.data.keyword.cloud_notm}} services to facilitate a disaster recovery provision and to enact your plan. Review the following overview of {{site.data.keyword.cloud_notm}} services for disaster recovery. Refer to each service's documentation set for more information.

### {{site.data.keyword.cos_full_notm}}
{: #KeyServicesCOS}

One key to DR is the ability to retrieve and recover data in the DR region. For many {{site.data.keyword.cloud_notm}} services, you can use {{site.data.keyword.cos_short}} to make data, like backups and snapshots, available in secondary regions by using a cross-regional bucket or a replicated bucket.

Data that is stored in {{site.data.keyword.cos_short}} is highly resilient and replicated across three locations based on the bucket type and its data dispersal method. Single site buckets have data dispersed in three locations in a data center. Regional buckets have data dispersed over the region’s three zones. Cross-region buckets have data dispersed over regions in the geo.

Cross-region buckets offer the simplest choice for disaster recovery and are available as follows:

* `ap-geo` – Asia Pacific (Japan, Australia)
* `us-geo` – North America (US, Canada)
* `eu-geo` – Europe (UK, Germany, Spain)

For a cross-region bucket, a tethered endpoint keeps all data ingress and egress within a specified region while still distributing the data. However, this mechanism does not provide an automated failover if the tethered region fails. Therefore, it’s important to connect services to your buckets by using an endpoint that is available in the region where it’s needed.

Cross-region buckets might not be suitable for users with regulatory compliance concerns. For example, a US-based organization might not be able to store data in Canada. Or, a European Union-based company might not be able to store data outside of the European Union, such as the UK.

In these cases, bucket replication offers a solution. With bucket replication, objects written to a source bucket are asynchronously copied to a target bucket, helping ensure redundancy with eventual consistency. You must set up and manage replication. By using this method, you can control the locations of the data and its sovereignty.

Versioning is a requirement for bucket replication so that buckets that are configured with an immutable {{site.data.keyword.cloud_notm}} policy can't be replicated. Server-side encryption with customer-provided keys is not supported. Where server-side encryption is required, use {{site.data.keyword.keymanagementserviceshort}}.

### {{site.data.keyword.bplong_notm}}
{: #KeyServicesSchematics}

Use automation to deploy or recover an environment quickly and accurately. Automating deployments with IaC, Configuration as Code, and toolchains helps you recover workloads faster and more accurately by re-creating lost environments more precisely than manual methods. Using automation also helps you react faster to unexpected scenarios, especially if you need to create and configure services on-demand in a different recovery region than you first planned.

{{site.data.keyword.bpshort}} is a service that can play an important part in any disaster recovery strategy. With {{site.data.keyword.bpshort}}, you can deploy infrastructure and services quickly and consistently though Infrastructure as Code (IaC), Terraform, Ansible, Helm, and Red Hat OpenShift Operators.

Codifying the workload environment means defining and automating the setup of your workload by using code. This way, the right services can be quickly provisioned and configured in any {{site.data.keyword.cloud_notm}} region. You can use Terraform and Ansible to provision services, including servers, storage, networking, and databases. They can also be used to configure the services, as can Helm and Operators.

IaC code should be maintained, version-controlled like application code, and stored for access across multiple regions. Using a Git-based repository is recommended.

As with any code, it is best practice not to hardcode elements, such as regions or zone names. Instead use variables, which can be resolved through the {{site.data.keyword.bpshort}} interface.

Employing DevOps toolchains can automate much of the environment build, including configuration, so invest time in considering how you can use these tools.

### Deployable Architectures
{: #KeyServicesDeployableArchitectures}

A deployable architecture is a preconfigured set of IaC assets that are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture. These deployable architectures enable you to meet {{site.data.keyword.cloud_notm}} Framework for Financial Services best practices. With a deployable architecture, you can accurately deploy the same architecture into different regions. These preconfigured architectures also provide a starting point for customization. Make sure that you maintain any customizations throughout each deployment.

When using a deployable architecture, you are responsible for the following disaster recovery actions:

* Provisioning disaster recovery environments, including any dependencies
* Data and configuration backup
* Replicating data and configuration to the disaster recovery environment
* Managing failover operations

### {{site.data.keyword.codeenginefull_notm}}
{: #KeyServicesCodeEngine}

You can use {{site.data.keyword.codeengineshort}} to deploy containerized workloads quickly by recovering certain containerized applications without the need to rebuild an entire Red Hat OpenShift or {{site.data.keyword.cloud_notm}} Kubernetes cluster. You can also use it to configure and run jobs, such as triggering database or server backups.
