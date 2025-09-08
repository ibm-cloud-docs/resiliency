---

copyright:
   years: 2020, 2025
lastupdated: "2025-09-08"

keywords: resilience testing, testing for resilience, dry test, switch over, resilience simulation

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Testing Workload Resilience on IBM Cloud
{: #testing-workload-resilience}

IBM Cloud operates on a shared responsibility model. Under this model, IBM Cloud is responsible for the resiliency of IBM Cloud, and IBM Cloud regularly conducts testing accordingly. Customers, meanwhile, are responsible for the resiliency of the workloads they deploy on IBM Cloud. This is an important distinction.

From a customer point-of-view, this means that workloads should be deployed in IBM Cloud in such a way that if an IBM Cloud component fails, the workload itself won't fail along with it. As with all resilience, this requires workload deployment that uses a resilient architecture, the building blocks of which IBM Cloud provides.

Of course, it's one thing to design and build resilience but you will never be completely sure that a workload is truely resilient until you test it aginst multiple failure scenarios.

## Failure Scenarios
{: #failure-scenarios}

To conduct testing, you need to define and document the different failure scenarios you are going to test. It's a good practice to consider failure scenarios as part of application design, allowing resilience to be built into the application architecture from day one. Whenever initially defining failure scenarios, try to be broad in your scope. You might consider workshopping a list with the business, end-users and technical team members represented. No scenario is a bad scenario but while broad is good, also consider the liklihood of a scenario coming to pass. For example, workshop particpants could decide that an alien spacecraft taking out a datacenter with a death ray is an issue, and it could cause considerable damage and a workload outage. However, the liklihood is pretty low, so you should be able to convice the group that there's little need to build a deflector sheild just yet.

While a frivolous example, it has another point. A defelctor sheild would cost a significant sum of money to build - millions if not billions of dollars to design, develop and test, let alone implement. Is this a realistic sum of money to expend to protect your workload from going off-line? In other words, when considering failure scenarios, also evaluate the cost of protection against the cost of workload failure and ensure the correct balance for your business is struck.

## Test Environments
{: test-environments}

Having documented failure scenarios, you need somewhere to test. A recommended approach is to always create a separate testing environment, using a separate testing account, on IBM Cloud. Using a separate account will ensure that testing is contained, does not accidentaly affect production and protects other environments from disruption. It also helps to separate the costs associated with testing, if that is a concern.

Plan out the environments that you need for resilience testing and document your environment strategy. The environment strategy will indicate which tests need which environment. There are several forms that suitable environments can take, based on cost and the number and types of tests planned. At one end of the scale, a full-sized, permanent mirror of production can be implemented.  At the other end of the scale, it's possible to use a deploy-on-demand environment, which is scaled dowm to contain just key components. Of course, there are also options in between. Ensure that whatever test environment you use, it will yeild meaningful results, based on the test(s) that you are conducting and parts of your workload you anticipate it may affect (the blast radius).

Test environment architecture must be reviewed regularly to prevent configuration drift away from production, to ensure results remain meaningful. Ideally, this will be automated using CI/CD pipelines.

## How to test
{: how-to-test}

Working out how to test resilience is probably the hardest part of the jigsaw. Also known as 'fault injection' each test scenario will need to have steps documented which detail how to inject faults and how to correct them (if needed).

Many faults can be injected into the environment using configuration changes. For example, changes to access control lists or security group configuration may be used to simulate connectivity issues for VPC. It's also possible to change access to components through IAM, which may also simulate outages. Of course, IBM Cloud has APIs which can be used within code to automate many changes too. Naturally, deliberately causing faults within your environment is an anti-pattern, but by studying the behavior of IBM Cloud APIs, it is possible to use them to 'break' an environment, as well as properly configure them.

If using APIs, they can of course be scripted and then run as part of a pipeline, first 'breaking' and then 'fixing' the environment. Intermitted faults can be simulated by continual breaking and fixing. Such scripts should be kept under source control. Execution can be part of an automated testing pipeline, or manually run as needed.

For more information related to testing, and chaos engineering in particular, see [Chaos Testing](/docs/resiliency?topic=resiliency-chaos-testing) and [Running a zone failure chaos experiment](/docs/resiliency?topic=resiliency-tutorial-litmuschaos).
