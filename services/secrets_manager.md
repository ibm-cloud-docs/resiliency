---

copyright:
  years: 2024, 2024
lastupdated: "2024-10-30"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.secrets-manager_short}} - Understanding high availability and disaster recovery

## High Availability
**This service** is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **Standard** plan.

The service is provisioned across three zones in a multi-zone region with no single point of failure. API requests are routed through a global load balancer to three HA instance nodes each in a different availability zone.

In the event of a HA instance node or availability zone failure, the service will continue to run with API requests being routed through a global load balancer to the surviving HA instance nodes. There may be a short period of time (seconds) between the outage and the global load balancer recognizing the failure, during which time, requests may be sent to the failed instance. Workloads that programmatically access the service instance should follow the [client availability retry logic](/docs/doesnotexist) to maintain availability. There is no noticeable degradation of service during a zonal failure.

IBM Cloud will resolve the outage and when the zone comes back on-line, the global load balancer will resume sending API requests to the restored instance node without need for customer action. 

## Customer disaster recovery

### Customer disaster definition

A disaster of an instance can be due to:
- Data corruption.
- Service becomes unavailable.
- Regional disaster.

Secret Manager secrets are generally updated through “rotation” where writing a value results in the creation of a new version of the secret. It may be possible to restore data corruption by restoring secrets from older versions. Only a fixed number of versions are persisted. See [managing secret versions](https://cloud.ibm.com/docs/secrets-manager?topic=secrets-manager-version-history).
{: .note}

### Customer disaster planning

To recover from a service instance outage a recovery service instance should be created in a recovery region. The recovery service instance should be configured with same data as the source service instance. The recovery service instance may have secrets that are unique to the recovery region.

Secret Manager service instance may have customer created dependencies on these optional services, make sure these exist in the recovered region:
- {{site.data.keyword.keymanagementservicefull}}
- {{site.data.keyword.hscrypto}}
- {{site.data.keyword.messagehub_full}}

Some secrets will need to be adjusted for the recovery region, for example connection strings or API keys to the regional services.

The recovery instance should align with the workload [disaster recovery approaches within IBM Cloud](https://test.cloud.ibm.com/docs/resiliency?topic=resiliency-dr-approaches)

The recovery instance should track data changes to primary service instance for data including groups, secrets, secret versions, certificates, event notifications,... Consider one of the following approaches for recovery:

1.	Use some combination of terraform, script or program as the source of truth. First update the source of truth and then use the source of truth to create/update the primary service instance and the recovery service instance. The source must be available to the restore version and is a single point of failure.
   - Zero footprint.
      - RTO - amount of time to provision and populate with data.
      - RPO - no data loss.
   - Minimal Operation. Restore instance is always fully populated.
      - RTO - 0 seconds
      - RPO no data loss
2.	It is possible to create a script or program to download secrets from your primary service instance by using the Secrets Manager API or and populate the recovery service instance with the data. The script can take advantage of Activity Tracker audit events of the primary instance to keep the recovery instance in sync along with Code Engine. Customer managed backups should be kept to restore from the disaster.
   - Minimal Operation. Restore instance is always fully populated.
      - RTO - 0 seconds
      - RPO - 15 min of data synchronization loss depending on details approach.

### Customer disaster recovery

In the event of a customer declared disaster in the primary instance the service in the recovery region will be used (Minimal Operation) or the created (Zero Footprint). Redirect your workload components to the recovered instance or optionally insert into the retry code within your application to redirect requests to the second instance (Minimal Operation). 

### Customer recovery from BYOK loss

If your service instance was provisioned by using the root key from either {{site.data.keyword.keymanagementservicefull}} or {{site.data.keyword.hscrypto}} and you accidentally deleted the root key, open a case and include the following information:
- Your service instance's CRN
- Your backup Key Protect or HPCS instance's CRN
- The new Key Protect or HPCS root key ID
- The original Key Protect or HPCS instance's CRN and key ID, if available

See Recovering from an accidental key loss for authorization in the Key Protect and HPCS docs.

## IBM Disaster Recovery
### IBM disaster definition
As described above this service is highly available, even in the presence of a zonal failure. The service has suffered a disaster when damaged and must be restored from backups.

### IBM disaster planning
All data associated with the service is backed up once daily by the service. There is no configuration or visibility to this backup.  The backup is in a cross-region Cloud Object Storage bucket managed by the service. There is a potential for 24-hour’s worth of data loss. **These backups are not available for customer managed disaster recovery.**

### IBM Disaster Recovery
When a service is recovered from backups the service ID for the instance will be restored so clients using the endpoint will not need to be updated with new connection strings.

## IBM Service Maintenance
Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by retry login within client applications.

All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Changes are thoroughly tested before release. Complex changes are enabled/disabled to control exposure. When updates are rolled out, they are planned on a region-by-region basis. Upgrades are implemented and then provisioned to insure zero downtime for the service instance unless notified.

See [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.
