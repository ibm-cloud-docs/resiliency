---

copyright:
  years: 2021, 2024
lastupdated: "2024-12-11"

keywords: DR testing, disaster recovery test, testing for a disaster scenario, dry test, switch over, DR simulation

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Disaster recovery testing
{: #dr-testing}

After you have a [disaster recovery (DR) plan](/docs/resiliency?topic=resiliency-PlanningforDR), regularly test the plan. You can avoid finding flaws when you face an actual disaster. Testing helps ensure that the plan works and achieves the result that you want. If the plan doesn't work during testing, you can make the necessary changes. Regular testing helps ensure that any changes in the workload environment are captured and adjustments are made if necessary.

To verify your plan, use different types of disaster recovery testing:

- DR dry test
- DR simulation
- Switch-over

## DR dry test
{: #dr-dry-test}

A dry test is a paper-based practice run of your DR plan. When you run a dry test, you don't perform any recovery, but you are checking that there are no obvious holes in your plan. For example, the dry test helps ensure that:

 - you have the right personnel involved
 - backups are present and available
 - communication channels between personnel are working
 - there are no missing steps in the DR runbooks
 - handoffs between teams work efficiently

A DR dry test requires the same effort in terms of skills and people as any other test. Since no actual recovery actions are performed, this type of testing is faster, so is normally run with higher frequency compared to the other testing types. You can choose whether to run through the plan in its entirety or to test individual parts and services within it.

## DR simulation
{: #dr-simulation}

DR simulation is a way to verify or audit the emergency runbooks and check the [recovery time objectives](#x3167918){: term} (RTO) and [recovery point objectives](#x3429911){: term} (RPO) provided by the solution by simulating the conditions of a real emergency and restoring data.

A DR simulation needs careful planning since you are introducing potential disruptions to data replication from the primary region, while avoiding impact on production workloads. While you are testing the DR environment, it might temporarily be unavailable for actual disaster recovery purposes. This risk depends on the specific cloud services and how they are deployed. Some services might allow for simultaneous testing and availability, while others might not.

A DR simulation creates a temporary copy of your production environment in the designated DR region for testing and validation. After the simulation ends, the test environment is deleted or reset, and any changes that are made during the test are discarded, as the primary production environment continues to operate normally.

## Switch-over
{: #switch-over}

Switch-over involves switching your production environment from one region to another. This method helps verify and audit the ability to run and sustain production operations for a long period in an alternative region. Production operations are gracefully brought to a stop in the first region, switched to the second region, and restarted after any data restoration that might be required.

After you verify that the second region works as expected, you can resume production activities and configure data replication to make the original region the new secondary region. Your production environment continues to run from this site until you decide to switch back again.

## DR testing frequency
{: #dr-test-frequency}

How often you test your DR plan depends on many factors, including what is mandated by regulatory compliance standards. If compliance isn’t a concern, aim to conduct a full DR test at least once a year and document the results for auditor review. It’s a good practice to run smaller-scale tests throughout the year to help ensure readiness.

Consider the following questions and adjust your test frequency:

How dynamic is my workload?
:   The more your workload changes, the more often you need to carry out some form of DR test. This way, you can verify that the changes don't affect your ability to recover. Changes might include new dependencies, other cloud services, infrastructure changes and more. Growing data sets take longer to recover, which might affect your ability to meet a specific RTO.

How dynamic is my staffing?
:   You might also consider staff turnover in your testing frequency. If the staff members that perform the recovery change, make sure that new members of the team understand how DR works and their role in the DR plan. If you have several new team members who are unsure or who aren't familiar with DR testing, you add risk to your recovery plan.

## What else should my tests focus on?
{: #test-focus}

The primary objective of any disaster recovery test is to confirm that you can successfully recover workloads. However, ensure that the following also work well:

* Key personnel: The DR plan should outline the personnel that are needed for a successful recovery and what their roles are. Consider whether you need more people or roles during tests, or whether some were surplus to requirements, and how well people were able to perform their role.
* Communication: The DR plan must clearly outline how to communicate is a disaster occurs. Consider how well communication between participants, including the communication channels used, worked during tests.
* Documented dependencies: Your DR plan likely outlines dependencies. Check that these are valid and don't hinder the recovery process. At the same time, make sure that any new dependencies are recorded.
* Other documentation: Runbooks might be used to implement the recovery, so it is important to understand how accurate and how effective they are. Under-documenting steps can lead to delays, while providing too much detail or detail that is not relevant might have the same effect. Have someone other than the author test the steps to help ensure that they’re clear. This way, the process is usable even if the author isn’t available during a disaster.

## After testing
{: #post-test}

After any test is complete, record the results as a benchmark for the next test. If you change the test procedure later, it is then easy to compare the results.

After each disaster recovery test, update the plan and related documentation based on the results. A DR plan is a living document that needs regular adjustments to remain effective. Use feedback from participants to identify what worked well and what didn’t, and incorporate those insights into future tests. Also, consider providing more training if needed, whether it’s to clarify roles, improve communication, or enhance technical skills.
