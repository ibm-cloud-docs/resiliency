# Service Example Template

Topics to cover:
Customer
- Single zone failure - restore to recovery zone
- Regional failure - restore to recovery region
- Service/instance failure - restore to new instance, perhaps in a recovery region
- Data corruption - restore to new instance, RTO to point in time, possibly to recovery region
- RTO/RPO possible options and how they feather into [active/nothing, active/active, active/standby, active/active](https://test.cloud.ibm.com/docs/resiliency?topic=resiliency-understanding-dr#dr-categories)
- Reference to client retry logic
- Reference to SLA verify it is correct
- Reference to SLO verify it is correct
- Configure for HA to meet SLA
- Configure for DR options

IBM
- IBM recovery from zone failure
- IBM recovery from region failure, restore from backups

Questions:
- Provisioned by resource controller?  Seems interesting but is it getting into the entire dependency tree?
- 

## High Availability
A very simple description of the underlying architecture that explains HA. It may fall into a few types:

1. Modern architecture built in the cloud distributed evenly across 3 zones.  See Secrets Manager. Zonal failure will be covered here

## Customer disaster recovery
Things to cover in the sections below
- Customer recover from zonal failure 
- Customer recover from regional failure
- Customer recover from service failure 
- Customer recover from data corruption
- Customer recovery from BYOK loss
### Customer disaster definition
Data corruption explanation


## IBM disaster recovery
### IBM recovery from zone failure 
### IBM recovery from regional failure
