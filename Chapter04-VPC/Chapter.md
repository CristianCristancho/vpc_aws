
<h1 align= "center" style="margin-top: 3cm"> Amazon VPC: Virtual Private Cloud</h1>
<h3 align= "center" style="margin-top: 3cm"> By Julián Forero</h3>

---

## **Agenda**
1. What is VPC?
2. Key concepts
3. VPC Console
4. Configuration
5. Important Tips

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

#Keep in Mind

Within a **region**, you can create **multiple** Amazon VPCs, and each Amazon VPC is **logically isolated** even if it shares its IP address space.

**This means that VPC are not cross-region**.

---

#Also

* In VPC creation, IP address range is established by a CIDR block.
* Address range of VPC **cannot** be changed after creation.

---

##Primary VPC Components

* **_Subnets:_** As a normal concept of networking, subnets on AWS are segments of Amazon VPC's address range where AWS resources are launched. (RDS,EC2, e.g.)

* **_VPC:_**
* **_VPC:_**
* **_VPC:_**






<!---
Amazon Elastic Compute Cloud (Amazon EC2) provides scalable computing capacity in the Amazon Web Services (AWS) cloud. EC2 allows you to acquire compute hardware (enough for your workload) through the launching of virtual servers called instances.

<img  src ="figures/vpc-logo.PNG" style=" display: block; margin: 0 auto;">
-->
