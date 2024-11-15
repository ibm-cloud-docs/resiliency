---

copyright:
  years: 2024
lastupdated: "2024-11-15"


keywords: disaster recovery plan, DR, resilient app, plan for DR, DR objectives

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Services specific high availability and disaster recovery
{: #service-ha-dr}

The services in the IBM cloud use the global, highly available, foundation as described in [IBM Cloud region and data center locations for resource deployment](/docs/overview?topic=overview-locations). They share similar structure but there are unique characteristics that need to be understood when they are used for assembling a highly available workload and considered for disaster recovery.

Wave 1
- [{{site.data.keyword.databases-for-postgresql_full}}](/docs/resiliency?topic=resiliency-databases-for-postgresql-understanding-high-availability-and-disaster-recovery)
- {{site.data.keyword.secrets-manager_full}}
- {{site.data.keyword.iamlong}}
- {{site.data.keyword.dns_full}}
- {{site.data.keyword.cos_full}}


Wave 2
- {{site.data.keyword.databases-for-mongodb_full_notm}}
-  {{site.data.keyword.databases-for-mysql_full}}
- {{site.data.keyword.vpc_full}}
- vpc components like file storage, block storage, load balancer, ...
- {{site.data.keyword.containerlong}}
- {{site.data.keyword.openshiftlong}}

Wave 3
- {{site.data.keyword.databases-for-enterprisedb_full}}
- {{site.data.keyword.databases-for-redis_full_notm}}
-  {{site.data.keyword.databases-for-etcd_full_notm}}
- {{site.data.keyword.databases-for-elasticsearch_full_notm}}
- {{site.data.keyword.messages-for-rabbitmq_full}}
