---

copyright:
  years: 2021, 2024
lastupdated: "2024-11-13"


keywords: cyber resiliency plan, resilient app, cyber recovery

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Designing for cyber resiliency
{: #cyber-resiliency-plan}

A cyber-resiliency strategy uses a risk-based approach to align organizational objectives with achievable goals. It begins by identifying critical assets and their dependencies, for instance, a Minimum Viable Company (MVC) concept defines a functional level of service which can continue to serve customers during disruptions. Not all applications will require the same level of protection, so it’s essential to balance the risk factors with the additional cost and complexity involved. For workloads deployed on {{site.data.keyword.cloud_notm}}, this inventory includes application images, configurations, data, and the dependent cloud services and infrastructure required to operate them at the necessary scale. 

Before we delve deeper, we need to clarify the scope of the attack and recovery which is inherent to a cloud. For customer workloads deployed on {{site.data.keyword.cloud_notm}}, there is a clear boundary between customer account and resources provisioned within and the common {{site.data.keyword.cloud_notm}} fabric and underlying resources that support all customer resources. This boundary is enforced by strict multi-layered security measures such as separation of concern, access control, network isolation etc. In case of a cyber incident affecting {{site.data.keyword.cloud_notm}} fabric itself, {{site.data.keyword.cloud_notm}} will excercise its cyber recovery plan and restore its services first before notifying and working with affected customers to perform recovery of their resources. This document provides guidance for the case wherein a cyber incident affects only the customer account and resources provisioned within while the underlying {{site.data.keyword.cloud_notm}} fabric is unaffected. You should familiarise yourself with the [shared responsibilities](/docs/overview?topic=overview-shared-responsibilities) between {{site.data.keyword.cloud_notm}} and customers for running workloads on {{site.data.keyword.cloud_notm}}. 

## Data Consistency
{: data-consistency}

An important consideration to address as you identify critical applications is also how the data backup and recovery needs to be performed. The type of data backup you take will affect whether further steps are needed post restoration. A consistent backup of live data typically means that a copy of the data can be captured without the need for stopping the application writing to it. Modern systems offer different types of backups including crash-consistent and application-consistent backup

* Crash-consistent - 
* Application-consistent -

## Architectural Concepts
{: architectural-concepts}

![Diagram depicting cyber resiliency architectural components](images/cyber-resiliency-arch.png "Diagram depicting cyber resiliency architectural components"){: caption="Diagram depicting cyber resiliency architectural components" caption-side="bottom"}

The critical workloads running in the production environment should be securely backed up into a cyber vault. The backup copies in the cyber vault then becomes a trusted source from which recovery can be orchestrated later. Some of the main architectural aspects of a effective cyber resiliency solution are: 

* Isolation - Multiple environments, each setup for a specific purpose and isolated both in terms of administrative boundaries and network boundaries ensures that there is clear separation of concern and any malicious actor or code does not laterally gain access.</ul>
* Virtual Air-Gap - Air-gapping ensures that no malicious actors or code can gain access to criticial systems like the cyber vault and the backup data within it. A virtual air-gap is typically enforced using network rules which allow only trusted traffic to go through and deny all others. In addition, the air-gap may be closed or opened only at scheduled periods. It can be implemented using an appliance or built-in capabilities like the {{site.data.keyword.cloud_notm}} VPC network access control lists and security groups</ul>
* Immutable Storage - Backups written to immutable storage and retained for a configurable period of time, provide a technical assurance that even with elevated privileges, a bad actor cannot cause [data corruption](){: term}. For instance, {{site.data.keyword.cloud_notm}} cloud object storage immutable storage feature preserves records and maintains data integrity in a write-once-read-many (WORM), non-erasable and non-rewritable manner until end of retention period and the removal of any legal holds.</ul>
* Cleanroom - Data backups are only useful if you can recover to a stable state from them. A recovery point could be a collection of individual backups of different components which when restored can bring an application to consistent state. A cleanroom is an isolated environment where recovery points can be verified and even potentially cleaned off malware, either by restoring them or using forensic tools. Additionally, the ability to rapidly tear down and spin up a new cleanroom environment is essential for having a clean state infrastructure everytime to start testing from and also for automating the whole process.</ul>

The [cyber resiliency pattern on VPC](/docs/pattern-cyber-resiliency-vpc?topic=pattern-cyber-resiliency-vpc-cyber-resiliency) provides an approach to build a cyber resiliency solution on top of a secure VPC environment.


## Roles and Responsibilities
{: roles-and-responsibilities}

Organizations often create a separate cyber response team, with representatives from every business department who are responsible for co-ordinating recovery. In cases where the disruption is extensive, the team has to rely on separation of tools, processes and environments to initiate recovery from. Often, finding the right owners
