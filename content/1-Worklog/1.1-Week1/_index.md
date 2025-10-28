---
title: "Week 1 Worklog"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
<!-- {{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


### Week 1 Objectives:

* Connect and get acquainted with First Cloud Journey team members
* Understand basic AWS services, how to use Console & CLI
* Learn cost optimization strategies
* Explore and configure IAM
* Create Groups and Users, and establish connection from local machine to cloud
* Understand cloud layer architecture

## Tasks to be implemented this week:

| Day | Task | Start Date | Completion Date | Documentation Source |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Get acquainted with FCJ team members <br> - Read and understand company policies and internship regulations | 09/09/2025   | 09/09/2025      | |
| 3   | - Learn about AWS and service categories <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database <br>&emsp; + Security & Identity | 12/08/2025   | 12/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Create AWS Free Tier account <br> - Learn AWS Console & AWS CLI <br> - **Hands-on Practice:** <br>&emsp; + Create AWS account <br>&emsp; + Install and configure AWS CLI <br> &emsp; + Learn how to use AWS CLI | 13/08/2025   | 13/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn EC2 basics: <br>&emsp; + Instance types <br>&emsp; + AMI (Amazon Machine Images) <br>&emsp; + EBS (Elastic Block Store) <br>&emsp; + Security Groups <br> - Methods to SSH remote into EC2 <br> - Learn about Elastic IP | 14/08/2025   | 15/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Hands-on Practice:** <br>&emsp; + Create EC2 instance <br>&emsp; + Establish SSH connection <br>&emsp; + Mount EBS volume <br>&emsp; + Configure Security Groups | 15/08/2025   | 15/08/2025      | <https://cloudjourney.awsstudygroup.com/> |

## Week 1 Achievements:

* Understanding of AWS and core service categories:
  * **Compute:** EC2, Lambda, ECS
  * **Storage:** S3, EBS, EFS
  * **Networking:** VPC, CloudFront, Route 53
  * **Database:** RDS, DynamoDB
  * **Security:** IAM, WAF

* Successfully created and configured AWS Free Tier account.

* Familiarized with AWS Management Console and learned how to find, access, and use services through web interface.

* Installed and configured AWS CLI on local machine including:
  * Access Key ID
  * Secret Access Key
  * Default region (ap-southeast-1)
  * Output format (json)

* Used AWS CLI to perform basic operations such as:
  * Check account information & configuration: `aws sts get-caller-identity`
  * Get list of regions: `aws ec2 describe-regions`
  * View EC2 services: `aws ec2 describe-instances`
  * Create and manage key pairs: `aws ec2 create-key-pair`
  * Check running service information

* Gained ability to seamlessly switch between web interface and CLI for AWS resource management.

* Understanding of cloud layer architecture:
  * **Data Centers:** Physical data center facilities
  * **Availability Zones (AZ):** Include one or more data centers, minimum 2 data centers connected by high-speed networks
  * **Regions:** Minimum 3 AZs per region, geographically distributed
  * **Edge Locations:** Secondary network to run edge network services
  
  **Main edge network services commonly used in Vietnam:**
  * **CloudFront (CDN):** Accelerate content access speed
  * **Web Application Firewall (WAF):** Creates protective layer to prevent DDoS attacks
  * **Route 53 (DNS service):** Create domain for applications and attach SSL certificates

* Cost optimization strategies:
  * Select appropriate compute resources and storage configurations based on actual needs
  * Avoid running full configuration when unnecessary
  * Use Free Tier for learning and experimentation
  * Turn off/delete unused resources to save costs