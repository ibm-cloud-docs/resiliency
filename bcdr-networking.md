---

copyright:
  years: 2021
lastupdated: "2024-08-06"

keywords: disaster recovery network, network aspects for DR, DR network, business continuity network considerations, disaster recovery network design

subcollection: resiliency

---

{{site.data.keyword.attribute-definition-list}}

# BCDR networking aspects
{: #networking-aspects}

Review the key aspects in network design that might impact the disaster recovery (DR) solution, including the common five network types, local area network (LAN) and wide area network (WAN) considerations, and the impact of partial failover.
{: shortdesc}

In a DR solution, we can basically classify the networks in five types:

- Management/Monitoring
- Replication
- Disaster recovery test
- Failover (or emergency)
- Fallback

In the cloud, the concept of these networks continues to be valid but might need to be implemented considering the specific network services and policies.

## Management/Monitoring
{: #mngmt-monitor}

The management/monitoring network type is the least demanding connectivity in terms of bandwidth since it must sustain only the following:

- Administrator access to remote systems
- Monitoring, logging, and support activities (SNMP, traps, logging, and so on)
- Antivirus and security patches

You can normally create network tunnels within the replication network to sustain these functionas.

## Replication
{: #replication}

Replication is the connectivity that is required to duplicate data from a primary to a DR site. It’s mainly determined by the synchronization method:

Online full synchronization
:   In the online synchronization, a full copy of the entire volume, including empty spaces, is transmitted over the network to the DR site with delta updates that are captured and sent too. In the online synchronization, you should consider that this might not be a one-shot operation. Sometimes, you need to periodically perform a full-refresh of the replicated data, or perform bulk-transfer of a good portion of your source data.

Offline synchronization
:   In the offline synchronization, most of the data is made available at the DR site through alternative means (disk images), therefore only delta updates that are captured and sent will traverse the network. If this meets the network requirement for replication, you should pay attention to the frequency that you need for the periodic refresh since this might impact your [recovery time objective](#x3167918){: term} (RTO) and [recovery point objective](#x3429911){: term} (RPO).

Another consideration is that a replication network must work in two ways:

1. To the DR site. This is the normal flow when production is run on premises, and the DR site is there just for protection.
2. From the DR site. This is the flow that you have in an emergency where production is running at the DR site and the on premises (or an alternate on premises) is the target of the replication.

A possible tradeoff for a replication network is to size your bandwidth requirement for an online synchronization to fully sustain the daily update, but also to have the bulk data transfer to be completed within a reasonable time, which is usually between two and seven days.

## Disaster recovery test
{: #dr-test}

Two types of disaster testing exist, and their network requirements are different.

DR simulations
:   DR simulations are completed while the production operations continue on premises. Disaster recovery test users can access the DR simulation environment by pointing to different IP addresses to connect to the servers reprovisioned at a DR site. The original IP addresses of the recovered servers needs to be NAT’ed to these different IP addresses to avoid duplicated IP addresses in production, or even worse, that real transactions are executed toward the DR servers instead of the real production servers.

   Another option might be to perform policy-based routing based on the source address of the test users.

Switch-over
:   It is run by moving the production to the DR site. From the network perspective, this is equivalent to the real emergency. The entire production networks must be routed to the DR site, while the replication network flow is reversed (from DR site to on premises) to bring the update back to the on premises, which now works as the DR site.

In the cloud, the network for testing is provisioned as part of the DR cloud resources and is often subject to limitations as mentioned before. If the DR is implemented on the same cloud, it would be easier to configure a network to simulate or switch the production environment.

## Failover (or emergency)
{: #failover}

The entire production networks must be routed to the DR site, while the replication network flow is reversed from DR site to on premises to bring the update back to the on premises, and suspended until the emergency is over at on-premises site.

During the emergency, the network functions (routing and security) are also transferred from on premises to the DR site, and if the primary is physical on premises, it might need to be virtualized to adapt to the target requirements. A precheck and maintenance of this physical-to-virtual network function is a key success factor during the emergency, and it has the same importance as the data replicator or the server reprovisioning technique.

For this network, also apply all the considerations described previously for cloud to DR cloud.

## Fallback
{: #fallback}

Fallback presents the same challenges as the DR simulation by switch-over seen before, as you keep having the production running at the DR site, while intercepting and sending updates back to the fallback site. The quantity of data that flows back to the fallback sites depends on the emergency that happened.

For short-term emergencies where the original site is unavailable for a period of time, but servers and storage remain intact, a delta-resynch might fit the need to bring the operation back to on premises.

For other emergencies, that have forced a change to the site, server, or storage in the original on premises, it might require a full resynchronization of data, so the fallback will happen tidily at the most convenient time after the sync point has been achieved.

## Cloud networking (LAN)
{: #cloud-network-lan}

Having a DR on cloud might also imply that you need to rebuild your network structures to adapt to the network structures and constraints on the cloud provider. In some cases, you are be forced to use Software Defined Network (SDN) and Network Functions Virtualization (NFV) to reproduce your complex enterprise network layout. A careful planning and evaluation of performance and scalability limitation is essential to assure your reprovisioned compute can work properly.

## Cloud networking (WAN)
{: #cloud-network-wan}

All the external networks connected to your primary (failed) site must be rerouted to the alternative cloud site. Different options are available, and you need to evaluate what options best fit your requirements. Consider the charges associated with the use of the network, such as download and upload charges, when using the cloud provider resources, as they might add a substantial cost to your DR total cost of ownership (TCO).

When implementing a DR on cloud, the solution is simplified by leveraging the same technology in the primary and secondary sites.

## Full versus partial failover
{: #full-partial-failover}

Partial failover is used to potentially increase the availability of the services by allowing to run and recover some of the services in DR while all the others are still running production on the customer data center.

Partial failover carries a lot more complexity in the network design that is associated with the solution, as it requires an extension of production site networks to the DR site that can happen on Layer 2 (such as L2TPv3, Cisco Overlay Transport Virtualization), Layer 3 (such as Cisco Locator/ID Separation protocol (LISP), Virtual Private LAN Services (VPLS), or Software Defined Network technique.

Additional considerations might imply the full control of the landing DR Hypervisor, so techniques like VXLAN overlays that are included in solutions like VMware NSX might not be applicable where this control is missing.

From a network perspective, this extension implies additional thinking because of possible impacts on security and performances, at least because the servers that were previously in the local LAN are now placed over a longer distance (WAN).

These impacts must be evaluated in two flavors: from a server to user perspective and from a server to server perspective, especially for time-sensitive applications or services.

### User to DR server
{: #user-dr-server}

Consider the scenario where only a server (Server A) has been failed over and is running at the DR data center in a partial failover situation.

The user-to-server A session reaches the server A DR-VLAN through the existing customer routing and security running on premises. For the customer-managed routing perspective, Server A is still on source-VLAN, in other words, the fact that source-VLAN is extended in the DR data center is transparent for the customer-managed router.

So, the customer-managed router places an address resolution protocol (ARP) frame for server A on source-VLAN. This broadcast frame is seen and picked up by the Network Extension Device/Function at the customer location, encapsulated in the L2TPv3 tunnel, and sent to the Network Extension Device/Function in the DR site. The frame is then placed on DR-VLAN where server A is running.

If the customer-managed router has static ARP table for Server A, this does not work or the static entry needs to be replaced.
{: .note}

While the previously mentioned is applicable to both internet and direct connected customers, the network design must consider the additional latency, due to the processing of the L2TPv3 tunnel, and the distance.

If this additional latency is somehow predictable within a range for directly connected clients with latency SLAs (the overall latency does not come from only the link latency), the internet connected customers might experience a different situation based on the unpredictable latency of the public internet.

The network design must also consider possible MTU adjustments or additional packet loss situations that the application running in the production site might demand in terms of requirements.

### Server to disaster recovery servers
{: #server-dr-servers}

Consider the scenario where two servers have been failed over and are currently running at the DR data center in a partial failover situation (server A on DR-VLAN1 and server B on DR-VLAN2), while all the rest of the servers keep on running on premises.

In this scenario, even the main concepts are similar to what we have seen in user-to-server case. Since the customer routing and security service is still running on premises, each frame has an additional latency to be considered, as all the routing and security functions are performed on premises.

The more servers in partial failover mode, the greater the impact of latency on the customer routing and security functions.

### Continuously available class
{: #continuously-available-class}

For [continuously available class](/docs/overview?topic=overview-understanding-dr#plan-objectives), because of the short recovery time, the best-suited DR solution is to adopt application clustering and an application-aware data replication to have a continuously available infrastructure ready to be activated in the case of an emergency.

Application-aware data replication, such as database replication, allows to have an application-consistent copy of data that can be immediately used by the alternative compute instance. The alternative compute instance for DR is already created and always-on.

Compute instances that make up the application or database cluster must be deployed over different fault domains to guarantee that at least one instance will survive an unavailability event.

The DR test will be completed on a new set of on-demand, dynamically provisioned compute instances, that are duplicated by using snapshot functions. This validates the DR runbooks that will be run on the real alternative passive DR compute instances.

![Continuously available class network architecture](images/BCDR-Architecture-Diagram-image12.svg "Continuously available class network architecture"){: caption="Figure 1. Continuously available class network architecture" caption-side="bottom"}

On the DR side, you can see a solution where components are deployed on the cloud infrastructure and strictly connected with the correspondent that is deployed on premises. The components on cloud are passive in the sense that they do not perform active production workload, but they are receiving updates from the on premises components that are performing all of the productive workload.

On the HA+DR side, you see a typical 3-site deploy, where two sites are performing productive workload (active-active couples), while the third site has passive components that just receive the updates from the two productive sites.

In both cases, you see an “on-demand” environment that is dynamically created and used solely to run DR test.

You should limit the continuous availability class to only those workloads really needing this service level.

To achieve a continuous availability level is expensive and might be limited by constraints such as the distance and network bandwidth between primary and secondary sites.

### Advanced recovery class
{: #advanced-recovery-class}

For [advanced recovery class](/docs/overview?topic=overview-understanding-dr#plan-objectives), a continuous data replication solution maintains a live copy of source disks over a different cloud site or zone. This can be achieved mainly in three ways:

Native Cloud function
:   Cloud offers disk replication functions to native duplicates data among cloud zones or sites.

DRaaS products
:   In cloud marketplace, there are products and tools available that can be acquired and implemented on cloud resources (compute, storage, network) to provide a user-controllable disk replication in support of a DR as a service solution. Most of these products replicate data with a software solution that is an agent installed in the source VM or Hypervisor that intercepts the updates and sends them to a correspondent receiving software-based replicator at the DR location.

Software Defined Storage (SDS)
:   Instead of using native storage functions of the cloud, SDS products from the marketplace use cloud resources (compute, storage, network) to provide a user-controllable disk subsystem, including data replication functions.

    OS disks might not be duplicated with an SDS solution as you might have limitations in having a VM to boot from an SDS replicated disk.
    {: .important}

For an emergency, compute could be provisioned on demand, leveraging dynamic provisioning, subject to verification that cloud can commit on a provisioning time that allows you to meet the RTO of the applications or can be pre-provisioned and in stand-by mode, eventually.
DR tests will be completed on a new set of on-demand, dynamically provisioned compute instances, that are duplicated by using snapshot functions.

![Advanced recovery class network architecture](images/BCDR-Architecture-Diagram-Image13.svg "Advanced recovery class network architecture"){: caption="Figure 2. Advanced recovery class network architecture" caption-side="bottom"}

In the diagram, you can see production is performed by applications, both in Production Zone 1 and Production Zone 2, and data is stored in their respective sites.

Production Zone 1 and Production Zone 2 might run the same workload type or can run different workloads. In the case that they run the same workload type (same application), it is likely that application data is stored in a single site.

The storage replication functions onboard the storage subsystems in both production sites perform the data replication (synchronous or asynchronous) to the correspondent SDS solution in the DR-cloud space, where data is kept available for emergency restarts.

Storage replication requires likewise storage on both sites, so the two options on Production Zone1 and Production Zone2 might also use different technologies, each one coupled with the appropriate SDS match.

Disaster recovery tests are completed by cloning the live data and attaching to a temporary on-demand DR test environment to validate the ability to restart operations.

Disaster recovery tests will be run on a new set of on-demand dynamic provisioned compute and storage instances that will be loaded with the restored data.

For an emergency, compute can be provisioned on demand leveraging dynamic provisioning, subject to verification that provisioning time allows to meet the RTO of the applications or can be pre-provisioned and in stand-by mode, eventually.

If your backup data retention timeframe is very short, you might still use the cloud native backup functions. Be aware that when moving the workloads on a new cloud, you need to keep the old workload installed on the previous cloud for the retention period, in case you need to recover an application data.
