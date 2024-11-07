---

copyright:
  years: 2024
lastupdated: "2024-11-07"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective

subcollection: repo-name

---

{{site.data.keyword.attribute-definition-list}}



# Understanding high availability and disaster recovery for _service-name_ 





[High availability](#x2284708){: term} (HA) is a core discipline in an IT infrastructure to keep your apps up and running, even after a partial or full site failure. The main purpose of high availability is to eliminate potential points of failures in an IT infrastructure. [Disaster recovery](#x2113280){: term} involves a set of policies, tools, and procedures for returning a system, an application, or an entire data center to full operation after a catastrophic interruption. It includes procedures for copying and storing an installed system's essential data in a secure location, and for recovering that data to restore normalcy of operation. Together, the two help ensure resiliency for your service.

## High availability
{: #ha-service-name}

_Service-name_ is a regional service that fulfills the defined [Service Level Objectives](/docs/resiliency?topic=resiliency-slo) with the **example standard** plan.

### High availability architecture
{: #ha-architecture}



Add short description... 

![Architecture](images/X-base.svg){: caption="Postgresql architecture" caption-side="bottom"}
{: style="text-align: center;"}

### High availability options
{: #ha-options}



| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| Example feature option 1 | Minor configuraion that changes the availability. | Consider xxx. |
| Example feature option 2| A configuration that effects availability that requires additional discussion. | Consider xxx. |
{: caption="High availability options" caption-side="bottom"}

To do: Add optional details describing how the options and features work.

#### Optional feature 1 
{: #service-name-feature-1}

To do: Add description

## Disaster recovery
{: #service-name-disaster-recovery}

To do: Deeper dive into disaster recovery.

![Architecture](images/X-restore.svg){: caption="X disaster recovery" caption-side="bottom"}
{: style="text-align: center;"}

## Disaster recovery options
{: #X-disaster-recovery-options}



_Service-name_ supports the following disaster recovery options:


| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| Example feature option 1 | Minor configuraion that changes the availability. | Consider xxx. |
| Example feature option 2| A configuration that effects availability that requires additional discussion. | Consider xxx. |
{: caption="Disater recovery options" caption-side="bottom"}

### How to use the options for business continuity
{: #options-for-business-continuity)

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
{: #rto-rpo-features} 

Each feature covered above has a related RTO/RPO time as discussed below.

| Feature | RTO and RPO |
| -------------- | -------------- |
| Feature 1 | RTO = x minutes, RPO = y seconds (todo if HA populate from test results) |
| Feature 2 | RTO = todo 10 min + 1min/10GB, RPO = todo maybe time of last backup |
| Feature 3 | RTO = todo 10 min + 1min/10GB, RPO = todo maybe time of last backup |

### Feature check list
{: #feature-checklist} 

Disaster recovery steps must be practiced on a regular basis. The following check list can help you create and practice your plan.

- [ ] Enabling feature 1
   - Step 1 
   - Step 2 

- [ ] Configuring Feature 2 
   - todo See dependent service configured in recovery region
   - todo See [ backups available](/service/backuphistory)

- [ ] Managing Feature 3 
    - todo step 1 
    - todo step 2 
    - todo step 3 

### Additional DR considerations
{: #additional-considerations} 

To do: anything not covered above, mabye entire resource deleted. When a servicxe instance is deleted the associated backups are deleted as well. It is not possible to copy backups off the {{site.data.keyword.cloud_notm}} so consider using the database specific tools for additional backup. It may be required to recover from malicious deletion/reclamation of a database. Carefully manage the IAM for critical resources. It may be possible to restore the service instance [using resource reclamations](/docs/account?topic=account-resource-reclamation)

## IBM disaster recovery
{: #ibm-disaster-recovery}

### IBM recovery from zone failure 
{: #ibm-recovery-from-zone-failure}


To do: What does IBM do when a zone goes down and comes back up?

### IBM recovery from regional failure
{: #recovery-from-regional-failure}

To do: After a regional has failed and has then been restored IBM will attempt to restore the service instance in the same region with the same connection strings from the last state in internal persistent storage.

- RTO - todo
- RPO - todo

In the event that IBM can not restore the database, the customer must restore the database using one of the disaster recovery features described above.

## IBM service maintenance
{: #X-ibm-service-maintenance}

To do: All upgrades follow the IBM service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that will be handled by [client availability retry logic](/docs/doesnotexist) within client applications. Changes are initially rolled out sequentially on a region-by-region basis.

Complex changes are enabled/disabled with feature flags to control exposure.
- todo is this true?  Do a number of changes get dropped in from the X open source?

Changes that impact customer workloads will be described by notifications. For more information, see [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.
