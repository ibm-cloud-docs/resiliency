---

copyright:
  years: 2024, 2024
lastupdated: "2024-11-05"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: resiliency

---



{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.X}} - Understanding high availability and disaster recovery
## High Availability
{: #X-high-availability}

This service is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **todo Standard** plan.

### High Availability Architecture
{: #X-high-availability-architecture}

![Architecture](images/X-base.svg){: caption="Postgresql architecture" caption-side="bottom"}
{: style="text-align: center;"}

todo description

### High Availability Options
{: #X-high-availability-options}



High availability features
Feature | Description | Consideration
--|--|--
option-1 | todo minor configuraion that changes availability | watch out for todo
**feature-1** | configuration that effects availability that requires additional discussion | watch out for todo

todo Optional details describing how the options and features work.


#### Optional feature-1
{: #X-feature-1}

todo description


## Disaster recovery
{: #X-disaster-recovery}

todo general info

![Architecture](images/X-restore.svg){: caption="X disaster recovery" caption-side="bottom"}
{: style="text-align: center;"}

## Disaster recovery options
{: #X-disaster-recovery-options}

The service supports the following disaster recovery options:

Feature | Description | Consideration
-|-|-
**Feature-2** | todo description of feature-1 | watch out
**Feature-3** | todo description of feature-2 | watch out

### How to use the options for business continuity
{:-#X-how-to-use-the-options-for-business-continuity)

Todo description

Disaster recovery steps must be practiced on a regular basis. When building your plan consider the following failure scenarios and resolution.

Failure | Resolution
-|-
Hardware failure (single point) | todo IBM provides a database that is resilient from single point of hardware failure within a zone - no configuration required.
Zone failure | **feature-1**. todo feature-1 yada yada
Data corruption | **feature-2**. todo feature-2 yada yada
Regional failure | **feature-3** todo feature-3 yada yada

### Feature RTO/RPO

Each feature covered above has a related RTO/RPO time as discussed below.

Feature | RTO/RPO
-|-
**feature-1** | RTO = x minutes, RPO = y seconds (todo if HA populate from test results)
**feature-2** | RTO = todo 10 min + 1min/10GB, RPO = todo maybe time of last backup
**feature-3** | RTO = todo 10 min + 1min/10GB, RPO = todo maybe time of last backup

### Feature check list

Disaster recovery steps must be practiced on a regular basis. The following check list can help you create and practice your plan.

**feature-1** - todo simple feature see: [Todo how to turn on feature](/docs/todo)

**feature-2** 
- todo See dependent service configured in recovery region
- todo See [ backups available](/service/backuphistory)

**feature-3** 
- todo step 1 
- todo step 2 
- todo step 3 

### Additional DR considerations

todo anything not covered above, mabye entire resource deleted. When a servicxe instance is deleted the associated backups are deleted as well. It is not possible to copy backups off the {{site.data.keyword.cloud_notm}} so consider using the database specific tools for additional backup. It may be required to recover from malicious deletion/reclamation of a database. Carefully manage the IAM for critical resources. It may be possible to restore the service instance [using resource reclamations](/docs/account?topic=account-resource-reclamation)

## IBM disaster recovery
{: #X-ibm-disaster-recovery}

### IBM recovery from zone failure 
{: #X-ibm-recovery-from-zone-failure}

todo What does IBM do when a zone goes down and comes back up?

### IBM recovery from regional failure
{: #X-ibm-recovery-from-regional-failure}

todo After a regional has failed and has then been restored IBM will attempt to restore the service instance in the same region with the same connection strings from the last state in internal persistent storage.
- RTO - todo
- RPO - todo

In the event that IBM can not restore the database, the customer must restore the database using one of the disaster recovery features described above.

## IBM Service Maintenance
{: #X-ibm-service-maintenance}

todo All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by [client availability retry logic](/docs/doesnotexist) within client applications. Changes are initially rolled out sequentially on a region-by-region basis.

Complex changes are enabled/disabled with feature flags to control exposure.
- todo is this true?  Do a number of changes get dropped in from the X open source?

Changes that impact customer workloads will be described by notifications. See [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.
