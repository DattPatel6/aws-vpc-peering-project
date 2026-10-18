# AWS VPC Peering — Route Table Plan

## 1. Purpose

This document describes the three custom route tables required for the EWB AWS VPC Peering project.

Each route table must be explicitly associated with its intended subnet. The peering routes will be added after the VPC peering connection becomes active.

## 2. Public Route Table — VPC A

**Name:** `ewb-rt-a-public`

**Subnet association:** `ewb-a-public`

| Destination | Target | Purpose |
|---|---|---|
| `10.10.0.0/16` | `local` | Communication within VPC A |
| `0.0.0.0/0` | `ewb-igw-a` | Internet access for the bastion |
| `10.20.0.0/16` | VPC peering connection | Reach VPC B through the bastion |

The peering route will be added after the peering connection is active.

## 3. Private Route Table — VPC A

**Name:** `ewb-rt-a-private`

**Subnet association:** `ewb-a-private`

| Destination | Target | Purpose |
|---|---|---|
| `10.10.0.0/16` | `local` | Communication within VPC A |
| `10.20.0.0/16` | VPC peering connection | App-A to VPC B |

Initially, this route table contains only the automatically created local route. Add the peering route after establishing the connection.

Do not add an internet default route.

## 4. Private Route Table — VPC B

**Name:** `ewb-rt-b-private`

**Subnet association:** `ewb-b-private`

| Destination | Target | Purpose |
|---|---|---|
| `10.20.0.0/16` | `local` | Communication within VPC B |
| `10.10.0.0/16` | VPC peering connection | Return path to VPC A |

Initially, this route table contains only the automatically created local route. Add the return route after establishing the connection.

VPC B will not have an Internet Gateway or internet default route.

## 5. Why Both Directions Need Routes

When App-A sends traffic to App-B, the route table in VPC A must direct traffic for `10.20.0.0/16` to the peering connection.

App-B's reply must also have a valid route back to `10.10.0.0/16` through the same peering connection.

Without the return route, communication may fail even if the peering connection is active.

## 6. Required Subnet Associations

| Route table | Subnet |
|---|---|
| `ewb-rt-a-public` | `ewb-a-public` |
| `ewb-rt-a-private` | `ewb-a-private` |
| `ewb-rt-b-private` | `ewb-b-private` |

## 7. Validation Checklist

- [ ] Public route table is associated with the public subnet.
- [ ] VPC A private route table is associated with its private subnet.
- [ ] VPC B private route table is associated with its private subnet.
- [ ] Public default route targets the attached Internet Gateway.
- [ ] Peering route exists in the public route table.
- [ ] Forward route exists in the VPC A private route table.
- [ ] Return route exists in the VPC B private route table.
- [ ] Actual peering connection ID is recorded.

## 8. Project Status

Documentation is prepared. Actual routes, targets, subnet associations, and test results will be verified in the AWS Console during implementation.
