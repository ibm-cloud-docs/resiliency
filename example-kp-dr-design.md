---

copyright:
   years: 2020, 2025
lastupdated: "2025-08-04"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation, key protect

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Basic DR design for Key Protect
{: #basic-kp-dr-design}

This page describes a basic design that can be used as a low-cost, cross-region, {{site.data.keyword.cloud_notm}} Key Protect disaster recovery solution. You can use this design as a starting point for protecting your {{site.data.keyword.cloud_notm}} Key Protect instances against a disaster.

{{site.data.keyword.keymanagementservicefull}} is an important {{site.data.keyword.cloud_notm}} service. It helps you provision or import encrypted keys for applications for many IBM Cloud services that can be managed from a central location. Rather than traditional disaster recovery, Key Protect offers cross-regional resiliency.

If you require a cross-region resiliency solution for Key Protect, you must provision your Key Protect instance in one of the regions where cross-region resilience is supported (`us-south`, `jp-tok` or `eu-de`). You must also select a pricing plan that includes cross-region resiliency.

If you do not select these regions or options, automated cross-region resilience is not available and your instance will default to in-region resilience. In such cases, in the event of a regional Key Protect outage, you will not be able to access your encryption keys until the region is restored, unless you have created and self-maintain a fully independent Key Protect instance with duplicate keys in a second region.

## Architecture
{: #kp-dr-architecture}

The following diagram shows the basic architecture Key Protect uses for cross-regional resilience. Three regions adopt this architecture and are paired as follows:

* `us-south` -> `us-east`
* `jp-tok`   -> `jp-osa`
* `eu-de`    -> Paris

![Diagram showing the basic KP cross-region architecture](images/kp-cross-region-resilience.svg "Diagram depicting a basic view of Key Protect cross-region architecture"){: caption=" A basic view of Key Protect cross-regional resilience architecture" caption-side="bottom"}

## How it works
{: #kp-dr-how-it-works}

When operating with cross-region resilience, Key Protect automatically replicates data to the region where it has failover support, into standby infrastructure.

In the event of a region-wide disaster where Key Protect suffers an outage, requests to Key Protect in the failed region are automatically routed to the corresponding failover region.

Note that the failover region is in a 'read-only' mode. This means that for the duration of the outage, you cannot create, update or delete keys with your affected Key Protect instance. The exception is where there is a prolonged outage of six hours or more, at which point write operations are enabled.

## How to enact a recovery
{: #kp-enact-recovery}

Using cross-region resilience, there is no recovery for the customer to perform. Swiching requests to the standby Key Protect instance is automatic.

## Failback
{: #kp-failback}

In the event of a regional outage, {{site.data.keyword.cloud_notm}} will recover your Key Protect instance, including its configuration and other data. Requests to Key Protect will then be routed to the recovered instance. Replication to the standby Key Protect instance will resume.

## Other considerations
{: #kp-dr-other-considerations}

These are considerations for cross-region resilience when using Key Protect.

### Consider your need for cross region resilience
{: #kp-dr-do-i-need}

Enabling cross-region resilience for Key Protect does require an additional monthly spend. The Recovery Time Objective (RTO) for Key Protect in the event of a regional failure is less than nine hours. Consider the length of outages that your workload can tolerate in deciding whether to use cross-region resilience.

### Location requirements
{: #kp-dr-location-requirements}

Cross-region reslience is only available in select regions. While workloads can access Key Protect instances from and in any region, provisioning Key Protect in a region that is close to your workload is recommended for faster, more reliable connections to the service. Certain regulatory requirements may also influence or restrict your choice of deployment region. Overall, {{site.data.keyword.cloud_notm}} recommends that you choose the region that is best for your use case.

## Further reading
{: #kp-dr-further-reading}

For additional information on high availability and DR for Key Protect, refer to the following documentation pages:

* [Understanding high availability and disaster recovery for Key Protect](/docs/key-protect?topic=key-protect-ha-dr&interface=ui)
* [Pricing for Key Protect on IBM Cloud](/docs/key-protect?topic=key-protect-pricing-plan)
