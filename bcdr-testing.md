---

copyright:
  years: 2021, 2024
lastupdated: "2024-12-03"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Disaster recovery testing
{: #dr-testing}

Once you have a disaster recovery plan (see [Planning for disaster recovery](/docs/resiliency?topic=resiliency-PlanningforDR)), you don't want to find out whether it works or not when facing an actual disaster, so it is good practice to regularly test the plan. Testing will ensure that the plan works with the desired result and if it doesn't, changes can be made accordingly. Regular testing helps ensure that any changes in the workload environment are captured and adjustments made if necessary.

There are different types of disaster recovery testing that you can do to verify your plan:

- DR dry test
- DR simulation
- Switch-over

## Disaster recovery dry test
{: #dr-dry-test}

A DR dry test is performed by checking all of the resource availability and runbooks on paper, without running a real DR simulation or switch-over.

This type of testing is normally run with higher frequency compared to the other testing flavors as no real activities are performed, but it does require the same effort in terms of skills and people.


## Disaster recovery simulation
{: #dr-simulation}

DR simulation is a way to verify or audit the emergency runbooks and check the [recovery time objectives](#x3167918){: term} (RTO) and [recovery point objectives](#x3429911){: term} (RPO) provided by the solution by simulating as much as possible in the same conditions as a real emergency.

This needs to be carefully planned as it potentially means introducing disruptions to the replication of data from the primary region, hence simulating its sudden loss, while of course not providing impact on production workloads. There may be some additional risk to the business at this point too, since the disaster recovery provision may effectively be disconnected and in use for the druation of the test, though the extent of this is dependent on the cloud services used and how they are deployed.

DR simulation deploys a duplicate of your production environment at your chosen DR region that you can use to perform validation and checking. This environment is cleaned or perhaps undeployed at the end of the simulation and updates that happened to the DR test environment are discarded, as the real production has continued on the primary region.

## Switch-over
{: #switch-over}

In some ways, the switch-over test isn't really a test at all, since it involves switching production running from one region to another to verify and audit the ability to run and sustain production operations for a long period in an alternate region. Here, production operations are gracefully brought to a stop in the first region, switched to the second region, and restarted, after any data restoration that might be required.

After a period of verification, which ensure that the second region is working as expected, production activities can be resumed and data replication is configured so that the original region effectively becomes the new secondary region.

Production continues to run from this site until you decide to switch back again.

## Disaster recovery test frequency
{: #dr-test-frequency}

The frequency at which you test your DR plan depends on many factors, including what may be mandated by regulatory compliance standards. In the absence of needing to meet regulatory compliance standards,  plan a full DR test at least once per year and document results that can be accepted by auditors to prove your ability to recover. It is also good practice to conduct smaller scale tests throughout the year.

Always consider the following question and adjust your test frequency accordingly:

How dynamic is my workload?
:   The more your workload changes, the more often you need to carry out some form of DR test to verify that the changes introduced don't affect your ability to recover it. This can include new dependencies, additional cloud services, infrastructure changes and so forth. Remember that growing data sets will take longer to recover too, and that may affect your ability to meet a specific recovery time objective.

## What else should my tests focus on?
{: #test-focus}

The primary objective of any disaster recovery test is to ensure that following it means that workloads are successfully recovered. However, ensure that the following work well too:

* Key personnel - the DR plan should outline the personnel that are needed for a succcessful recovery and what their roles are. Consider whether addtional people / roles were needed during tests (or whether some were surplus to requirements) as well as how well people were able to perform their role.
* Communication - the DR plan must be clear on how communication works in the event of a disaster, so consider how well communication between participants, including the comminication channels used, worked during tests.
* Documented dependencies - the plan will likely outline dependencies. Check that these are valid and don't unnecessarily hinder the recovery process. At the same time, ensure that any new dependencies are recorded.
* Other documentation - runbooks may be used to implement the recovery, so it is important to understand how accuruate and how effective they were. Under-documenting steps can lead to delays, while over-documenting (providing too much detail or detail that is not relevant) may have the same effect. If possible, have people that did not write the steps implement the steps in at least one test to ensure they are clear and can be followed, just in case the author isn't available when disaster strikes.

## Post test
{: #post-test}

Once any test is completed, it's important to record the results as a benchmark for the next test. If changes are later made to the test procedure, it is then easy to compare results.

 Following a test, also ensure that any adjustments that are needed to the plan or related documentation are made. Remember that the disaster recovery plan is a living document that should be kept up-to-date to ensure that it can be executed with the desired outcomes. This may involve making small tweaks or large changes, depending on how testing went. All those that participated in a test should be encouraged to provide feedback on what went well, and what didn't go well and to document in an appropriate place anything they had to do differently, which can be incorporated into the next test. Think too about any additional training people may need, be that in understanding the plan and their role in it, through to communication or technical skills.
