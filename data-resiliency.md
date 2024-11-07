---

copyright:
  years: 2023, 2024
lastupdated: "2024-11-07"

keywords:

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Data resiliency
{: #data-resiliency}
Data resiliency refers to the ability to access, maintain or quickly recover data in the event of failures or disasters. It is related to the previous discussed concepts of High Availability, Disaster Recovery and Cyber Resiliency, and the previous planning sessions address techniques to be considered to achieve data resiliency. {: shortdesc} For more general info on Data Resiliency, see [IBM Well-Architected Framework](https://www.ibm.com/architectures/well-architected/resiliency#Practices){: external}.

Another aspect that data resiliency needs to consider is data residency, that is, where the data will be located (both for production, backup or recovery) and any restrictions or requirements on its placement. The next section explains IBM Cloud capabilities and provides considerations on this topic.

# Understanding data residency in {{site.data.keyword.cloud_notm}}

{{site.data.keyword.cloud_notm}}'s global network of locations provides you with the flexibility of choosing where you want to run your workloads.

For regional and zonal services, you request instances of the service to be deployed in a specific region in accordance with your specific geographic requirements. {{site.data.keyword.cloud_notm}} ensures that content that is provided by you and your workload (as defined in the [{{site.data.keyword.cloud_notm}} Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304&cc=us&lc=en){: external}) is stored and processes locally in the selected region location. For a complete list of the locations where {{site.data.keyword.cloud_notm}} services are available see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

{{site.data.keyword.cloud_notm}} also maintains internal encrypted backups of the customer content within the same geography where the regional or zonal service is located for recovery in case of data corruption or a major data center disaster.

About your business contact and account usage information (as defined in the [{{site.data.keyword.cloud_notm}} Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304&cc=us&lc=en){: external}), also called client’s metadata, {{site.data.keyword.cloud_notm}} stores and processes them where the control planes of the regional and global services are located.

- Regional services typically have control planes that are located in the same region where you selected for the service except for the services indicated in [Services with global control planes](/docs/overview?topic=overview-zero-downtime#service-global-control-plane).
- Global services control planes locations are indicated in [Global platform services](/docs/overview?topic=overview-zero-downtime#global-platform).

For a complete list of data attributes that are stored and processed by each single {{site.data.keyword.cloud_notm}} service you can refer to the documentation of the API of each service in the [API and SDK reference library](https://cloud.ibm.com/docs?tab=api-docs).

All data in transit is encrypted. Only TLS 1.2 and 1.3 are supported in {{site.data.keyword.cloud_notm}} with TLS 1.1 and below explicitly disabled to prevent downgrading to a vulnerable version of the protocol.

{{site.data.keyword.cloud_notm}} data privacy processing processes and procedures are documented within the {{site.data.keyword.cloud_notm}} DPA. This Data Processing Addendum (DPA) and its applicable DPA Exhibits apply to the Processing of Personal Data by {{site.data.keyword.cloud_notm}} on behalf of Client (Client Personal Data). The processing of Personal Data is subject to the General Data Protection Regulation 2016/679 (GDPR). It is also subject to any other data protection laws that are identified at [Data Protection Laws](https://www.ibm.com/support/customer/csol/terms/?id=DPA-DPL&lc=en){: external} in order to provide services (Services) according to the Agreement between Client and {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.cloud_notm}} DPA can be found at [Data Processing Addendum](https://www.ibm.com/support/customer/csol/terms/?id=Z126-7870&lc=en){: external}.

In addition to the DPA, the cloud services provide DPA exhibits that can be found on the [{{site.data.keyword.cloud_notm}} Terms site](https://www.ibm.com/support/customer/csol/terms/){: external}.
