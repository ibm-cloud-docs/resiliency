---

copyright:
  years: 2024, 2024
lastupdated: "2024-10-18"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Secrets Manager - Understanding high availability and disaster recovery

## Resource Availability
**This service** is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **Standard** plan.

This service is provisioned via the [Resource Controller](/apidocs/resource-controller/resource-controller) see [Resource Controller - Understanding high availability](/docs/doesnotexist).

The service and associated service instance resources are provisioned across three zones in a multi-zone region with no single point of failure. API requests are routed through a global load balancer to three HA instance nodes each in a different availability zone.

In the event of a HA instance node or availability zone failing, the service will continue to run with API requests being routed through a global load balancer to the surviving HA instance nodes. There may be a short period of time (seconds) between the outage and the global load balancer recognizing the failure, during which time, requests may be sent to the failed instance. Workloads that programmatically access the service instance should follow the [client availability retry logic](/docs/doesnotexist) to maintain availability. There is no noticeable degradation of service during a zonal failure.

IBM Cloud will resolve the outage and when the zone comes back on-line, the global load balancer will resume sending API requests to the restored instance node without need for customer action. 

## Customer disaster recovery
### Customer disaster definition
A disaster of an instance can be due to:
- Accidental or malicious data corruption.
- Instance in an unavailable region

Secret Manager secrets are generally updated through “rotation” that creates a new version of the secret maintaining a fixed number of previous secret versions and it may be possible to repair accidental data corruption by restoring secrets from older versions.
{: .note}

### Customer disaster planning
<COMMON> To reduce the potential disruption caused by a regional outage of a primary service instance a recovery service instance should be created in a recovery region. The recovery service instance should be configured with same data as the source service instance. The recovery service instance may have some dependencies on the recovery region.</COMMON>

Secret Manager service instance dependencies to consider when designing a recovery plan:
- Key Protect or HPCS service instance in the recovery region when used.
- Event notification service when used.
- Keys, or configurations that may be unique to the recovery region.

The recovery instance should track data changes to primary service instance for data including groups, secrets, secret versions, certificates, event notifications,... Consider one of the following strategies for keeping the recovery service instance up to date:
1.	Use some combination of terraform, script or program as the source of truth. First update the source of truth and then use the source of truth to create/update the primary service instance and the recovery service instance.
2.	It is possible to create a script or program to download secrets from your primary service instance by using the Secrets Manager API or and populate the recovery service instance with the data. The script can take advantage of Activity Tracker audit events of the primary instance to keep the recovery instance in sync along with Code Engine. Customer managed backups should be kept to restore from the disaster.

### Customer disaster recovery
In the event of a customer declared disaster, redirect your applications to the Secrets Manager instance in the second region or optionally insert into the retry code within your application to redirect requests to the second region. 

### Customer recovery from BYOK loss
If your service instance was provisioned by using the root key from either Key Protect or IBM Cloud® Hyper Protect Crypto Services (HPCS) and you accidentally deleted the root key, open a case and include the following information:
•	Your service instance's CRN
•	Your backup Key Protect or HPCS instance's CRN
•	The new Key Protect or HPCS root key ID
•	The original Key Protect or HPCS instance's CRN and key ID, if available
See Recovering from an accidental key loss for authorization in the Key Protect and HPCS docs.

## IBM Disaster Recovery
### IBM disaster definition
The service is damaged in such a way that it can only be restored from backups.

### IBM disaster planning
All data associated with a service instance is backed up daily by the service. There is no configuration or visibility to this backup.  The backup is in a cross-region Cloud Object Storage bucket managed by the service. There is a potential for 24-hour’s worth of data loss. **These backups are not available for customer managed disaster recovery.**

### IBM Disaster Recovery
When a service is recovered from backups the service ID for the instance will be restored so clients using the endpoint will not need to be updated with new connection strings.

### Service Maintenance
Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals.

All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Changes are thoroughly tested before release. Complex changes are enabled/disabled to control exposure. When updates are rolled out, they are planned on a region-by-region basis. If at any point a release causes an issue, the release is automatically stopped. Upgrades are implemented and then provisioned to insure zero downtime for the service instance unless notified.

IBM provides advanced notice of all planned maintenance events which may cause disruption to your service instance through the IBM Cloud Dashboard with configurable notifications. 
