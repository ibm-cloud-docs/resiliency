---

copyright:
  years: 2021, 2024
lastupdated: "2024-12-10"


keywords: chaos testing, resilient app, client testing
subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Chaos testing
{: #chaos-testing}

Now that you can design and deploy resilient applications, which can withstand both simple and complex failures, you must verify that your applications work as planned during a failure. Waiting for a failure in your production environment to test your preparedness might be a costly mistake, often requiring rushed solutions that you develop in real time. Failures are inevitable and can be the result of several issues, including security breaches, misconfigurations, or service disruptions. As your cloud workload footprint grows, the likelihood of encountering various types of failures increases.

One approach to address potential failures is chaos engineering. [Chaos engineering](https://www.ibm.com/topics/chaos-engineering) is the intentional and controlled injection of failures in pre-production and production environments to understand their impact and verify your preparedness. The process isn't random, where you turn off dependencies or instances or shutdown services. The process begins by identifying potential issues, forming a hypothesis about how the system might behave, and testing it through an experiment. Observe the results to determine whether your hypothesis holds, or if a different approach is needed.
{: shortdesc}

## Target environments
{: #chaos-target-environments}

Ideally, you conduct chaos experiments in a production environment, since you want the test to closely mimic the real-world issues that you are likely to face. However, by intentionally disrupting live environments, you might get into lengthy and complex recovery processes that affect customer experience, which are valid deterrents. Pre-production environments are a good starting point as you get familiar with the process. In pre-production, you can build confidence in your ability to run tests and handle issues that arise. You might take another approach, like testing during lean hours or against a canary deployment that runs alongside your production but does not handle production traffic.

Start small in a staging or testing environment that resembles the production deployment and target specific scenarios before you expand the scope of your test.

## Fault space
{: #chaos-fault-types}

Chaos experiments are meant to mirror real-world incidents as closely as possible instead of complex and unlikely scenarios. In any cloud environment, infrastructure and instances of replicated components can fail. Dependencies can fail and connectivity to dependent services can also be interrupted intermittently or for longer durations. Replicating your past incidents is a good step toward identifying all failure scenarios.

Given the complexity of applications and their dependencies, the experiment scope can increase quickly if you consider all combinations of components and single or multiple concurrent failures. To address this complexity, tailor your experiments to reflect the specific characteristics of your application, for instance, only running cpu stress tests against cpu intensive components.

The process relies heavily on your knowledge of the system that you're testing and the cloud environment it's running on. You can also use profiling tools like [chaos-recommender](https://github.com/redhat-chaos/krkn/tree/main/utils/chaos_recommender) to augment your understanding.

You can use {{site.data.keyword.cloud_notm}} [public APIs](/docs?tab=api-docs) to perform update operations on infrastructure services, which help re-create failure scenarios. For containerized applications that run on [{{site.data.keyword.cloud_notm}} Kubernetes Service](/docs/containers) or on [Red Hat OpenShift on {{site.data.keyword.cloud_notm}}](/docs/openshift), there are many chaos engineering frameworks that help mimic failure scenarios, like [ChaosMesh](https://chaos-mesh.org), [LitmusChaos](https://litmuschaos.io), and Red Hat [KrKn](https://krkn-chaos.github.io/krkn/). These frameworks run within the cluster and inject faults with a small blast radius, often within the scope of the namespace.


## Running within your CI/CD pipeline
{: #chaos-ci-cd}

Run chaos testing as part of a regular CI/CD workflow to catch change induced failures early on. Use tools like [{{site.data.keyword.cloud_notm}} DevSecOps](https://cloud.ibm.com/docs/devsecops) to start chaos engineering early in the process. For example, you might run the continuous integration (CI) pipeline after a development environment deployment if it closely resembles the production environment. Alternatively, if a development environment infrastructure differs greatly from the production environment, consider moving the chaos tests to the continuous deployment (CD) pipeline.

Chaos tools vary in how they integrate with pipelines. Litmus and ChaosMesh both require a management cluster to run the chaos test suites from. The pipeline can communicate with test suites through an API for Litmus or through GitHub workflows for ChaosMesh. Krkn can be installed on the management plane, but it might be less resource intensive to run the required images locally within the pipeline to keep a more lightweight profile.

## Running chaos tests on {{site.data.keyword.cloud_notm}}
{: #chaos-ibmcloud}

Learn how to set up one of the chaos engineering frameworks, LitmusChaos, and run scenarios on Red Hat OpenShift on {{site.data.keyword.cloud_notm}}. The example application is a stateless web application that is made up of multiple [microservices](#x8379238){: term}. As discussed in [Target environments](#chaos-target-environments), test in a testing or staging environment first.

### Architecture
{: #litmus-arch}

This architecture builds on the secure and compliant deployable reference architecture, [Red Hat OpenShift Container Platform on VPC landing zone](docs/deployable-reference-architectures?topic=deployable-reference-architectures-ocp-ra). The management cluster hosts the management plane components of LitmusChaos. The workload cluster hosts the application that is under test and the execution plane components, which inject faults locally as part of the chaos experiments.

![Exemplary architecture](images/chaos-roks-arch.drawio.svg "Chaos framework exemplary architecture that shows LitmusChaos components"){: caption="Chaos framework exemplary architecture showing LitmusChaos components" caption-side="bottom"}

Typically, an administrator sets up the clusters, configures the environment, creates application namespaces, and assigns access for development and testing teams to deploy into their respective namespaces. LitmusChaos deploys as just another application alongside the application that is under test. When the environment is configured correctly, non-administrator role can complete the remaining steps.

### Considerations
{: #considerations}

Network traffic rules
:   The deployments in both clusters pull images from public container registries like quay.io and docker.io. Hence, you must allow external access to the registries' secure HTTPS port. Also, the subscriber component that is deployed in the workload cluster fetches scheduled workflow runs from backend server and pushes logs and data back to it. Allow inter-VPC traffic over a secure HTTPS port in VPC security groups and access control lists.

Security
:   Most Kubernetes experiments use only the Kubernetes APIs within the namespace scope, experiment pods do not need privilege escalation. However, some like pod-network-corruption and node-kill or stressors require privileged mode as well as greater cluster level permissions and are not suitable for all environments. On {{site.data.keyword.cloud_notm}} Red Hat OpenShift, a cluster administrator controls what actions and access pods can perform by using [security context constraints](docs/openshift?topic=openshift-openshift_scc) by default. For now, use only [pod-delete](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-delete/) and [pod-network-partition](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-network-partition/) experiments, which are namespace scoped and do not require extra privileges.

:   By default, the `litmus-admin` service account is used by the experiments that has broader permissions, however as part of hardening, use a restricted service account that can run with the default `restricted-v2` security context constraint.

### Management plane setup
{: #mgmt-plane}

Review [ChaosCenter installation](https://docs.litmuschaos.io/docs/getting-started/installation/) to deploy Litmus ChaosCenter on the management cluster. If you use the basic setup, you can use {{site.data.keyword.IBM}}-provided [ingress](docs/containers?topic=containers-managed-ingress-about) to setup TLS termination and secure your endpoints. A MongoDB replica set is used as database and is installed by the helm chart. It has only a ClusterIP service with connectivity that is limited to within your cluster.

### Execution plane setup
{: #exec-plane}

Install the resources for the execution plane in the same namespace as your application on the workload cluster. Verify that the chaos operator is running, the component pods start, and that they are working correctly before you define the chaos environment from the ChaosCenter UI.

Verify that the Chaos Experiment resources are installed in the namespace or install them from the experiment-specific links. Instead of the preinstalled `litmus-admin` service account, create service accounts with only enough permissions to run the identified experiments. For instance, a [pod-delete](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-delete/#minimal-rbac-configuration-example-optional) service account does not require node level privileges.

### Test scenarios
{: #test-scenarios}

In this approach to chaos testing, you start with a hypothesis and craft a multi-step experiment to verify it. A workflow in LitmusChaos can have multiple serial and parallel steps. In the simplest case, it has a pre-setup, inject fault step and post-cleanup step. We tailor the injected fault step according to each scenario. You can toggle between visual and yaml editor to incorporate security hardening requirements in the `securityContext` section and switch the `chaosServiceAccount` from `litmus-admin` to one that you create for each experiment.

Hypothesis 1: Multiple replicas of a pod ensure resiliency from intermittent failures
:   To simulate a pod crash, target for deletion 50% of the pods of a specific microservice and continue deleting new pods every chaos interval (2s) during chaos. During this time, affected pods do not start to ready state. Resiliency probes in LitmusChaos allow you to verify your hypothesis and resiliency of the application by using a continuous `httpProbe` that performs an HTTP Get or Post against the application service URL and expects a 200 response code.

:   When the application has just 1 replica, the experiment completes but with a resiliency score of 0 as the application is not continuously available. Scaling up the application to 2 or more replicas helps ensure that the experiment succeeds, thus proving the hypothesis true.

Hypothesis 2: Multiple replicas need to be spread across availability zones to handle zone failures
:   In this experiment, you simulate a limited version of zone failure by making all the pods of an application that run in a specific availability zone unreachable. Use the pod-network-partition fault to target specific pods that are scheduled on worker nodes in a single availability zone. The fault creates a network policy that denies both ingress and egress traffic for those pods.

:   Begin with five replicas of pods distributed across three worker nodes. Each node is located in a different availability zone. By applying [anti-affinity](docs/containers?topic=containers-app#affinity) rules, the pods are scheduled among multiple nodes. This placement helps ensure that requests are handled by pods that run in other availability zones. As a result, the experiment achieves a full resiliency score. However, when we apply affinity rules to ensure that all pods are on the same availability zone, the experiment fails the resiliency test.

### Next steps
{: #next-steps}

Gradually build on your initial experiments by varying parameters. For example, run tests under different load conditions, scenarios, fault types, and expand to other applications. Stateful applications might have other considerations and behave differently under graceful or forced shutdown scenarios. Observability dashboards that monitor resource and application metrics and automated health checks can verify system health after you run a chaos experiment.
