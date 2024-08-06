---

copyright:
  years: 2021, 2023
lastupdated: "2024-08-06"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Disaster recovery testing
{: #dr-testing}

Disaster recovery (DR) testing is what you must do to keep your ability to be resilient, hoping not to need to do it for real. There are different types of disaster recovery testing that you can do to verify your resiliency capabilities:

- DR dry test
- DR simulation
- Switch-over

## Disaster recovery dry test
{: #dr-dry-test}

A DR dry test is performed by checking all of the resource availability and runbooks on paper, without running a real DR simulation or switch-over.

This type of testing is normally run with higher frequency compared to the other testing flavors as no real activities are performed, but it does require the same effort in terms of skills and people.

The adoption of a recovery orchestrator software improves the whole dry testing process and reduces the time and effort to be run because most of the operations are performed by the software itself. This also increases the preparedness checking of a real DR simulation or switch-over by periodic scanning that all of the components of the DR solutions are in place and works as expected.

## Disaster recovery simulation
{: #dr-simulation}

DR simulation is a way to verify or audit the emergency runbooks and check the [recovery time objectives](#x3167918){: term} (RTO) and [recovery point objectives](#x3429911){: term} (RPO) provided by the solution by simulating as much as possible in the same conditions as a real emergency.

This means introducing disruptions on replication network connections before interrupting the communications among the sites, hence simulating the sudden loss of the primary site.

You can do this only if your data replication solution is resilient and not providing impact on the production, which continues on the primary site. If this is not the case, then look at the documentation for [plan and design for the worst conditions](/docs/overview?topic=overview-understanding-dr#worst-conditions).

DR simulation deploys a duplicate of your production environment on the DR site that you can use to perform validation and checking. This environment is cleaned at the end of the simulation and updates that happened to the DR test environment are discarded, as the real production has continued on the primary site.

It is important thus that network streams flowing to the DR test environment are copies of the real production environment. Usually these network flows are intercepted and duplicated at the primary site (that receives the real flow) and sent to the DR site, which operates on separated and different network subnets than production.

## Switch-over
{: #switch-over}

Opposite to the DR simulation, in the switch-over test, production is moved from primary site to DR site to verify and audit the ability to run and sustain production operations for a long period.

In this option, we don’t test DR runbooks, as this would imply the emergency restart, which has an impact on the production environment.

To avoid any impact to the production environment, you should pose all the possible attention in performing this switch, such to minimize all those impacts.

Production operations are closed orderly on the primary site and reopened from the DR site.

Data replication is reverted once everything in the DR site has been verified and before network streams are rerouted to the DR site.

Production runs in the DR site for whatever period you have chosen before returning to the primary site at your most convenient time. During this period, your production site is performing as the DR until the next switch-over.

Updates happened to the production running in the DR site is kept and replicated back to the primary site.

## Disaster recovery test frequency
{: #dr-test-frequency}

DR test frequency depends on many factors, but in general you should have a DR test at least once per year. This is the bare minimum frequency that is accepted by auditors to prove your ability to recover.

In setting the frequency though, you should increase this base minimum to a level depending on multiple elements specific of the cloud, such as:

How dynamic is your production environment
:   The more your production environment is dynamic, the more you need to exercise a DR test to verify that changes introduced didn’t affect your ability to recover.

How dynamic is your infrastructure
:   As an example, if you have chosen to provision some of the DR resources on demand, you should consider that you have no control on the type of resources or technologies that you will find at the time of a disaster. In fact, changes in the underlying technologies (for example, the servers), configurations (for example, network topologies), or service levels (for example, time to provision new resources) from your last test. So, it is recommended to increase the testing frequency in such a case, especially if you believe that your applications might be sensitive to the underlying infrastructure technology.
