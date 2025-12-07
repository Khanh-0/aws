---
title: "Week 1 Worklog"
date: 2024-01-01T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


### Week 1 Objectives:

* Connect and get acquainted with members of First Cloud Journey.
* Understand basic AWS services, how to use console & CLI.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Get acquainted with FCJ members <br> - Read and take note of internship unit rules and regulations                                                                                                   | 09/08/2025 | 09/08/2025      |
| 3   | - Learn about AWS and its types of services <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database <br>&emsp; + ... <br>                                              | 09/09/2025 | 09/12/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Create AWS Free Tier account <br> - Learn about AWS Console & AWS CLI <br> - **Practice:** <br>&emsp; + Create AWS account <br>&emsp;                                                                | 09/10/2025 | 09/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn basic budget types: <br>&emsp; + Cost budget <br>&emsp; + Usage budget <br>&emsp; + Saving plans budget <br>&emsp; + Reservation budget <br>                                                   | 09/11/2025 | 09/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Learn and practice:** <br>&emsp; + AWS support packages <br>&emsp; + Change AWS support packages <br>&emsp; + Manage support request                                                               | 09/12/2025 | 09/12/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Week 1 Achievements:

* Understood what AWS is and mastered the basic service groups:
* Compute: Used to run applications, process data, create virtual server environments or containers.
    Main services:
    EC2 (Elastic Compute Cloud): Virtual servers running on AWS (like VPS but more flexible).
    Lambda: Run serverless code, no server management needed, pay per execution.
    ECS/EKS (Elastic Container Service / Elastic Kubernetes Service): Manage containers (Docker/K8s).
    Elastic Beanstalk: Automatic app deployment (like PaaS)
* Storage: Store data, from small files to Big Data.
    S3 (Simple Storage Service): Object storage – used for files, images, videos.
    EBS (Elastic Block Store): Block storage (used with EC2 like hard drives).
    EFS (Elastic File System): File system storage, multiple machines can access simultaneously.
    Glacier: Long-term storage, cheap (for backup/archive).
* Networking:
    Connect services, secure and distribute applications.
    VPC (Virtual Private Cloud): Create private virtual network in AWS (like private data center).
    Route 53: DNS service, domain management.
    CloudFront: CDN distributes content faster to global users.
    ELB (Elastic Load Balancer): Load balancing between multiple servers.
    API Gateway: Manage and secure APIs.
* Database:
    Store and manage structured/unstructured data.
    RDS (Relational Database Service): Manage relational databases (MySQL, PostgreSQL, Oracle, SQL Server).
    Aurora: AWS-developed database, MySQL/PostgreSQL compatible, faster than RDS.
    DynamoDB: NoSQL Database (non-relational), high speed, auto-scaling.
    Redshift: Data warehouse for big data analysis.
    ElastiCache: Data caching (Redis/Memcached).

* Successfully created and configured AWS Free Tier account.

* Became familiar with AWS Management Console and learned how to find, access, and use services via web interface.

* Installed and configured AWS CLI on computer including:
  * Access Key
  * Secret Key
  * Default Region

* Used AWS CLI to perform basic operations such as:
  * Check account & configuration information
  * Retrieve list of regions
  * View EC2 service
  * Create and manage key pairs
  * Check information about running services
