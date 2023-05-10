# AWS - VPC

### VPC and AWS Region

VPCs, like most (but not all) resources, are region-specific.

Each region comes with a default VPC. The VPC will have one "public" subnet per availability zone within the region.

The subnets are "public" because internet traffic (that's not going through the private network itself) is routed through an Internet Gateway (IGW). This is a setting (route tables + routes) that is specified in each subnet.

Routing traffic through an IGW means two things:

1. By default, servers within the public subnet will get assigned a public IP address
2. By default, servers within the public subnet will be able to reach the outside internet, and the outside internet will be able to reach the servers (via the public IP address).

### IP Addressing

We see how to handle IP addresses when creating a VPC and thinking about the subnets.

The VPC gets an IP range of private network IP addresses.

The three networks you can choose to create from are:

1. 10.0.0.0/16 (or smaller)
2. 172.31.0.0/16 (or smaller)
3. 192.168.0.0/16 (or smaller)

> I usually create smaller networks, such as `10.0.0.0/17`, which is half as large as the `/16` IP address range. We use [this tool](https://www.davidc.net/sites/default/subnets/subnets.html) to help calculate IP address ranges.

Once you create VPC with an IP address range, you can split that range up using Subnets.

### Subnets

Some basic Subnet facts to know when creating them:

1. Each Subnets is non-overlapping IP address range within the VPC's IP address range
2. Each Subnet is created within a specific AZ in the current region.
You'll typically create at least one subnet per availability zone, but often multiple to handle multiple cases.

For example, I like to create 2 subnets per AZ:

1. Each AZ gets a Public Subnet - with an IGW to allow public internet access to resource in this subnet
2. Each AZ gets a Private Subnet - with a NAT Gateway, to allow outbound internet access, but disallow the outside world from reaching a server in this subnet

> It's very common to have multiple VPC's as well, to segment applications, environments, development teams, or any logical separation you can think of.


### Creating Subnets


We cover Subnet IP addressing a bit more in detail - specifically how to divide up your VPC into smaller chunks of IP address ranges.

Terraform has some tooling to help generate contiguous IP address ranges that you can use to create subnets with.

```
> cidrsubnets("10.0.0.0/17", 4, 4, 4)
tolist([
  "10.0.0.0/21",
  "10.0.8.0/21",
  "10.0.16.0/21",
])
 
 
> cidrsubnets("10.20.0.0/17", 4, 4, 4, 4, 4, 4)
tolist([
  "10.20.0.0/21",
  "10.20.8.0/21",
  "10.20.16.0/21",
  "10.20.24.0/21",
  "10.20.32.0/21",
  "10.20.40.0/21",
])

```

### Internet Gateways
---

The default VPC has one route table that is used across all subnets created within it. This Route Table has an IGW assigned to call traffic headed to `0.0.0.0/0` (basically if some outbound network traffic in the server goes anywhere but the private network, it will get routed through the IGW, allowing public internet access).

Put another way: IGWs allows public network access (which makes any subnet using the route table that has an IGW a "Public Subnet").

There's no extra charge (except for "regular" bandwidth usage charges) for Internet Gateways.

### NAT Gateway
---
The default VPC has no NAT Gateways created, as the default VPC is optimized towards allowing you to quickly spin up a server and have it available on the public internet.

NAT Gateways allows private-network-only servers access to the internet (but they can't be reached from the public internet). NAT is "Network Addreess Translation" and is a way to map local private addresses to a public one before transferring the information. In other words, outbound traffic from a server in a private subnet is routed through the NAT Gateway and then to the outside internet. This allows the resource (server) to talk to the outside internet without allowing the outside internet to reach the resource (server) directly.

In additional to standard bandwidth charges, there are additional charges for NAT Gateways:

1. There is an hourly charge for NAT Gateways (for example in `us-east-2`, an hour charge of `$0.45/hr` - to calculate the monthly charge: `$0.045*730 = $32.85/mo`)
2. There is a charge per gigabyte transferred through a NAT Gateway (for example in `us-east-2`, a charge of `$0.045/GB`). This is in addition to regular bandwidth charges!


### Private vs Public Subnets
---

We'll create two servers, one in each subnet type.

1. The server in the public subnet gets assigned a public IP address. The public internet can reach it, and it can reach the public internet.
2. The server in the private subnet does not get a public IP and is not accessible from the outside internet (but it can reach the internet through the NAT Gateway).

We also SSH into the "public" server, which then allows us to SSH into the "private" server - we use the public server as a bastion (jump) host to gain access to the private network.