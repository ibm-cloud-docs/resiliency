---

copyright:
   years: 2020, 2025
lastupdated: "2025-10-08"

keywords: chaos testing, resiliency, client testing

---

{{site.data.keyword.attribute-definition-list}}

# Running chaos experiments by using APIs
{: #chaos-experiments}

After you become comfortable running simple chaos testing scenarios that target specific application pods, you can expand your radius and start exploring additional scenarios by taking advantage of existing IBM Cloud APIs.

## Expanding the experiment scope
{: #chaos-existing-apis}

Chaos faults can affect a single resource or a set of resources based on some attributes. Identifying resources to target can be done by either listing all service instances that you have access to (e.g., list all clusters in containers API) or by using tags or resource groups. The [Global Search API](https://cloud.ibm.com/apidocs/search) allows you to search and filter for resources using a lucene syntax query string.

Identifying sub-resources like worker nodes in clusters might require additional API calls exposed by the service such as the [GetClusterWorkers API](https://containers.cloud.ibm.com/global/swagger-global-api/#/clusters/GetClusterWorkers) which takes filters such as the worker pool ID within the cluster.

### Scenario: AZ Failure Affecting IKS/ROKS Cluster Worker Node(s)
{: #chaos-cluster-failure}

One approach to simulating a worker node failure would be to cordon off the selected node(s) followed by drain of workloads running in it. This is a very controlled operation which first marks the node(s) as NotSchedulable and then reschedules any active application pods running on it to other nodes.

A reboot of the worker node is more disruptive and closer to real-world behavior without actually pulling the plug. This is not possible using Kubernetes APIs and doing it over ssh using an admin user credentials, while feasible might not be acceptable from a security perspective.

Here, we leverage existing IBM Cloud APIs to inject the chaos to perform a OS level reboot. 

Care must be taken to target only selected nodes, especially since the chaos framework is also running on the same cluster. Node selection can be limited by worker pool or zone. Additionally, sequential chaos injection, rebooting one node at a time allowing one to study the behavior incrementally.
{: note}

### Scenario: Failure Affecting Networking Components
{: #chaos-network-failure}

#### Direct Link
{: #chaos-service-directlink}

Use route filters in Direct Link to simulate failure by targeting specific instances in a redundant topology. This can be tested by using route filters to block traffic on specific instances. For a full AZ Failure, use Input: `0.0.0.0` to block all traffic.

*   [POST /gateways/{gateway_id}/export_route_filters](https://cloud.ibm.com/apidocs/direct_link#create-gateway-export-route-filter)
*   [POST /gateways/{gateway_id}/import_route_filters](https://cloud.ibm.com/apidocs/direct_link#create-gateway-import-route-filter)

Reverting the change: Unlike the `os_reboot` operation in Kubernetes, a new operation is needed to terminate the chaos experiment, by reverting the change. A different API needs to be called to revert by either replacing the filters to previously captured values or deleting the introduced ones.

*   [PUT /gateways/{gateway_id}/export_route_filters](https://cloud.ibm.com/apidocs/direct_link#replace-gateway-export-route-filters) to replace all existing export route filters configured on the Direct Link gateway
*   [DELETE /gateways/{gateway_id}/export_route_filters/{id}](https://cloud.ibm.com/apidocs/direct_link#delete-gateway-export-route-filter) to delete an export route filter.

#### Transit Gateway
{: #chaos-service-transit-gateway}

While each Transit Gateway has a high degree of redundancy built-in, having a pair of local or global transit gateway offers regional redundancy. This can be tested by using prefix filters to block traffic on specific instances.

*   [POST /transit_gateways/{transit_gateway_id}/connections/{id}/prefix_filters](https://cloud.ibm.com/apidocs/transit-gateway#create-transit-gateway-connection-prefix-filter)

Reverting the change: To terminate the chaos scenario, revert the change using the following APIs.

*   [PATCH /transit_gateways/{transit_gateway_id}/connections/{id}/prefix_filters/{filter_id}](https://cloud.ibm.com/apidocs/transit-gateway#update-transit-gateway-connection-prefix-filter) to update the filters with previous/valid ones.
*   [DELETE /transit_gateways/{transit_gateway_id}/connections/{id}/prefix_filters/{filter_id}](https://cloud.ibm.com/apidocs/transit-gateway#delete-transit-gateway-connection-prefix-filter) to delete a prefix filter

#### VPC
{: #chaos-service-vpc}

Access control lists and their associated rules can be used to control network connectivity to a VPC by blocking traffic. Failover to a redundant VPC or other region can also be tested in a chaos experiment.


*   [POST /network_acls/{network_acl_id}/rules](https://cloud.ibm.com/apidocs/vpc/latest#create-network-acl-rule)

Reverting the change: to terminate the scenario, revert the change by deleting the relevant ACL rule.

*   [DELETE /network_acls{network_acl_id}/rules/{rule_id}](https://cloud.ibm.com/apidocs/vpc/latest#delete-network-acl-rule)

## Writing your own chaos scenarios
{: #chaos-custom-scenarios }

This list is, of course, non-exhaustive. However, you can test the resiliency of most IBM Cloud components and services through some common approaches. 

* Deleting, testing and re-creating resources using APIs can be very useful in determining if your application has redundant components to deal with an unexpected outage of these resources. Be careful when deleting resources - testing storage volumes would not benefit from this approach due to the loss of data, but resources such as network filters, load balancers etc can be easily recreated provided the right configuration is understood before undertaking the chaos experiments. 

* Use network tools such as filters and security groups to block access to a chaos probe to simulate network failure without needing to delete resources. 
