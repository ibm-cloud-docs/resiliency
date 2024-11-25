---

copyright:
  years: 2024
lastupdated: "2024-11-25"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, recovery time objective, recovery point objective, postgresql

subcollection: content-kit

---

{{site.data.keyword.attribute-definition-list}}

# Client retry logic for highly available applications
{: #client-retry-logic-for-ha}

It is up to application clients written in programming languages to recover from transient network errors as well as transient errors introduced by a service's High Availability implementation. See [Services specific high availability and disaster recovery](/docs/resiliency?topic=resiliency-service-ha-dr). Retrying connections with exponential backups for a fixed amount of time is good programming practice.

The IBM SDK [automatic retry](https://github.com/IBM/ibm-cloud-sdk-common?tab=readme-ov-file#automatic-retries) describes the recommended retry mechanism and even implements it specifically for HTTP 429 and 503 errors. Note that configuration is required by the client to take advantage of the IBM SDK provided retry logic. The failures from other status codes need to be evaluated to determine how to recover. It can be appropriate to use an open source SDKs for accessing {{site.data.keyword.cloud_notm}} services. Carefully examine the specifications to determine if they are useful for your application.

Failed read and HTTP GET operations with retry friendly HTTP status codes can generally be retried using exponential back off with a fixed time period.

Failed write, HTTP PUT, POST, DELETE, ... operations are likely not recoverable using a simple retry mechanism, unless it is clear that the operation did not complete and client logic indicates a retry is appropriate. Idempotent operations can be retried.  However, the result of the failed state changing operation can be unknown and more advanced service specific recovery mechanism will be required.

Logging failures and accumulating metrics using [cloud-{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-getting-started) an aggregating into metrics with alert notifications will provide observability into failures.
