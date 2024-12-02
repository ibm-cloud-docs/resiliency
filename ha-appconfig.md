---

copyright:
  years: 2024
lastupdated: "2024-12-02"

keywords: high availability, regions, zones, resiliency

subcollection: ha-infrastructure

---

# High availability design for your workloads
{: #high-availability-design}

{{site.data.keyword.cloud_notm}} supports high availability application deployments within a single zone, across multiple zones in a multi-zone region, and across multiple regions.

Failure domains determine the degree of protection from infrastructure failures for each deployment option. An application instance deployed in a single zone is not protected against a failure of that zone. Application instances deployed in multiple availability zones are protected against a failure of a single zone.  The multiple availability zones are within the same metropolitan area and are connected by low latency network links that allow data to be synchronously replicated across the zones. Application instances deployed in multiple regions are protected against a failure of an entire region.  Different regions are located in different countries or in different parts of a single country.  The distances between regions typically only allows data to be replicated asynchronously.

The following table shows application deployment options based on failure domains available in a public cloud.



| Deployment          | Availability | Failure domain                | Cost and complexity |
|-------------------------------|--------------|-------------------------------|---------------------|
| Single-zone, \n single-region | Low/Med      | Virtual Server / Physical Host| Low                 |
| Multi-zone, \n single-region  | High         | Zone                          | Medium              |
| Multi-zone, \n multi-region   | Very high    | Region                        | High                |
{: caption="High availability deployment options and their respective availability, failure domain, and cost and complexity to maintain." caption-side="bottom"}

## Single-zone deployment
{: #single-zone}

In single-zone deployments, multiple application instances are deployed in one zone.  If an application instance runs in a single virtual server, [Placement Groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc) allow these virtual servers to be provisioned in separate physical hosts. [VPC Autoscale](/docs/vpc?topic=vpc-creating-auto-scale-instance-group) can be used to enable dynamic adjustment of capacity based on workload changes. Single zone deployments provide cost-effective solutions with 99.9% infrastructure availability.  This may be appropriate for non-production environments or non-business critical applications. However, single-zone deployments provide no protection from zone outages.

## Multi-zone, single-region deployment
{: #multi-zone-single-region}

In a multi-zone, single-region deployment, multiple application instances are deployed across two or more availability zones within the region. Multi-zone, single region deployments can provide up to 99.99% infrastructure availability, when the application is deployed across three availability zones. This deployment protects the application from zone failures, and is suitable for production-level enterprise workloads with \>99.9% availability requirements. Actual application availability will depend on the application high availability design.

## Multi-zone, multi-region deployment
{: #multi-zone-multi-region}

A multi-zone, multi-region deployment provides protection against region outages. This is the recommended deployment for mission-critical applications with continuous or near-continuous availability requirements. This deployment also supports out of region disaster recovery and business continuity for applications with cross-geography or specific separation distance requirements.

Multi-zone deployments rely on application-aware data replication across availability zones and support active-active and active-standby architecture patterns. Multi-zone, multi-region deployments support architecture patterns for enterprise applications with continuous availability/always-on requirements. The following tables show a comparison of the different deployment options and recommended use.

| Deployment    | Availability | Description   | Recommended use   |
|------------------|------------------|------------------|------------------|
| Single-zone                | 99.9%         | - Multiple compute instances in one zone \n - Protection from infrastructure failures \n - Low/Medium cost \n | - Low to medium priority applications \n - Non-critical production workloads |
| Multi-zone, single-region | 99.99%        | - Multiple compute instances across 2 or more availability zones \n - Synchronous data replication across zones \n - Protection from zone outages \n - Medium/high cost | - Core business applications \n - Production level workloads with stringent resiliency requirements \n - Business continuity policies with country boundaries or geographic data residence constraints |
| Multi-zone, multi-region  | &amp;gt;99.99%     | - Multiple compute instances across multiple availability zones in 2 or more regions \n - Asynchronous data replication between regions \n - Protection from region outages \n - High cost | - Mission-critical applications with continuous or near continuous availability requirements \n - Business continuity policies with cross geography or specific separation distance requirements \n - Disaster recovery |
{: caption="High availability deployment recommendations" caption-side="bottom"}

The following Architecture Framework provides design considerations and architecture decisions for deploying resilient applications on IBM Cloud Virtual Private Cloud (VPC) infrastructure. It covers the following solution aspects and domains:

- **Networking:** Load balancing, Domain name system
- **Security:** Data security
- **Resiliency:** High availability, Backup and restore, Disaster recovery
- **Service Management:** Monitoring, Logging, Auditing, Alerting

![VPC resiliency architecture design scope](images/heat-map-vpc-resiliency.svg){: caption="VPC resiliency architecture design scope" caption-side="bottom"}

The [Architecture Design Framework](/docs/architecture-framework) provides a consistent approach to design cloud solutions by addressing requirements across a set of aspects and domains, which are technology-agnostic architectural areas that need to be considered for any enterprise solution.

## Client retry logic for highly available applications
{: #client-retry-logic-for-ha}

It is up to application clients written in programming languages to recover from transient errors. These include network errors and temporary failures introduced by a service's High Availability implementation, like when a regional service recovers from a zonal failure. Check [Services specific high availability and disaster recovery](/docs/resiliency?topic=resiliency-service-ha-dr) for more information on specific {{site.data.keyword.cloud_notm}} services.

Many of the {{site.data.keyword.cloud_notm}} SDKs are built on the {{site.data.keyword.cloud_notm}} SDK Common that supports [automatic retries](https://github.com/IBM/ibm-cloud-sdk-common?tab=readme-ov-file#automatic-retries) designed to handle specific HTTP errors, like 429 and 503 errors. Not all errors are automatically handled by the SDK. Note that configuration is required by the client to take advantage of the retry logic.

Some {{site.data.keyword.cloud_notm}} services support open source protocols, and it can be appropriate to use open source SDKs. Examine these SDKs to determine if they are useful for your application and offer suitable retry functionality.

Retry logic varies depending on the type of {{site.data.keyword.cloud_notm}} service and the type of operation. Failed read and HTTP GET operations can generally be retried using exponential backoff with a fixed time period. The failures that should be retried will depend on the type of failure and the specific {{site.data.keyword.cloud_notm}} service. Check the {{site.data.keyword.cloud_notm}} service and SDK documentation for details.

Failed write, HTTP PUT, POST, DELETE, and other operations are likely not recoverable using a simple retry mechanism, unless it is clear that the operation did not complete and the documented client logic indicates a retry is appropriate. The result of a failed state changing operation in general is unknown and more advanced {{site.data.keyword.cloud_notm}} service specific recovery mechanism will be required. Check the {{site.data.keyword.cloud_notm}} service and SDK documentation for details.

Client retry will improve the availability of a single client. Workloads can be composed of many clients. Logging client failures to a centralized logging service like [{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started) allows failure and availability analysis of the entire workload.
