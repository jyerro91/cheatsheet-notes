

## VPC

### Internet Gateways (IGW)
A combination of hardware and software that provides your private network with a route to outside world (meaning the internet) of the VPC

##### AWS Definition:

An internet gateway is a horizontally scaled, redundant and highly available VPC component that allows communication between instances in your VPC and the internet. Therefore imposes no availability risks or bandwidth constraints on your network traffic.

> Note: Your default VPC already has an IGW attached

Route tables rules and details you need to know:
1. Only 1 IGW can be attached to a VPC at a time
2. An IGW cannot be detached from a VPC while there are active AWS resources in the VPC (such as an EC2 instance or RDS)


### Route Tables (RTs)

##### AWS Definition:

A route table contains a set of rules, called routes, that are used to determine where network traffic is directed

> Note: Your default VPC already has a main route table

Route table rules and details you need to know:
1. Unlike an IGW, you can have multiple active route tables in a VPC
2. You cannot delete a route table if it has dependencies (associated subnets)

### Network Access Control List (NACLs)

##### AWS Definition

A network acess control list is an `optional layer of security` for your VPC that acts as a firewall for controlling in and out of one or more `subnets`.

> Note: Your default VPC already has an NACL in place and associated with the default subnets.

NACL rules and details you need to know:
1. Rules are evaluated from lowest to highest based on rule #
2. The first rule found that applies to the traffic type is immediately applied, regardless of any rules that come after it (have a higher rule #)
3. The default NACL allows all traffic to the default subnets
4. Any new NACLs you create DENY all traffic by default
5. A subnet can be associated with one NACL at a time.
6. An NACL allows or denies traffic from entering a subnet. Once inside the subnet. other AWS resources (e.g EC2) may have additional layers (security groups)

### Subnets

##### AWS Definition

When you create a VPC, it spans all of the AZ in the region. After creating a VPC, you can add one or more subnets in each AZ. Each subnet must reside entirely within one AZ and cannot span zones.

> Note: Default VPC already has a subnet created by default

Subnet rules and details you need to know:
1. Subnets must be associated with a route table
2. A `Public` subnet has a route to the internet
3. A `Private` subnet does not have a route to the internet
4. A subnet is located in one specific AZ

### Availability Zones

##### Simplified Definition

Any AWS resource that you launch (like EC2/RDS) must be placed in a VPC subnet. Any given subnet must be located in a an AZ. You can (and should) utilize multiple AZ to create redundancy in your architecture. This is what allows for `High Availabilty` and `Fault Tolerant` systems.

##### AWS Definition/Explanation

When you create a VPC, it spans all of the AZ in the region. After creating a VPC, you can add one or more subnets in each AZ. Each subnet must reside entirely within one AZ and cannot span zones.

> Availability Zones are distint locations that are engineered to be isolated from failures in other AZ. by launching instances in a separate AZ you can protect your applications from the failure of a single location.

---

## EC2

### Elastic Block Storage (EBS)

##### Simplified Definition

EBS is a storage volumen for an EC2 instance (like a hard-drive)

##### AWS Definition

Amazon EBS provides block level storage volumes for use with EC2 instances. EBS volumes are highly available and reliable storage volumes that can be attached to any running instance that is in the same AZ. EBS volumes that are attached to an EC2 instance are exposed as storage volumes that persist independently froem the life of the instance.



`IOPS` -  Input and Output Per Second



