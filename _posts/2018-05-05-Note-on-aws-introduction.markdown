---
layout: post
title: Note on AWS Introduction / AWS基础 笔记
date: 2018-05-05 14:02
categories: [blog]
tags: [AWS, Note]
description:
---
## AWS Could Concepts

### With Could:
* Initiated within seconds.
* Treat these as "temporary and disposable"
* Free from inflexibility and constraints.
* More agile and efficient

### Agility:
Speed, Experimentation, Culture of innovation

###  The AWS Infrastructure
* Elasticity, scalability, and reliability of computing resources
* Regions
  1.  Physical location in the world
  2.  Contains multiple Available Zones.
* Available Zones(AZs)
  1.  One or more discrete data centers
  2.  Redundant: power/networking/connectivity
  3.  Housed in separate facilities

## Core Services

### Services & Categories:

Compute, Storage, Database, Networking & Content Delivery, Security.

### AWS Global Infrastructure

1. Regions

A region may have one or more Availability Zones.

2. Availability Zones
 * Collection of data centers within a region. Each AZ physically distinct.
 * Isolate - protect from failures
 * Best practice: multiple availability zones.

3. Edge locations
  * Content delivery network - Amazon CloudFront.
  * Delivery content to customers.
  * Delivered faster.

###  Amazon Virtual Private Cloud(VPC)

1. A private, virtual network in the AWS Cloud.
 * Use same concepts as on premise networking.

2. Allows complete control of network configuration.
  * Ability to isolate and expose resources inside VPC.

3. Offers several layers of security controls
  * Ability to allow and deny specific internet and internal traffic.

4. Other AWS services deploy into VPC
  * Services inherent security built into network.

### Features:

1. Builds upon high availability of AWS Regions and Availability Zones(AZ)
  * Amazon VPC lives within a Region
  * Multiple VPCs per account

2. Subnets
  * Used to divide Amazon VPC
  * Allows Amazon VPC to span multiple AZs

3. Route tables
  * Control traffic going out of the subnets.

4. Internet Gateway(IGW)
  * Allows access to the Internet from access Internet.

5. NAT Gateway
  * Allows private subnet resources to access Internet.

6. Network Access Control Lists(NACL)
  * Control access to subnets; stateless.


### Example VPC:
1. Select a region.
2. Create a VPC, give a name like `Test-VPC`. and find the ip address of the VPC, like 10.0.0.0/16
3. Create a subnet, named `A1`, assign ip to 10.0.0.0/24
4. Create another subnet, named `B1`, assign ip to 10.0.2.0/24
5. Add an Internet gateway called `Test-IGW`, bridging A1 and public Internet.
6. A1 will become to a public subnet. B1 will be our private subnet.

### Security Groups

* One of the highest priorities
* Built-in firewalls
* Accessibility

### Create a Security Group:

1. Go to EC2 -> Network & security -> Security Groups
2. Click `Create Security Group`
3. You may configure inbound/outbound here.

### Compute services

* Broad Catalog of Compute Services:
  1. Virtual private servers
  2. Serverless computing

* AWS: Flexible and Cost effictive

* AWS EC2: Flexible configuration and control.

* AWS Lambda: Pay only for what you use. No administration.

* Amazon Lightsail?

* Amazon ECS: Managed Containers.

### Amazon Elastic Compute Cloud(EC2)

* Instances:
  * pay as you go
  * broad selection of HW/SW
  * Global hosting

* Build and configure EC2 instance:
 1. Login to AWS console.
 2. Choose a region
 3. Launch EC2 Wizard.
 4. Select AMI(SW)
 5. Select instance type(HW)
 6. configure network
 7. configure Storage
 8. configure key pairs
 9. Launch & connect

> When using putty: default user: ec2-user @ DNS

### AWS Lambda

* Fully-Managed serverless compute
* Event-driven execution
* Sub-second metering
* Multiple languages supported


**Getting start:**

1. Upload your code to AWS Lambda.
2. Set up code to trigger from other AWS services, HTTP endpoints or in-app activity.
3. Lambda runs your code only when triggered, using only the compute resources needed.
4. Pay just for the compute time you use.

**Demo: Image Recognition Demo**

1. User captures an image for their property listing.
2. The mobile app uploads the new image to S3.
3. A Lambda function is triggered and calls Recognition.
4. Recognition retrieves the image from S3 and returns labels for the detected property and amenities.

### AWS Elastic Beanstalk

Elastic Beanstalk provides: Application service, HTTP service, Operating system, Language interpreter, Host.

1. Platform as a Service.
2. Allows quick deployment of your applications
3. Reduces management complexity
4. Keeps control in you hands.
  * Choose your instance Type
  * Choose your Database
  * Set and adjust Auto Scaling.
  * Update your application
  * Access server log files
  * Enable HTTPS on load balancer
5. supports a large range of platforms
6. Easily implemented.
7. Update your application as easily as you deploy it.

### Application Load balancer

**Supported Protocols:**
HTTP, HTTPS, HTTP/2, and WebSockets.

**CloudWatch Metrics:**
Additional load balance metrics and Target Group metric dimension

**Access Logs:**
Ability to see connection details for WebSocket connection

**Health Checks:**
Insight into target and application health at more granular level.

**Additional Features:**

* Path and Host-based Routing: Path-based provides rules that forward requests to different target groups; Host-based can be used to define rules that forward requests to different target groups based on host name.
* Native IPv6 Support
* AWS WAF
* Dynamic Ports: Amazon ECS integrates with Application Load balancer to expose Dynamic Ports utilized by scheduled containers.
* Deletion Protection & Request Tracing: Request tracing can be used to track HTTP requests from clients to target.

### Elastic Load balancer(Classical load balancer)

* Multiple Availability zones
* Sticky Sessions
* Cross-zone Balancing
* Health Checks

**Use Cases:**

* Access through single point
* Decouple application environment
* Provide high availability and fault tolerance
* Increase elasticity and scalability

> If you are distributing HTTP/HTTPS requests, then it uses simple Round Robin for these requests.

> If you are distributing TCP requests, then it uses Least Outstanding requests for the backend instances.

> Elastic Load balancer also helps distributing traffic across different AZs.

**Monitoring:**

* View HTTP responses
* See number of healthy and unhealthy hosts.
* Filter metrics based on AZs or load balancer.

### Auto Scaling

Auto Scaling helps you ensure that you have the correct number of Amazon EC2 instances available to handle the load for your application.

Auto Scaling adjusting capacity as needed.

**Auto Scaling Components:**

1. Launch configuration (What)
   * AMI
   * Instance Type
   * Security Groups
   * Roles

2. Auto Scaling Group (Where)
  *  VPC and subnets
  *  Load balancer
  *  Minimum instances
  *  Maximum instances
  *  Desired capacity

3. Auto Scaling Policy (When)
  * scheduled
  * On-demand
  * Scale-out Policy
  * Scale-in Policy

### Amazon Elastic Block Store(EBS)

**EBS Volumes:**

* Choose between HDD and SSD types
* Persistent and customizable block storage for EC2 Instances
* Replicated in the same Availability Zone.
* Backup using Snapshots.
* Easy and Transparent Encryption
* Elastic Volumes

### Amazon Simple Storage Services(S3)

Managed could storage service
Store virtually unlimited number of objects.
Access any time, from anywhere
Rich security controls

Common Use cases:
Storing Application Assets.
Static Web hosting
Backup & Disaster Recovery
Staging area for Big Data
Many more...

### Amazon Glacier (Data archiving solution)

* Long-term storage at low cost.
* 99.999999999% durability
* Access limited by vault policies.

**Lifecycle Policy:**

Amazon S3 Standard ---30 days---> Amazon S3 Standard - Infrequent Access ---60 days---> Amazon Glacier---365 days---> Deleted.

**Data Retrieving:**

Bulk: 5-12 hours; Standard: 3-5 hours; Expedited: 1-5 minutes.

**Security:**

* Control access with AWS Identity and Access management(IAM)
* Glacier Encrypt your data with AES-256
* Glacier manages your keys for you.

### Amazon Relational Database Services(RDS)

> RDS is a managed service that sets up and operates a relational database in the cloud.

* You manage: Application optimization
* AWS manages: OS installation and patches, database software install and patches, database backups, high availability, scaling, power and rack & stack, server maintenance.

**Amazon RDS Read Replicas:**

* Asynchronous replication method used.
* Offload read queries from the master DB instance
* Ideal for read-heavy database workloads.
* Read replica can be promoted to Master if needed.

**Use cases:**

* Web and mobile applications:
 * High throughput
 * Massive storage scalability
 * High availability
* E-commerce Applications:
 * Low-cost Database
 * Data Security
 * Fully managed solution
* Mobile and online games:
 * Rapidly grow capacity
 * Automatic Scaling
 * Database Monitoring

### Amazon DynamoDB

* NoSQL database tables as a service
* Store as many items as you want
* Items may have differing attributes
* Low-latency queries
* Scalable read/write throughput

**Common Use cases:**

Web, Mobile Apps, Internet of Things, Ad Tech, Gaming.

### Amazon Redshift

**Use Cases:**

* Enterprise data warehouse(EDW)
 * Migrate at a pace that customers are comfortable with.
 * Experiment without a large upfront cost or commitment.
 * Respond faster to business needs.

* Big Data:
 * Low price point for small customers.
 * Managed service for ease of deployment and maintenance
 * Focus more on data and less on database management.

* SaaS:
 * Scale the data warehouse capacity as demand grows.
 * Add analytic functionality to application.
 * Reduce hardware and software costs by an order of magnitude.

### Amazon Aurora

Fast and available, Simple, Compatible, Pay-as-you-go, Managed service.

### AWS Trusted Advisor

* Keeping track of your AWS resources.
* Trusted Advisor provides best practice(or checks) in four categories: Cost optimization, Performance, Security, Fault Tolerance.

**Benefits to Trusted Advisor**

Over 50 million recommendations provided to AWS customers.
Resulted in $500M+ in cost savings for users of Trusted Advisor.


## Security

### Introduction

**Keep your data safe:**

* Resilient Infrastructure, High security, Strong Safeguards.
* Continual improvement:
* Rapid innovation, Constantly evolving security services.
* Pay for what you need:
* Meet needs at lower operational costs.
* Meet Compliance Requirements:
* Governance-enable features.
* AWS Shared Responsibility Model:
* Inherit AWS security controls.
* Layer your controls.
* Security Products and Features:
* Tools from AWS and partners.
* Monitoring and logging.
* Network Security:
* Built-in firewalls.
* Encryption in transit.
* Private/Dedicated connections.
* DDOS mitigation.
* Inventory and configuration management:
* Manage creation/decommissioning of AWS resources.
* Inventory and configuration Tools.
* Template definition and tools.
* Data Encryption:
* Encryption capabilities for AWS storage/database services.
* Key management options.
* Hardware-based cryptographic key storage options.
* Access Control and management:
* Identity and access management(IAM).
* Multifactor authentication.
* Integration and federation with corporate directories.
* Monitoring and logging:
* Deep visibility into API calls.
* Log aggregation and options.
* Alert notifications.
* Reduce your risk profile.
* AWS Marketplace:
* Partner security Products.
* Complement AWS feature and tools.

### AWS Shared Responsibility Model

Customer and AWS Share Responsibility for securing data.

* AWS: security of the cloud.
* Customer: security in the cloud. Full control over security measures.

### AWS Access control and Management

* Control access to AWS resources.
* Authentication: Who can access resources.
* Authorization: How they can use resources.
* Manage Compute, Storage, Database, Application services.

**Use IAM, you can:**

create users and groups.

**Use permissions**

Manage users and their access.
Manage roles and their permissions.
Manage federated users and their permissions.

**AWS Account root user recommendations:**

* Delete root user access keys
* create an IAM user.
* Grant administrator access
* Use those credentials.

**AWS Authentication:**

* Programmatic access:
  * Enables access key ID/secret access key
  * AWS API, CLI, SDK, etc.

* Management Console access:
  * Sign-in to the AWS Management Console.
  * Use AWS account name and password.
  * MFA prompts for code.

**IAM Authorization:**

* Have to be authoried to access an AWS service.
* AWS IAM Policy, can be assigned to IAM User, IAM group or IAM Roles.


### AWS Security Compliance Programs

* AWS Compliance Approach:
* Shared Responsibility Model:
  * AWS/customers share control
* AWS provides:
  * Highly secure and controlled platform.
  * Wide array of security features.
* Customer:
  * configuring their IT.

**AWS Security Information Sharing:**

* Obtaining industry certifications.
* Publishing security and control practices.
* Providing documentation directly under NDA.

**Assurance Programs:**

* certifications/Attestations.
* Laws, Regulations, and Privacy.
* Alignments/Frameworks.

**AWS Risk and Compliance Programs:**

* Information about AWS controls.
* Assist customers in documenting their framework.

**Risk Management:**

  * Strategic business plan includes risk management.
  * Re-evaluated at least biannually.
  * Identify risks.
  * Implement appropriate measures.
  * Additional internal/external risk asseements.

**Control environment:**

  * Includes policies, processes, control activities.
  * Secure delivery of AWS' service offering.
  * Supports the operating effectiveness of AWS' control framework.
  * integrates controls by leading cloud computing industry bodies.
  * AWS monitors for leading practices.

**Information Security:**

  * Designed to protect: Confidentiality, Integrity, Availability.
  * Security whitepaper.

**Customer Compliance:**

  * Governance over the entire IT control environment
  * Understanding of required Compliance objectives.
  * Establishment of a control environment.
  * Validation based risk tolerance.
  * Verification of effectiveness

**One Approach:** Review, Design, Identify, Verify.


## Architecting

### Well-Architected Framework

The Five Pillars: Security, Reliability, Performance Efficiency, Cost optimization, Operation Excellence.

**Security Pillar:**

* Identity and access management.
* Detective controls.
* Infrastructure protection.
* Data Protection.
* Incident response.

**Security - Design Principles:**

* Security at ALL layers.
* Enable traceability.
* Principle of least privilege.
* Focus on securing your system.
* Automate.

**Reliability Pillar:**

* Recover from issues/failures.
* Foundations.
* Change management.
* failures management.
* Anticipate, response, prevent.

**Reliability - Design Principles:**

* Testing Recovery.
* Automatically recover.
* Scale horizontally.
* Stop guessing capacity.
* Manage change in automation.

**Performance Efficiency Pillar:**

* Selection: customizable solution
* Review: Continually innovate
* Monitoring: AWS services
* Tradeoffs:

**Performance Efficiency - Design Principles:**

* Democratize advanced technologies.
* Global in minutes.
* Serverless architecture.
* Experiment.
* Mechanical sympathy.

**Cost optimization Pillar:**

* Cost effective resources: Most appropriate.
* Matching supply with demand: Elasticity in the cloud
* Expenditure awareness: See, understand, breakdown.
* Optimizing over time.

**Cost optimization - Design Principles:**

* Consumption Model.
* Measure overall efficiency.
* Stop with data center operations.
* Analyze and attribute.
* Managed series.

**Operation Excellence Pillar:**

* Managing and automating
* Response
* Defining the standards.
* Whitepaper and additional information is coming soon.

### Fault Tolerance & High Availability

* Fault Tolerance
 * Remain operational even if components fail.
 * Built-in redundancy of an application's components.
* High Availability
 * Always functioning and accessible.
 * Downtime is minimized.
 * Without human intervention.
* AWS:
 * Build fault-tolerant, highly available systems
 * Minimal human interaction.
 * Minimal up-front financial investment.

**On premise VS. AWS**

* Traditional:
  * Very expensive.
  * Only mission-critical apps.

*AWS:
  * Multiple servers
  * AZ's
  * Regions.
  * Fault tolerant services.

**High Availability Services Tools:**

* Elastic load balancer.
* Elastic IP addresses
* Amazon Route 53
* Auto Scaling.
* Amazon CloudWatch

**Fault Tolerance Tools:**

* Amazon simple queue service
* Amazon simple storage service
* Amazon simple DB
* Amazon relational database service.

### Web Hosting

**Common web applications:**

* Company website
* Content management system
* Social media app development
* Internal sharepoint site.

**Cost effictive:**

* On-demand provisioning
* Prevents wasted capacity
* Continuously adjust to actual traffic patterns.

**Scalable:**

* Auto-scaling prevents response failures
* Utilized in minutes.
* Scalability goes both up and down.

**On-demand**

* Provision testing fleets.
* Develop staging in minutes.
* Simulate user traffics.

**Key architecture considerations:**

* No more physical network appliances.
* Firewalls everywhere
* Consider multiple data centers

## Pricing and Support

### Fundamental

**Utility Pricing:**

* Pay as you go
  * No large up front expenses
  * Lower variable costs.
  * Pay only for what you use
  * Pay only as long as you need

* Pay less when you reserve
  * Save up to 75%
  * All up-front(AURI): largest discount
  * Partial up-front(PURI): lower discounts.
  * No up-front(NURI): smaller discount.

* Pay even less per unit by using more.
  * Saving as usage increase.
  * Tiered pricing for services such as S3, EC2.
  * Data transfer IN is always free of charge.
  * Storage service options to help you lower pricing.

* Pay even less as AWS grows.
  * AWS is focused on lowering cost of doing business.
  * Results in AWS passing savings back to you.

**AWS Free Tier:**

* Get started in the cloud.
* Limitations: New customers, Up to 1 year, Certain service and options.

**No extra costs:**

* Amazon VPC
* AWS Elastic Beanstalk
* AWS CloudFormation.
* AWS IAM
* Auto Scaling.
* AWS OpsWorks.

### Pricing Details

**Fundamentals:**

* Pay for:
  * compute
  * Storage
  * Data transfer out.

* No charge:
  * Data transfer in
  * Data transfer between

* Aggregated outbound charge


**Services pricing considerations:**

* Amazon EC2
 * Resizable compute capacity in the Cloud
 * Configure capacity with minimal friction
 * Complete control
 * Charge only for capacity used.

* Amazon S3
 * Storage for the Internet.
 * Interface to store and retrieve data at any time, from anywhere.

* Amazon EBS
 * Block level storage from instance
 * Independently from the instance.
 * Analogous to virtual disks in the cloud.
 * Three volume types: General Purpose(SSD), Provisioned IOPS(SSD), Magnetic.

* Amazon RDS
 * Relational database in the cloud.
 * Cost-efficient and resizable capacity
 * Manages time-consuming admin tasks

* Amazon CloudFront
 * Web service for content delivery.
 * Distribute content to end users: low latency, High data transfer speed. No minimum commitments.
