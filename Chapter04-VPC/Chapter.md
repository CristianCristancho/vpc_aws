
<h1 align= "center" style="margin-top: 3cm"> Amazon VPC: Virtual Private Cloud</h1>
<h3 align= "center" style="margin-top: 3cm"> By Julián Forero</h3>

---

## **Agenda**
1. What is VPC?
2. VPC Components
    * Subnets
    * Route Tables
    * Internet Gateways
    * DHCP
    * Elastic IP - ENI
    * Endpoints
    * Peering
    * Security Groups vs ACLs
    * NAT Gateways and instances
    * Virtual Private Gateways and VPN
3. Keep in Mind

---

## **Amazon Virtual Private Cloud (VPC)**

*"Virtual Private Cloud is the networking layer for Amazon EC2. It allows you to build your own virtual network within AWS."*

This includes creation and configuration of your **OWN**:
* *IP Address Range*
* *Subnets*
* *Route Tables*
* *Network Gateways*
* *Security Settings*

---

## What is the meaning of VPC?

*"An elastic network populated by infrastructure, platform, and application services that share common security and interconnection". ([AWS Glossary](http://docs.aws.amazon.com/general/latest/gr/glos-chap.html#VPC))*

There are two different networking platforms available within AWS: _**EC2_Classic**_ and _**EC2_VPC**_

---
## VPC Components

An Amazon VPC consists of the following _**mandatory**_ components:
* Route tables
* Dynamic Host Configuration Protocol (DHCP) option sets
* Security groups
* Network Access Control Lists (ACLs)

---
An Amazon VPC has the following **optional** components:
* Internet Gateways (IGWs)
* Elastic IP (EIP) addresses
* Elastic Network Interfaces (ENIs)
* Endpoints
* Peering
* Network Address Translation (NATs) instances and NAT gateways
* Virtual Private Gateway (VPG), Customer Gateways (CGWs), and Virtual Private
Networks (VPNs)

---

Let's view the above figure:

<img  src ="figures/vpc.png" style="width: 400px; height: 400px; display: block; margin: 0 auto;">

Figure 1: [VPC, subnets, and a route table](Book)

---

#Keep in Mind 1.0

* Within a **region**, you can create **multiple** Amazon VPCs, and each Amazon VPC is **logically isolated** even if it shares its IP address space.

* **This means that VPC are not cross-region**.

* In VPC creation, IP address range is established by a CIDR block.

* Address range of VPC **cannot** be changed after creation.

---

##Primary VPC Components

####Subnets:
As a normal concept of networking, subnets on AWS are segments of Amazon VPC's address range where AWS resources are launched. (RDS,EC2, e.g.)

* AWS has 5 IP addresses reserved on each subnet for networking process.

* _**AWS default VPC has attached one subnet per availability zone**_.

---

There are three types of subnets:

* **_Public:_** Directs subnet's traffic to an AWS Internet Gateway.
* **_Private:_** Opposite to Public subnet.
* **_VPN Only:_** Directs subnet's traffic to an AWS Virtual Private Gateway without having a route to an AWS IGW.

**_By default subnet's internal IP address range is always private._**

---

####Route Tables:

Set of rules applied to subnets in order to determine where network traffic is directed(EC2 instances from different AZ communication e.g.)

This allows you to:
* Specify which subnets are public (attached to an IGW) or private.
* Directs traffic to a VPG or a NAT Instance.

---

#Keep in Mind 2.0

* Your VPC has an implicit router.
* Your VPC automatically comes with a main route table that you can modify.
* _**Each subnet must be associated with a route table, which controls the routing for the subnet**_.
* Each route in a table specifies a destination CIDR and a target.

---

####Internet Gateways:

VPC Component that allows communication between instances in VPC and Internet, doing the translation from internal private IP address to Elastic IP Address, and from this to Internet.

<img  src ="figures/igw.png" style="width: 450px; height: 350px; display: block; margin: 0 auto;">

Figure 2: [VPC and IGW](Book)

---
####Dynamic Host Configuration Protocol (DHCP):

Working like a normal DHCP Server, it allows to get DNS records to instances over AWS VPC's IGW.

It allows configuration from DNS Servers, Domain Names, NTP Servers and NetBIOS Name Servers.

---

####Elastic IP and Elastic Networks Interfaces

**_EIP:_** Static public IP address from a region used to attach AWS resources within AWS VPC, having a one-to-one relationship with ENI.

**_ENI:_** Virtual Network Interface that can be attached to an instance in a VPC. It is associated with a subnet upon creation.

ENI allows management network creation, network and security appliances in a VPC, and the possibility of having a instance with network presence in different subnets.

---

####Endpoints

Allows the creation of a **_private connection_** between AWS VPC and AWS Resources without access by Internet or through a NAT instance, VPN o AWS Direct Connect.

Multiple Endpoints for a Single Service can be done with different route tables to enforce different access policies to different subnets to the same resource.

---

####Peering

Networking connection between two AWS VPC, in order to communicate resources in different VPCs (in another AWS account, e.g.)

<img  src ="figures/peering.png" style="width: 300px; height: 220px; display: block; margin: 0 auto;">

Figure 3: [Peering on AWS](Book)

---

* As peering connections are created through a request/accept protocol it needs to be approved by VPC owner.

* **_Peering IS NOT TRANSITIVE_**
* Peering is not allowed between VPCs with overlapping CIDR blocks.

* **_Peering is only allowed in VPCs from the same region_**.
---

####Security Groups and ACLs

**_SG:_** Virtual Firewall that controls inbound and outbound network traffic to AWS resources and Amazon EC2 instances.

Their rules must have source  or destination direction, protocol (TCP or UDP) and port.

**_ACLS:_** Stateless Firewall on a subnet level that contains a numbered list of rules that AWS evaluates in order from low to high, to determine which traffic is allowed on subnets associated with ACL.

---

#Keep in Mind 3.0

* In SG you can specify allow rules, but no deny rules. In ACLS you can do both.
* **_SG are stateful. Responses to allowed inbound traffic are allowed to flow outbound regardless of outbound rules and vice versa_**. ACLS are Stateless.
* By default, inbound traffic is not allowed. As Opposite, outbound traffic is.

---
#A good Chart

Security Group | Network ACL |
--- | --- | ---
| Operates at the instance level (first layer of defense) | Operates at the subnet level (second layer of defense) |
| Supports allow rules only | Supports allow rules and deny rules|
| Stateful: Return traffic is automatically allowed, regardless of any rules | Stateless: Return traffic must be explicitly allowed by rules.
| AWS evaluates all rules before deciding whether to allow traffic | AWS processes rules in number order when deciding whether to allow traffic.
| Applied selectively to individual instances | Automatically applied to all instances in the associated subnets; this is a backup layer of defense, so you don’t have to rely on someone specifying the security group.
---

####NAT Gateways and instances

AWS provides NAT instances and Gateways to allowed private subnets communication with Internet, with the purpose of establishing a connection to Internet without having a direct reverse connection to a resource.

**_NAT Instance:_** AMI designed to accept traffic within a private subnet, translating source IP address to public IP address of the NAT instance forwarding traffic to IGW.

**_NAT_Gateway:_** Managed resource by AWS that works in the same way that the instance, but has the feature of HA.

---

####Virtual Private Gateways, Customer Gateways and VPN.

There are two ways to connect an existing Data Center to AWS VPC: VPGW and CGW.

**_VPGW:_** VPN concentrator on AWS side of the VPN Connection between the two networks.

**_NAT_Gateway:_** Represents a physical device or a software application on the customer’s side of the VPN connection.

---
Let's see how is the communication in the figure 4:

<img  src ="figures/vpgw.png" style="width: 320px; height: 280px; display: block; margin: 0 auto;">

Figure 4: [VPN Connection in AWS](Book)

---

<h2 align= "center" style="margin-top: 3cm"> Thank you</h3>
<h1 align= "center" style="margin-top: 1cm"> Questions?</h3>
