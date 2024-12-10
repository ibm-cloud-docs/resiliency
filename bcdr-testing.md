---

copyright:
  years: 2021, 2024
lastupdated: "2024-12-10"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Disaster recovery testing
{: #dr-testing}

Once you have a [disaster recovery (DR) plan](/docs/resiliency?topic=resiliency-PlanningforDR), regularly test the plan. You can avoid finding flaws when you face an actual disaster. Testing helps ensure that the plan works with the desired result. If the plan doesn't work during testing, you can make changes accordingly. Regular testing helps ensure that any changes in the workload environment are captured and adjustments made if necessary.

There are different types of disaster recovery testing that you can do to verify your plan:

- DR dry test
- DR simulation
- Switch-over

## DR dry test
{: #dr-dry-test}

A dry test is a paper-based practice run of your DR plan. When running a dry test, you don't actually perform any recovery at all, but you are ensuring that there are no obvious holes in your plan. For example, the dry test wil ensure that:

 - you have the right personnel involved
 - backups are present and available
 - communication channels between personnel are working
 - there are no missing steps in the DR runbooks
 - handoffs between teams work efficiently

A DR dry test requires the same effort in terms of skills and people as any other test. Since no actual recovery actions are performed, this type of testing is faster, so is normally run with higher frequency compared to the other testing types. You can choose whether to run through the plan in its entirety or to test individual parts / services within it.

## DR simulation
{: #dr-simulation}

DR simulation is a way to verify or audit the emergency runbooks and check the [recovery time objectives](#x3167918){: term} (RTO) and [recovery point objectives](#x3429911){: term} (RPO) provided by the solution by simulating the conditions of a real emergency and restoring data.

A DR simulation needs careful planning since you are introducing potential disruptions to data replication from the primary region, while of course avoiding impact on production workloads. While you are testing the DR environment, it might temporarily be unavailable for actual disaster recovery purposes. This risk depends on the specific cloud services and how they are deployed. Some services might allow for simultaneous testing and availability, while others might not.

A DR simulation creates a temporary copy of your production environment in the designated DR region for testing and validation. After the simulation ends, the test environment is deleted or reset, and any changes made during the test are discarded, as the primary production environment continues to operate normally.

## Switch-over
{: #switch-over}

Switch-over involves switching your production environment from one region to another. This method helps verify and audit the ability to run and sustain production operations for a long period in an alternate region. Production operations are gracefully brought to a stop in the first region, switched to the second region, and restarted after any data restoration that might be required.

After you verify that the second region works as expected, you can resume production activities and configure data replication to make the original region the new secondary region. Your production environment continues to run from this site until you decide to switch back again.

## DR testing frequency
{: #dr-test-frequency}

How often you test your DR plan depends on many factors, including what is mandated by regulatory compliance standards. If compliance isn’t a concern, aim to conduct a full DR test at least once a year and document the results for auditor review. It’s a good practice to run smaller-scale tests throughout the year to help ensure readiness.

Always consider the following question and adjust your test frequency accordingly:

How dynamic is my workload?
:   The more your workload changes, the more often you need to carry out some form of DR test. This way, you can verify that the changes don't affect your ability to recover. Changes might include new dependencies, additional cloud services, infrastructure changes and more. Growing data sets take longer to recover, which might affect your ability to meet a specific RTO.

You may also need to consider staff turnover in your testing frequency too. If the staff who are performing the recovery change, ensure that new members of the team understand how DR works and their role in the DR plan. If there are a number of new team members who are unsure or who have not been through DR testing of some kind, this adds risk to your recovery plan.

## What else should my tests focus on?
{: #test-focus}

The primary objective of any disaster recovery test is to confirm that workloads can be successfully recovered. However, ensure that the following work well too:

* Key personnel: The DR plan should outline the personnel that are needed for a succcessful recovery and what their roles are. Consider whether you need addtional people or roles during tests, or whether some were surplus to requirements, as well as how well people were able to perform their role.
* Communication: The DR plan must clearly outline how to communicate in the event of a disaster. Consider how well communication between participants, including the comminication channels used, work during tests.
* Documented dependencies: Your DR plan likely outlines dependencies. Check that these are valid and don't unnecessarily hinder the recovery process. At the same time, ensure that any new dependencies are recorded.
* Other documentation: Runbooks might be used to implement the recovery, so it is important to understand how accuruate and how effective they are. Under-documenting steps can lead to delays, while providing too much detail or detail that is not relevant might have the same effect. If possible, have people that did not write the steps test them in at least one test to make sure they are clear, just in case the author isn't available when a disaster occurs.

## After testing
{: #post-test}

After any test is complete, record the results as a benchmark for the next test. If you make changes to the test procedure later, it is then easy to compare results.

After each disaster recovery test, update the plan and related documentation based on the results. A DR plan is a living document that needs regular adjustments to remain effective. Use feedback from participants to identify what worked well and what didn’t, and incorporate those insights into future tests. Also, consider providing additional training if needed, whether it’s to clarify roles, improve communication, or enhance technical skills.
