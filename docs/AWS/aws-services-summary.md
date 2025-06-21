---
title: AWS Services Overview
sidebar_position: 1
---

# AWS Services Overview

This document summarizes a wide range of AWS services, their use cases, security responsibilities, and pricing models. It is intended as a reference guide for preparing cloud architecture, certification, or technical evaluations.

## Categories Covered
- **Security and Identity Management**
- **Compute**
- **Storage**
- **Databases**
- **Networking & CDN**
- **Migration & Transfer**
- **Analytics**
- **Machine Learning**
- **Developer Tools**
- **Management & Governance**

---

### 🔐 Security & IAM
- **AWS WAF**: Web Application Firewall to block SQL injection/XSS before traffic reaches origin. Works with CloudFront, ALB, API Gateway, AppSync.
- **AWS Secrets Manager**: Manage secrets like DB credentials, OAuth tokens across lifecycle.
- **IAM & SCP**: Define permissions for users, roles, and groups across organization.
- **AWS KMS**: Encrypt EBS volumes, S3, RDS, etc. via managed keys.
- **AWS CloudHSM**: Dedicated hardware security module inside your VPC.
- **Amazon Cognito**: User identity & token service for web/mobile apps.
- **Amazon GuardDuty**: Threat detection and continuous monitoring.
- **Amazon Macie**: ML-based PII & sensitive data discovery in S3.
- **AWS Shield / Route 53 / ELB / WAF**: Built-in DDoS protection.
- **AWS Security Hub**: Unified view of security posture and compliance.
- **AWS Trusted Advisor**: Recommends security best practices (overly permissive rules, unused access keys, etc).

---

### ⚙️ Compute Services
- **EC2**: Pay-as-you-go VM with flexible instance types.
- **Lambda**: Serverless function based on execution count/time.
- **Elastic Beanstalk**: PaaS, simplified deployment for web apps.
- **AWS Fargate**: Serverless containers, integrated with ECS/EKS.
- **Lightsail**: Simplified VPS for small-scale use.
- **OpsWorks**: Infrastructure automation via Chef/Puppet.

---

### 🗃️ Storage
- **Amazon S3**: Object storage with lifecycle management, versioning, encryption.
- **EBS**: Block-level storage for EC2. AZ-scoped, snapshots stored in S3.
- **Amazon EFS**: Shared file system for EC2, supports TLS encrypted transfer.
- **Amazon Glacier**: Archive storage for long-term backups.
- **AWS Storage Gateway**: Bridge between on-prem and cloud storage.

---

### 🧩 Database Services
- **Amazon RDS**: Relational DB with multi-AZ, backups, failover.
- **Aurora**: MySQL/PostgreSQL-compatible high-performance DB.
- **DynamoDB**: NoSQL key-value and document DB with millisecond latency.
- **DocumentDB**: MongoDB-compatible document database.
- **Redshift**: Fully managed data warehouse. Also supports Redshift Serverless.

---

### 🌐 Networking & Content Delivery
- **VPC**: Private cloud network. Peering, NAT Gateway, Virtual Gateway, etc.
- **CloudFront**: CDN that integrates with WAF, Shield, ACM.
- **Route 53**: DNS and health check service.
- **API Gateway**: Fully managed API proxy layer. Supports HTTPS.
- **NAT Gateway**: Secure outbound internet access from private subnet.

---

### 📦 Messaging & Integration
- **Amazon SQS**: Fully managed message queueing service.
- **Amazon SNS**: Pub/sub messaging to multiple endpoints.
- **AWS Step Functions**: Serverless orchestration of Lambda and other services.

---

### 🔄 Migration & Disaster Recovery
- **CloudEndure Migration/Disaster Recovery**: Lift-and-shift tools across clouds and on-prem.
- **DMS**: Database Migration Service for RDS, Redshift, etc.
- **AWS Control Tower**: Multi-account governance automation.

---

### 📊 Analytics
- **Amazon Kinesis**: Real-time data stream ingestion & analytics (e.g., F1 telemetry).
- **Amazon Athena**: Serverless SQL queries over S3.
- **Amazon EMR**: Big data processing using Spark, Hive, Presto.
- **Amazon QuickSight**: BI dashboard and reporting.
- **CloudSearch**: Managed full-text search engine.

---

### 🤖 Machine Learning
- **Amazon SageMaker**: Train and deploy ML models at scale.
- **Amazon Comprehend**: NLP to extract insights from text.
- **Amazon Polly**: Converts text to natural speech.

---

### 🧑‍💻 Developer & DevOps Tools
- **Cloud9**: Web-based IDE.
- **CodeArtifact**: Package manager integration and secure artifact storage.
- **EC2 Image Builder**: Automate VM/container image creation.

---

### 🧭 Governance & Cost Management
- **AWS Well-Architected Tool**: Reviews workloads against 5 pillars.
- **AWS Config**: Resource compliance auditing.
- **AWS Systems Manager**: Ops center for hybrid/cloud management.
- **AWS CloudTrail**: Event logging and audit history.
- **AWS Concierge**: Enterprise support and billing help.
- **AWS Personal Health Dashboard**: Service health updates tailored to your environment.
- **AWS Assurance Program**: Compliance and certifications summary.

---

### 💸 Pricing Concepts (by category)
- **Compute**: EC2, Lambda, Beanstalk – charged by usage/time.
- **Storage**: S3, EBS, Glacier – charged by volume, request type.
- **Databases**: RDS, DynamoDB, Redshift – instance size + I/O.
- **Network/CDN**: VPC, CloudFront, Route 53 – data transfer + query cost.
- **Security**: IAM free, Shield Advanced paid, KMS by usage.
- **Monitoring**: CloudTrail basic free, Config billed by evaluations.
- **Analytics**: Athena (per query size), EMR (per instance-hour), Kinesis (per shard or data).

---

This reference is maintained for AWS service orientation and cloud architecture preparation.

