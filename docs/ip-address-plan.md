# IP Address Plan — AWS VPC Peering

## 1. Purpose

This document defines the IPv4 CIDR blocks, subnet ranges, and private IP addresses for the AWS VPC Peering project.

The project uses two VPCs with non-overlapping CIDR blocks so that private traffic can be routed between them through VPC peering.

## 2. VPC Configuration

| Resource | Name | IPv4 CIDR |
|---|---|---|
| VPC A | `ewb-vpc-a` | `10.10.0.0/16` |
| VPC B | `ewb-vpc-b` | `10.20.0.0/16` |

## 3. Subnet Configuration

| Subnet | VPC | IPv4 CIDR | Purpose |
|---|---|---|---|
| `ewb-a-public` | VPC A | `10.10.1.0/24` | Public bastion host |
| `ewb-a-private` | VPC A | `10.10.2.0/24` | Private App-A |
| `ewb-b-private` | VPC B | `10.20.1.0/24` | Private App-B |

## 4. EC2 Instance Address Plan

| Instance | Subnet | Private IPv4 | Public IPv4 |
|---|---|---|---|
| `ewb-bastion` | `ewb-a-public` | `10.10.1.10` | Yes |
| `ewb-app-a` | `ewb-a-private` | `10.10.2.10` | No |
| `ewb-app-b` | `ewb-b-private` | `10.20.1.10` | No |

The public IPv4 address of the bastion host will be recorded after the instance is launched.

## 5. Network Resource Names

| Resource | Name |
|---|---|
| Internet Gateway | `ewb-igw-a` |
| Public Route Table | `ewb-rt-a-public` |
| VPC A Private Route Table | `ewb-rt-a-private` |
| VPC B Private Route Table | `ewb-rt-b-private` |
| Bastion Security Group | `ewb-sg-bastion` |
| App-A Security Group | `ewb-sg-app-a` |
| App-B Security Group | `ewb-sg-app-b` |
| VPC Peering Connection | `ewb-a-b-peer` |
| EC2 Key Pair | `ewb-lab` |

## 6. Why the VPC CIDRs Must Not Overlap

VPC A uses `10.10.0.0/16`, while VPC B uses `10.20.0.0/16`.

These ranges are non-overlapping. Each subnet belongs to the CIDR range of its own VPC. Non-overlapping VPC ranges allow the peering connection to route traffic between the two networks without ambiguous destination ranges.

## 7. Planned Traffic Flow

### Administration

Laptop → Bastion Host → App-A or App-B

SSH access to the private instances will use the bastion host as the connection point.

### Application Connectivity

App-A (`10.10.2.10`) ↔ VPC Peering ↔ App-B (`10.20.1.10`)

Application traffic will use private IPv4 addresses and the required routes and security group rules.

VPC B will not receive internet access through VPC A's Internet Gateway.

## 8. Project Status

- [x] VPC and subnet CIDRs planned
- [x] Private IP addresses planned
- [x] Resource names defined
- [ ] AWS resources created and verified
- [ ] Peering routes configured
- [ ] Connectivity tests completed

Actual AWS resource IDs and test evidence will be recorded during implementation.
