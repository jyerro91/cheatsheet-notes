---
title: Azure Compute Services
date: 2023-05-12
tags: [azure, lesson, compute-services]
---

# Azure Compute Services and Networking
---

Azure compute is an on-demand computing service that provides computing resources such as disks, processors, memory, networking and operating systems

**Azure Virtual Machines (VM)** are software emulations of physical computers

- Includes virtual processors, memory, storage, and networking
- IaaS offering that provides total control and customization
- Over 200 types to choose from

**VM Scale sets** sets provide a load-balanced opportunity to automatically scale resouces.

- Scale out when resource needs increase
- Scale in when resource needs are lower
- Elasticity
- 2 or more VM running the exact same code
- with LoadBalancer in front to direct traffic randomly to one of the machines
- Can handle up to 100 VMs in a single scale set and can be configure to 1000
- 

**VM availability sets**

![[availability-sets.png]]

**Azure Virtual Desktop** is a desktop and app virtualization that runs in the cloud

- Create a full desktop virtualization environment without having to run additional gateway servers
- Reduce risk of resource being left behind
- True multi-sessions deployments

**App Services** 

- a new paradigm for running code in the cloud
- Platform as a Service (PaaS)
- Promise of performance but no access to hardware

**Containers**

- Fastest and easiest to deploy
- Another paradigm for running code in the cloud

**Azure Container Instance (ACI)** - single instance quickest way to deploy a container
**Azure Kubernetes Service (AKS)** - runs on a cluster of servers, enterprise-grade


## Networking
---

### Typ es of networking services

- Connectivity Services
	- **Virtual Networks or in AWS (VPC)** : Emulating a physical network
	- **Subnet** : a subdivision of a virtual network, that you control, that has its own security rules
	- **Virtual Private Network (VPN)** : connecting two networks as if they were on the same network, uses a *Network Gateway*
	- **ExpressRoute** : high-speed private connection to azure
	- **DNS Services** : domain name resolution
- Protection Services 
	- **DDoS Protection** : Distributed Denial of Service attack protection
	- **Azure Firewall**
	- **Network Security Groups**
	- **Private Link**
- Delivery Services
	- **Load Balancer** : Distribute traffic evenly between multiple backend servers
	- **Application Gateway** : a higher-level of load balancer with an optional firewall
	- **Content Delivery Network (CDN)** : stores common static files on the edge, closer to the users for (perceived) improved performance
	- **Azure Front Door Service** : a load balancer, CDN and firewall all-in-one
- Monitoring Services


