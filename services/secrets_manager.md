---

copyright:
  years: 2024, 2024
lastupdated: "2024-11-08"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: resiliency

---



{{site.data.keyword.attribute-definition-list}}

# Understanding high availability and disaster recovery for {{site.data.keyword.secrets-manager_short}}
{: #secrets-manager-ha-dr}

## High Availability
{: #secrets-manager-high-availability}

This service is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **Standard** plan.

### High Availability Architecture
{: #secrets-manager-high-availability-architecture}

The service is provisioned across three zones in a multi-zone region with no single point of failure. API requests are routed through a global load balancer to three HA instance nodes each in a different availability zone.

In the event of a HA instance node or availability zone failure, the service will continue to run with API requests being routed through a global load balancer to the surviving HA instance nodes. There may be a short period of time (seconds) between the outage and the global load balancer recognizing the failure, during which time, requests may be sent to the failed instance. Workloads that programmatically access the service instance should follow the [client availability retry logic](/docs/doesnotexist) to maintain availability. There is no noticeable degradation of service during a zonal failure.



### High Availability Options
{: #secrets-manager-high-availability-options}



The {{site.data.keyword.secrets-manager_full}} instances are highly available with no configuration required.

## Disaster recovery
{: #secrets-manager-disaster-recovery}

To recover from a service instance outage a recovery service instance should be created in a recovery region. In general the recovery service instance should be configured with same data as the source service instance, but there are exceptions to this guidance.

Some secrets will need to be adjusted for the recovery region, for example connection strings or API keys may reference regional specific service instances. These values will be different in the recovery service instance.

The Secret Manager service instance may have customer created dependencies on these optional services, make sure these exist in the recovered region:
- {{site.data.keyword.keymanagementservicefull}}
- {{site.data.keyword.hscrypto}}
- {{site.data.keyword.messagehub_full}}


### Disaster recovery options
{: #secrets-manager-disaster-recovery-options}

Plan for the recovery into a recovery region. The recovery instance should align with the workload [disaster recovery approaches within IBM Cloud](https://test.cloud.ibm.com/docs/resiliency?topic=resiliency-dr-approaches) The recovery instance should track data changes to primary service instance for data including groups, secrets, secret versions, certificates, event notifications.

If the disaster does not impact the production service instance, for example data corruption, it may be possible for a customer to repair the data in the service instance in place.

The service supports the following disaster recovery options:

Feature | Description | Consideration
-|-|-
Rotation | Restore previous secret version | The production service instance must be available. There are a limited number of version in the version history. See [known issues and limits](https://cloud.ibm.com/docs/secrets-manager?topic=secrets-manager-known-issues-and-limits&interface=ui).

All other disaster recovery options are created and supported by the customer. Two possible customer managed options are discussed below.

Feature | Description | Consideration
-|-|-
External source of truth | All secrets created via a script, described below. | Customer must create the script and persist where it can be used during disaster
Live synchronization | Secret changes in production are automatically observed and propagated to recover service instance, see description below | Customer must create and maintain tools. Data corruption will be synchronized to the recovery instance.

### Rotation
{: #secrets-manager-rotation}

Secret Manager secrets are generally updated via “rotation” where writing a value results in the creation of a new version of the secret. It may be possible to restore data corruption by restoring secrets from older versions in the production instance. Only a fixed number of versions are persisted. See [managing secret versions](https://cloud.ibm.com/docs/secrets-manager?topic=secrets-manager-version-history).


### External source of truth
{: #secrets-manager-external-source-of-truth}

The customer must create and use some combination of terraform, script or program as the source of truth. First update the source of truth and then use the source of truth to create/update the primary service instance and the recovery service instance. The source must be available to the restore version and is a single point of failure.

In the event of a customer declared disaster in the primary instance the service in the recovery region will be used (Minimal Operation) or the created (Zero Footprint). Redirect your workload components to the recovered instance or optionally insert into the retry code within your application to redirect requests to the second instance (Minimal Operation). 

The repository that contains the source of truth should have point in time recovery like {{site.data.keyword.cos_short}} versioned buckets or Github repositories.

### Live synchronization
{: #secrets-manager-live syncrhonization}

It is possible for the customer to create a script or program to download secrets from your primary service instance by using the Secrets Manager API or and populate the recovery service instance with the data. The script can take advantage of Activity Tracker audit events of the primary instance to keep the recovery instance in sync along with Code Engine. Customer managed backups should be kept to restore from the disaster.

### How to use the options for business continuity
{:-#secrets-manager-how-to-use-the-options-for-business-continuity}

Disaster recovery steps must be practiced on a regular basis. When building your plan consider the following failure scenarios and resolution.

Failure | Resolution
-|-
Hardware failure (single point) | IBM provides an instance that is resilient from single point of hardware failure within a zone - no configuration required.
Zone failure | IBM provides an instance that is resilient from single point of hardware failure within a zone - no configuration required.
Data corruption | Use rotation to restore previous secret version in an available service instance.
Data corruption | Restore a point in time uncorrupted version from the external source of truth
Regional failure | Switch critical workloads to use the restored version in a recovery region. Restore the instance using external source of truth or live synchronization.

### Feature RTO/RPO

Each feature covered above has a related RTO/RPO time as discussed below.

Feature | RTO/RPO
-|-
Restore previous secret version | RTO = minutes, practice and potentially scripting will you to identify recovery time, RPO = 0.
External source of truth - zero footprint | RTO = few minutes. Amount of time to provision and populate with data. Also consider the amount of time to adjust recovered workloads to the new service instance endpoint. RPO = 0, the source of truth is changed before making production changes.
External source of truth - minimal operation| RTO = few seconds, RPO = 0. Enhance the zero footprint description to keep a live service instance in the recovery region.
Live synchronization | RTO = minutes, RPO = minutes.

### Feature check list

Disaster recovery steps must be practiced on a regular basis. The following check list can help you create and practice your plan.

Rotation
- Create a test resource instance and practice rotating secrets versions and restoring a secret version.

External source of truth
- Verify the source of truth is in a repository that is available to the restore location.
- Verify the source of truth is not dependent on the disaster region to avoid dependency on a failed region.

Live syncrhonization
- Verify the recovery service instance is currently available in the restore region

For both external source of truth and live syncrhonization:
- Verify recovery workloads in the recovery region are integrated with the recover service instance
- Verify regional specific secrets are available in the recovery service instance.

### Customer recovery from BYOK loss

If your service instance was provisioned by using the root key from either {{site.data.keyword.keymanagementservicefull}} or {{site.data.keyword.hscrypto}} and you accidentally deleted the root key, open a case and include the following information:
- Your service instance's CRN
- Your backup Key Protect or HPCS instance's CRN
- The new Key Protect or HPCS root key ID
- The original Key Protect or HPCS instance's CRN and key ID, if available

See Recovering from an accidental key loss for authorization in the Key Protect and HPCS docs.

## IBM disaster recovery
{: #secrets-manager-ibm-disaster-recovery}

### IBM recovery from zone failure 
{: #secrets-manager-ibm-recovery-from-zone-failure}

IBM Cloud will resolve the outage and when the zone comes back on-line, the global load balancer will resume sending API requests to the restored instance node without need for customer action. 

### IBM recovery from regional failure
{: #secrets-manager-ibm-recovery-from-regional-failure}

todo After a regional has failed and has then been restored IBM will attempt to restore the service instance in the same region with the same connection strings from the last state in internal persistent storage.
- RTO - todo
- RPO - todo

In the event that IBM can not restore the service instance, the customer must restore as described in the disaster recovery section above.

All data associated with the service is backed up once daily by the service. There is no configuration or visibility to this backup.  The backup is in a cross-region Cloud Object Storage bucket managed by the service. There is a potential for 24-hour’s worth of data loss. **These backups are not available for customer managed disaster recovery.**

When a service is recovered from backups the service ID for the instance will be restored so clients using the endpoint will not need to be updated with new connection strings.

## IBM Service Maintenance
{: #secrets-manager-ibm-service-maintenance}

All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by [client availability retry logic](/docs/doesnotexist) within client applications. Changes are initially rolled out sequentially on a region-by-region basis.

Complex changes are enabled/disabled with feature flags to control exposure.

Changes that impact customer workloads will be described by notifications. See [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.
