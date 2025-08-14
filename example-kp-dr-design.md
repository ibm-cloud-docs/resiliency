---

copyright:
   years: 2020, 2025
lastupdated: "2025-08-14"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation, key protect

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Basic DR designs for Key Protect
{: #basic-kp-dr-design}

Described here are two basic design options that can be used to provide a cross-region {{site.data.keyword.cloud_notm}} Key Protect disaster recovery solution.

{{site.data.keyword.keymanagementservicefull}} is an important {{site.data.keyword.cloud_notm}} service. It helps you provision or import encrypted keys for applications for many IBM Cloud services that can be managed from a central location. Rather than traditional disaster recovery, Key Protect offers cross-regional resiliency through either replication or multiple key deployments.

## Option One - Cross Region Resilience (replication)
{: #kp-dr-option-one}
If you require a cross-region resiliency solution for Key Protect that uses replication, you must provision your Key Protect instance in one of the regions where cross-region resilience is supported (`us-south`, `jp-tok`, and `eu-de`). You must also select a pricing plan that includes cross-region resiliency.

If you do not select these regions or options, automated cross-region resilience is not available and your instance defaults to in-region resilience. In-region resilience means that if there is a regional Key Protect outage, access to your encryption keys fail until the region is restored, or you opt for Option Two.

### Architecture
{: #kp-dr-architecture-option-one}

The following diagram shows the basic architecture Key Protect uses for cross-regional resilience. Three regions adopt this architecture and are paired as follows:

* `us-south` -> `us-east`
* `jp-tok`   -> `jp-osa`
* `eu-de`    -> Paris

![Diagram showing the basic KP cross-region architecture](images/kp-cross-region-resilience.svg "Diagram depicting a basic view of Key Protect cross-region architecture"){: caption=" A basic view of Key Protect cross-regional resilience architecture" caption-side="bottom"}

### How it works
{: #kp-dr-how-it-works-option-one}

When operating with cross-region resilience, Key Protect automatically replicates data to the region where it has failover support, into standby infrastructure.

If there is a region-wide disaster where Key Protect suffers an outage, requests to Key Protect in the failed region are automatically routed to the corresponding failover region.

The failover region is in a 'read-only' mode. This means that during of the outage, you cannot create, update, or delete keys with your affected Key Protect instance. The exception is where there is a prolonged outage of six hours or more, at which point write operations are enabled.

### How to enact a recovery
{: #kp-enact-recovery-option-one}

Using cross-region resilience, there is no recovery for the customer to do. Switching requests to the standby Key Protect instance is automatic.


## Option Two - Multiple Key Deployments
{: #kp-dr-option-two}
An alternative option to Key Protect replication is to deploy an additional Key Protect instance in a second region and to create a second key that uses the same key material. This option is especially useful where the Key Protect replication option isn't available, due to location restrictions, for example. Since an application can use a key from a Key Protect instance in any region, you can also deploy to as many other locations as you like.

### How it works
{: #kp-dr-how-it-works-option-two}

When implementing this option, you deploy a second instance of Key Protect into a separate region. You then create a new root key in that second region, by using the same key material used to create the root key in the first region. Both root keys are identical. That means they are able to unwrap data encryption keys created by either root key. Encryption keys can then be duplicated in each region, bearing in mind that they have different key_id and service instance ID values. Your application can then be coded in such a way that if it fails to interact with the primary key (or rather the primary instance of Key Protect), it can then try a secondary key.

However, when a root key is rotated, new key material is added to the key, creating a new version of the key. Make sure that you use the updated key material when rotating the duplicate keys.

### How to enact a recovery
{: #kp-enact-recovery-option-two}

No customer recovery is required with this option, while your application is configured to interact with both keys.


## Failback
{: #kp-failback}

If there is a regional outage, {{site.data.keyword.cloud_notm}} recovers your Key Protect instance, including its configuration and other data. Requests to Key Protect are routed to the recovered instance, or handled in the usual way if using Option Two. If you are using replication, then replication to the standby Key Protect instance resumes.

## Other considerations
{: #kp-dr-other-considerations}

Consider the following points when using Key Protect.

### Consider your need for cross-region resilience
{: #kp-dr-do-i-need}

Enabling cross-region resilience for Key Protect does require additional monthly expenditure. The Recovery Time Objective (RTO) for Key Protect in the event of a regional failure is less than nine hours. Consider the length of outages that your workload can tolerate in deciding whether to use cross-region resilience.

### Location requirements
{: #kp-dr-location-requirements}

Cross-region resilience is only available in select regions. While workloads can access Key Protect instances from and in any region, provisioning Key Protect in a region that is close to your workload is recommended for faster, more reliable connections to the service. Certain regulatory requirements can also influence or restrict your choice of deployment region. Overall, {{site.data.keyword.cloud_notm}} recommends that you choose the region that is best for your use case, which might influence your choice between the preceding recovery options, too.

## Further reading
{: #kp-dr-further-reading}

For additional information on high availability and DR for Key Protect, refer to the following documentation pages:

* [Understanding high availability and disaster recovery for Key Protect](/docs/key-protect?topic=key-protect-ha-dr&interface=ui)
* [Pricing for Key Protect on IBM Cloud](/docs/key-protect?topic=key-protect-pricing-plan)
