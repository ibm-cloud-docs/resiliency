---

copyright:

  years: 2021, 2024

lastupdated: "2025-03-26"

keywords: SLO, service level objectives

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cloud_notm}} service level objectives
{: #slo}

Service level objectives (SLOs) describe the design points that the {{site.data.keyword.cloud}} services are engineered to meet in the following areas:

* Availability
* Operations management
* Performance

The SLO is not a warranty and IBM will not issue credits for failure to meet an objective. Refer to the [Service Level Agreements (SLAs)](/docs/overview?topic=overview-slas) for commitments and credits that are issued for failure to meet any committed SLAs.

The following tables describe the service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} services in a high availability configuration. As you aggregate or deploy services into a single zone, your SLO might vary. For more information about how {{site.data.keyword.cloud_notm}} is striving to ensure the highest availability of services, see [How IBM Cloud ensures high availability and disaster recovery](/docs/overview?topic=overview-zero-downtime).

## Service level objectives for {{site.data.keyword.cloud_notm}} platform services and other vital services
{: #platform-slo}

These services include our control plane and self-service interfaces, IAM service, key management service, virtual machine compute instances, block and object storage services, virtual networks service, web or Layer 7 load-balancing service, and relational DBaaS.

### Platform services
{: #slo-platform-services}

The following table describes the service level objectives (SLOs) for the vital {{site.data.keyword.cloud_notm}} platform services in a high availability configuration.

| Platform service | Availability target | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.cloud_notm}} console | 99.999% | [High availability for the platform](/docs/overview?topic=overview-zero-downtime) |
| {{site.data.keyword.cloud_notm}} CLI | 99.999% | [Understanding high availability and disaster recovery for the IBM Cloud CLI](/docs/cli?topic=cli-ha-dr) |
| {{site.data.keyword.cloud_notm}} network | 99.9999% |[High availability for the network](/docs/overview?topic=overview-zero-downtime#network-backbone) |
| {{site.data.keyword.cloud_notm}} catalogs | 99.999% | [High availability for the platform](/docs/overview?topic=overview-zero-downtime) |
| Identity and Access Management (IAM) | 99.999% | [High availability for the platform](/docs/overview?topic=overview-zero-downtime) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} platform services" caption-side="bottom"}

### Compute services
{: #slo-compute-services}

The following table describes the service level objectives (SLOs) for the vital {{site.data.keyword.cloud_notm}} compute services in a high availability configuration.

| Compute service | Availability target | Performance target | High availability guidance |
|----------|---------|---------|---------|
| {{site.data.keyword.vpc_short}} | 99.999% |Network Performance > 90% in 99.9% of the time  \n NVMe Performance > 90% in 99.9% of the time | [Understanding high availability and disaster recovery](/docs/vpc?topic=vpc-ha-dr-vpc) |
| {{site.data.keyword.powerSys_notm}} | 99.999% |Network Performance > 90% in 99.9% of the time | [Understanding high availability and disaster recovery](/docs/power-iaas?topic=power-iaas-ha-dr) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} compute services" caption-side="bottom"}

### Networking services
{: #slo-network-services}

The following table describes the service level objectives (SLOs) for the vital {{site.data.keyword.cloud_notm}} networking services in a high availability configuration.

| Networking service | Availability target | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.vpc_short}} (Gen 2) | 99.999% | [Understanding high availability and disaster recovery](/docs/vpc?topic=vpc-ha-dr-vpc) |
| {{site.data.keyword.vpn_full}} | 99.999% | [Connecting to your on-premises network](/docs/vpc?topic=vpc-vpn-onprem-example) |
| {{site.data.keyword.alb_full}} | 99.999% | [High Availability mode](/docs/vpc?topic=vpc-nlb-vs-elb#nlb-ha-mode) |
| {{site.data.keyword.nlb_full}} | 99.999% | [High Availability mode](/docs/vpc?topic=vpc-nlb-vs-elb#nlb-ha-mode) |
| {{site.data.keyword.vpe_full}} | 99.999% | [Endpoints available](/docs/vpc?topic=vpc-service-endpoints-for-vpc) |
| {{site.data.keyword.cloud_notm}} service endpoints| 99.999% | [Secure access to services using service endpoints](/docs/account?topic=account-service-endpoints-overview) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} networking services" caption-side="bottom"}

### Storage services
{: #slo-storage-services}

The following table describes the service level objectives (SLOs) for the vital {{site.data.keyword.cloud_notm}} storage services in a high availability configuration.

| Storage service | Availability target | Performance target | Performance detail | Data durability target | High availability guidance |
|----------|---------|---------|---------|---------|---------|
| {{site.data.keyword.cos_full_notm}} | 99.999% |  |  | 99.99999999999999% | [Understanding high availability and disaster recovery for {{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-cos-ha-dr) |
| {{site.data.keyword.block_storage_is_short}} | 99.999% | Disk Performance > 90% in 99.9% of the time | [Block storage capacity and performance](/docs/vpc?topic=vpc-capacity-performance#iops-profiles) | 99.99999999999% | [About {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-about) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} storage services" caption-side="bottom"}

### Database services
{: #slo-database-services}

The following table describes the service level objectives (SLOs) for the vital {{site.data.keyword.cloud_notm}} database services in a high availability configuration.

| Databases service | Availability target | Performance detail | Storage back end used for data | Storage back end used for backups | High availability guidance |
|----------|---------|---------|---------|---------|---------|
| {{site.data.keyword.databases-for-postgresql_full_notm}} | 99.999% | [Performance](/docs/databases-for-postgresql?topic=databases-for-postgresql-performance) | Block Storage | Object Storage | [High-Availability](/docs/databases-for-postgresql?topic=databases-for-postgresql-high-availability) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} databases services" caption-side="bottom"}

### Security services
{: #slo-security-services}

The following table describes the service level objectives (SLOs) for the vital {{site.data.keyword.cloud_notm}} security services in a high availability configuration.

| Security service | Availability target | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.keymanagementservicefull_notm}} | 99.999% | [High availability and disaster recovery](/docs/key-protect?topic=key-protect-ha-dr) |
| {{site.data.keyword.hscrypto}} | 99.999% | [High availability and disaster recovery](/docs/hs-crypto?topic=hs-crypto-ha-dr) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} security services" caption-side="bottom"}


## Service level objectives for high impact {{site.data.keyword.cloud_notm}} services
{: #high-impact-slo}

High impact services are not vital, but have great influence over the rest of the environment. These services are generally called upon in most deployments and might be of interest as they impact performance or usability of the environment.

### Platform services
{: #slo--high-platform-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} platform services in a high availability configuration.

| Platform service | Availability target | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.cloud-shell_notm}} | 99.999% | [Understanding high availability and disaster recovery for Cloud Shell](/docs/cloud-shell?topic=cloud-shell-ha-dr) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} platform services" caption-side="bottom"}

### Compute services
{: #slo-high-compute-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} compute services in a high availability configuration.

| Compute service | Availability target | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.openwhisk_short}} | 99.999% | [Understanding high availability and disaster recovery for Cloud Functions](/docs/openwhisk?topic=openwhisk-ha_dr) |
| {{site.data.keyword.satellitelong_notm}} | 99.999% | [High availability and disaster recovery for Satellite](/docs/satellite?topic=satellite-ha) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} compute services" caption-side="bottom"}

### Containers services
{: #slo-high-container-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} containers services in a high availability configuration.

| Containers service | Availability target | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.registrylong_notm}} | 99.999% | [Understanding high availability and disaster recovery for Container Registry](/docs/Registry?topic=Registry-ha-dr) |
| {{site.data.keyword.containerlong_notm}} | 99.999% | [High availability and disaster recovery for IBM Cloud Kubernetes Service](/docs/containers?topic=containers-ha_clusters) |
| {{site.data.keyword.openshiftlong_notm}} | 99.999% | [Understanding high availability and disaster recovery for Red Hat OpenShift on IBM Cloud](/docs/openshift?topic=openshift-ha_clusters) |
| {{site.data.keyword.codeenginefull_notm}} | 99.999% | [High availability and disaster recovery for Code Engine](/docs/codeengine?topic=codeengine-ha-dr) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} container services" caption-side="bottom"}

### Networking services
{: #slo-high-network-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} networking services in a high availability configuration.

| Networking service | Availability target | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.dns_full_notm}} | 99.999% | [Understanding high availability and disaster recovery for DNS Services](/docs/dns-svcs?topic=dns-svcs-ha-dr) |
| {{site.data.keyword.tg_full_notm}} | 99.999% | [High availability and disaster recovery](/docs/transit-gateway?topic=transit-gateway-ha-dr) |
| {{site.data.keyword.cis_full_notm}} | 99.999% | [Use Virtual Servers to build highly available and scalable web app](/docs/solution-tutorials?topic=solution-tutorials-vpc-multi-region) |
| {{site.data.keyword.dl_full_notm}} | 99.999% | [High Availability and disaster recovery for Direct Link](/docs/dl?topic=dl-ha-dr) |
| {{site.data.keyword.fl_full}} | 99.999% | [About IBM Cloud Flow Logs for VPC](/docs/vpc?topic=vpc-flow-logs) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} networking services" caption-side="bottom"}

### AI/Machine Learning services
{: #slo-high-ai-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} AI or machine learning services in a high availability configuration.

| AI/Machine Learning service | Availability target | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.languagetranslatorshort}} | 99.999% | [High availability and disaster recovery](/docs/language-translator?topic=language-translator-ha-dr) |
| {{site.data.keyword.speechtotextshort}} | 99.999% | [High availability and disaster recovery](/docs/speech-to-text?topic=speech-to-text-ha-dr) |
| {{site.data.keyword.nlushort}} | 99.999% | [High availability and disaster recovery](/docs/natural-language-understanding?topic=natural-language-understanding-ha-dr) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} AI/machine learning services" caption-side="bottom"}

### Analytics services
{: #slo-high-analytics-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} analytics services in a high availability configuration.

| Analytics service | Availability target  | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.iae_full_notm}} | 99.995% | [High availability and disaster recovery](/docs/AnalyticsEngine?topic=AnalyticsEngine-responsibilities-serverless#disaster-recovery) |
| {{site.data.keyword.sqlquery_notm}} | 99.999% | [Disaster recovery and backup](/docs/sql-query?topic=sql-query-disaster) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} analytics services" caption-side="bottom"}


### Database services
{: #slo-high-datbase-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} database services in a high availability configuration.

| Databases service | Availability target | Performance detail | Storage back end used for data | Storage back end used for backups | High availability guidance |
|----------|---------|---------|---------|---------|---------|
| {{site.data.keyword.databases-for-mongodb_full_notm}} | 99.999% | [Performance](/docs/databases-for-mongodb?topic=databases-for-mongodb-performance) | Block Storage | Object Storage | [Understanding high availability and disaster recovery for Cloud Databases](/docs/databases-for-mongodb?topic=databases-for-mongodb-ha-dr) |
| {{site.data.keyword.databases-for-redis_full_notm}} | 99.999% |[Performance](/docs/databases-for-redis?topic=databases-for-redis-performance) | Block Storage | [Object Storage](/docs/databases-for-redis?topic=databases-for-redis-security-compliance#data-resilience) | [High-Availability](/docs/databases-for-redis?topic=databases-for-redis-high-availability) |
| {{site.data.keyword.databases-for-elasticsearch_full_notm}} | 99.999% | [Performance](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-performance) | Block Storage | Object Storage | [High-Availability](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-high-availability) |
| {{site.data.keyword.messages-for-rabbitmq_full_notm}} | 99.999% |[Performance](/docs/messages-for-rabbitmq?topic=messages-for-rabbitmq-performance) | Block Storage | [Object Storage](/docs/messages-for-rabbitmq?topic=messages-for-rabbitmq-security-compliance#data-resilience) | [High-Availability](/docs/messages-for-rabbitmq?topic=messages-for-rabbitmq-high-availability) |
| {{site.data.keyword.cloudant_short_notm}} | 99.999% | [Provisioned Throughtput Capacity](/docs/Cloudant?topic=Cloudant-ibm-cloud-public#provisioned-throughput-capacity) | Local Storage: [Data is stored in triplicate across 3 servers for data durability](/docs/Cloudant?topic=Cloudant-disaster-recovery-and-backup#in-region-automatic-data-redundancy), in 3 availability zones where available |  | [High availability, disaster recovery, and backup in a data center](/docs/Cloudant?topic=Cloudant-ibm-cloud-public#high-availability-disaster-recovery-and-backup-in-a-data-center) |
| {{site.data.keyword.databases-for-enterprisedb_full_notm}} | 99.999% | [Performance](https://cloud.ibm.com/docs/databases-for-enterprisedb?topic=databases-for-enterprisedb-performance) | Block Storage | [Object Storage](/docs/databases-for-enterprisedb?topic=databases-for-enterprisedb-security-compliance#security-compliance-data-resilience)  | [High-Availability](/docs/databases-for-enterprisedb?topic=databases-for-enterprisedb-high-availability) |
| {{site.data.keyword.Db2_on_Cloud_short}} | 99.999% |  | Block Storage | Object Storage  | [High availability (HA)](/docs/Db2onCloud?topic=Db2onCloud-ha) |
| {{site.data.keyword.dashdblong_notm}} | 99.999% |  | Block Storage | Object Storage  | [High availability (HA)](/docs/Db2whc?topic=Db2whc-ha) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} databases services" caption-side="bottom"}


### Developer tools services
{: #slo-high-developer-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} developer tools services in a high availability configuration.

| Developer tools service | Availability target  | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.contdelivery_short}} | 99.999% | [High availability and disaster recovery](/docs/ContinuousDelivery?topic=ContinuousDelivery-ha-dr) |
| {{site.data.keyword.bpfull_notm}} | 99.999% | [High availability](/docs/schematics?topic=schematics-high-availability) |
| {{site.data.keyword.en_full_notm}}  | 99.99% | [Understanding high availability for Event Notifications](/docs/event-notifications?topic=event-notifications-en-high-availability) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} developer tools services" caption-side="bottom"}

### Observability services
{: #slo-high-log-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} observability services in a high availability configuration.

| Logging and monitoring service | Availability target  | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.logs_full_notm}} | 99.99% |  [High availability and disaster recovery](/docs/cloud-logs?topic=cloud-logs-cloud-logs-ha-dr) |
| {{site.data.keyword.mon_full_notm}} | 99.999% |  [High availability and disaster recovery](/docs/monitoring?topic=monitoring-ha-dr) |
| {{site.data.keyword.atracker_full_notm}} | 99.999% |  [High availability and disaster recovery](/docs/atracker?topic=atracker-atracker-ha-dr) |
| {{site.data.keyword.logs_routing_full_notm}} | 99.999% |  [High availability and disaster recovery](/docs/logs-router?topic=logs-router-logs-router-ha-dr) |
| {{site.data.keyword.metrics_router_full_notm}} | 99.999% |  [High availability and disaster recovery](/docs/metrics-router?topic=metrics-router-metrics-router-ha-dr) |
| {{site.data.keyword.at_full_notm}} [Deprecated]{: tag-deprecated} | 99.99% | [Incident and operations management](/docs/activity-tracker?topic=activity-tracker-shared-responsibilities#incident-and-ops) |
| {{site.data.keyword.la_full_notm}} [Deprecated]{: tag-deprecated} | 99.99% | [Adoption guidelines for regulated and highly available workloads](/docs/log-analysis?topic=log-analysis-adoption) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} logging and monitoring services" caption-side="bottom"}

### Integration services
{: #slo-high-integrate-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} integration services in a high availability configuration.

| Integration service | Availability target | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.messagehub}} | 99.999% |  [What do you need to consider to achieve this availability?](/docs/EventStreams?topic=EventStreams-sla#sla_availability) |
| {{site.data.keyword.apiconnect_short}} | 99.999% | [Understanding high availability and disaster recovery for API Connect](/docs/apiconnect?topic=apiconnect-ha-dr) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} integration services" caption-side="bottom"}

### Security services
{: #slo-high-security-services}

The following table describes the service level objectives (SLOs) for the high impact {{site.data.keyword.cloud_notm}} security services in a high availability configuration.

| Security service | Availability target | High availability guidance |
|----------|---------|---------|
| {{site.data.keyword.appid_full_notm}} | 99.999% |  [Understanding high availability and disaster recovery for App ID](/docs/appid?topic=appid-ha-dr) |
| {{site.data.keyword.secrets-manager_full_notm}} | 99.999% |  [Understanding high availability and disaster recovery for Secrets Manager](/docs/secrets-manager?topic=secrets-manager-ha-dr) |
| {{site.data.keyword.compliance_long}} | 99.999% | [Understanding high availability for Security and Compliance Center](/docs/security-compliance?topic=security-compliance-ha) |
{: caption="Service level objectives (SLOs) for the {{site.data.keyword.cloud_notm}} security services" caption-side="bottom"}


## Service level objectives for other {{site.data.keyword.cloud_notm}} services
{: #other-slo}

| Service | Availability target |
|----------|---------|
| Any product not listed | 99.99% |
{: caption="Service level objectives (SLOs) for other {{site.data.keyword.cloud_notm}} services" caption-side="bottom"}
