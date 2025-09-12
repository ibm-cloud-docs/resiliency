---

copyright:
  years: 2021, 2025
lastupdated: "2025-09-12"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation, kubernetes, IKS

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Designing DR for {{site.data.keyword.cloud_notm}} Kubernetes Service
{: #basic-iks-dr-design}

Protecting your {{site.data.keyword.cloud_notm}} Kubernetes Service workload via a disaster recovery solution can be achieved in any number of ways. However, to help you create a plan, consider starting with one of the following design options to protect your clusters against a potential disaster. Then, you can modify and test the options to meet your specific disaster recovery goals.


## Option 1: Standby clusters
{: #iks-dr-option-one}

In the event of a disaster, you could recover your workload from a standby cluster. To do so, you must create a cluster in a DR region which operates as a cold standby. Be sure to take into consideration scalability and data storagd in that region as well as how users connect to workloads that are deployed on the service.

Standby clusters work best when Infrastructure as Code (IaC) is used to build and maintain the cluster. Autoscaling is a key component too. The entire build must be scripted, or at least precisely documented, in such a way that the build can be replicated in another region. Then the cluster is built either from the script or the carefully documented configuration of the primary cluster in a secondary region. At first, it has a minimal configuration but scales when load increases.

In either option, scripting or documentation, careful configuration management must be maintained in both primary and standby cluster as drift between them is a risk. It's recommended that you use continuous delivery toolchains or similar to help ensure that applicaitons and updates are deployed consistently to both the active and standby clusters. Each cluster has a unique ID and the workloads that are deployed on them have unique network connection properties.

Depending on your RTO, consider whether you need to maintain a permanent cold standby. Using IaC, it may be possible to build a new cluster on-demand and still meet your RTO.
{: note}

### Understanding storage considerations
{: #iks-dr-storage-considerations-option-one}

If your cluster requires persistent storage, you need to consider options that support operation and recovery across regions.

* Cloud Object Storage - Cloud Object Storage buckets can be accessed from any region. Optimally, use cross-region buckets, since an outage can affect both IBM Kubernetes Service and Cloud Object Storage. Attach the storage to both clusters simultaneously by using the appropriate Cloud Object Storage end point.

* Portworx - Portworx provides a highly resilient Software Defined Storage (SDS) solution for IBM Kubernetes Service. Portworx has two configuration options. Optimally, use Option One - asynchronous DR for Portworx. Option Two involves the recovery of your Portworx installation and data at the DR site, if the disaster also impacts your Portworx instance. For more information, see the relevant sections under [About Portworx](/docs/containers?topic=containers-storage_portworx_about).

* IBM Cloud Databases (ICD) - Applications can access and store data in ICD instances. While clusters can access ICD instances that are located in other regions, consider whether network latency becomes an issue, especially in the medium term. Overall, decide whether running your cluster in a different region after a recovery means that you need to recover any databases into the same region. Incorporate that decision into your plan.

### Enacting a recovery
{: #icd-enact-recovery-option-one}

Following a disaster, connections to deployed workloads need to be redirected to the standby cluster by using DNS. When the standby cluster detects connections, the autoscaling properties of the cluster activate, scaling to an optimal configuration for the load placed upon it.

### Failing back
{: #iks-failback-option-one}

After the cause of the outage is resolved and the cluster is recovered in the primary region, then connections can be redirected back to the primary cluster. The standby cluster scales back as workload connections reduce back to zero.

## Option 2: Highly available clusters
{: #iks-dr-option-two}

An alternative to a standby cluster is to simultaniously run two clusters in different regions, which is in effect a high availability option. While this option provides the lowest possible RTO / RPO, it does increase operational costs.

For this option to work, the second cluster must be identitcal to the primary cluster in terms of build and configuration because users access your workload through a global load balancer, which is provisioned with IBM Cloud Internet Services (CIS). From the outset, both clusters are serving user requests. Since load is sprad across both clusters, each is configured to bear half of the user load in normal circumstances. However, autoscaling must be configured to allow the clusters to handle the full capacity of user requests in the event that one of the clusteres suffers an outage. It is vital that both clusters maintain the same configuration and workload versions which is possible to achieve through continuous delivery practices.

As an alternative deployment model, you can continue to have one cluster handle all user requests while having the second cluster ready and waiting in the event of an outage. If a failure is detected in the active cluster, connections can automatically be directed to the secondary cluster.
{: note}


### Enacting a recovery
{: #kp-enact-recovery-option-two}

No customer recovery is required with this option. If one of the clusters stops responding to health-checks initiated by the global load balancer, then all connections requests are sent to the remaining cluster.


### Failing back
{: #iks-failback-option-two}

After the cause of the outage is resolved and the cluster is recovered, the global load balancer will detect it through successful health checks. If the cluster's network configuration changes as a result of recovery, then the global load balancer needs to be reconfigured too. The global load balancer then starts to send requests to both clusters, as before the outage. Autoscaling will rebalance the sizes of the clusters.

## Other considerations
{: #iks-dr-other-considerations}

It is a recommended best practice to maintain container images in IBM Cloud Container Registry. Container Registry is a highly available, regional, service. Data that is written to Container Registry is replicated over the availability zones in the region where your instance resides. To prevent loss of access to your images in the event of a regional failure, you can choose to push your images to multiple registry regions. This is a key consideration in your overall IKS DR strategy.

Also, coinsider using Continuous Integration / Continuous Deployment (CI-CD) pipelines to deploy your applications in an IKS environment. Test you CI/CD pipelines regularly to ensure they are operating correctly. Using CI/DC will automate the process of code integration and delivery for your clusters, which helps maintain standby clusters or quickly deploy workloads onto new ones. For standby clusters, consider adding a delay to automated deployment. This can prevent a problamatic release from being deployed to the standby too.

## Further reading
{: #iks-dr-further-reading}

For additional information on DR for {{site.data.keyword.cloud_notm}} Kubernetes Service, refer to the following documentation pages:

* [Creating a highly available cluster strategy](/docs/containers?topic=containers-strategy)
* [Documenting your environment architecture](/docs/containers?topic=containers-document-environment)
* [Continuous integration and delivery for app development and deployment](/docs/containers?topic=containers-cicd)
* [Backing up and restoring storage data](/docs/containers?topic=containers-storage_br)
* [Understanding high availability and disaster recovery for IBM Cloud Kubernetes Service](/docs/containers?topic=containers-iks-ha-dr&interface=terraform)
* [High availability for Container Registry](/docs/Registry?topic=Registry-ha-dr)
