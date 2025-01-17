# Infrastructure

This document describes the infrastructure needed to support DR and HA.
The existing infrastructure (before the addition of DR and HA) is summarized in Appendix 1 for reference. 

## AWS Zones
### Zone 1 Region: us-east-2
- us-east-2a
- us-east-2b
- us-east-2c
### Zone 2 (DR) Region: us-west-1
- us-west-1a
- us-west-1b

## Servers and Clusters

### Table 1.1 Summary
| Asset            | Purpose                     | Size          | Qty                                    | DR                                         |
|------------------|-----------------------------|-------------|----------------------------------------|--------------------------------------------|
| VPC              | Virtual Private Cloud       | N/A         | 1 per region                           | Created in multiple locations for failover |
| Internet Gateway | Expose Public Subnets       | N/A         | 1 per region                           | Created in multiple locations for failover |
| Private Subnet   | Private Subnet              | N/A         | 1 per availability zone in each region | Created in multiple locations for failover |
| Public Subnet    | Public Subnet               | N/A         | 1 per availability zone in each region | Created in multiple locations for failover |
| NAT Gateway      | Network Address Translation | N/A         | 1 per availability zone in each region | Created in multiple locations for failover |
| EIP              | Elastic IP Address          | N/A         | 1 per availability zone in each region | Created in multiple locations for failover |
| ALB              | Application Load Balancer   | N/A         | 1 per region                           | Created in multiple locations for failover |
| EC2 Server       | Web server                  | t3.micro    | 3 per region                           | Created in multiple locations for failover |
| RDS Cluster      | Database cluster            | db.t2.small | 1 cluster (3 instances) per region     | Clusters replicated between zones 1 and 2  |
| EKS Cluster      | Monitoring cluster          | t3.medium   | 1 cluster (2 nodes minimum) per region | Created in multiple locations for failover |

### Descriptions
#### VPC
Virtual Private Cloud -  secure isolated private cloud within a public AWS region.

#### Internet Gateway
Enables communication between VPC and the Internet.

#### Private Subnet
Logical subnet with no access to/from Internet.

#### Public Subnet
Logical subnet with access to/from Internet via Internet Gateway.

#### NAT Gateway
Network Address Translation service. Provides connectivity between Internet Gateway and private subnets. Hosted in a public subnet.

#### EIP
Elastic IP address (static IPv4 address) representing public address of NAT Gateway.

#### ALB
Application Load Balancer - distributes HTTP requests across EC2 web server instances.

#### EC2 Server
Amazon Elastic Compute Cloud (EC2) web server for running e-commerce application on Ubuntu.

#### RDS Cluster
AWS Relational Database Service (RDS) cluster using Amazon Aurora database engine.

#### EKS Cluster
AWS Elastic Kubernetes Service (EKS) cluster to support Prometheus monitoring and Grafana visualization.

## DR Plan
### Pre-Steps:
Steps to modify the infrastructure to support DR to Zone 2 and also meet HA requirements:
1. Create load balancer in Zone 1
2. Increase Zone 1 EC2 server count to 3, and route traffic to the servers via the Zone 1 load balancer
3. Increase Zone 1 RDS instances to 3
4. Increase EKS minimum node count to 2
5. Create load balancer in Zone 2
6. Create DR EC2 servers in Zone 2, and route traffic to the servers via the Zone 1 load balancer
7. Create DR RDS cluster in Zone 2
8. Set up replication between Zone 1 and Zone 2 RDS clusters and ensure that backup retention is set to 5 days
9. Create DR EKS cluster in Zone 2

Increasing server / node / instance counts should be done in such a way as to maximise the spread across availability zones within the region, for optimum availability. This requires that the VPC in the region has IPs in the availability zones.

### Steps:
Steps to "fail-over" the application and database cluster to Zone 2:
1. Change site DNS to point to Zone 2 load balancer instead of Zone 1
2. Failover RDS clusters such that Zone 2 becomes primary
3. Recreate database replication once Zone 1 database cluster has been fully brought back online. This is now from primary in Zone 2 to secondary in Zone 1

# Appendix: Original Infrastructure

The section describes the existing infrastructure before amending for DR and HA.

## AWS Zones
### Region: us-east-2
- us-east-2a
- us-east-2b
- us-east-2c

## Servers and Clusters
| Asset            | Purpose                     | Size        | Qty                                    |
|------------------|-----------------------------|-------------|----------------------------------------|
| VPC              | Virtual Private Cloud       | N/A         | 1                                      |
| Internet Gateway | Expose Public Subnets       | N/A         | 1                                      |
| Private Subnet   | Private Subnet              | N/A         | 1                                      |
| Public Subnet    | Public Subnet               | N/A         | 1                                      |
| NAT Gateway      | Network Address Translation | N/A         | 1                                      |
| EIP              | Elastic IP Address          | N/A         | 1                                      |
| EC2 Server       | Web server                  | t3.micro    | 1                                      |
| RDS Cluster      | Database cluster            | db.t2.small | 1 cluster (with 1 instance)            |
| EKS Cluster      | Monitoring cluster          | t3.medium   | 1 cluster (with 1 node minimum)        |
