# AWS VPC Peering Project

## Project Overview

This project demonstrates how to establish private network connectivity between two Amazon Virtual Private Clouds (VPCs) using AWS VPC Peering.

The project uses three EC2 instances: one public bastion host for administration and two private application servers. The application servers communicate with each other through their private IPv4 addresses over the VPC peering connection.

## Objectives

- Create two VPCs with non-overlapping CIDR blocks.
- Configure public and private subnets.
- Create and associate custom route tables.
- Configure an Internet Gateway for the public bastion host.
- Configure security groups with restricted inbound access.
- Establish and activate a VPC peering connection.
- Configure forward and return routes.
- Verify private connectivity using ping and SSH.
- Demonstrate a controlled connectivity failure and recovery.
- Document the implementation, test results, and cleanup.

## Architecture and IP Address Plan

| Resource | CIDR / Private IP |
|---|---|
| VPC A | `10.10.0.0/16` |
| Public Subnet A | `10.10.1.0/24` |
| Private Subnet A | `10.10.2.0/24` |
| VPC B | `10.20.0.0/16` |
| Private Subnet B | `10.20.1.0/24` |
| Bastion Host | `10.10.1.10` |
| Application Server A | `10.10.2.10` |
| Application Server B | `10.20.1.10` |

The bastion host will have a public IPv4 address. Both application servers will remain private, without public IPv4 addresses.

## AWS Resources

- Two VPCs
- Three subnets
- One Internet Gateway
- Three custom route tables
- Three security groups
- Three EC2 instances
- One VPC peering connection
- One EC2 key pair

## Implementation Plan

1. Create the GitHub repository and document the project.
2. Create VPC A and its public and private subnets.
3. Create VPC B and its private subnet.
4. Configure the Internet Gateway, route tables, and subnet associations.
5. Configure security groups and launch the EC2 instances.
6. Configure SSH access through the bastion host.
7. Create and accept the VPC peering connection.
8. Add the required forward and return routes.
9. Test connectivity and demonstrate controlled failure and recovery.
10. Capture evidence, prepare the final report, and clean up AWS resources.

## Validation Plan

The following tests will be performed during implementation:

- **Baseline test:** Ping from App-A to App-B before peering routes are configured; no replies are expected.
- **Forward connectivity:** Ping from App-A to App-B after peering and routes are configured.
- **Reverse connectivity:** Ping from App-B to App-A.
- **Private SSH:** Connect to App-B through the bastion host.
- **Isolation:** Confirm that App-A and App-B have no public IPv4 addresses.
- **Controlled failure:** Temporarily remove the required inbound ICMP rule, repeat the ping test, restore the rule, and verify recovery.

Actual results and screenshots will be added after each test. Expected results are not recorded as completed tests.

## Security Considerations

- Restrict SSH access to the required source IP addresses.
- Permit application-server SSH access only from the bastion host's private IP.
- Use private IP addresses for application-server communication.
- Never commit private keys, AWS credentials, passwords, or access tokens to GitHub.
- Delete project resources after saving the required evidence.

## Project Status

**Current stage:** Repository created; documentation in progress.

## Learning Outcomes

This project will demonstrate practical knowledge of AWS VPC networking, subnet design, route tables, security groups, VPC peering, SSH ProxyJump, and connectivity troubleshooting.

## References

- EWB Cloud Labs — AWS VPC Peering Student Project Workbook, October 2026.
- [Amazon VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [VPC Peering Documentation](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)
