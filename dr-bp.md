---

copyright:
  years: 2018, 2024
lastupdated: "2024-10-28"

keywords: DR, high availability, disaster recovery, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}


# Best practices for {{site.data.keyword.cloud_notm}} disaster recovery
{: #disaster-recovery}

Follow our best practices to learn about {{site.data.keyword.cloud_notm}} disaster recovery options and tools that you can use to recover and restart after any disaster event.

All {{site.data.keyword.cloud_notm}} applications automatically recover and restart after any disaster event. Recovery is from electronic backups at a recovery center or alternative computing facilities that restore computing. Before any potential disaster, the disaster recovery plan includes the systems and hosting requirements for hardware, software, networking connectivity, and offsite backup capabilities.

The following list includes the requirements that {{site.data.keyword.IBM_notm}} adheres to for a disaster recovery plan:

- A design document explains how load balancing is used to keep a service highly available.
- Where a multi-site failover occurs, the disaster recovery plan must explain who does what to cause the failover and help ensure restart.
- The disaster recovery plan must define how the solution works and include restore point objectives that clearly explain how much data might be lost in the outage, if any. The disaster recovery plan also includes a detailed recovery workflow for restoring services and data if a multi-availability zone failure.
- It must confirm how the Maximum Tolerable Downtime is met and be stored on the Disaster Recovery Plan database.
- The disaster recovery plan specifies the security controls for running in Disaster mode, if they are different from what's running in production.

## Management of the disaster recovery plan
{: #dr-plan-mgmt}

The requirements that {{site.data.keyword.cloud_notm}} follows are:

- The disaster recovery plan must be updated after any major infrastructure change, major application release, and after any test.
- It must be approved annually.

For more information about the specific consideration for your workloads disaster recovery, see [Understanding disaster recovery](/docs/overview?topic=overview-understanding-dr).
