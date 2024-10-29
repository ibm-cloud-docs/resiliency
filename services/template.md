# Service Example Template

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
**This service** is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **Standard** plan.

A simple description of the underlying architecture that explains HA. It may fall into a few types:

1. Active/Active. Modern architecture built in the cloud distributed evenly across 3 zones.  See Secrets Manager. Zonal failure will be covered here

The service is provisioned across three zones in a multi-zone region with no single point of failure. API requests are routed through a global load balancer to three HA instance nodes each in a different availability zone.

In the event of a HA instance node or availability zone failure, the service will continue to run with API requests being routed through a global load balancer to the surviving HA instance nodes. There may be a short period of time (seconds) between the outage and the global load balancer recognizing the failure, during which time, requests may be sent to the failed instance. Workloads that programmatically access the service instance should follow the [client availability retry logic](/docs/doesnotexist) to maintain availability. There is no noticeable degradation of service during a zonal failure.

IBM Cloud will resolve the outage and when the zone comes back on-line, the global load balancer will resume sending API requests to the restored instance node without need for customer action. 


1. Active/Passive. Along with an active node a hot standby is available to take over and switched to when a problem is detected. Software is deployed by first updating the standby and failing over. The new standby can then be updated. Active and standby are in different zone to ensure availability in a zone failure. Some services support two standbys allowing two simultaneous zone failures.

## Customer High Availability
Optional section describing configuration requirements for reaching levels of HA.

1. Zonal
High availability is achieved for an individual instance at the zonal level by removing single point of failure where possible including:
- power
- networking connectivity
- ?

To achieve high availability it is up to customers to create a workload from the zonal components distributed over multiple zones using a load balancer or other description.
## Customer disaster recovery
Things to cover in the sections below
- Customer recover from zonal failure. could be detailed like VPC block storage, or could be just a note like ICD - see HA above
- Customer recover from regional failure
- Customer recover from service failure may be same as regional failure
- Customer recover from data corruption
- Customer recovery from BYOK loss this was covered in Secrets Manager is it a general problem

### Customer disaster definition
A disaster of an instance can be due to data corruption or a service instance that becomes unavailable.
- Data corruption.
- Service becomes unavailable to a regional or service disaster.

### Customer disaster planning
To recover from a service instance outage a recovery service instance should be created in a recovery region. The recovery service instance should be configured with same data as the source service instance.

The recovery instance should align with the workload [disaster recovery approaches within IBM Cloud](https://test.cloud.ibm.com/docs/resiliency?topic=resiliency-dr-approaches)

### Customer disaster recovery
In the event of a customer declared disaster in the primary instance the service in the recovery region will be used (Minimal Operation) or the created (Zero Footprint). Redirect your workload components to the recovered instance or optionally insert into the retry code within your application to redirect requests to the second instance (Minimal Operation). 


## IBM disaster recovery
### IBM recovery from zone failure 
### IBM recovery from regional failure
After a regional failure a service instance is recovered from ...
If dependent storage devices in the region are damaged the service is recovered from backups previously stored in a regional COS bucket - these may be customer visible and managed backups like those in ICD or invisible backups like those kept by Secrets Manager.

## IBM Service Maintenance
Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by retry login within client applications.

All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Changes are thoroughly tested before release. Complex changes are enabled/disabled with configuration to control exposure. When updates are rolled out, they are planned on a region-by-region basis. Upgrades are implemented and then provisioned to insure zero downtime for the service instance unless notified.

See [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.
