---

copyright:
  years: 2024, 2024
lastupdated: "2024-10-30"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.iamshort}} - Understanding high availability and disaster recovery
{: #iam-high-availability-disaster-recovery}

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
{: #iam-high-availability}

This service is a global platform service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo). See [How IBM Cloud ensures high availability and disaster recovery](https://cloud.ibm.com/docs/overview?topic=overview-zero-downtime) and specifically [Global platform services](https://cloud.ibm.com/docs/overview?topic=overview-zero-downtime#global-platform).

## Customer Disaster Recovery - DR
{: #iam-customer-disaster-recovery-dr}

Things to cover in the sections below
- Customer recover from zonal failure. could be detailed like VPC block storage, or could be just a note like ICD - see HA above
- Customer recover from regional failure
- Customer recover from service failure may be same as regional failure
- Customer recover from data corruption
- Customer recovery from BYOK loss this was covered in Secrets Manager is it a general problem

### Customer disaster definition
{: #iam-customer-disaster-definition}

A disaster of an instance can be due to:
- Data corruption.
- Service becomes unavailable.

### Customer disaster planning
{: #iam-customer-disaster-planning}

The data that is managed by {{site.data.keyword.iamshort}} can be backed. Consider one of the following approaches for recovery:

1.	Use some combination of terraform, script or program as the source of truth. First update the source of truth and then use the source of truth to create/update the global service. The source must be available to the restore version and is a single point of failure and should be persisted off {{site.data.keyword.cloud_notm}}. Restore data by replaying all or part of the source of truth.
- RTO - amount of time to restore the data.
- RPO - no data loss.
1.	It is possible to create a script to periodically read and persist the IAM data using the API or SDK described in the [API Docs](https://cloud.ibm.com/apidocs/iam-identity-token-api) or the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started). Data should be persisted off {{site.data.keyword.cloud_notm}}. Restore data by replaying all or part of the source of truth.
- RTO - amount of time to restore the data.
- RPO - time since last backup

### Customer disaster recovery
{: #iam-customer-disaster-recovery}

When a global service becomes unavailable there is no action that can be taken by a customer to resolve the issue. [Create a support case](https://cloud.ibm.com/docs/account?topic=account-open-case&interface=ui)

In the event that data is corrupted the data must be recreated. See [customer disaster planning](#iam-customer disaster-recovery) to restore the corrupted data.

## IBM disaster recovery
{: #iam-ibm-disaster-recovery}

### IBM recovery from global platform failure
{: #iam-ibm-recovery-from-global-platform-failure}

After a global platform failure IBM will attempt to restore the data from the last state in internal persistent storage.
- RTO - TODO
- RPO - TODO

It may not be possible for IBM to restore all state, and it will be required for the customer restore the database - see [customer disaster recovery](#iam-customer-disaster-recovery)

## IBM Service Maintenance
{: #iam-ibm-service-maintenance}

All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by [client availability retry logic](/docs/doesnotexist) within client applications. Changes are initially rolled out sequentially on a region-by-region basis.

Complex changes are enabled/disabled with feature flags to control exposure.

Changes that impact customer workloads will be described by notifications. See [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.
