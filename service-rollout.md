---

copyright:

  years: 2022, 2024
lastupdated: "2024-08-06"

keywords: rollout

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Service rollout policy
{: #service-rollout}

{{site.data.keyword.cloud}} has a resilient global network of locations to host your highly available cloud workload. To ensure that the cloud infrastructure and services are consistent and stable across our deployment locations, we created best practices for our service catalog management. These best practices help us to accomplish rollouts in the most efficient manner and minimize business impact, costs, and risks. The following information describes our guidelines on when to expect or how to request that a service is available in your region.

This policy covers all {{site.data.keyword.cloud_notm}} public [MZRs](/docs/overview?topic=overview-locations#table-mzr), public [single-campus MZRs](/docs/overview?topic=overview-locations#single-campus-mzr) and public [data centers](/docs/overview?topic=overview-locations#data-centers).

{{site.data.keyword.IBM}} classifies our services deployed to our public locations as core or market-driven.

## Core services
{: #core-services}

All {{site.data.keyword.IBM}} multi-zone regions contain the following core services, which are the most basic and vital services that are needed for the majority of customer workloads.

* {{site.data.keyword.cloud_notm}} platform (console, CLI, Identity and Access Management, and global catalog)
* {{site.data.keyword.vpc_full}}
   * {{site.data.keyword.block_storage_is_full}}
   * {{site.data.keyword.vsi_is_full}}
   * {{site.data.keyword.vpn_vpc_full}}
   * {{site.data.keyword.cloud}} Transit Gateway
   * {{site.data.keyword.nlb_full}}
   * {{site.data.keyword.alb_full}}
   * {{site.data.keyword.vpe_full}}
   * {{site.data.keyword.cloud}} DNS Services
* {{site.data.keyword.cos_full_notm}}
* {{site.data.keyword.databases-for-postgresql_full_notm}}
* {{site.data.keyword.keymanagementservicefull_notm}}
* {{site.data.keyword.contdelivery_full}}
* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.openshiftlong_notm}}

The {{site.data.keyword.cloud_notm}} platform, including the console, CLI, Identity and Access Management, and global catalog, is a globally accessible instance that is independent of any region or zone. Global resources like the platform are accessible from a global endpoint.
{: note}

## Deployment Tiers
{: #deployments}

{{site.data.keyword.IBM_notm}} identifies the following deployment tiers that can contain core services, market driven services or both.

| Deployment tier |Core service   |Market-driven services|
|------------------------------------------|---------------------------------------------|---------------------------------------------|
|[MZR](/docs/overview?topic=overview-locations#table-mzr)| ![Checkmark icon](../icons/checkmark-icon.svg)    |    ![Checkmark icon](../icons/checkmark-icon.svg)   |
|[Single campus MZR](/docs/overview?topic=overview-locations#single-campus-mzr)|    ![Checkmark icon](../icons/checkmark-icon.svg)   |    ![Checkmark icon](../icons/checkmark-icon.svg)    |
|[Data center](/docs/overview?topic=overview-locations#data-centers)|       |    ![Checkmark icon](../icons/checkmark-icon.svg)    |
{: caption="Table 1. Deployment tiers and service types" caption-side="top"}


### Core service deployments
{: #core-deploy}

Adding new core service to existing MZRs
:   After a new core service is deployed in the first MZR and added to this {{site.data.keyword.cloud_notm}} service rollout policy, the new core service will be deployed to all other MZRs within a period of 90 days.

Updating existing core services in existing MZRs
:   After a generally available update to an existing core service is deployed in the first MZR and documented in a release note, the same update will be deployed to all other MZRs within a period of 30 days.

Not all hardware dependent profiles and features are available in all MZRs. If the service you want depends on such a profile or feature, contact [{{site.data.keyword.cloud_notm}} Sales](https://www.ibm.com/cloud?contactmodule){: external} for details on availability.
{: important}

Some services could deploy sooner.
{: note}

### Market-driven deployments
{: #market-driven-deploy}

Market-driven services are deployed based on sufficient customer demand. To request that one of these services is available in your region, contact [{{site.data.keyword.cloud_notm}} Sales](https://www.ibm.com/cloud?contactmodule){: external}.

Deployments into any location other than an MZR are always market-driven.

Market-driven classification covers any case other than those specified under the MZRs' description.
{: note}

## Dependency and service availability standards
{: #availability-standards}

For each service, you can review our dependency and availability standards.

* {{site.data.keyword.cloud_notm}} services are available in several [regions worldwide](/docs/overview?topic=overview-services_region).
* IBM services can be deployed by using various models, and the SLA standards that apply to each model are outlined in the [Service Level Agreements](https://www.ibm.com/support/customer/csol/terms/?id=i126-6605&lc=en){: external}
* Follow our best practices to ensure your environment that integrates the use of the {{site.data.keyword.cloud_notm}} services is as resilient as possible. Check out the [resiliency documentation](https://www.ibm.com/topics/cyber-resilience){: external} that is published in the architecture center.
* To build a solution that meets your needs, it is important for you to see the [Shared responsibility matrix](/docs/overview?topic=overview-shared-responsibilities) for more information on the scope of the services.
* For quick reference, we include links to the high availability documents for the services within our [SLO](/docs/overview?topic=overview-slo) description.

### Dependency levels
{: #dependency-levels}

Low-level implementation and details of services change based on your setup and configuration requirements. As such, we've provided a high-level explanation for the dependencies.

These levels might be considered layers or tiers in an architectural block diagram. Each layer might depend only on the layers below it, although there are some dependencies that might be fulfilled in the same layer in the case of some complex services.

Core services form the foundation for our cloud infrastructure. These services are required for a new MZR location to go live and are required to be updated across the entire footprint when there is a fundamental change.

Market-driven services are based on market demand and depend on the core services and some other services within this market-driven category. The other services are services that are needed to support regulation that applies in different regions or industries.

Market-driven classification covers all services other than those specified under the core service description.
{: important}

### Service-to-service dependencies
{: #service-service-dependency}

The {{site.data.keyword.cloud_notm}} backend services support all of the other services within our deployment units.

{{site.data.keyword.cloud_notm}} services have a set of dependencies that are self-contained within the MZR deployment units. The rest of the service dependencies use global services that are served from cross regional locations. Any solution that is not hosted on an MZR might require an associated MZR that hosts the control plane services.

If you are a current {{site.data.keyword.cloud_notm}} customer and have a non-disclosure agreement with IBM, you can request the service dependency reports by going to [Compliance support](/docs/overview?topic=overview-compliance#reports) under PaaS compliance report and submit a request.
{: note}

For the key dimensions of compute, storage, networking, and authentication or authorization, we indicate the following dependencies.

Control planes
:   Common dependencies for control planes are:

    * The identity or platform data plane for authentication and authorization
    * The audit tracking service
    * Internal services that provide, for example, workflow, metadata storage, monitoring and logging
    * Load balancers, VPN, and other network infrastructure services

    Some control planes obviously have service-specific dependencies. For example, the compute control plane, when starting a bare metal or VM instance, depends on:
    * Object Storage to retrieve the required system images
    * Block Volumes control plane for provisioning and attaching the volumes
    * Networking control plane to provision and attach NICs, subnets, and so on

Data planes
:   Core service data planes follow the general principle that each data plane is designed to have minimal dependencies in order to achieve high availability, fast time to diagnosis, and fast time to recovery. Therefore, the systems continue to function (with some limitations, for example cannot provision, reboot, or decom) even when there is a control plane outage.

Network
:   The networking data and control planes exist in every deployment unit.

   Some services depend on others in the core, for example Compute services like bare metal and VM instances depend the Block Volumes data plane and the Networking data plane, and therefore will be impacted by the data planes for these services going down. But as stated before the control planes might suffer an outage and the Compute instances continue to function, if they do not require the associated service control plane (that is, increasing volume storage size, and so on)

Internal services
:   All client facing internal or external services depend on the identity and platform data plane for authentication and authorization. Control planes for monitoring, logging, and {{site.data.keyword.cloud_notm}} CLI and data plane services for security services depend on the identity platform data plane. (Specific configurations might vary.)

Storage
:   The Object Storage data plane does not depend on Block Volumes or File Storage. Services that support backup and restore depend on Object Storage to operate. Therefore, those services require that the Block Volumes data and control planes exist in every deployment unit.

## Related documents
{: #related-docs}

| Document and Link                         | Description                                 |
|-------------------------------------------|---------------------------------------------|
| [SLA](https://www.ibm.com/support/customer/csol/terms/?id=i126-6605&lc=en){: external} | {{site.data.keyword.cloud_notm}} SLA  |
| [Shared Responsibility Matrix](/docs/overview?topic=overview-shared-responsibilities)  | {{site.data.keyword.IBM_notm}}'s customer shared responsibility matrix |
| [Availability of services](/docs/overview?topic=overview-services_region) | Listing of services available by location   |
| [SLO](/docs/overview?topic=overview-slo)  | {{site.data.keyword.cloud_notm}} SLO  |
{: caption="Table 2. Related documents" caption-side="top"}
