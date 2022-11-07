# Infrastructure

This document describes the infrastructure needed to support DR and HA.

## AWS Zones
### Zone 1 Region: us-east-2
- us-east-2a
- us-east-2b
- us-east-2c
### Zone 2 Region: us-west-1
- us-west-1a
- us-west-1b

## Servers and Clusters

### Table 1.1 Summary
| Asset      | Purpose           | Size                                                                   | Qty                                                             | DR                                                                                                           |
|------------|-------------------|------------------------------------------------------------------------|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Asset name | Brief description | AWS size eg. t3.micro (if applicable, not all assets will have a size) | Number of nodes/replicas or just how many of a particular asset | Identify if this asset is deployed to DR, replicated, created in multiple locations or just stored elsewhere |
| EC2 Server | Web server        | t3.micro                                                               | 3 per region                                                     | Created in multiple locations: 3 instances deployed to each of Zone 1 and Zone 2 to allow DR failover       |
| RDS Cluster| Database cluster  | db.t2.small                                                            | 1 cluster (with 3 instances) per region                          | Clusters replicated between zones 1 and 2                                                                   |
| EKS Cluster| Monitoring        | Kubernetes cluster for Prometheus and Grafana                          | 1 cluster (with 2 nodes) per region                              | Created in multiple locations to allow DR failover                                                          |

### Descriptions
More detailed descriptions of each asset identified above.

## DR Plan
### Pre-Steps:
List steps you would perform to setup the infrastructure in the other region. It doesn't have to be super detailed, but high-level should suffice.

## Steps:
You won't actually perform these steps, but write out what you would do to "fail-over" your application and database cluster to the other region. Think about all the pieces that were setup and how you would use those in the other region
