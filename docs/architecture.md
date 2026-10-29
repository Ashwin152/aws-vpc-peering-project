# AWS VPC Peering Architecture

## 1. Overview

The architecture consists of two Amazon VPCs connected through an active VPC Peering connection. VPC A contains a public subnet with a bastion host and a private subnet with App-A. VPC B contains a private subnet with App-B.

## 2. Network Topology

                         Internet
                            |
                      Internet Gateway
                            |
                  VPC A: 10.10.0.0/16
                            |
                    Public Subnet A
                     10.10.1.0/24
                            |
                    Bastion Host
                     10.10.1.10
                            |
                   Private Subnet A
                     10.10.2.0/24
                            |
                          App-A
                     10.10.2.10
                            |
                 VPC Peering Connection
                            |
                  VPC B: 10.20.0.0/16
                            |
                   Private Subnet B
                     10.20.1.0/24
                            |
                          App-B
                     10.20.1.10
```

## 3. VPC and Subnet Details

| Resource | IPv4 CIDR |
|---|---|
| `ewb-vpc-a` | `10.10.0.0/16` |
| `ewb-a-public` | `10.10.1.0/24` |
| `ewb-a-private` | `10.10.2.0/24` |
| `ewb-vpc-b` | `10.20.0.0/16` |
| `ewb-b-private` | `10.20.1.0/24` |

The CIDR ranges are non-overlapping, allowing the two VPCs to communicate through peering routes.

## 4. EC2 Instance Details

| Instance | Private IP | Public IPv4 |
|---|---|---|
| `ewb-bastion` | `10.10.1.10` | Assigned |
| `ewb-app-a` | `10.10.2.10` | None |
| `ewb-app-b` | `10.20.1.10` | None |

The bastion host is the administrative entry point. The two application instances remain private.

## 5. Route Tables

### Public route table: `ewb-rt-a-public`

| Destination | Target |
|---|---|
| `10.10.0.0/16` | Local |
| `0.0.0.0/0` | Internet Gateway |
| `10.20.0.0/16` | VPC Peering Connection |

### Private route table: `ewb-rt-a-private`

| Destination | Target |
|---|---|
| `10.10.0.0/16` | Local |
| `10.20.0.0/16` | VPC Peering Connection |

### VPC B route table: `ewb-rt-b-private`

| Destination | Target |
|---|---|
| `10.20.0.0/16` | Local |
| `10.10.0.0/16` | VPC Peering Connection |

## 6. Security Design

- The bastion security group permits SSH from the administrator's current public IP address.
- App-A and App-B permit SSH from the bastion's private IP address.
- ICMP inbound rules permit the required ping tests between the two application instances.
- Default outbound security group rules allow outbound traffic unless modified.
- The application instances have no public IPv4 addresses.

## 7. Traffic Flow

For a ping from App-A to App-B, traffic starts at `10.10.2.10`, follows the route for `10.20.0.0/16` through the peering connection, and reaches App-B at `10.20.1.10`. The reverse route enables replies to return to App-A.

The controlled failure test demonstrates that removing the appropriate inbound ICMP security group rule prevents the ping from succeeding. Restoring the rule allows connectivity to recover.

## 8. Region

AWS Region: Asia Pacific (Sydney), `ap-southeast-2`.