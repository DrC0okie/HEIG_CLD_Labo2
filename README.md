# CLD - LAB02 : App Scaling On Amazon Web Services

**Group U : A. David, T. Van Hove**

**Date : 06.04.2023**

**Teacher : Prof. Marcel Graf**

**Assistant : Rémi Poulard**

## Table of contents

[TOC]


## Introduction


This document descibes the successive steps necesseray to successfully complete the laboratory #1 of the CLD course. It will also allow our group to answer the various questions asked in the lab instructions. We decided to include the precise procedure for every step. This would be useful in case we have to do in again later.

TO DO : complete



## 1 Create a database using RDS

### 1.1 Estimated monthly cost for the database

Firstly, we will explore some of the features available when choosing an AWS RDS instance.

#### On demand vs reserved RDS instances

> In terms of compute options and configurations, Reserved Instances and On Demand instances are the same. The only difference between the two is that a Reserved Instance is one you rent (“reserve”) for a fixed duration, and in return you receive a discount on the base price of an On Demand instance.
>
> The issue with AWS On Demand vs. Reserved Instance Pricing is that Standard Reserved Instance pricing is fixed. If prices are reduced during the life of a one or three year contract (as they have been year-on-year during the past decade), customers see no benefit from the price reduction and are committed to paying the Reserved Instance price for the duration of the contract.
>
>  [Source](https://blogs.vmware.com/cloudhealth/aws-reserved-instances-vs-on-demand/)

Our RDS instance is an on demand one.

#### DB instance classes

The DB instance class determines the computation and memory capacity of an Amazon RDS DB instance. A DB instance class consists of both the DB instance type and the size. In our case we selected the db.t3.micro. It provides a baseline performance level, with the ability to burst to full CPU usage. These instance classes provide more computing capacity than the previous db.t2 instance classes. They are powered by the AWS Nitro System, a combination of dedicated hardware and lightweight hypervisor.

#### Single vs Multi AZ deployment

> Failures are rare, but as a best practice, applications should design around potential failures. The RDS Multi-AZ configuration is the recommended approach for production environments due to its ability to support low *RTO* (recovery time objective) and *RPO* (recovery point objective) requirements. RTO is the targeted amount of time for a recovery to complete in the event of failure. RPO is the targeted amount of time during which data is at risk for loss in the event of a failure.
>
> [Source](https://aws.amazon.com/blogs/database/amazon-rds-under-the-hood-single-az-instance-recovery/)

> In an Amazon RDS Multi-AZ deployment, Amazon RDS automatically creates a primary database (DB) instance and synchronously replicates the data to an instance in a different AZ. When it detects a failure, Amazon RDS automatically fails over to a standby instance without manual intervention.
>
> ![](figures/MultiAZ.png)
>
> [Source](https://aws.amazon.com/rds/features/multi-az/)

Additionally, it is possible to deploy RDS Multi-AZ with 2 readable standbys. It means that 2 standby DB instances instead of 1 to act as failover targets and serve read traffic, and can perform up to 2x faster transaction commits compared to Amazon RDS Multi-AZ with one standby.

In the other hand, with a single-AZ deployment, there is no such automatic failover mechanism.

You can watch an introduction video to RDS multi-AZ [here.](https://youtu.be/_MROZtLtCcA)

#### Pricing

The following estimated pricing is given when creating the RDS instance.

| Qty  | Instance type | Deployment option | Storage type | Storage capacity | Backup storage |
| ---- | ------------- | ----------------- | ------------ | ---------------- | -------------- |
| 1    | db.t3.micro   | Single AZ         | gp2          | 20GB             | no             |

![Estimated_monthly_costs](figures/RDS_MonthlyCosts.png)



### 1.2 Cost of RDS vs EC2 comparison

The following EC2 estimate pricing has been calculated with the new [AWS Pricing Calculator](https://calculator.aws/#/). We chose this calculator because the [AWS simple monthly calculator](https://calculator.s3.amazonaws.com/index.html) will be retired (Friday, March 31, 2023).

![EC2_Costs](figures/EC2_MonthlyCosts.png)

We can see that the RDS is more expensive that the corresponding EC2 instance (5.12 USD, so 30% more expensive). This difference can be explained by comparing the following table ([Source](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html#Welcome.Concepts.RDS)) :

| Feature                     | Amazon EC2 management | Amazon RDS management |
| --------------------------- | --------------------- | --------------------- |
| Application optimization    | Customer              | Customer              |
| Scaling                     | Customer              | AWS                   |
| High availability           | Customer              | AWS                   |
| Database backups            | Customer              | AWS                   |
| Database software patching  | Customer              | AWS                   |
| Database software install   | Customer              | AWS                   |
| OS patching                 | Customer              | AWS                   |
| OS installation             | Customer              | AWS                   |
| Server maintenance          | AWS                   | AWS                   |
| Hardware lifecycle          | AWS                   | AWS                   |
| Power, network, and cooling | AWS                   | AWS                   |

We can see that a lot of feature is managed by AWS instead of the customer (fully managed), and this management has an extra cost.



### 1.3 RDS vs EC2 analysis

We can see on the previous table that RDS is fully managed in comparison with an EC2 instance running a database engine. Now if we compare with on-premise management, it would be fully done by the user. So an IT manager would assume full responsibility for the server, operating system, and software.

#### RDS

RDS offers automatic backups and encryption. RDS takes full responsibility for the database. Configuration, management, maintenance, and security is automated by AWS. It allows to configure read replicas or set up synchronous replication across multiple AZ for enhanced performance and availability. [Source](https://serverguy.com/comparison/pros-cons-rds-vs-ec2-mysql-aws/)

#### EC2 with DB engine

It gives more flexibility and granularity on various aspects of the system. for example, it is possible to choose a specific OS running a specific version, or you could use EBS RAID and stripping configurations for higher performance. Finally EC2 costs less than RDS. [Source](https://serverguy.com/comparison/pros-cons-rds-vs-ec2-mysql-aws/)

In conclusion, it depends heavily on the needed DB flexibility, the nature of the data that is stored on the DB and the performance needed for the application. 



### 1.4 Database endpoint

````
gru-vanhove-drupal-db.c4ryp0xx1ffk.us-east-1.rds.amazonaws.com
````



## 2 Configure Drupal instance to use the RDS DB


## Part 3 : Create a custom virtual machine image

## Part 4 : Create a load balancer

## Part 5 : Lauch a second instance from the custom image

## Part 6 :  Test the distribued application

## Part 7 : Release cloud ressources
