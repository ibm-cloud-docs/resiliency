---

copyright:
  years: 2021, 2024
lastupdated: "2024-12-06"


keywords: chaos testing, resilient app, client testing
subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Chaos testing
{: #chaos-testing}

Now that you have designed and deployed resilient applications which can withstand simple and complex failures, you need to ensure that they work as planned. Waiting for a failure to occur in your production to verify your preparedness could prove to be costly mistake as solutions might be needed on-the-fly. Failures are inevitable and could be due to several issues, including security breaches, misconfigurations or service disruptions. As your workload footprint deployed on cloud increases, so does the chances that they will fail in multitude of ways. 

One approach to address this, is chaos engineering. [Chaos engineering](https://www.ibm.com/topics/chaos-engineering) is the intentional and controlled injection of failures in pre-production and production environments to understand their impact and verify your preparedness. It is not a random process where dependencies are turned off or instances or services shutdown. The process begins with identifying potential future issues and creating hypothesis about how your system will behave and verifying that by running an experiment and observing the results. Did your hypothesis hold true or do you need to handle them differently?

## Target environments
{: #chaos-target-environments}

Chaos experiments are ideally conducted in a production environment as they should closely mimic the real-world issues one is likely to face. However, intentionally disrupting live environments, getting into potentially lengthy and complex recovery processes and affecting customer experience are valid deterents. Pre-production environments are a good starting point as one gets familiar with the process and builds a level of confidence in running these tests and handling issues that arise. Other strategies could be to run these during lean hours or against a canary deployment. It is advisable to start small, in a staging or testing environment which resembles the production deployment, stay targetted to specific scenarios before expanding the scope.

## Fault space
{: #chaos-fault-types}

Chaos experiments should mirror real world incidents as closely as possible instead of complex and unlikely scenarios. In any cloud environment, infrastructure and instances of replicated components can fail. Dependencies can fail and connectivity to dependent services can also be interrupted intermittently or for longer durations. Replicating your past incidents and verifying them could be a good starting point towards identifying all failure scenarios.

Given the complexity of applications and its dependencies, the experiment space can explode quickly if one considers all combinations of components and single or multiple concurrent failures. It is important hence to also model the experiments based on application characteristics. This comes primarily with knowledge of the system under test and the cloud environment its running on, but can be augmented by profiling tools like [chaos-recommender](https://github.com/redhat-chaos/krkn/tree/main/utils/chaos_recommender).

{{site.data.keyword.cloud_notm}} [public APIs](/docs?tab=api-docs) allow actions (like start/stop virtual server instance) to be performed on many infrastructure services which can help recreate some of these scenarios. For containerized applications running on [{{site.data.keyword.cloud_notm}} Kubernetes Service](/docs/containers) or on [Red Hat OpenShift on {{site.data.keyword.cloud_notm}}](/docs/openshift), there are many chaos engineering frameworks like [ChaosMesh](https://chaos-mesh.org), [LitmusChaos](https://litmuschaos.io) and Red Hat [KrKn](https://krkn-chaos.github.io/krkn/) which run within the cluster and allow faults to be injected with a small blast radius, often within the scope of the namespace.


## Running within your CI/CD pipeline
{: #chaos-ci-cd}

It is recommended to run chaos testing as part of a regular CI/CD workflow. For tools such as [{{site.data.keyword.cloud_notm}} DevSecOps](https://cloud.ibm.com/docs/devsecops), chaos engineering could be undertaken early in the process - for example, running in the CI pipeline after a development environment deploy usually used for dynamic UI/API scans. Alternatively, if a development environment differs greatly from the expected production environment with regard to infrastructure, consider moving the chaos tests to the continuous deployment pipeline. Here, the application could be deployed into a pre-production or staging environment whose infrastructure ideally mirrors the production environment exactly.

Chaos tools differ in their implementation, with regard to pipelines. Litmus and ChaosMesh both require a management cluster to run the chaos test suites from, and the pipeline can communicate with these through an API (for Litmus) or through Github workflows (ChaosMesh). Krkn can be installed on the management plane, but it may be easier to run the required images locally within the pipeline to keep a more lightweight profile.

[//]: # "Regardless of which tool is chosen to perform chaos testing, it is recommended to try the chaos-recommender tool mentioned previously in order to determine which tests/experiments should be prioritized. "
[//]: # "If using Litmus, [litmusctl](https://docs.litmuschaos.io/docs/litmusctl/installation) should be used to automate the creation and running of chaos experiments."
[//]: # "Be wary of chaos experiments exiting early, as they could leave the target application in a bad state. For example, a network deny policy could remain in the event of the corresponding chaos experiment failing and not recovering the correct infrastructure configuration. "

## Running Chaos tests on IBM Cloud
{: #chaos-ibmcloud} 

In this section, we illustrate how to setup one such framework, LitmusChaos, and run a few scenarios on Red Hat OpenShift on {{site.data.keyword.cloud_notm}}. The application under test is a sample, stateless web application made up of multiple microservices. As discussed in [Target environments](#chaos-target-environments), it is recommended to attempt this in a testing or staging environment first.

### Architecture
{: #litmus-arch}
In this setup, we build upon the secure and compliant deployable reference architecture, [Red Hat OpenShift Container Platform on VPC landing zone](docs/deployable-reference-architectures?topic=deployable-reference-architectures-ocp-ra). The management cluster hosts the management plane components of LitmusChaos, while the workload cluster hosts the application under test and the execution plane components which injects faults locally as part of the chaos experiments. 

![Exemplary architecture](images/chaos-roks-arch.drawio.svg "Chaos framework exemplary architecture showing LitmusChaos components"){: caption="Chaos framework exemplary architecture showing LitmusChaos components" caption-side="bottom"}

Typically, an administrator would setup the clusters, configure the environment, create application namespaces and provide access for development and testing teams to deploy into their respective namespaces. LitmusChaos would deploy as just another application alongside the application under test. Once the environment is configured correctly, rest of the steps should be performed by a non-administrator role.

### Considerations
{: #considerations}

Network traffic rules:

The deployments in both clusters pull images from public container registries like quay.io and docker.io. Hence, external access to registries' secure https port needs to be allowed. Additionally, the subscriber component deployed in workload cluster fetches scheduled workflow runs from backend server and pushes logs and data back to it. Allow inter-vpc traffic over secure https port in VPC security groups and access control lists.

Security:

Most kubernetes experiments leverage only the kubernetes APIs within namespace scope and so does not need escalated privileges. However, some like pod-network-corruption and node-kill or stressers require privileged mode as well as greater cluster level permissions and are not suitable for all environments. On {{site.data.keyword.cloud_notm}} Red Hat OpenShift, a cluster administrator controls what actions and access pods can perform by using [security context constraints(SCC)](docs/openshift?topic=openshift-openshift_scc) by default. We will use only [pod-delete](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-delete/) and [pod-network-partition](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-network-partition/) experiments which are namespace scoped and do not require additional privileges. 

By default, the `litmus-admin` service account is used by the experiment which has broader permissions, however as part of hardening we will instead use a restricted service account which can run with the default `restricted-v2` SCC.

### Management plane setup
{: #mgmt-plane}

Review the [ChaosCenter installation](https://docs.litmuschaos.io/docs/getting-started/installation/) steps listed at product site to deploy Litmus ChaosCenter on the management cluster. If using the basic setup, you can use IBM-provided [ingress](docs/containers?topic=containers-managed-ingress-about) to setup TLS termination and secure your endpoints. A mongodb replicaset is used as database and is installed by the helm chart. It should only have a ClusterIP service with connectivity limited to within your cluster. 

### Execution plane setup
{: #exec-plane}

The resources for the execution plane should be installed in the same namespace as your application in the workload cluster. Verify that the chaos operator is running and the component pods startup and are healthy, before defining the chaos environment from the ChaosCenter UI.

Verify that the Chaos Experiment resources are installed in the namespace or install them from the experiment specific links. In lieu of the pre-installed `litmus-admin` service account, create service accounts with just enough permissions to run the identified experiments. For instance, [pod-delete](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-delete/#minimal-rbac-configuration-example-optional) service account does not require node level priviliges.

### Test scenarios
{: #test-scenarios}

In this approach to chaos testing, we start with a hypothesis and craft a multi-step experiment to verify it. A workflow in LitmusChaos can have multiple serial and parallel steps. In the simplest case it has a pre-setup, inject fault step and post-clean step. We tailor the inject fault step as per each scenario. You can toggle between visual and yaml editor to incorporate security hardening requirements into `securityContext` section and switch the `chaosServiceAccount` from `litmus-admin` to one created for each experiment.

Hypothesis 1: Multiple replicas of a pod ensure resiliency from intermittent failures.
:   To simulate a pod crash, we target 50% of the pods of a specific microservice for deletion and continue deleting new pods every chaos interval (2s) for the duration of chaos. During this time, affected pods do not start up to ready state. Resiliency probes in LitmusChaos allow us to verify our hypothesis and resiliency of the application, we use a continuous `httpProbe` that performs a HTTP Get or Post against the application service URL and expects a 200 response code. 

:   When the application has just 1 replica, the experiment completes but with a resiliency score of 0 as the application is not continuously available. Scaling up the deployment to 2 or more replicas ensures that the experiment succeeds, thus proving our hypothesis.

Hypothesis 2: Multiple replicas need to be spread across availability zones for zone failures.
:   In this experiment, we simulate a limited version of zone failure by making all the pods of an application running in a specific availability zone as unreachable. We leverage the pod-network-partition fault targetting specific pods scheduled on worker nodes in a single availability zone. The fault creates a network policy which denies both ingress and egress traffic from those pods. 

:   We begin with five replicas of pods distributed across three worker nodes. Each node is located in a different availability zone. By applying [anti-affinity](docs/containers?topic=containers-app#affinity) rules, the pods are scheduled among multiple nodes. This ensures that requests are handled by pods running in other availability zones. As a result, the experiment achieves a full resiliency score. However, when we apply affinity rules to ensure all pods are located on the same availability zone, the experiment fails resiliency test.

### Next steps
{: #next-steps}

It is recommended to gradually build upon your initial experiments by varying parameters, for instance, running them under different load conditions, different scenarios and fault types and expanding to other applications. Stateful applications might have additional considerations and behave differently under graceful or force shutdown scenarios. Observability dashboards which monitor resource and application metrics and automated health checks routines can verify system health post chaos experiment runs.
