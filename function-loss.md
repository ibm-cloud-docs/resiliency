---

copyright:
   years: 2020, 2025
lastupdated: "2025-11-19"

keywords: DR, high availability, disaster recovery, help, get support, dr support, dr help, ha help, ha suppprt

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Loss of function in platform services
{: #loss-of-function}

{{site.data.keyword.cloud_notm}} platform core services are global in nature. This means that multiple instances of each microservice run in different locations. Requests to these services are made to a global endpoint, and then routed to one of the instances. If one instance requires maintenance or is experiencing issues, traffic is seamlessly routed to another instance without the caller being aware that there is an issue. The databases that are used by the global services are also globally replicated so that all instances have access to the same data.
{: shortdesc}

This architecture allows the global services to be highly resilient in most circumstances. Only if a condition affects all instances and causes them to be unavailable does an outage occur. If one instance of a global service is not responding, there is normally no impact to users. However, in some cases there might be a slight delay as the outage is detected and requests are rerouted to a healthy instance. If all regions of a global service are down, services and capabilities that rely on that global service will experience full or partial outages. This document details the precise loss of function that client workloads would experience in case of full outages of different global components.

## IAM Identity
{: #iam-function}

All Cloud logins via UI, CLI, and API do not work. Applications are unable to acquire or refresh tokens, so when their existing tokens expire, the applications are unable to call {{site.data.keyword.cloud_notm}} APIs. Existing unexpired tokens can continue to be used for API calls.

IBM services continue to work if the services’ internal tokens are not expired. When they expire, internal API calls fail, which causes outages of all {{site.data.keyword.cloud_notm}} services.

## IAM Policy Decision Point
{: #iam-decision-function}

Services are unable to perform authorization checks for API calls made to their service. Services typically cache authorization decisions for up to ten minutes, so if a decision is cached, API calls to the service that uses that cached decision continue to work. Some services implement a longer stale cache, so that if the authorization endpoint is not reachable cache entries that are older than 10 minutes continue to be used, but this is service-specific and can vary from one service to the next. Any access of a resource by an identity that has not recently accessed that resource is not present in the cache and fails if the Policy Decision Point cannot be reached.

## IAM Policy Administration Point
{: #iam-admin-fuction}

IAM policy cannot be managed or viewed; however, enforcement of the policies is not affected.

## Context-Based Restrictions (CBR)
{: #cbr-function}

If Context-Based Restrictions are configured in your account, API calls to services with active CBR rules fail.

## User Management
{: #user-mgmt-function}

If User Management is down, you are unable to add, remove, list, or otherwise manage the users in your account, and users are unable to authentication. Users with existing, unexpired tokens and other identities continue to be able to perform actions in the platform.

## Access Groups
{: #access-groups-function}

If your account contains IAM policies that use access groups, IAM authorization for these policies depends upon being able to query access group membership. There is a cache that can continue to be used if Access Groups are unavailable. So, Access Groups that have been queried in the past two minutes and are cached continue to work until Access Groups becomes responsive again. Retrieving any Access Groups that are not cached fails, and access is denied to identities that should have access based on their access group membership.

## Account Management
{: #account-mgmt-function}

Due to IAM’s dependency on Account Management, IAM Identity and Authorization is affected if Account Management is unavailable. Users are unable to authenticate, although existing workloads with unexpired tokens continue to be able to make API calls.

## Resource Controller
{: #resource-controller-function}

Users are unable to provision new resources, or to list or delete existing instances. Existing resources are not affected and continue to work.

## Metering and Billing
{: #billing-function}

Services are temporarily unable to report usage to the platform but report usage once service has been restored. Usage data is unavailable during the outage.

## Global Catalog
{: #catalog-function}

Users are unable to provision new resources, manage or view services and software-based offerings and their plans, view pricing, and manage or view regions.

## Global Search and Tagging
{: #search-tag-function}

Users are unable to manage tags on resources or perform searches of resources.
