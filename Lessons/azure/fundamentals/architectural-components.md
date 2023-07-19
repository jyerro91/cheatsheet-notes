# Core Architectural Components
---

## Regions
---

> [!abstract] 
> *Azure offers more global regions than any other cloud provider with 60+ regions representing over 140 countries*

- Areas of the world where Azure has a **set of datacenters** (min 3 in a set)
- Usually regions are connected to another region to make region pair
- Region pairs have highest speed connections and special treatment during Azure updates
- Regions are made up of one or more datacenters in close proximity
- Provide flexibility and scale to reduce customer latency
- Preserve data residency with a comprehensive compliance offering

## Availability Zones
---

- Provide protection against downtime due to datacenter failure
- Physically separate datacenters within the same region
- Each datacenter is equipped with independent power, cooling and networking
- Connected through private fiber-optic networks

![[availability-zones.png]]

### Three Types of AZ Services

- **Zonal Services**
	- You can choose a specific AZ to deploy the service to
	- You then should deploy duplicate service to another zone to achieve resiliency (e.g VM)
- **Zone-Redundant Services**
	- Automatically deployed across zones for you
	- No need to be configure (e.g Azure SQL)
- **Always Available Services**
	- These are global servicecs and microsoft takes care of the ensuring that they are always on
	- Also called "non-regional services" (e.g Azure Portal, Azure AD)


## Region Pairs
---

- At least 300 miles of separation between region pairs.  
- Automatic replication for some services.  
- Prioritized region recovery in the event of outage.  
- Updates are rollout sequentially to  minimize downtime.


## Azure Sovereign Regions (US Government Services)
---

Meets the security and compliance needs of US federal agencies, state and local governments and their solution providers.

- Separate instance of Azure
- Physically isolated from non-US governement deployments
- Accessible only to screened, authorized personnel

## Azure Sovereign Regions (China)
---

Microsoft is China's first foreign public cloud service provider in compliance with government regulations

- Physically separated instance of Azure cloud services operated by **21Vianet**
- All data stays within China to ensure compliance

## Azure Resources
---

Azure resources are components like storage, virtual machines, and networks that are available to build cloud solutions.

- Virtual Machines
- Storage Accounts
- Virtual Networks
- App Services
- SQL Databases
-  Functions

## Resource Groups
---

is a container to manage and aggregate resources in a single unit.

- Resources can exist in only one resource group
- Resources can exist in different regions
- Resources can be moved to different resource groups
- Applications can utilize multiple resource groups

## Azure Subscriptions
---

An Azure subscription provides you with authenticated and authorized access to Azure accounts.

- **Billing Boundary** : generate separate billing reports and invoices for each subscription
- **Access Control Boundary** : manage and control access to the resources that users can provision with specific subscriptions

## Management Groups 
---

- Management groups can include multiple Azure subscriptions
- Subscriptions inherit conditions applied to the management group
- 10,000 management groups can be supported in a single directory
- A management group tree can support up to six levels of depth

![[mangement-groups.png]]