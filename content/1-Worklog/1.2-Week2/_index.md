---
title: "Week 2 Worklog"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


### Week 2 Objectives:

* Master knowledge of network security in AWS (Security Groups, NACL).
* Understand VPC connectivity and network connection methods.
* Learn about load balancing with AWS ELB and Load Balancer types.

### Tasks to be implemented this week:
| Day | Tasks | Start Date | Completion Date | Documentation Source |
| --- | --- | --- | --- | --- |
| 2 | - Learn about Security Groups: <br>&emsp; + Concepts and stateful characteristics <br>&emsp; + How to create and configure rules <br>&emsp; + Apply to Elastic Network Interface | 08/11/2025 | 08/11/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Learn about Network ACL (NACL): <br>&emsp; + Concepts and stateless characteristics <br>&emsp; + Differences between NACL and Security Groups <br>&emsp; + Rule evaluation mechanism from top to bottom | 08/12/2025 | 08/12/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - **Hands-on Practice:** <br>&emsp; + Create Security Group for EC2 instance <br>&emsp; + Configure NACL for subnet <br>&emsp; + Compare security effectiveness between 2 methods <br> - Learn about VPC Flow Logs | 08/13/2025 | 08/13/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Learn about VPC connectivity: <br>&emsp; + VPC Peering and limitations <br>&emsp; + Transit Gateway <br>&emsp; + Site-to-Site VPN <br>&emsp; + AWS Direct Connect | 08/14/2025 | 08/14/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Learn about Elastic Load Balancing: <br>&emsp; + ELB concepts and types <br>&emsp; + Application Load Balancer (ALB) <br>&emsp; + Network Load Balancer (NLB) <br>&emsp; + Sticky Session and Health Check <br> - **Hands-on Practice:** <br>&emsp; + Create ALB to distribute traffic <br>&emsp; + Configure Target Groups | 08/15/2025 | 08/15/2025 | <https://cloudjourney.awsstudygroup.com/> |


### Week 2 Achievements:

* Gained clear understanding of Security Groups and NACL:
  * Security Groups are stateful firewalls that only allow "allow" rules
  * NACL is a stateless firewall applied to subnets
  * Security Groups affect individual instances, NACL affects multiple servers in a subnet

* Learned how to use VPC Flow Logs to monitor IP traffic in VPC without capturing packet contents.

* Understood VPC connection methods:
  * VPC Peering to connect 2 VPCs, does not support transitive routing
  * Transit Gateway to connect multiple VPCs and on-premises networks
  * Site-to-Site VPN with Virtual Private Gateway and Customer Gateway
  * AWS Direct Connect with low latency (20-30ms)

* Mastered knowledge of Elastic Load Balancing:
  * 4 types: ALB, NLB, Classic LB, Gateway LB
  * ALB operates at Layer 7, supports HTTP/HTTPS and path-based routing
  * NLB operates at Layer 4, supports TCP/TLS and static IP
  * Features: Health Check, Sticky Session, Access Logs

* Successfully completed hands-on practice:
  * Created Security Groups for EC2 with appropriate rules
  * Configured NACL for subnet with logical rule ordering
  * Set up Application Load Balancer with Target Groups
  * Verified Load Balancer operation and traffic distribution

* Gained ability to compare and select appropriate security solutions and network connectivity for specific use cases.