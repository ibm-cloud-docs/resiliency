---

copyright:
   years: 2020, 2025
lastupdated: "2025-09-12"

keywords: resilience testing, testing for resilience, dry test, switch over, resilience simulation

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Testing workload resiliency
{: #testing-workload-resilience}

{{site.data.keyword.cloud_notm}} operates on a share responsibility model in which {{site.data.keyword.cloud_notm}} is responsible for the resiliency of the cloud and customers are responsible for the workloads that they deploy. {{site.data.keyword.cloud_notm}} regularly conducts testing to ensure the resiliency of the cloud. 
{: shortdesc}

From a customer point-of-view, workloads should be deployed in such a way that if an {{site.data.keyword.cloud_notm}} component fails, the workload itself won't fail along with it. As with all resiliency, this requires a resilient architecture - the building blocks of which {{site.data.keyword.cloud_notm}} provides. Of course, it's one thing to design and build resilience but you will never be completely sure that a workload is truely resilient until you test it aginst multiple failure scenarios.

## Failure Scenarios
{: #failure-scenarios}

To conduct testing, you need to define and document the different failure scenarios you are going to test. It's a good practice to consider failure scenarios as part of application design, allowing resiliency to be built into the application architecture from day one. When you initially define failure scenarios, try to be broad in your scope. It can help to create the list during a business workshop in conjunction with both technical team members and end-users. 

Although starting broad in your scope is good, be sure to also consider the likelihood of a scenario coming to pass. For example, workshop participants might decide that an alien spacecraft taking out a datacenter via space lazer to be an issue. While that could cause considerable damage or create an outage, the likelihood of something similar occuring is quite low. So, you should be able to convince the group that there is little need to build a shield just yet. While a frivolous example, it has another point - a deflector shield costs a significant sum of money to design, develop, and test. One important question to ask while designing your failure scenarios - Is this a realistic sum of money to spend to protect your workload from going off-line? Ensuring that you strike the correct balance for your business between the cost of protection and the cost of workload failure is important.


## Test environments
{: test-environments}

After you've documented your failure scenarios, the next step is to test. It is a best practice to always create a separate testing environment, in a separate test account, on {{site.data.keyword.cloud_notm}}. By conducting your testing in a separate account you can ensure that all testing is contained and does not accidentally affect production and protects other environments from disruption. It also helps to separate the costs associated with testing which can help keep line items separate in billing for your business.

To get started, make a plan for the number and type of environments that you need to resiliency testing and then document your strategy. There are several forms that a suitable environment might take based on cost and the number or type of tests planned. At one end of the scale, a full-sized, permanant mirror of production can be implemented. At the other end, you might use a deploy-on-demand environment that is scaled to contain just key components. Or, you might land somewhere in between. Whichever test environment that you use, be sure that it will yield meaningful results by considering the type of tests that you are conducting and the parts of your workload that it might affect. Then, be sure to review your testing architecture and strategy reguarly to prevent configuration drift away from production. Ideally, this can be automated by using CI/CD pipelines.


## How to test
{: how-to-test}

Determining how to test resiliency can be one of the hardest parts of the puzzle. Also known as "fault injection" each test scenario needs to have documented steps taht detail how to introduce failures and how to correct them if needed. 

One way in which faults can be injected into an environment is through configuration change. For example, a change to an access control list or security group configuration might simulate connectivity issues for Virtual Private Cloud. You might also simulate an outage by changing access to components through IAM. Of course, {{site.data.keyword.cloud_notm}} has APIs that can be used within code to automate manage changes and can be used to deliberately cause faults within your environment. While it is definitely an anti-pattern, if you understand the intended behavior of the APIs, it is possible to use them to "break" an environment in addition to properly configuring it. If you're using APIs, they can be scripted and then run as part of a pipeline, either automatically or manually - first "breaking" and then "fixing" the environment. Intermitted faults can be simulated by continual breaking and fixing. Such scripts should be kept in a source controlled environment. 


For more information related to testing, and chaos engineering in particular, see [Chaos Testing](/docs/resiliency?topic=resiliency-chaos-testing) and [Running a zone failure chaos experiment](/docs/resiliency?topic=resiliency-tutorial-litmuschaos).
