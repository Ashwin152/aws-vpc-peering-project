# AWS VPC Peering Project

## Project Overview
This project demonstrates private connectivity between two Amazon Virtual Private Clouds (VPCs) using VPC Peering.

## Objectives
- Create two VPCs with non-overlapping CIDR ranges.
- Configure public and private subnets.
- Configure an Internet Gateway, route tables and security groups.
- Launch three EC2 instances: one bastion host and two private application servers.
- Establish VPC Peering and configure forward and return routes.
- Test connectivity, controlled failure and recovery.

## Architecture and IP Address Plan

| Resource | CIDR / Private IP |
|---|---|
| VPC A | 10.10.0.0/16 |
| Public subnet A | 10.10.1.0/24 |
| Private subnet A | 10.10.2.0/24 |
| VPC B | 10.20.0.0/16 |
| Private subnet B | 10.20.1.0/24 |
| Bastion host | 10.10.1.10 |
| App-A | 10.10.2.10 |
| App-B | 10.20.1.10 |

## AWS Services
Amazon VPC, VPC Peering, Amazon EC2, Internet Gateway, Route Tables and Security Groups.

## Validation
- Baseline ping failure before peering.
- Successful forward and reverse ping tests.
- SSH access to App-B through the bastion host.
- Verification that both application servers have no public IPv4 address.
- Controlled ICMP failure and recovery.

## Screenshots and Evidence
Screenshots will be added after the corresponding AWS tasks have been completed and verified.

## Project Status
In Progress


