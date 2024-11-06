---

copyright:
  years: 2021, 2024
lastupdated: "[{2024-11-06}]"

keywords: disaster recovery, DR, what is disaster recovery, DR strategy, disaster recovery options, disaster recovery strategy

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Disaster recovery FAQ
{: #dr-testing}

This page discusses some common frequently asked questions about disaster recovery.

## Why is high availability not disaster recovery? Are both required?
{: #WhyIsHAnotDR}

High availability and disaster recovery are easily confused, but they are distinctly different. Designing high availability into a workload is an effort to prevent a workload from failing. For example, spreading data over multiple disks in a RAID array is an effort to prevent an outage caused by disk failure. Multiple power supplies in a server are an effort to prevent a failure caused by an outage in a power circuit, while running applications across multiple servers in different zones is an effort to prevent an outage caused by a server failure and zone failure.

Disaster recovery is the practice of recovering a workload after it has failed, despite the availability of resources at its disposal. For example, a workload might be highly available and resilient to failure because it runs across multiple hardware devices in multiple data centers. This might afford the system 99.999% availability but it’s still susceptible to disasters such as accidental or malicious data erasure, or a natural disaster that takes out a wide area that includes all the data centers.

Usually, the more highly available a workload needs to be, the more valuable and vital it is to the business. Since it’s vital to the business, the more likely the need for it to have a well-documented, stringent disaster recovery plan.

## What counts as a disaster?
{: #WhatCountsasaDisaster}

An IT disaster is an event that causes serious disruption to the functioning of an application or environment that goes beyond its ability to survive. These can be short or long-term events but their effect is typically damaging to the business, either financially, reputationally, or both.

Such events will normally be the result of:

* Natural disasters such as floods, fires, earthquakes.
* Infrastructure problems such as power outages or broader network failures.
* Accidental or malicious actions that delete data, services, or configuration.
* Rolling out a software update, which contains a bug or other error that was not anticipated.

In each event, it is important to have a planned recovery approach that provides for the restoration of service within a given time frame, to an agreed point in time before the disaster occurred. It’s probably also advisable to be aware of the costs involved, both in terms of the cost of downtime to the business and the cost of the solution put in place.

## Can DR take place in one region?
{: #DR-same-region}

The short answer to this is yes – but in practical terms. It comes down to how quickly recovery is needed for your circumstances. In practice, true DR should take place in a separate region.

{{site.data.keyword.cloud_notm}} will always seek to recover services within the region, in line with any published SLOs. This might take minutes or hours but in the worst-case scenarios, perhaps where there has been physical destruction of property, it can take days, weeks, or months to achieve.

In {{site.data.keyword.cloud_notm}}, if there is an outage where {{site.data.keyword.cloud_notm}} calls a disaster, {{site.data.keyword.cloud_notm}} might have to recover services before you can recover your workloads. If so, you need to add the time it takes for {{site.data.keyword.cloud_notm}} to complete its recovery to the time it takes to do yours. Bear this in mind when deciding whether to rely on just a single region.

## Do all disasters require recovery in a second region?
{: #dr-second-region}

Not all disasters require recovery at a second region. A database administrator might accidentally drop a database table, an automated release might go catastrophically wrong, or an essential managed cloud service can stop functioning correctly. In these cases, data recovery is key. Depending on how widespread the issue is, how prepared the organization is, and how quickly recovery needs to be achieved, such a disaster might not force failover to another region. Depending on the scenario, recovery can be achieved in the same region. For this reason, it's important to consider multiple disaster scenarios, and to plan accordingly.
