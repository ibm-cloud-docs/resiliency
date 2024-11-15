---

copyright:
  years: 2024, 2024
lastupdated: "2024-11-15"

keywords: load balancing, global load balancing, HA, DR, high availability, disaster recovery, HA for the platform, high availability for platform, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: design

---

{{site.data.keyword.attribute-definition-list}}

# Reference architectures for resiliency in {{site.data.keyword.cloud_notm}}
{: #resiliency-reference-arch}

Reference architecture patterns are industry-standard solutions that are reusable and for solving common and recurring problems across different business domains. Resiliency patterns on {{site.data.keyword.cloud_notm}} offer standard recovery solutions from adverse conditions while performing the operations and maintenance of the workloads across the zones, regions and globally. The table provides you with standard resiliency related reference architecture based on the workloads and are grouped based on concepts of high availability and disaster recovery.

| Concept | Pattern |
| ------- | ------- |
| High availability| * [VSI on VPC landing zone](/docs/deployable-reference-architectures?topic=deployable-reference-architectures-vsi-ra) \n * [Red Hat OpenShift Container Platform on VPC landing zone](/docs/deployable-reference-architectures?topic=deployable-reference-architectures-ocp-ra)|
| Disaster recovery | * [Cyber resiliency pattern on VPC](/docs/pattern-cyber-resiliency-vpc?topic=pattern-cyber-resiliency-vpc-cyber-resiliency) \n * [Oracle Database Disaster Recovery on IBM PowerVS](/docs/pattern-oracle-disaster-recovery-on-powervs?topic=pattern-oracle-disaster-recovery-on-powervs-oracle-database-disaster-recovery-on-ibm-powervs-cross-region) |
| High availability and disaster recovery | * [Web app cross-region resiliency](/docs/pattern-vpc-vsi-cross-region-resiliency?topic=pattern-vpc-vsi-cross-region-resiliency-web-app-cross-region) \n * [IBM Power Virtual Server resiliency on AIX](/docs/pattern-pvs-aix-resiliency?topic=pattern-pvs-aix-resiliency-power-virtual-server-on-AIX) \n * [IBM Cloud resiliency for Veeam on VMware](/docs/vmware-cross-region-dr?topic=vmware-cross-region-dr-deploy-veeam-arch) |
{: caption="Resiliency related reference architectures" caption-side="bottom"}
