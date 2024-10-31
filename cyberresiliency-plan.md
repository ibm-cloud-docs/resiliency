---

copyright:
  years: 2021, 2024
lastupdated: "2024-10-31"


keywords: cyber resiliency plan, resilient app, cyber recovery

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Designing for cyber resiliency
{: #cyber-resiliency-plan}

A cyber-resiliency strategy uses a risk-based approach to align organizational objectives with achievable goals. It begins by identifying critical assets and their dependencies, for instance, a Minimum Viable Company (MVC) concept defines a functional level of service which can continue to serve customers during disruptions. Not all applications will require the same level of protection, so it’s essential to balance the risk factors with the additional cost and complexity involved. For workloads deployed on {{site.data.keyword.cloud_notm}}, this inventory includes application images, configurations, data, and the dependent cloud services and infrastructure required to operate them at the necessary scale. 

Before we delve deeper, we need to clarify the scope of the attack and recovery which is specific to a cloud or hosted environment. For customer workloads deployed on {{site.data.keyword.cloud_notm}}, there is a clear boundary between customer account and resources provisioned within and the common {{site.data.keyword.cloud_notm}} fabric and underlying resources that support all customer resources. This boundary is enforced by strict multi-layered security measures such as separation of concern, access control, network isolation etc. In case of a cyber incident affecting {{site.data.keyword.cloud_notm}} fabric itself, {{site.data.keyword.cloud_notm}} will excercise its cyber recovery plan and restore its services first before notifying and working with affected customers to perform recovery of their resources. This document focuses on the case wherein a cyber incident occurs that affects the customer account and resources provisioned within only and the underlying {{site.data.keyword.cloud_notm}} fabric is unaffected and enables the customer to recover from such a scenario. You should familiarise yourself with the [shared responsibilities](https://cloud.ibm.com/docs/overview?topic=overview-shared-responsibilities) between {{site.data.keyword.cloud_notm}} and customers for running workloads on {{site.data.keyword.cloud_notm}}. 

Organizations often create a separate cyber response team, with representatives from every business department who are responsible for co-ordinating recovery. In cases where the disruption is extensive, the team has to rely on separation of tools, processes and environments to initiate recovery from.


## Securing the backups
