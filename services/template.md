---

copyright:
  years: 2024, 2024
lastupdated: "2024-10-31"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# template - Understanding high availability and disaster recovery
{: #template-high-availability-disaster-recovery}

Customer visible HA and DR concerns. How to configure for HA. What are the different kinds of disasters. How to recover from common failures and disasters. HA and DR planning and design considerations for workloads that consume this service.

Topics to cover:
Customer
- Single zone failure (if applicable) - restore to recovery zone
- Regional failure - restore to recovery region
- Service/instance failure - restore to new instance, perhaps in a recovery region
- Disaster Data corruption - restore to new instance.
- RTO/RPO within [Disaster recovery approaches within IBM Cloud](/docs/resiliency?topic=resiliency-dr-approaches):
  - Zero footprint
  - Basic standby
  - Minimal operation
- Configure for DR options
- Reference to client retry logic
- Reference to SLA verify it is correct
- Reference to SLO verify it is correct
- Configure for HA to meet SLA

IBM
- IBM recovery from zone failure.
- IBM recovery from region failure, restore from backups.
- IBM updates the software with fixes and new features with minimal impact....

Questions:
- Provisioned by resource controller?  Seems interesting but is it getting into the entire dependency tree?
   - do not cover dependencies

## Service High Availability
{: #template-high-availability}

This service is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **Standard** plan.

1. Active/Active. Modern architecture built in the cloud distributed evenly across 3 zones.  See Secrets Manager. Zonal failure will be covered here

The service is provisioned across three zones in a multi-zone region with no single point of failure and is resilient in the event of a zonal failure. API requests are routed through a global load balancer to three HA instance nodes each in a different availability zone.

In the event of a HA instance node or availability zone failure, the service will continue to run with API requests being routed through a global load balancer to the surviving HA instance nodes. There may be a short period of time (seconds) between the outage and the global load balancer recognizing the failure, during which time, requests may be sent to the failed instance.

Workloads that programmatically access the service instance should follow the [client availability retry logic](/docs/doesnotexist) to maintain availability.

IBM Cloud will resolve the outage and when the zone comes back on-line, the global load balancer will resume sending API requests to the restored instance node without need for customer action. 

`A simple description of the underlying architecture that explains HA. It may fall into a few types:`


1. Active/Passive. Along with an active node a hot standby is available to take over and switched to when a problem is detected. Software is deployed by first updating the standby and failing over. The new standby can then be updated. Active and standby are in different zone to ensure availability in a zone failure. Some services support two standbys allowing two simultaneous zone failures.

## Customer High Availability - HA
{: #template-customer-high-availability}

Optional section describing configuration requirements for reaching levels of HA.

1. Zonal
High availability is achieved for an individual instance at the zonal level by removing single point of failure where possible including:
- power
- networking connectivity
- ?

To achieve high availability it is up to customers to create a workload from the zonal components distributed over multiple zones using a load balancer or other description.

## Customer Disaster Recovery - DR
{: #template-customer-disaster-recovery-dr}

TODO REMOVE: Things to cover in the sections below
- Customer recover from zonal failure. could be detailed like VPC block storage, or could be just a note like ICD - see HA above
- Customer recover from regional failure
- Customer recover from service failure may be same as regional failure
- Customer recover from data corruption
- Customer recovery from BYOK loss this was covered in Secrets Manager is it a general problem

### Customer disaster definition
{: #template-customer-disaster-definition}

A disaster of an instance can be due to:
- Data corruption.
- Service becomes unavailable.
- Regional disaster.

### Customer disaster planning
{: #template-customer-disaster-planning}

To recover from a service instance outage a recovery service instance should be created in a recovery region. The recovery service instance should be configured with same data as the source service instance.

The recovery instance should align with the workload [disaster recovery approaches within IBM Cloud](https://test.cloud.ibm.com/docs/resiliency?topic=resiliency-dr-approaches)

**Zero Footprint** -
Restore from backups ...
- RTO ...
- RPO ...

**Basic Standby** - 
...
- RTO ...
- RPO ...

**Minimal Operation** - Create a [read-only replicas](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas) for cross-regional failover. [Promote the read-only replica](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas&interface=ui#promoting-read-only-replica) to recover from a disaster.
- RTO - few minutes
- RPO - few minutes

**Active/Active**
Data in postgreSQL can be lost due to bugs in software, accidental or malicious operations. See **Active/Nothing** above for recovery. If only a portion of the database has been corrupted consider using the newly created database instance to harvest the corrupted data.


### Customer disaster recovery
{: #template-customer-disaster-recovery}

This service instance may have customer created dependencies on these optional services, make sure these exist in the recovered region:
- {{site.data.keyword.keymanagementservicefull}}
- {{site.data.keyword.hscrypto}}

In the event of a customer declared disaster in the primary instance the service in the recovery region will be used (Minimal Operation) or the created (Zero Footprint). Redirect your workload components to the recovered instance or optionally insert into the retry logic to redirect requests to the second instance (Minimal Operation). 

## IBM disaster recovery
{: #template-ibm-disaster-recovery}

### IBM recovery from zone failure 
{: #template-ibm-recovery-from-zone-failure}

### IBM recovery from regional failure
{: #template-ibm-recovery-from-regional-failure}

After a regional failure IBM will attempt to restore the service instance with the same connection strings from the last state in internal persistent storage.
- RTO - TODO
- RPO - TODO

It may not be possible for IBM to restore the service instance, and it will be required for the customer restore the database - see [customer disaster recovery](#template-customer-disaster-recovery)

## IBM Service Maintenance
{: #template-ibm-service-maintenance}

All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by [client availability retry logic](/docs/doesnotexist) within client applications. Changes are initially rolled out sequentially on a region-by-region basis.

ADD THIS IF APPROPRIATE:
Complex changes are enabled/disabled with feature flags to control exposure.

Changes that impact customer workloads will be described by notifications. See [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.

## TODO Issues to resolve in this template

Customer recovery from BYOK loss -  this was covered in Secrets Manager is it a general problem?
Term to use for a service instance: service instance, cluster, resource
