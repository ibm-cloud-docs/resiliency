---

copyright:
   years: 2020, 2025
lastupdated: "2025-09-12"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation, key protect

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Designing DR for Key Protect
{: #basic-kp-dr-design}

Protecting your {{site.data.keyword.keymanagementservicefull}} resources via a disaster recovery solution can be achieved in any number of ways. However, to help you create a plan, consider starting with one of the following design options for a cross-region solution. Then, you can modify and test the options to meet your specific disaster recovery goals. Disaster recovery solutions for Key Protect are slightly different than other {{site.data.keyword.cloud_notm}} services as Key Protect offers cross-regional resiliency through either replication or multiple key deployments. 


## Option 1: Cross-region resiliency (replication)
{: #kp-dr-option-one}

If you require a cross-region resiliency solution for Key Protect that uses replication, you must provision your Key Protect instance in one of the regions where cross-region resiliency is supported (`us-south`, `jp-tok`, and `eu-de`). You must also select a pricing plan that includes cross-region resiliency. If you do not select these options, automated cross-region resiliency is not available and your instance defaults to in-region resiliency, which means that if there is a regional Key Protect outage you cannot access your encryption keys until the region is restored.

When operating with cross-region resiliency, the service automatically replicates data to the region where it has failover support. 

If cross-region resiliency is configured, Key Protect automatically replicates data to the failover region. Then, if there is a region-wide disaster in which Key Protect suffers an outage, all requests can be automatically routed to the corresponding failover region. If the failover region is in `read-only` mode, you can't create, update, or delete keys with your affected instance. But, if an outage persists longer than 6 hours, write operations are automatically enabled. 


### Understanding the architecture
{: #kp-dr-architecture-option-one}

The following diagram shows the basic architecture Key Protect uses for cross-regional resilience. Three regions adopt this architecture and are paired as follows:

* `us-south` -> `us-east`
* `jp-tok`   -> `jp-osa`
* `eu-de`    -> Paris

![Diagram showing the basic KP cross-region architecture](images/kp-cross-region-resilience.svg "Diagram depicting a basic view of Key Protect cross-region architecture"){: caption=" A basic view of Key Protect cross-regional resilience architecture" caption-side="bottom"}


### Enacting a recovery
{: #kp-enact-recovery-option-one}

With cross-region resiliency enabled a customer does not need to enact a recovery. Switching requests to the standby instance of the service is done automatically. 


## Failing back
{: #kp-failback-option-two}

If there is a regional outage, {{site.data.keyword.cloud_notm}} recovers your Key Protect instance, including its configuration and other data. Requests to Key Protect are routed to the recovered instance and replication to the standby instance resumes.


## Option 2: Multiple key deployments
{: #kp-dr-option-two}

An alternative to replication is to deploy an additional Key Protect instance in a second region. Then, you create a new root key in the secondary region by using the same key material that was used to create the root key in the primary region. The root keys are identical which means that they are able to unwrap data encryption keys created by either root key. Encryption keys can then be duplicated in each region, bearing in mind that they have different key and service ID's. You app can then be coded in such a way that if it fails to interact with the primary key or instance of Key Protect, it can then try the secondary key.

It is important to note that when a root key is rotated, new material is added to the key, which creates a new version of it. Be sure that you use the updated key material when you rotate the duplicate keys in your secondary region.

This option can be especially useful in situations where the replication option isn't available - when there are location restrictions for example.


### Enacting a recovery
{: #kp-enact-recovery-option-two}

If your application is configured to interact with both keys, there is no customer recovery required with this option. 


## Failing back
{: #kp-failback-option-two}

If there is a regional outage, {{site.data.keyword.cloud_notm}} recovers your Key Protect instance, including its configuration and other data. Requests to Key Protect are routed to the recovered instance.

## Understanding other considerations
{: #kp-dr-other-considerations}

Consider the following points when you use the Key Protect service.

### Considering your need for cross-region resilience
{: #kp-dr-do-i-need}

Enabling cross-region resiliency for Key Protect requires additional monthly expenditure. The Recovery Time Objective (RTO) for Key Protect in the event of a regional failure is less than nine hours. Consider the length of outages that your workload can tolerate in deciding whether to use cross-region resilience.

### Undesrtanding location requirements
{: #kp-dr-location-requirements}

Cross-region resiliency is only available in select regions. While workloads can access Key Protect instances from and in any region, provisioning Key Protect in a region that is close to your workload is recommended for faster, more reliable connections to the service. Certain regulatory requirements can also influence or restrict your choice of deployment region. Overall, {{site.data.keyword.cloud_notm}} recommends that you choose the region that is best for your use case, which might influence your choice between the preceding recovery options, too.

## Further reading
{: #kp-dr-further-reading}

For additional information on high availability and DR for Key Protect, refer to the following documentation pages:

* [Understanding high availability and disaster recovery for Key Protect](/docs/key-protect?topic=key-protect-ha-dr&interface=ui)
* [Pricing for Key Protect on IBM Cloud](/docs/key-protect?topic=key-protect-pricing-plan)
