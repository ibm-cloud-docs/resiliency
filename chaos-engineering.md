---

copyright:
  years: 2021, 2024
lastupdated: "2024-11-25"


keywords: chaos testing, resilient app, client testing
subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Chaos testing
{: #chaos-testing}

Now that you have designed and deployed resilient applications which can withstand simple and complex failures, you need to ensure that they work as planned. Waiting for a failure to occur in your production to verify your preparedness could prove to be costly mistake as solutions might be needed on-the-fly. Failures are inevitable and could be due to several issues, including security breaches, misconfigurations or service disruptions. As your workload footprint deployed on cloud increases, so does the chances that they will fail in multitude of ways. 

One approach to address this, is chaos engineering. Chaos engineering is the intentional and controlled injection of failures in production and pre-production environments to understand their impact and verify your preparedness. It is not a random process where dependencies are turned off or instances or services shutdown. The process begins with identifying potential future issues and creating hypothesis about how your system will behave and verifying that by running an experiment and observing the results. Did your hypothesis hold true or do you need to handle them differently?

## Where should you run chaos tests?
{: #chaos-target-environments}

Chaos experiments are ideally conducted in a production environment, yet the risks associated with intentionally disrupting live operations—and potentially impacting customer experience—make this approach challenging to many. While pre-production environment might mimic production, the differences in configuration and load characteristics from production make running chaos tests and observing its impact on live production environment more beneficial. Hence, pre-production environments are a good starting point as one gets familiar with the process and builds a level of confidence in running these tests and handling issues that arise. 

## Fault space
{: #chaos-fault-types}

Chaos experiments should mirror real world incidents as closely as possible instead of complex and unlikely scenarios. In any cloud environment, infrastructure can fail and instances of replicated components can fail. Dependencies can fail and connectivity to dependent services can also be interrupted intermittently or for longer durations.

{{site.data.keyword.cloud_notm}} [public APIs](/docs?tab=api-docs) allow actions (like start/stop instance) to be performed on few infrastructure services which can help recreate some of these scenarios. For containerized applications running on [{{site.data.keyword.cloud_notm}} Kubernetes Service](/docs/containers) or on [Red Hat OpenShift on {{site.data.keyword.cloud_notm}}](/docs/openshift), there are many chaos engineering frameworks like [ChaosMesh](https://chaos-mesh.org), [LitmusChaos](https://litmuschaos.io) and Red Hat [KrKn](https://krkn-chaos.github.io/krkn/) which run within the cluster and allow faults like pod deletion, container kills to be injected with a small blast radius.

Given the complexity of applications and its dependencies, the experiment space can explode quickly if one considers all combinations of components and single or multiple concurrent failures. It is important hence to model the experiments based on application characteristics. This comes primarily with knowledge of the system under test and the cloud environment its running it, but can be augmented by profiling tools like [chaos-recommender](https://github.com/redhat-chaos/krkn/tree/main/utils/chaos_recommender)


## Observability
{: #chaos-observability}


## Running within your CI/CD pipeline
{: #chaos-ci-cd}

It is recommended to run chaos testing as part of a regular CI/CD workflow. For tools such as [IBM Cloud DevSecOps](https://cloud.ibm.com/docs/devsecops), chaos engineering could be undertaken early in the process - for example, running in the CI pipeline after a development environment deploy usually used for dynamic UI/API scans. Alternatively, if a development environment differs greatly from the expected production environment with regard to infrastructure, consider moving the chaos tests to the continuous deployment pipeline. Here, the application could be deployed into a pre-production or staging environment whose infrastructure ideally mirrors the production environment exactly.

Chaos tools differ in their implementation, with regard to pipelines. Litmus and ChaosMesh both require a management cluster to run the chaos test suites from, and the pipeline can communicate with these through an API (for Litmus) or through Github workflows (ChaosMesh). Krkn can be installed on the management plane, but it may be easier to run the required images locally within the pipeline to keep a more lightweight profile.

Regardless of which tool is chosen to perform chaos testing, it is recommended to try the chaos-recommender tool mentioned previously in order to determine which tests/experiments should be prioritized. 

If using Litmus, [litmusctl](https://docs.litmuschaos.io/docs/litmusctl/installation) should be used to automate the creation and running of chaos experiments.

Be wary of chaos experiments exiting early, as they could leave the target application in a bad state. For example, a network deny policy could remain in the event of the corresponding chaos experiment failing and not recovering the correct infrastructure configuration. 


Recoverability from testing
replicas of mubench on every zone
permissions
