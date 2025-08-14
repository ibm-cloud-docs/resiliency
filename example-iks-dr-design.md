---

copyright:
  years: 2021, 2024
lastupdated: "2025-08-14"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation, kubernetes, IKS

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Basic DR design for {{site.data.keyword.cloud_notm}} IBM Kubernetes Service
{: #basic-iks-dr-design}

Here are two designs that can be used as a cross-region disaster recovery solution for an {{site.data.keyword.cloud_notm}} Kubernetes Service (IBM Kubernetes Service) workload based on VPC infrastructure. You can use these designs as a starting point for protecting your IBM Kubernetes Service cluster against a disaster. Modify and test the design to meet your specific disaster recovery goals.

## Option One - Active / Standby Clusters
{: #iks-dr-option-one}
Option One involves creating a second cluster in a DR region of choice, which operates as a passive standby. The key considerations are cluster scaling and data recovery in the DR region. You also need to consider how users connect to workloads that are deployed on IBM Kubernetes Service and how ad where persistent data is stored.

### How it works
{: #iks-dr-how-it-works-option-one}

This option works optimally when Infrastructure as Code (IaC) is used to build and maintain the cluster. Autoscaling is a key component too. The entire build must be scripted, or at least precisely documented, in such a way that the build can be replicated at a second region. Ideally, autoscaling is employed to provide a minimal cluster configuration that scales to an optimal configuration under load.

The standby cluster is then built in a second region, by using the scripting or carefully documented configuration of the primary. It too has a minimal configuration but scales later when load is placed upon it.

Strong configuration management must be maintained in both instances. Care must be taken that updates to the active cluster configuration are replayed into the standby cluster. Configuration drift is a risk.

After the clusters are ready, workloads can be deployed. Use continuous delivery toolchains or similar to help ensure that applications and updates are deployed consistently to both the active and standby clusters.

Each cluster has a unique cluster ID and the workloads that are deployed on them have unique network connection properties.

### Storage Considerations
{: #iks-dr-storage-considerations-option-one}

If your cluster requires persistent storage, you need to consider options that support operation and recovery across regions.

* Cloud Object Storage - Cloud Object Storage buckets can be accessed from any region. Optimally, use cross-region buckets, since an outage can affect both IBM Kubernetes Service and Cloud Object Storage. Attach the storage to both clusters simultaneously by using the appropriate Cloud Object Storage end point
* Portworx - Portworx provides a highly resilient Software Defined Storage (SDS) solution for IBM Kubernetes Service. Portworx has two configuration options. Optimally, use Option One - asynchronous DR for Portworx. Option Two involves the recovery of your Portworx installation and data at the DR site, if the disaster also impacts your Portworx instance. For more information, see the relevant sections under [About Portworx](/docs/containers?topic=containers-storage_portworx_about)
* IBM Cloud Databases (ICD) - Applications can access and store data in ICD instances. While clusters can access ICD instances that are located in other regions, consider whether network latency becomes an issue, especially in the medium term. Overall, decide whether running your cluster in a different region after a recovery means that you need to recover any databases into the same region. Incorporate that decision into your plan.

### How to enact a recovery
{: #icd-enact-recovery-option-one}

Following a disaster, connections to deployed workloads need to be redirected to the standby cluster by using DNS. When the standby cluster detects connections, the autoscaling properties of the cluster activate, scaling to an optimal configuration for the load placed upon it.

### Failback
{: #iks-failback-option-one}

After the cause of the outage is resolved and the cluster is recovered in the primary region, then connections can be redirected back to the primary cluster. The standby cluster scales back as workload connections reduce back to zero.

## Option Two - Active / Active deployment
{: #iks-dr-option-two}
An alternative option, which is in effect a high availability option, is to run two clusters, in two different regions, in an active-active deployment. Option two provides the lowest possible RTO / RPO, though can be complex to set up and manage.

### How it works
{: #iks-dr-how-it-works-option-two}

Using this option, you deploy a second IBM Kubernetes Service cluster into the second region of your choice. The two clusters must be identical in terms of build and configuration. Users access the workloads on the clusters through a Global Load Balancer, which is provisioned with IBM Cloud Internet Services (CIS). From the outset, both clusters are serving user requests. Since load is spread across the two clusters, each cluster is configured to bear half the user load under normal circumstances. However, autoscaling must be configured to allow the clusters to grow to handle full capacity to cope with an outage on the other.

Care must be taken to help ensure that both clusters maintain the same configuration and workload versions (best configured through continuous delivery practices).

### How to enact a recovery
{: #kp-enact-recovery-option-two}

No customer recovery is required with this option. If one of the clusters stops responding to health-checks initiated by the global load balancer, then all connections requests are sent to the remaining cluster.


### Failback
{: #iks-failback-option-two}

After the cause of the outage is resolved and the cluster is recovered, the global load balancer will detect it through successful health checks. If the cluster's network configuration changes as a result of recovery, then the global load balancer needs to be reconfigured too. The global load balancer then starts to send requests to both clusters, as before the outage. Autoscaling will rebalance the sizes of the clusters.

## Further reading
{: #iks-dr-further-reading}

For additional information on DR for IBM Kubernetes Service, refer to the following documentation pages:

* [Creating a highly available cluster strategy](/docs/containers?topic=containers-strategy)
* [Documenting your environment architecture](/docs/containers?topic=containers-document-environment)
* [Continuous integration and delivery for app development and deployment](docs/containers?topic=containers-cicd&interface=terraform)
* [Backing up and restoring storage data](docs/containers?topic=containers-storage_br&interface=terraform)
* [Understanding high availability and disaster recovery for IBM Cloud Kubernetes Service](/docs/containers?topic=containers-iks-ha-dr&interface=terraform)
