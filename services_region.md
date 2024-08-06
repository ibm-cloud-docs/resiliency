---

copyright:

  years: 2015, 2023
lastupdated: "2024-08-06"

keywords: location, regions, data centers, service location, service availability, multizone regions, MZR

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Service and infrastructure availability by location
{: #services_region}

{{site.data.keyword.Bluemix}} makes it easier for you to implement, host, and scale services, infrastructure, and apps so you can focus on your application logic and application design. {{site.data.keyword.cloud_notm}}'s global network of locations provides three tiers of regions: [multizone regions](#x9774820){: term} (MZR),[single-campus multizone regions](#x10127487){: term}, and [data centers](#x2439906){: term}. To achieve low application latency, deploy your apps in a region near your customers. For details about the available {{site.data.keyword.cloud_notm}} regions and data centers for specific products, review the following sections.
{: shortdesc}

IBM provides SDKs and APIs for all services that are general availability. Check out the reference docs in the [API & SDK reference library](/docs?tab=api-docs).

Not all services and infrastructure are available for purchase in every {{site.data.keyword.Bluemix_notm}} location.

## Services
{: #paas-services}

Some services are available to purchase in a location, but that service's data might be hosted in a different location. The following table shows the services that are provided by IBM. For the full list of resources that are available, see the [catalog](https://cloud.ibm.com/catalog){: external} in the {{site.data.keyword.Bluemix_notm}} console.

Services that are hosted globally create resources that operate across multiple locations. For example, with {{site.data.keyword.cos_full_notm}}, you can choose to deploy data in a single data center, or even a combination of locations by selecting the endpoint where your application sends REST API requests. For more information about the global network of locations, see [Locations for resource deployment](https://cloud.ibm.com/docs/overview?topic=overview-locations).





| Service | Dallas | Washington DC | Toronto | Sao Paulo | 
|-----|-----|-----|-----|-----|
| API Connect | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Analytics Engine | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| App Configuration | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| App ID | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Auto Scale for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Bare Metal Server for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Block Storage Snapshots for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Block Storage for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Client VPN for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Activity Tracker | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Logs |  |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Cloud Monitoring | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Object Storage | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Cloudant | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Code Engine | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Compose for Elasticsearch | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Compose for MongoDB | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Compose for PostgreSQL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Compose for RabbitMQ | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Compose for Redis | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Compose for etcd | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Container Registry | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Continuous Delivery | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cost and Asset Management | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| DNS Services | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Data Engine (previously SQL Query) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Data Product Hub | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Data Replication | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| DataStage | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Databases for EDB | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for Elasticsearch | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for MongoDB | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for MySQL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for PostgreSQL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for Redis | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for etcd | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Db2 | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Db2 Warehouse | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Dedicated Host for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Direct Link Dedicated | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Event Notifications | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Event Streams | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| File Storage for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Floating IP for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Flow Logs for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Functions | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Historical Instrument Analytics | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Hyper Protect Crypto Services | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hyper Protect Virtual Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| IBM Cloud Backup for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Pak for Data | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| IBM Cloud Reservations for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cognos Dashboard Embedded | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| IBM Knowledge Catalog | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| IBM Match 360 with Watson | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| IBM Verify |  |  |  |  | 
| IBM watsonx Code Assistant | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Image Service for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Instrument Analytics | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Internet Services | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Internet of Things Platform |  |  |  |  | 
| Investment Portfolio | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Key Protect | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Knowledge Studio | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Kubernetes Service | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Load Balancer for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Log Analysis | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| MQ | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Messages for RabbitMQ | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Multi Volume Snapshots for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Natural Language Understanding | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Netezza Performance Server | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Network ACL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| OpenPages | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Partner with Technology Expert Labs | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Placement Groups for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Planning Analytics | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Portfolio Optimization | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Predictive Market Scenarios | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Public Gateway | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Qiskit Runtime |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Real-Time Payments | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Red Hat OpenShift on IBM Cloud | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSH Key for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Satellite | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Satellite Infrastructure Service |  |  |  |  | 
| Schematics | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Secrets Manager | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secure Gateway | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Security Group for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Security and Compliance Center | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Security and Compliance Center Workload Protection | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Simulated Historical Instrument Analytics | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Simulated Instrument Analytics | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Software Instance | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Speech to Text | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Streaming Analytics | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Subnet | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Text to Speech | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Toolchain | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Transit Gateway | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| VCF as a Service - Cloud Director Site | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| VCF as a Service - Virtual Data Center | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| VMware Solutions | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| VPN for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Network Interface for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Private Cloud | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Private Endpoint for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Server for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Watson Discovery | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Watson Machine Learning | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Watson Query | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Watson Studio | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Workspace for Power Virtual Server | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| watsonx Assistant | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| watsonx Orchestrate | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| watsonx.data | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| watsonx.governance | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
{: row-headers}
{: caption="Table 1. Service availability - Americas" caption-side="top"}
{: #paas-table1}
{: tab-title="Americas"}
{: tab-group="PaaS"}
{: class="simple-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}

| Service | London | Frankfurt | Madrid | 
|----|----|----|----|
| API Connect | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Analytics Engine |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| App Configuration | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| App ID | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Auto Scale for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Bare Metal Server for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Block Storage Snapshots for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Block Storage for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Client VPN for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Activity Tracker | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Logs |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Monitoring | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Object Storage | Hosted Globally | Hosted Globally | Hosted Globally | 
| Cloudant | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Code Engine | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Compose for Elasticsearch | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Compose for MongoDB | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Compose for PostgreSQL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Compose for RabbitMQ | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Compose for Redis | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Compose for etcd | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Container Registry | Hosted Globally | Hosted Globally | Hosted Globally | 
| Continuous Delivery | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cost and Asset Management |  |  |  | 
| DNS Services | Hosted Globally | Hosted Globally | Hosted Globally | 
| Data Engine (previously SQL Query) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Data Product Hub |  |  |  | 
| Data Replication | Hosted Globally | Hosted Globally | Hosted Globally | 
| DataStage |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Databases for EDB | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for Elasticsearch | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for MongoDB | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for MySQL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for PostgreSQL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for Redis | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for etcd | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Db2 | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Db2 Warehouse | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Dedicated Host for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect | Hosted Globally | Hosted Globally | Hosted Globally | 
| Direct Link Dedicated | Hosted Globally | Hosted Globally | Hosted Globally | 
| Event Notifications | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Event Streams | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| File Storage for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Floating IP for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Flow Logs for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Functions | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Historical Instrument Analytics |  |  |  | 
| Hyper Protect Crypto Services | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hyper Protect Virtual Server for Classic |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| IBM Cloud Backup for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Pak for Data | Hosted Globally | Hosted Globally | Hosted Globally | 
| IBM Cloud Reservations for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cognos Dashboard Embedded | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| IBM Knowledge Catalog | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| IBM Match 360 with Watson |  |  |  | 
| IBM Verify |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| IBM watsonx Code Assistant |  |  |  | 
| Image Service for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Instrument Analytics |  |  |  | 
| Internet Services | Hosted Globally | Hosted Globally | Hosted Globally | 
| Internet of Things Platform |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Investment Portfolio |  |  |  | 
| Key Protect | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Knowledge Studio | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Kubernetes Service | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Load Balancer for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Log Analysis | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| MQ | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Messages for RabbitMQ | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Multi Volume Snapshots for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Natural Language Understanding | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Netezza Performance Server |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Network ACL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| OpenPages | Hosted Globally | Hosted Globally | Hosted Globally | 
| Partner with Technology Expert Labs | Hosted Globally | Hosted Globally | Hosted Globally | 
| Placement Groups for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Planning Analytics | Hosted Globally | Hosted Globally | Hosted Globally | 
| Portfolio Optimization |  |  |  | 
| Predictive Market Scenarios |  |  |  | 
| Public Gateway | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Qiskit Runtime |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Real-Time Payments |  |  |  | 
| Red Hat OpenShift on IBM Cloud | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| SSH Key for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Satellite | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Satellite Infrastructure Service |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Schematics | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Secrets Manager | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secure Gateway | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Security Group for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Security and Compliance Center |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Security and Compliance Center Workload Protection | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Simulated Historical Instrument Analytics |  |  |  | 
| Simulated Instrument Analytics |  |  |  | 
| Software Instance | Hosted Globally | Hosted Globally | Hosted Globally | 
| Speech to Text | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Streaming Analytics | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Subnet | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Text to Speech | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Toolchain | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Transit Gateway | Hosted Globally | Hosted Globally | Hosted Globally | 
| VCF as a Service - Cloud Director Site |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| VCF as a Service - Virtual Data Center |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| VMware Solutions | Hosted Globally | Hosted Globally | Hosted Globally | 
| VPN for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Network Interface for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Private Cloud | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Private Endpoint for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Server for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Watson Discovery | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Watson Machine Learning | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Watson Query | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Watson Studio | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Workspace for Power Virtual Server |  |  |  | 
| watsonx Assistant | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| watsonx Orchestrate |  |  |  | 
| watsonx.data | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| watsonx.governance |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
{: row-headers}
{: caption="Table 2. Service availability - Europe" caption-side="top"}
{: #paas-table2}
{: tab-title="Europe"}
{: tab-group="PaaS"}
{: class="simple-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}

| Service | Sydney | Tokyo | Osaka | Chennai | 
|-----|-----|-----|-----|-----|
| API Connect | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Analytics Engine |  |  |  |  | 
| App Configuration | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| App ID | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Auto Scale for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Bare Metal Server for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Block Storage Snapshots for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Block Storage for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Client VPN for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Cloud Activity Tracker | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Logs |  |  |  |  | 
| Cloud Monitoring | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Cloud Object Storage | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Cloudant | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Code Engine | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Compose for Elasticsearch | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Compose for MongoDB | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Compose for PostgreSQL | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Compose for RabbitMQ | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Compose for Redis | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Compose for etcd | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Container Registry | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Continuous Delivery | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Cost and Asset Management |  |  |  |  | 
| DNS Services | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Data Engine (previously SQL Query) |  |  |  |  | 
| Data Product Hub |  |  |  |  | 
| Data Replication | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| DataStage |  |  |  |  | 
| Databases for EDB | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for Elasticsearch | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for MongoDB | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for MySQL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for PostgreSQL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for Redis | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Databases for etcd | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Db2 | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Db2 Warehouse | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Dedicated Host for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Direct Link Connect | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Direct Link Dedicated | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Event Notifications | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Event Streams | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| File Storage for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Floating IP for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Flow Logs for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Functions | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Historical Instrument Analytics |  |  |  |  | 
| Hyper Protect Crypto Services |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Hyper Protect Virtual Server for Classic |  |  |  |  | 
| IBM Cloud Backup for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| IBM Cloud Pak for Data | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| IBM Cloud Reservations for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| IBM Cognos Dashboard Embedded |  |  |  |  | 
| IBM Knowledge Catalog |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| IBM Match 360 with Watson |  |  |  |  | 
| IBM Verify |  |  |  |  | 
| IBM watsonx Code Assistant |  |  |  |  | 
| Image Service for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Instrument Analytics |  |  |  |  | 
| Internet Services | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Internet of Things Platform |  |  |  |  | 
| Investment Portfolio |  |  |  |  | 
| Key Protect | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Knowledge Studio | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Kubernetes Service | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Load Balancer for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Log Analysis | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| MQ |  |  |  |  | 
| Messages for RabbitMQ | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Multi Volume Snapshots for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Natural Language Understanding | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Netezza Performance Server |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Network ACL | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| OpenPages | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Partner with Technology Expert Labs | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Placement Groups for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Planning Analytics | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Portfolio Optimization |  |  |  |  | 
| Predictive Market Scenarios |  |  |  |  | 
| Public Gateway | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Qiskit Runtime |  |  |  |  | 
| Real-Time Payments |  |  |  |  | 
| Red Hat OpenShift on IBM Cloud | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSH Key for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Satellite | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Satellite Infrastructure Service |  |  |  |  | 
| Schematics |  |  |  |  | 
| Secrets Manager | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Secure Gateway | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Security Group for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Security and Compliance Center |  |  |  |  | 
| Security and Compliance Center Workload Protection | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Simulated Historical Instrument Analytics |  |  |  |  | 
| Simulated Instrument Analytics |  |  |  |  | 
| Software Instance | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| Speech to Text | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Streaming Analytics |  |  |  |  | 
| Subnet | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Text to Speech | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Toolchain | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Transit Gateway | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| VCF as a Service - Cloud Director Site |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| VCF as a Service - Virtual Data Center |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| VMware Solutions | Hosted Globally | Hosted Globally | Hosted Globally | Hosted Globally | 
| VPN for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Virtual Network Interface for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Virtual Private Cloud | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Virtual Private Endpoint for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Virtual Server for VPC | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Watson Discovery | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Watson Machine Learning |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Watson Query |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Watson Studio |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Workspace for Power Virtual Server |  |  |  |  | 
| watsonx Assistant | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| watsonx Orchestrate |  |  |  |  | 
| watsonx.data | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| watsonx.governance |  |  |  |  | 
{: row-headers}
{: caption="Table 3. Service availability - Asia Pacific" caption-side="top"}
{: #paas-table3}
{: tab-title="Asia Pacific"}
{: tab-group="PaaS"}
{: class="simple-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}



## Classic infrastructure
{: #iaas-service-infra}

Classic infrastructure services are available to be deployed in data centers.

### Americas
{: #iaas-service-americas}

The following classic infrastructure resources are available in North and South America. Match the resource row with the column representing the data center. A check mark indicates the resource is available in that location.

| Service | Montreal 01 | Toronto 01 | 
|---|---|---|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  |  | 
| Direct Link Dedicated Hosting on Classic |  | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps |  | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 4. Americas infrastructure availability - Canada" caption-side="top"}
{: #iaas-table1}
{: tab-title="Canada"}
{: tab-group="Americas"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


| Service | Dallas 09 | Dallas 10 | Dallas 12 | Dallas 13 | Dallas 14 | San Jose 03 | San Jose 04 | Washington DC (MZR) | Washington DC 04 | Washington DC 06 | Washington DC 07 | 
|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  |  |  |  |  |  |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| Direct Link Dedicated Hosting on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  |  |  |  |  |  |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 5. Americas infrastructure availability - United States" caption-side="top"}
{: #iaas-table2}
{: tab-title="United States"}
{: tab-group="Americas"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


| Service | Sao Paulo 01 | 
|--|--|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM |  | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  | 
| Direct Link Dedicated Hosting on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps |  | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 6. Americas infrastructure availability - Brazil" caption-side="top"}
{: #iaas-table3}
{: tab-title="Brazil"}
{: tab-group="Americas"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


### Europe
{: #iaas-service-europe}

The following classic infrastructure resources are available in Europe. Match the resource row with the column representing the data center. A check mark indicates the resource is available in that location.

| Service | Amsterdam 03 | 
|--|--|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  | 
| Direct Link Dedicated Hosting on Classic |  | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 7. Europe infrastructure availability - Netherlands" caption-side="top"}
{: #iaas-table4}
{: tab-title="Netherlands"}
{: tab-group="Europe"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


| Service | Frankfurt 02 | Frankfurt 04 | Frankfurt 05 | 
|----|----|----|----|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  |  |  | 
| Direct Link Dedicated Hosting on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  |  |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 8. Europe infrastructure availability - Germany" caption-side="top"}
{: #iaas-table5}
{: tab-title="Germany"}
{: tab-group="Europe"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


| Service | London 02 | London 04 | London 05 | London 06 | 
|-----|-----|-----|-----|-----|
| Bare Metal Server for Classic |  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  |  |  |  | 
| Direct Link Dedicated Hosting on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  |  |  |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 9. Europe infrastructure availability - United Kingdom" caption-side="top"}
{: #iaas-table6}
{: tab-title="United Kingdom"}
{: tab-group="Europe"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


| Service | Milan 01 | 
|--|--|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM |  | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  | 
| Direct Link Dedicated Hosting on Classic |  | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps |  | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 10. Europe infrastructure availability - Italy" caption-side="top"}
{: #iaas-table7}
{: tab-title="Italy"}
{: tab-group="Europe"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


| Service | Paris 01 | 
|--|--|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  | 
| Direct Link Dedicated Hosting on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 11. Europe infrastructure availability - France" caption-side="top"}
{: #iaas-table8}
{: tab-title="France"}
{: tab-group="Europe"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


| Service | Madrid 02 | Madrid 04 | Madrid 05 | 
|----|----|----|----|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX |  |  |  | 
| Cloud HSM |  |  |  | 
| Cloud Load Balancer |  |  |  | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Direct Link Connect on Classic |  |  |  | 
| Direct Link Dedicated Hosting on Classic |  |  |  | 
| Direct Link Dedicated on Classic |  |  |  | 
| Direct Link Exchange on Classic |  |  |  | 
| File Storage for Classic |  |  |  | 
| FortiGate Security Appliance 10Gbps |  |  |  | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall |  |  |  | 
| IBM Cloud Backup for Classic |  |  |  | 
| IPSec VPN |  |  |  | 
| Juniper vSRX |  |  |  | 
| SSL Certificates |  |  |  | 
| Secondary Subnets |  |  |  | 
| Virtual Router Appliance |  |  |  | 
{: row-headers}
{: caption="Table 12. Europe infrastructure availability - Spain" caption-side="top"}
{: #iaas-table9}
{: tab-title="Spain"}
{: tab-group="Europe"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


### Asia Pacific
{: #iaas-service-asia}

The following classic infrastructure resources are available in Asia and the Pacific region. Match the resource row with the column representing the data center. A check mark indicates the resource is available in that location.

| Service | Sydney 01 | Sydney 04 | Sydney 05 | 
|----|----|----|----|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  |  |  | 
| Direct Link Dedicated Hosting on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  |  |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 13. Asia Pacific infrastructure availability - Australia" caption-side="top"}
{: #iaas-table10}
{: tab-title="Australia"}
{: tab-group="Asia Pacific"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


| Service | Chennai 01 | 
|--|--|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Dedicated Hosting on Classic |  | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps |  | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 14. Asia Pacific infrastructure availability - India" caption-side="top"}
{: #iaas-table11}
{: tab-title="India"}
{: tab-group="Asia Pacific"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


| Service | Tokyo 02 | Tokyo 04 | Tokyo 05 | 
|----|----|----|----|
| Bare Metal Server for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | 
| Direct Link Dedicated Hosting on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  |  |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 15. Asia Pacific infrastructure availability - Japan" caption-side="top"}
{: #iaas-table12}
{: tab-title="Japan"}
{: tab-group="Asia Pacific"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


| Service | Singapore 01 | 
|--|--|
| Bare Metal Server for Classic |  | 
| Citrix NetScaler VPX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Cloud HSM |  | 
| Cloud Load Balancer | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Content Delivery Network | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Connect on Classic |  | 
| Direct Link Dedicated Hosting on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Dedicated on Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Direct Link Exchange on Classic |  | 
| File Storage for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| FortiGate Security Appliance 10Gbps | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Gateway Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Hardware Firewall | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IBM Cloud Backup for Classic | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| IPSec VPN | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Juniper vSRX | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| SSL Certificates | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Secondary Subnets | ![Checkmark icon](../icons/checkmark-icon.svg) | 
| Virtual Router Appliance | ![Checkmark icon](../icons/checkmark-icon.svg) | 
{: row-headers}
{: caption="Table 16. Asia Pacific infrastructure availability - Singapore" caption-side="top"}
{: #iaas-table13}
{: tab-title="Singapore"}
{: tab-group="Asia Pacific"}
{: class="comparison-tab-table"}
{: summary="Use the buttons for the countries to change the context of the table. This table has row and column headers. The row headers identify the service. The column headers identify where that service is located. To understand where a service is located in the table, navigate to the row, and find the for the location you are interested in."}


