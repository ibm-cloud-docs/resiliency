---

copyright:
  years: 2024, 2024
lastupdated: "2024-11-13"

keywords: load balancing, global load balancing, HA, DR, high availability, disaster recovery, HA for the platform, high availability for platform, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: design

---

{{site.data.keyword.attribute-definition-list}}

# High Availability and Redundancy Design within IBM Cloud
{: #ha-redundancy}

## How {{site.data.keyword.cloud_notm}} ensures high availability through redundancy
{: #zero-downtime}

{{site.data.keyword.cloud}} provides you with a global infrastructure and portfolio of cloud services to deploy workloads and applications according to your global strategy, availability, and business continuity needs.
{: shortdesc}

{{site.data.keyword.cloud_notm}} services are designed with different redundant deployments and fault isolation patterns depending on their location and availability scopes across the different {{site.data.keyword.cloud_notm}} regions and data centers.

It's important that you understand how {{site.data.keyword.cloud_notm}} services are designed and their deployment in {{site.data.keyword.cloud_notm}} global locations so that you can make appropriate choices about the dependencies on services and their location that you select in designing your workload and application high availability.

## What levels of resiliency do the different zones and regions provide
{: #landing-levels-of-resiliency}

![Levels of Resiliency](images/ha-resiliency-infographic.png){: caption="Levels of Resiliency" caption-side="bottom"}

Whether the data centers are single-zone or multi-zone, all of them avoid a single-point-of-failure (SPOF) between zones and regions by providing:

* Multiple power feeds 
* Fiber links
* Dedicated generators
* Battery backup 

While all the data centers have multiple power feeds, several of the more mature sites have some 1U single socket server chassis that might not accommodate a dual power feed. If you have a 1U single socket server in one of these sites, you might want to consider a 2U chassis with redundant power supplies. For more information about availability zones, see [Locations for resource deployment](/docs/overview?topic=overview-locations).
{: note}

## {{site.data.keyword.cloud_notm}} services architecture for high availability and resiliency
{: #services-architecture}

{{site.data.keyword.cloud_notm}} services architecture implements the following architecture patterns to design our services to achieve high availability and resilience to different fault types that might impact the distributed IBM cloud infrastructure.
{: shortdesc}

### Service data plane protection from control plane faults
{: #service-data-control-plane}

{{site.data.keyword.cloud_notm}} services are architected to separate the components that implement them into data plane and control plane.

The data plane components are responsible to provide the primary functions of the service. They process requests from users and client applications, such as implementing data processing, persistence, load balancing, and so on.

For example, the following are data plane responsibilities:

- Running and hosting the Virtual Server Instance (VSI)
- Reading and writing to block storage volumes
- Getting and setting objects into Cloud Object Storage Buckets
- Running, processing queries and updates to IBM Cloud Databases PostegreSQL database

The control plane components are responsible to administer and configure the data plane components to do their work. They process requests from administrators to manage the data plane lifecycle through the resource creation, configuration, upgrade and decommission phases of service instances.

For example, the following are control plane responsibilities:

- Listing the Virtual Server Instances (VSI) in the account and provisioning a new VSI, orchestrating the creation of virtual machines from an OS image, block storage creation, attachment and configuration of the network endpoints
- Configuring, resizing, and mounting block storage volumes
- Creating new Cloud Object Storage Buckets

To improve resiliency and business continuity, service data planes are designed to continue to deliver their primary function even in cases of failures of the control plane. As an example, data plane access to infrastructure resources, once provisioned, has no dependency on the control plane, and therefore is not affected by any control plane issues.

Control plane failures may impact the ability to create, modify, or delete resources but there is no impact to existing resources which remain available.

### Zonal service independence
{: #zonal-service}

Zonal services allow the request of service instances to be deployed in a specific zone of a multizone region or a specific data center.

These service instances deployed in a specific zone or specific data center are implemented and operated independently within its region, without dependencies on components of the services in other zones or data centers. Therefore failures in one zone may impact the instance that is hosted in that zone but they will not impact any instance in other zones in the same region or in other regions.

Zonal services architecture uses a zonal data plane that is deployed in each zone of a region and managed from the local in-region control plane component.

The user or application interacts with the service instance function by using a zonal API endpoint that is located in each target zone.

The service control plane, with some exceptions described in the [Global service redundancy](#global-service) section, is located in the same region of the data plane and deployed across 3 zones of the regions. It is independent from control planes in other regions. Therefore a control plane failure in one region may only impact service functions in that region but it will not impact service functions in other regions.

If there is a failure of the control plane in one zone, or if a zone is completely unavailable, then administrator requests to manage the data plane lifecycle phases (resource creation, configuration, upgrade and decommission) are performed by the control plane in the remaining two zones.

In exceptional cases where the control plane is globally deployed, it is still deployed across multiple regions to ensure high availability. Therefore failures in one region would not impact service functions in the other regions.

For more information about the specific options for deploying your workloads that use zonal service, see [Locations for resource deployment](/docs/overview?topic=overview-locations) and [Considerations for high availability](/docs/overview?topic=overview-ha-considerations).


### Regional service redundancy
{: #regional-service}

Regional services allow the request of service instances to be deployed in a specific region as a whole without specifying a single target zone or data center.

These service instances deployed in one region are implemented and operated with redundant components deployed in multiple zones within the same region. In this way, there is no single point of failure on any specific zone within a region.

Regional services architecture uses a regional data plane deployed across 3 zones in each region that is managed from the local in-region control plane. If there is a data plane failure in one zone, or if a zone is completely unavailable, then requests from users and client applications are automatically rerouted to the data plane in the remaining two zones.

The user or application interacts with the service instance function by using a regional API endpoint that is located in each target region.

The service control plane, with some exceptions that are described in the [Global service redundancy](#global-service) section, are located in the same region of the data plane and deployed across 3 zones of the regions, independent from other regions control planes. This guarantees that control plane failures in one region might impact only the service functions in that region and do not impact service functions in other regions.

If there is failure of control plane in, or even completely losing, one zone the request from administrators to manage the data plane lifecycle through the resource creation, configuration, upgrade, decommission phases of service instances are performed by the control plane on the remaining zones.

Even in the exceptional cases that the control plane is a globally deployed, it is still deployed across multiple regions to ensure high availability to guarantee that failures in one region do not impact service functions in other regions.

For more information about the specific options for deploying your workloads that use regional service, see [Locations for resource deployment](/docs/overview?topic=overview-locations) and [Considerations for high availability](/docs/overview?topic=overview-ha-considerations).


### Global service redundancy
{: #global-service}

A subset of {{site.data.keyword.cloud_notm}} services uses a global deployment model with components that are not located in each region but deployed across multiple regions in different locations and geography. These services provide common functions that other zonal or regional services depend upon. Or, these can be specific control plane components within a service that provide global functions.

Services that use a global deployment model implement a distributed architecture with components replicated in multiple regions. The components are load balanced across these regions with an automatic failover design to keep the services up and running without the need of an operator's action.

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

Global control planes components within a service provide functions with global scope. In this case, certain operations with zonal and regional services in a specific region may have an underlying dependency on a region that is different from where the resource is located. 

If there is an availability-impacting event in the regions in which the components of a global platform service are located. The management operations provided by the service can be degraded or not available.

If there is an availability-impacting event in a region in which the components of a global platform service are located, then management operations provided by the service may be degraded or unavailable.

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

The {{site.data.keyword.cloud_notm}} network is designed so that there is never a single point of failure. There is redundant connectivity at every point of the network by using diverse telecommunication providers for the service connectivity whenever possible within each region.

Diverse dark fiber providers are used to connect edge sites to all of the regional compute facilities. Additionally, each edge site has a redundant backbone connectivity into other regions, and peers with multiple providers, directly and indirectly through a local exchange.

### Zonal and regional service isolation from cross region dependencies
{: #zone-region-service-isolation}

In general, if there is an availability-impacting event in one region, only zonal and regional services in that region are impacted. Services in other regions are not impacted.

In general, if there is an availability-impacting event in one region then it is limited to zonal and regional services within that region. Services in other regions would not be impacted.

In particular, the data planes of zonal and regional services located in a region depend on resources located within the same region. That is for dependencies on other regional services for base functions, such as infrastructure resources, container orchestration, databases, security, and so on.

Data plane of a service located in a region depends also on service instances that are provided by the user to support the following service-to-service functions:

- Key Protect instance for bring-your-own-key (BYOK) encryption support.
- Hyper Protect Crypto instance for keep-your-own-key (KYOK) encryption support.
- Cloud Object Storage buckets for storing backups, Security Control Center evidence and results, archived logs, and so on, and in general for any function that supports to store or process large amount of data into/from Cloud Object Storage buckets.

Customers should carefully select the region for service allocation to ensure availability, with the recommendation to place services in the same region as dependent services to prevent cross-region failure impacts.
{: note}

Each service documentation provides clear directions on how customers can use them, the location and configuration choices, to architect their applications for the wanted level of resilience.
