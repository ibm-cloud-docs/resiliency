---

copyright:
  years: 2018, 2024
lastupdated: "2024-12-11"

keywords: load balancing, global load balancing, HA, DR, high availability, disaster recovery, HA for the platform, high availability for platform, disaster recovery plan, disaster event, zero downtime, workloads, failover, failover design, network resiliency, recovery time objective, recovery point objective

subcollection: resiliency

---

# How {{site.data.keyword.cloud_notm}} prepares for disaster recovery
{: #disaster-recovery}

The disaster recovery sections of this solution guide focus on what you need to do as a user of {{site.data.keyword.cloud_notm}} to prepare for potential disasters. Of course, {{site.data.keyword.cloud_notm}} is also be prepared for potential disasters and able to recover services, should they fail.

{{site.data.keyword.cloud_notm}} follows best practices for disaster recovery. All {{site.data.keyword.cloud_notm}} applications are designed to automatically recover and restart after any disaster event. Recovery is from electronic backups at a recovery center or alternative computing facilities that restore computing. The disaster recovery plan includes the systems and hosting requirements for hardware, software, networking connectivity, and offsite backup capabilities.

The following list includes the requirements that {{site.data.keyword.IBM_notm}} adheres to for a disaster recovery plan:

- A design document that explains how load balancing is used to keep a service highly available.
- Where multi-site failover occurs, the disaster recovery plan must explain who does what to cause the failover and ensure the service restarts.
- The disaster recovery plan must define how the solution works and include restore point objectives that clearly explain how much data might be lost in the outage, if any. The disaster recovery plan also includes a detailed recovery workflow for restoring services and data if a multi-availability zone failure occurs.
- The plan must confirm how the Maximum Tolerable Downtime is met.
- The disaster recovery plan specifies the security controls for running in Disaster mode, if they are different from what's running in production.
- The plan must be stored on the Disaster Recovery Plan database.

## Management of {{site.data.keyword.cloud_notm}}'s disaster recovery plan
{: #dr-plan-mgmt}

The requirements that {{site.data.keyword.cloud_notm}} follows are:

- The disaster recovery plan must be updated after any major infrastructure change, major application release, and after any test.
- It must be approved annually.

For more information about the specific consideration for your workloads disaster recovery, see [Understanding disaster recovery](/docs/resiliency?topic=resiliency-understanding-dr).
