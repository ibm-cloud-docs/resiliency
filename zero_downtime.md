---

copyright:
  years: 2018, 2024
lastupdated: "2024-10-09"

keywords: load balancing, global load balancing, HA, DR, high availability, disaster recovery, HA for the platform, high availability for platform, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# How {{site.data.keyword.cloud_notm}} ensures high availability and disaster recovery
{: #zero-downtime}

{{site.data.keyword.cloud}} provides you with a global infrastructure and portfolio of cloud services to deploy workloads and applications according to your global strategy, availability, and business continuity needs.
{: shortdesc}

{{site.data.keyword.cloud_notm}} services are designed with different redundant deployments and fault isolation patterns depending on their location and availability scopes across the different {{site.data.keyword.cloud_notm}} regions and data centers.

It's important that you understand how {{site.data.keyword.cloud_notm}} services are designed and their deployment in {{site.data.keyword.cloud_notm}} global locations so that you can make appropriate choices about the dependencies on services and their location that you select in designing your workload and application high availability and disaster recovery.

## {{site.data.keyword.cloud_notm}} services architecture for high availability and resiliency
{: #services-architecture}

{{site.data.keyword.cloud_notm}} services architecture implements the following architecture patterns to design our services to achieve high availability and resilience to different fault types that might impact the distributed IBM cloud infrastructure.
{: shortdesc}

### Service data plane protection from control plane faults
{: #service-data-control-plane}

{{site.data.keyword.cloud_notm}} services are architected to separate the components that implement them into data plane and control plane.

The data plane components of a service are responsible for provides the primary function of the service. They process all requests from users and client applications implementing the data processing, persistence, load balancing, and so on.

For example, the following are all data plane responsibilities:

- Running and hosting the Virtual Server Instance (VSI)
- Reading and writing to block storage volumes
- Getting and setting objects into Cloud Object Storage Buckets
- Running, processing queries and updates to IBM Cloud Databases PostegreSQL database

The control plane components of a service are responsible to administer and configure the data plane components to do their work. They process the request from administrators to manage the data plane lifecycle through the resource creation, configuration, upgrade, decommission phases of service instances.

For example, the following are all control plane responsibilities:

- Listing the Virtual Server Instance instances (VSI) in the account and provisioning a new the Virtual Server Instance (VSI) orchestrating the creation of virtual machines from an OS image, block storage creation, attachment and configuration of the network endpoints
- Configuring, resizing, and mounting block storage volumes
- Creating new Cloud Object Storage Buckets

To improve resiliency and business continuity, service data planes are designed to minimize dependencies on the control plane and continue to deliver their primary function even in cases of failures of the control plane. As an example, data plane access to infrastructure resources, once provisioned, has no dependency on the control plane, and therefore is not affected by any control plane issue.

Control plane failures impact only the ability to create, modify, or delete resources, but is not impacting existing resources that therefore remain available.

### Zonal service independence
{: #zonal-service}

Zonal services are the services that enable to request instances of the service to be deployed in a specific zone of a multizone region or a specific data center.

These service instances that are deployed in one zone in a region or data center are implemented and operated independently in each zone within a region, without dependencies on components of the services in other zones or data centers. This guarantees that failures in one zone might impact only the instance that is hosted in that zone and do not impact instances on another zone in the same region or in other regions.

Zonal services architecture uses a zonal data plane that is deployed in each zone of a region and managed from the local in-region control plane component.

The user or application interacts with the service instance function by using a zonal API endpoint that is located in each target zone.

The service control plane, with some exceptions that are described in the [Global service redundancy](#global-service) section, are located in the same region of the data plane and deployed across 3 zones of the regions, independent from other regions control planes. This guarantees that control plane failures in one region might impact only the service functions in that region and do not impact service functions in other regions.

If there is failure of control plane in, or even completely losing, one zone the request from administrators to manage the data plane lifecycle through the resource creation, configuration, upgrade, decommission phases of service instances are performed by the control plane in the remaining zones.

Even in the exceptional cases that the control plane is a globally deployed, it is still deployed across multiple regions to ensure high availability to guarantee that failures in one region do not impact service functions in other regions.

For more information about the specific options for deploying your workloads that use zonal service, see [Locations for resource deployment](/docs/overview?topic=overview-locations) and [Considerations for high availability](/docs/overview?topic=overview-ha-considerations).


### Regional service redundancy
{: #regional-service}

Regional services are the services that enable to request instances of the service to be deployed in a specific region as a whole without specifying a single target zone or data center.

These service instances that are deployed in one region are implemented and operated with redundant components that are deployed in multiple zone of the same region without single point of failures on single zones of the region.

Regional services architecture uses a regional data plane that is deployed across 3 zones in each region that is managed from the local in-region control plane. If there is failure of data plane in, or even completely losing, one zone the requests from users and client applications are automatically rerouted to the data plane on the remaining zones keeping the services up and running with a [Service Level Agreement](/docs/overview?topic=overview-slas) of 99.99% availability.

The user or application interacts with the service instance function by using a regional API endpoint that is located in each target region.

The service control plane, with some exceptions that are described in the [Global service redundancy](#global-service) section, are located in the same region of the data plane and deployed across 3 zones of the regions, independent from other regions control planes. This guarantees that control plane failures in one region might impact only the service functions in that region and do not impact service functions in other regions.

If there is failure of control plane in, or even completely losing, one zone the request from administrators to manage the data plane lifecycle through the resource creation, configuration, upgrade, decommission phases of service instances are performed by the control plane on the remaining zones.

Even in the exceptional cases that the control plane is a globally deployed, it is still deployed across multiple regions to ensure high availability to guarantee that failures in one region do not impact service functions in other regions.

For more information about the specific options for deploying your workloads that use regional service, see [Locations for resource deployment](/docs/overview?topic=overview-locations) and [Considerations for high availability](/docs/overview?topic=overview-ha-considerations).


### Global service redundancy
{: #global-service}

A subset of {{site.data.keyword.cloud_notm}} services uses a global deployment model with components that are not located in each region but deployed across multiple regions in different locations and geography. They are typically services that provide common functions that other zonal or regional services depend upon or specific control plane components within a service that provide functions with a global scope.

Services that use a global deployment model implement a distributed architecture with components that are replicated in multiple Regions with load balanced across them and an automatic failover design to keep the services up and running without need of operators action with a [Service Level Agreement](/docs/overview?topic=overview-slas) of 99.99% availability.

This section provided details about all services that use a global deployment model and their cross-region impact on the dependencies from other zonal or regional services.

The intent is to provide clarity how this approach helps remove single points of failure in your architecture but might represent potential cross-region impacts, even when you are operating in a region that is different from where the global service control plane is hosted.

#### Global platform services
{: #global-platform}

Global platform services provide common functions that other zonal or regional services depend upon. They are control plane only that have the purpose of orchestrating user interfaces, user identities and accounts, access, billing, and so on, across all the {{site.data.keyword.cloud_notm}} global infrastructure.

The global platform services use global load-balancing strategies to ensure a redundant, highly available platform is available for you to access and manage your cloud services.

If there is an availability-impacting event in the regions in which the components of a global platform service are located, the management functions provided by the service can be degraded or not available.

The following are global platform services, their control plane location in the {{site.data.keyword.cloud_notm}} regions and the functions they provide that are not impacted unless there is an availability-impacting event in all of the listed regions.

| Service | Management function | Location | High availability |
| -------------- | -------------- | -------------- | -------------- |
| Console  \n [Navigating the {{site.data.keyword.cloud_notm}} console](/docs/overview?topic=overview-ui) | The IBM Cloud console provides the user interface that enables administrators to manage all {{site.data.keyword.cloud_notm}} resources and accounts, order new services instances, view pricing and billing information, get support, or check the status  | * us-south \n * us-east \n * eu-gb \n * eu-de \n * jp-tok \n * au-syd | Active/Active |
| Catalogs  \n [Catalog Management API](/apidocs/resource-catalog/private-catalog) | The Catalog management service enables to interact with the {{site.data.keyword.cloud_notm}} catalog to order provisioning of {{site.data.keyword.cloud_notm}} service instance and to manage the visibility of the {{site.data.keyword.cloud_notm}} catalog and controlling access to products in the public catalog and private catalogs for users in your account. | * us-south \n * eu-de \n * au-syd | Active/Active |
| Global search and tagging  \n [Global Search API](/apidocs/search), [Global Tagging API](/apidocs/tagging) | The search and tagging service enables to  \n * search cloud resource based on their attributes. \n * create, delete, search, attach, or detach tags to resources. | * us-south \n * eu-gb \n * eu-de \n * au-syd | Active/Active |
| Identity and Access management  \n [IAM Identity Services API](/apidocs/iam-identity-token-api) | The IAM control plane enables to  \n * authenticate and authorize the users log on and other action requests. \n * manage service identifiers, trusted profiles, and API key identities. \n * create, update, view, and delete IAM policies. An IAM policy enables a subject to access a resource. \n * create, update, view, and delete access groups allow for the assignment of policies to Users, service IDs and trusted profiles  | * us-south \n * eu-gb \n * eu-de \n * jp-tok \n * au-syd | Active/Active |
| Business Support Services   \n [User Management API](/apidocs/user-management)   \n [Usage Metering API](/apidocs/usage-metering)   \n [Usage Reports API](/apidocs/metering-reporting) | The Business Support Services enables to  \n * manage accounts, enterprises, and users. \n * manage the users within account, such as inviting, retrieving, updating, or removing users. \n * update user profiles and settings. \n * collect services usage metrics and generate billing reports  | * us-south \n * us-east \n * eu-gb \n * eu-de \n * jp-tok \n * au-syd | Active/Active |
| Cloud projects  \n [Projects API](/apidocs/projects) | The Project service enables to  \n * create, update, view, and delete projects. \n * deployment by using projects  | * us-south \n * us-east \n * eu-gb \n * eu-de  | Active/Active |
{: caption="Global platform services" caption-side="bottom"}

#### Services with global control planes
{: #service-global-control-plane}

Services with global control planes components within a service that provide functions with a global scope, not entire services like the previous categories.

While you interact with zonal and regional services in the region you specify, certain operations have an underlying dependency on a single region that is different from where the resource is located.

If there is an availability-impacting event in the regions in which the components of a global platform service are located. The management operations provided by the service can be degraded or not available.

The following are services with global control planes, their control plane location in the {{site.data.keyword.cloud_notm}} regions and the functions they provide that are impacted if there is an availability-impacting event in those regions.

| Service | Control plane management functions | Location | High availability |
| -------------- | -------------- | -------------- | -------------- |
| Classic infrastructure resource management | The infrastructure resource management service control plane enables to:  \n * create, update, view, and delete Classic virtual and bare metal servers resources on Classic networks/VLANs \n * create, update, and delete Classic networks/VLANs and Classic network routes or spans between those networks  | * us-south \n * us-east | Primary/Secondary |
| Public IP address management | Assign new public IP addresses or subnets for Internet/public load balancers, elastic IPs or virtual and bare metal servers resources with public addresses.  | * us-south \n * us-east | Primary/Secondary |
| IBMid  \n [My IBM](https://www.ibm.com/account/ca/en/){: external} | IBMid service control plane enables to \n * authenticate and authorize the IBMid users log on and other action requests. \n * create, update, view, and delete IBMid user identities.  | * us-south \n * us-east | Primary/Secondary |
| {{site.data.keyword.dns_short}}  \n [{{site.data.keyword.dns_short}} API](/apidocs/dns-svcs#introduction-to-dns-services-api) | IBM Cloud DNS Services allow you to:  \n * create, update, view, and delete e zones that are collections for holding domain names \n * create, update, view, and delete DNS resource records under these zones \n * create, update, view, and delete global load balancers to resolve hostnames to different IP addresses based on location policies.  | * us-south \n * us-east | Primary/Secondary |
| Transit Gateway  \n [Transit Gateway API](/apidocs/transit-gateway) | Transit Gateway service control plane enables to  \n * create, update, view, and delete transit gateways to connect VPCs together or with classic infrastructure networks.  \n * attach, detach connections to VPCs or classic infrastructure networks to multiple local gateways and a single global gateway. | * us-south  \n * us-east | Primary/Secondary |
| Direct Link  \n [Direct Link API](/apidocs/direct_link) | Direct Link service control plane enables to  \n * create, update, view, and delete direct links to connect VPCs or classic infrastructure networks with on-premises networks.  \n * attach, detach connections to on-premises networks to direct links. \n * configure import and export filters for a direct link. | * us-south  \n * us-east | Primary/Secondary |
| Cloud Object Storage Provisioning | Cloud Object Storage service control plane enables to  \n * create or delete a new Cloud Object Storage bucket with a unique global name in a region.  \n NOTE: All other control plane APIs on Cloud Object Storage buckets are hosted in the same region or geography as the chose region or geography for each Cloud Object Storage bucket. | * us-south  \n * us-east | Primary/Secondary |
 {: caption="Services with global control planes" caption-side="bottom"}

For more information about the specific options for best practices when you use platform services for high availability, refer to the following documentation.

| Platform service | Details |
|------------------|----------------|
|  Account management  |    [Best practices for setting up your account](/docs/account?topic=account-account_setup) and [Best practices for billing and usage](/docs/billing-usage?topic=billing-usage-best-practices)     |
| Catalogs    |    [Managing catalog settings](/docs/account?topic=account-filter-account)               |
| {{site.data.keyword.cloud-shell_short}} | [Understanding high availability and disaster recovery for Cloud Shell](/docs/cloud-shell?topic=cloud-shell-ha-dr) |
| Console | [Navigating the console](/docs/overview?topic=overview-ui) |
|      Global search and tagging         |    [Searching for resources](/docs/account?topic=account-searching-for-resources) and [Working with tags](/docs/account?topic=account-tag)        |
| IAM       |      [What is IBM Cloud Identity and Access Management?](/docs/account?topic=account-iamoverview)  |
| {{site.data.keyword.cloud_notm}} CLI | [Understanding high availability and disaster recovery for the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-ha-dr) |
| {{site.data.keyword.cloud_notm}} projects| [Understanding high availability and disaster recovery for projects](/docs/secure-enterprise?topic=secure-enterprise-ha-dr) |
| {{site.data.keyword.compliance_short}} | [Understanding high availability](/docs/security-compliance?topic=security-compliance-ha) and [disaster recovery](/docs/security-compliance?topic=security-compliance-bc-dr) for {{site.data.keyword.compliance_short}} |
{: caption="Platform services" caption-side="top"}

### Network backbone redundancy
{: #network-backbone}

The {{site.data.keyword.cloud_notm}} network is designed in such a way that a single point of failure never happens. Diverse, redundant connectivity exists at every point of the network, by using diverse telecommunication providers for the same service connectivity whenever possible within each region.

Diverse dark fiber providers are used to connect every edge site to all of the regional compute facilities. Additionally, each edge site has redundant backbone connectivity into other regions, and peers with multiple providers, both directly and indirectly, through a local exchange.

No single event should ever result in a service disruption that is noticed by our customers.

### Zonal and regional service isolation from cross region dependencies
{: #zone-region-service-isolation}

In general, if there is an availability-impacting event in one region, only zonal and regional services in that region are impacted. Services in other regions are not impacted.

In particular, the data planes of zonal and regional services that are located in a region that depend on other regional services for base functions like: infostructure resources, container orchestration, databases, security, and so on, are architected to depend only on instances located in the same region.

Data plane of a service located in a region depends also on service instances that are provided by the user to support the following service-to-service functions:

- Key Protect instance for bring-your-own-key (BYOK) encryption support.
- Hyper Protect Crypto instance for keep-your-own-key (KYOK) encryption support.
- Cloud Object Storage buckets for storing backups, Security Control Center evidence and results, archived logs, and so on, and in general for any function that supports to store or process large amount of data into/from Cloud Object Storage buckets.

Customers must be careful in selecting which region they allocate those services and the consequent availability impact to their services that depend on the "As a base rule". The recommendation is to locate them in the same region of the dependent service to avoid cross-region failure impacts.
{: note}

Each service documentation provides clear directions on how customers can use them, the location and configuration choices, to architect their applications for the wanted level of resilience.


## {{site.data.keyword.cloud_notm}} disaster recovery best practices
{: #disaster-recovery}

{{site.data.keyword.cloud_notm}} follows best practices for disaster recovery. All {{site.data.keyword.cloud_notm}} applications automatically recover and restart after any disaster event. Recovery is from electronic backups at a recovery center or alternative computing facilities that restore computing. Before any potential disaster, the disaster recovery plan includes the systems and hosting requirements for hardware, software, networking connectivity, and offsite backup capabilities.

The following list includes the requirements that {{site.data.keyword.IBM_notm}} adheres to for a disaster recovery plan:

- A design document explains how load balancing is used to keep a service highly available.
- Where multi-site failover occurs, the disaster recovery plan must explain who does what to cause the failover and ensure restart.
- The disaster recovery plan must define how the solution works and include restore point objectives that clearly explain how much data might be lost in the outage, if any. The disaster recovery plan also includes a detailed recovery workflow for restoring services and data if a multi-availability zone failure.
- It must confirm how the Maximum Tolerable Downtime is met and be stored on the Disaster Recovery Plan database.
- The disaster recovery plan specifies the security controls for running in Disaster mode, if they are different from what's running in production.

### Management of the disaster recovery plan
{: #dr-plan-mgmt}

The requirements that {{site.data.keyword.cloud_notm}} follows are:

- The disaster recovery plan must be updated after any major infrastructure change, major application release, and after any test.
- It must be approved annually.

For more information about the specific consideration for your workloads disaster recovery, see [Understanding disaster recovery](/docs/overview?topic=overview-understanding-dr).
