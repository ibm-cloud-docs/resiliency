---

copyright:
  years: 2024
lastupdated: "2024-11-08"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: repo-name

---

{{site.data.keyword.attribute-definition-list}}



# Understanding high availability and disaster recovery for _service-name_ 
{: #_service-name_-ha-dr}





[High availability](#x2284708){: term} (HA) is the ability for a service remain operational and accessible in the presence of unexpected failures.  [Disaster recovery](#x2113280){: term} process of recovering the service working state.

## High availability
{: #_service-name_-ha}

_Service-name_ is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **example standard** plan.

### High availability architecture
{: #_service-name_-ha-architecture}



Add short description... 

![Architecture](images/X-base.svg){: caption="Postgresql architecture" caption-side="bottom"}
{: style="text-align: center;"}

### High availability options
{: #_service-name_-ha-options}



| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| HA feature option 1 | Minor configuraion that changes the availability. | Consider xxx. |
| HA feature option 2| A configuration that effects availability that requires additional discussion. | Consider xxx. |
{: caption="High availability options" caption-side="bottom"}

To do: Add optional details describing how the options and features work.

#### Optional feature 1 
{: #_service-name_-feature-1}

To do: Add description

## Disaster recovery
{: #_service-name_-disaster-recovery}

To do: Deeper dive into disaster recovery.

![Architecture](images/X-restore.svg){: caption="X disaster recovery" caption-side="bottom"}
{: style="text-align: center;"}

## Disaster recovery options
{: #_service-name_-disaster-recovery-options}



_Service-name_ supports the following disaster recovery options:


| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| DR feature option 1 | Minor configuraion that changes the availability. | Consider xxx. |
| DR feature option 2| A configuration that effects availability that requires additional discussion. | Consider xxx. |
{: caption="Disater recovery options" caption-side="bottom"}

### How to use the options for business continuity
{: #_service-name_-options-for-business-continuity}

To do: Add description.

The disaster recovery steps must be practiced on a regular basis. When building your plan, consider the following failure scenarios and resolution.


| Failure | Resolution |
| -------------- | -------------- |
| Hardware failure (single point) | IBM provides a database that is resilient from single point of hardware failure within a zone - no configuration required. |
| Zone failure | Resolution for zone failure |
| Data failure | Resolution for data failure |
| Regional failure | Resolution for regional failure |
{: caption="Failure scenarios for DR" caption-side="bottom"}

### Features for Recovery Time Objective (RTO) and Recovery Point Objective (RPO)
{: #_service-name_-rto-rpo-features} 

Each feature covered above has a related RTO/RPO time as discussed below.



| Feature | RTO and RPO |
| -------------- | -------------- |
| HA Feature 2 | RTO = x minutes, RPO = y seconds (todo if HA populate from test results) |
| DR Feature 2 | RTO = todo 10 min + 1min/10GB, RPO = todo maybe time of last backup |
| DR Feature 3 | RTO = todo 10 min + 1min/10GB, RPO = todo maybe time of last backup |

### Feature check list
{: #_service-name_-feature-checklist} 

Disaster recovery steps must be practiced on a regular basis. The following check list can help you create and practice your plan. 



- [ ] HA feature 2
   - Verify 1
   - Verify 2

- [ ] DR Feature 1
   - Verify dependent service configured in recovery region
   - See [ backups available](/service/backuphistory)

- [ ] DR Feature 2
   - Verify 1
   - Verify 2

### Additional DR considerations
{: #_service-name_-additional-considerations} 



## IBM disaster recovery
{: #_service-name_-ibm-disaster-recovery}

### IBM recovery from zone failure 
{: #_service-name_-ibm-recovery-from-zone-failure}



### IBM recovery from regional failure
{: #_service-name_-recovery-from-regional-failure}

To do: After a regional has failed and has then been restored IBM will attempt to restore the service instance in the same region with the same connection strings from the last state in internal persistent storage.

- RTO - todo
- RPO - todo

In the event that IBM can not restore the database, the customer must restore the database using one of the disaster recovery features described above.

## IBM service maintenance
{: #_service-name_-X-ibm-service-maintenance}

To do: All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by [client availability retry logic](/docs/doesnotexist) within client applications. Changes are initially rolled out sequentially on a region-by-region basis.

Complex changes are enabled/disabled with feature flags to control exposure.
- todo is this true?  Do a number of changes get dropped in from the X open source?

Changes that impact customer workloads will be described by notifications. For more information, see [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.
