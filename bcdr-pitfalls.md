---

copyright:
  years: 2021
lastupdated: "2024-08-06"

keywords: DR for IBM Cloud, disaster recovery, common mistakes for disaster recovery, plan a disaster recovery strategy

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Avoiding common disaster recovery pitfalls
{: #common-pitfalls}

To ensure that your plans provide the results that you require in the event of a disaster, be aware of the following common disaster recovery (DR) pitfalls when planning your solution.
{: shortdesc}

## Plan and design for the “best conditions”
{: #best-conditions}

It is important that the solution and the testing methodology are planned and designed to mimic what the real conditions might be.

A DR plan is a set of policies, tools, and procedures that enable organizations to recover from and continue the operation of vital technology systems following a natural or human-induced disaster. The business continuity and disaster recovery (BCDR) infrastructure capacity should be able to manage the workloads of the production sites. Assuming that this condition will not occur might seem attractive from a cost perspective, but it can make the effects of a disaster much more severe when there is not enough capacity to manage normal business.

Avoid reusing dismissed production components or lower grade and quality assets to build the DR solution because it affects the reliability of the solution. The amount of time that this solution needs to manage production is not predictable, and this variable introduces more risks.


## Avoiding single-point-of-failure (SPOF)
{: #spof}

SPOF can be anywhere in a solution, not only on the technology side. Your solution might depend on people, vendors, providers, and other external dependencies. You must know and identify clearly in advance most of your SPOFs and have a plan to mitigate your dependencies. Be prepared to discover SPOFs during the first sessions of your [disaster recovery test](/docs/overview?topic=overview-dr-testing).

Among SPOFs, provider risk is a condition to consider in your DR plan. When you have both production and DR on the same provider, your risk condition is increased and must be carefully considered.

## Having only a plan A
{: #plan-a}

Risks can't be eliminated; they can be only mitigated. The more risk that you mitigate with a single solution, the more the solution would cost. Thus, any solutions have a residual risk, as budget for disaster recovery is normally a finite or limited number.

Having a plan B with a different [recovery time objective](#x3167918){: term} (RTO) might help to mitigate additional risks, like increased resiliency and not add too much cost to the budget.

Your plan B RTO might not be the one expected in plan A, meaning you will need more time to restart your operations, and thus you will have more economic impact by the emergency. But, restarting later might be far better than not restarting at all.

A possible plan B, in a two sites topology, might include a periodic backup that is secured off-site from the two sites and at a distance to be considered reasonably safe.

## Preparing a poor DR testing methodology
{: #poor-dr-testing}

An untested DR solution is like running blindfolded in the woods. There are high chances that you will encounter an obstacle that stops your running. Moreover, you discover that only during the most important run of your life.

If testing is essential to guarantee that you have a valid solution, then the conditions you are going to test are also important.

If you plan to perform a DR test by doing a planned close of operation on one site and a well-organized restart on the other, you are sure your DR tests works, but is this what will happen during an emergency?

You should design your tests to mimic the potential emergency conditions as closely as possible by simulating a so-called “rolling disaster condition”, where your IT service is impacted progressively by the emergency. This is the best way to test your solution and have a reasonable understanding if it is resilient and has the ability to resist stress conditions.

Testing is one of the most important activities in the disaster recovery process but often hard to complete in the proper manner on public cloud. Because of the standardized nature of the cloud, you can't take for granted the ability to run testing as you need when using on-demand resources, for example:

- The frequency and the moment for the test
- Test by application compared to test of the full environment
- Continue to mirror data to secondary site during the test

You should analyze in detail if the requirements for testing can be met in the selected cloud.
