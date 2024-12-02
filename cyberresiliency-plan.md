---

copyright:
  years: 2021, 2024
lastupdated: "2024-12-02"


keywords: cyber resiliency plan, resilient app, cyber recovery

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Designing for cyber resiliency
{: #cyber-resiliency-plan}

A cyber resiliency strategy uses a risk-based approach to align organizational objectives with achievable goals. It begins by identifying critical assets and their dependencies. For instance, a Minimum Viable Company (MVC) concept defines the functional level of resiliency that is required in order to continue to serve their customers during disruptions. Not all applications require the same level of protection, so it’s essential to balance the risk factors with the additional cost and complexity involved. For workloads that are deployed on {{site.data.keyword.cloud_notm}}, inventory includes application images, configurations, data, and the dependent cloud services and infrastructure that are required to operate at the necessary scale.
{: shortdesc}

Before we delve deeper, we need to clarify the scope of the attack and recovery that is inherent to a cloud environment. For workloads that are deployed on {{site.data.keyword.cloud_notm}}, there is a clear boundary between a client account and the resources that are provisioned within and the common {{site.data.keyword.cloud_notm}} fabric and underlying resources that support all of our clients. This boundary is enforced by strict multilayered security measures such as separation of concern, access control, and network isolation etc. In the event of a cyber incident that affects the {{site.data.keyword.cloud_notm}} fabric itself, {{site.data.keyword.cloud_notm}} will excercise its cyber recovery plan and restore its services first before notifying and working with affected clients to perform recovery of their resources.

This document provides guidance for situations where a cyber incident affects only the client account and resources provisioned within while the underlying {{site.data.keyword.cloud_notm}} fabric is unaffected. You should familiarize yourself with the [shared responsibilities](/docs/overview?topic=overview-shared-responsibilities) between {{site.data.keyword.cloud_notm}} and clients for running workloads on {{site.data.keyword.cloud_notm}}.

## Data consistency
{: #data-consistency}

An important consideration to address as you identify critical applications is how their data backup and recovery needs to be performed. The type of data backup you take will influences whether further steps are needed post restoration. A consistent backup of live data typically means that a copy of the data can be captured without the need for stopping the application writing to it. Modern systems offer different types of backups that include the following examples.

Crash-consistent
:   Records all the data written to persistent store but not those in memory or pending I/O. Such types of backups are fast as no special application quiescing or memory flush operation is needed, but applications should be able to recover from a crash-consistent backup. For instance, many databases use a special log-based recovery approach to recover the database to a consistent state. A copy of transaction logs and data volumes is sufficient to bring the database back to consistent state.

Application-consistent
:   An application aware backup, quiesces the application stopping it from processing any requests and flushes any dirty data in memory and pending I/O queues to persistent store before capturing the backup. This is typically done through hooks into the application or as pre and post scripts that are run on servers. During recovery, since the backup is already consistent, the application does not need to do any additional processing, which can make recovery faster and more reliable.

## Architectural concepts
{: #architectural-concepts}

![Diagram depicting cyber resiliency architectural components](images/cyber-resiliency-arch.svg "Diagram depicting cyber resiliency architectural components"){: caption="Diagram depicting cyber resiliency architectural components" caption-side="bottom"}

Critical workloads that run in a production environment should be securely backed up in a cyber vault. The backup copies in the vault then become a trusted source from which recovery can be orchestrated if needed. Some of the main architectural aspects of an effective cyber resiliency solution are as follows.

Isolation
:   Multiple environments, each set up for a specific purpose and isolated both in terms of network and administrative boundaries, ensure that there is clear separation of concern and any malicious actor or code does not laterally gain access to systems. {{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) enables highly secure private cloud environments powered by a robust software defined network (SDN), built-in security, and regulatory compliance standards.

Virtual air-gap
:   Air-gapping ensures that no malicious actors or code can gain access to criticial systems like the cyber vault and the backup data within it. A virtual air-gap is typically enforced using network rules which allow only trusted traffic to go through and deny all others. In addition, the air-gap may be opened or closed only at scheduled periods. It can be implemented by using a firewall appliance or by leveraging built-in capabilities like the {{site.data.keyword.cloud_notm}} VPC network access control lists and security groups.

Immutable storage
:   Backups written to immutable storage and retained for a configurable period of time, provide a technical assurance that even with elevated privileges, a bad actor cannot cause data corruption. For instance, {{site.data.keyword.cloud_notm}} cloud object storage's [immutable storage](/docs/cloud-object-storage?topic=cloud-object-storage-immutable) feature preserves records and maintains data integrity in a write-once-read-many (WORM), non-erasable and non-rewritable manner until the end of retention period and the removal of any legal holds.

Cleanroom
:   Data backups are only useful if you can recover to a stable state from them. A recovery point could be a collection of individual backups of different components which when restored can bring an application to a consistent state. A cleanroom is an isolated environment where recovery points can be verified and even potentially cleaned off malware, either by restoring them or using forensic tools. Cleanroom environments can be torn down after testing and recreated as a clean infrastructure state to test upon.

Recovery environment
:   In the event of a cyber incident when production services are down, the recovery environment is bulit to host the criticial applications restored to a valid recovery point and takeover the job of production. The recovery environment could be in the same region as old production or in a different region. Over time, a recovery environment could be promoted as new production or could cease to exist if another environment takesover as production.

The [cyber resiliency pattern on VPC](/docs/pattern-cyber-resiliency-vpc?topic=pattern-cyber-resiliency-vpc-cyber-resiliency) provides an approach to build a cyber resiliency solution on top of a secure VPC environment.

## Automation and orchestration
{: #orchestration}

Rapid service restoration requires a well-orchestrated, end-to-end recovery process. Automating distinct steps such as provisioning secure environments, periodically testing backups for new vulnerabilities, and restoring data into a recovery environment as workflows can streamline this process. Routinely testing orchestration tools and processes strengthens confidence in an organization's preparedness to respond effectively to incidents.

{{site.data.keyword.cloud_notm}} [Deployable Architectures](/docs/secure-enterprise?topic=secure-enterprise-understand-module-da) provides cloud automation for deploying pre-created architectural patterns and is designed to be repeatable, scalable and modular. [Projects](/docs/secure-enterprise?topic=secure-enterprise-config-project) can help organize and manage the deployment configurations by ensuring commit checks, vulnerability scans and cost estimation. After your infrastructure is deployed, your [DevSecOps toolchains](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-overview) can be configured to deploy your applications using continuous deployment pipelines.
