---
title: "Week 2 Worklog"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Goals:

* Understand Amazon Virtual Private Cloud (VPC)
* Understand how to deploy Amazon EC2 instances
* Learn how to configure AWS Site-to-Site VPN

### Tasks to be implemented this week:
| Day | Task | Start Date | Completion Date | References |
| --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Learn about Amazon Virtual Private Cloud (Amazon VPC): <br>&emsp; + Subnets <br>&emsp; + Route Table <br>&emsp; + Internet Gateway <br>&emsp; + NAT Gateway <br> - **Practice:** <br>&emsp; + Create VPC <br>&emsp; + Create Subnet <br>&emsp; + Create Internet Gateway <br>&emsp; + Create Route Table <br>  | 15/09/2025   | 15/09/2025   | - <https://000003.awsstudygroup.com/vi/1-introduce/> <br> - <https://000003.awsstudygroup.com/vi/3-prerequisite/> |
| 3   | - Learn about Firewall in VPC: <br>&emsp; + Security Group <br>&emsp; + Network ACLs <br>&emsp; + Networking VPC Resource Map <br> - **Practice:** <br>&emsp; + Create Security Group <br>&emsp; + Enable VPC Flow Logs <br>   | 16/09/2025   | 16/09/2025  | - <https://000003.awsstudygroup.com/vi/2-firewallinvpc/> <br> - <https://000003.awsstudygroup.com/vi/3-prerequisite/> |
| 4   |  - Deploy Amazon EC2 Instances: <br>&emsp; + Deploy Production-Ready EC2 Infrastructure <br>&emsp; + Production-Ready Features <br> - **Practice:** <br>&emsp; + Launch EC2 Instance <br>&emsp; + Test connectivity <br>&emsp; + Create NAT Gateway <br>&emsp; + Use Reachability Analyzer <br>&emsp; + Create EC2 Instance Connect Endpoint (Optional) <br>&emsp; + AWS Systems Manager Session Manager <br>&emsp; + CloudWatch Monitoring & Alerting <br> | 17/09/2025   | 17/09/2025    | <https://000003.awsstudygroup.com/vi/4-createec2server/> |
| 5   | - Configure Site-to-Site VPN: <br>&emsp; + Create VPN environment <br>&emsp; + Configure VPN connection <br>&emsp; + Configure VPN with strongSwan using Transit Gateway (Optional) <br> - Resource cleanup <br> - **Practice:** <br>&emsp; + Create VPC for VPN <br>&emsp; + Launch EC2 Instance <br>&emsp; + Create Virtual Private Gateway <br>&emsp; + Create Customer Gateway <br>&emsp; + Create VPN Connection <br>&emsp; + Configure Customer Gateway <br>&emsp; + Customize AWS VPN Tunnel <br>&emsp; + Advanced VPN Configuration <br>&emsp; + Troubleshoot VPN <br>&emsp; + Create Transit Gateway Attachment <br>&emsp; + Configure Route Tables <br>| 18/09/2025   | 18/09/2025      | - <https://000003.awsstudygroup.com/vi/5-vpnsitetosite/> <br> - <https://000003.awsstudygroup.com/vi/6-cleanup/> |
| 6   | - Infrastructure as Code Templates: <br>&emsp; + Automate VPC Deployment with Infrastructure as Code <br>&emsp; + Benefits of IaC for VPC Deployment <br>&emsp; + CloudFormation Template | 19/09/2025   | 19/09/2025      | <https://000003.awsstudygroup.com/vi/7-infrastructureascode/> |

### Results achieved in Week 2:

* Understood and deployed Amazon VPC: 
  * Subnets 
  * Route Table
  * Internet Gateway
  * NAT Gateway.

* Successfully configured Security Group, Network ACLs, and VPC Flow Logs.

* Created and tested EC2 Instance, applied NAT Gateway, Reachability Analyzer, Instance Connect Endpoint, Session Manager, and CloudWatch.

* Set up Site-to-Site VPN: 
  * Created and configured Virtual Private Gateway, Customer Gateway, VPN Connection, Route Tables
  * Experimented with strongSwan configuration and troubleshooting.

* Gained familiarity with infrastructure automation using Infrastructure as Code (CloudFormation Templates).
