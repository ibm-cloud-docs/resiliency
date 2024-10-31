---

copyright:
  years: 2021, 2024
lastupdated: "2024-10-31"


keywords: chaos testing, resilient app, client testing
subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# Chaos testing
{: #chaos-testing}

Now that you have designed and deployed resilient applications which can withstand simple and complex failures, you need to ensure that they work as planned. Waiting for a failure to occur in your production to verify your preparedness could prove to be costly mistake as solutions might be needed on-the-fly. Failures are inevitable and could be due to several issues, including security breaches, misconfigurations or service disruptions. As your workload footprint deployed on cloud increases, so does the chances that they will fail in multitude of ways. 

One approach to address this, is chaos engineering. Chaos engineering is the intentional and controlled injection of failures in production and pre-production environments to understand their impact and verify your preparedness. It is not a random process where dependencies are turned off or instances or services shutdown. The process begins with identifying potential future issues and creating hypothesis about how your system will behave and verifying that by running an experiment and observing the results. Did your hypothesis hold true or do you need to handle them differently?

## Where should you run chaos tests?
{: #target-environments}

Chaos tests are ideally conducted in a production environment, yet the risks associated with intentionally disrupting live operations—and potentially impacting customer experience—make this approach challenging. Pre-production environments are a good starting point as one gets familiar with the process and builds a level of confidence in running these tests and handling issues that arise. While pre-production environment might mimic production, the differences in configuration and load characteristics from production make running chaos tests and observing its impact on live production environment more beneficial. 

## Types of faults
