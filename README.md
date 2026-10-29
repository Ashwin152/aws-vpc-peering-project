# \# AWS VPC Peering Project

# 

# \## 1. Project Overview

# 

# This project demonstrates private network communication between two Amazon Virtual Private Clouds (VPCs) using VPC Peering. It uses a bastion host for secure SSH access to private EC2 instances and demonstrates how route tables and security groups control connectivity.

# 

# \## 2. Objectives

# 

# \- Create two VPCs with non-overlapping CIDR ranges.

# \- Configure public and private subnets.

# \- Configure an Internet Gateway and route tables.

# \- Launch one bastion host and two private application instances.

# \- Establish VPC Peering and configure routes in both directions.

# \- Test SSH access and bidirectional ICMP connectivity.

# \- Demonstrate controlled connectivity failure and recovery.

# \- Verify that application instances have no public IPv4 addresses.

# 

# \## 3. Architecture and IP Address Plan

# 

# | Resource | CIDR / Private IP |

# |---|---|

# | VPC A (`ewb-vpc-a`) | `10.10.0.0/16` |

# | Public subnet A (`ewb-a-public`) | `10.10.1.0/24` |

# | Private subnet A (`ewb-a-private`) | `10.10.2.0/24` |

# | VPC B (`ewb-vpc-b`) | `10.20.0.0/16` |

# | Private subnet B (`ewb-b-private`) | `10.20.1.0/24` |

# | Bastion (`ewb-bastion`) | `10.10.1.10` |

# | App-A (`ewb-app-a`) | `10.10.2.10` |

# | App-B (`ewb-app-b`) | `10.20.1.10` |

# 

# \## 4. AWS Services and Tools

# 

# \- Amazon VPC

# \- VPC Peering

# \- Amazon EC2

# \- Internet Gateway

# \- Route Tables

# \- Security Groups

# \- Network ACLs

# \- Windows PowerShell and OpenSSH

# 

# \## 5. Network Configuration

# 

# VPC A uses an Internet Gateway for internet connectivity from its public subnet. The private subnet hosts App-A, while VPC B hosts App-B in its private subnet.

# 

# The VPC peering connection links VPC A and VPC B. Route tables on both sides contain routes to the opposite VPC's CIDR range.

# 

# Security groups control SSH and ICMP access. The bastion host provides the SSH entry point to the private instances. App-A and App-B do not have public IPv4 addresses.

# 

# \## 6. Implementation and Testing

# 

# 1\. Created VPC A and VPC B with non-overlapping CIDR ranges.

# 2\. Created the required public and private subnets.

# 3\. Configured the Internet Gateway and route tables.

# 4\. Created security groups and launched the EC2 instances.

# 5\. Established an active VPC peering connection.

# 6\. Configured routes for communication in both directions.

# 7\. Connected to the private instances through the bastion host.

# 8\. Verified ping connectivity from App-A to App-B and from App-B to App-A.

# 9\. Temporarily removed the ICMP inbound rule allowing traffic from App-A to App-B and verified 100% packet loss.

# 10\. Restored the ICMP rule and verified that connectivity recovered.

# 

# \## 7. Results

# 

# The lab demonstrated bidirectional private connectivity between the two VPCs. The controlled failure test showed that security group rules can block ICMP traffic, and restoring the required rule re-established connectivity.

# 

# The application instances remained private, without public IPv4 addresses.

# 

# \## 8. Screenshots and Evidence

# 

# The following evidence should be added to the `screenshots/` directory:

# 

# \- VPCs, subnets, and route tables

# \- Internet Gateway and active VPC peering connection

# \- Security groups and EC2 instance configuration

# \- Successful ping from App-A to App-B

# \- Successful ping from App-B to App-A

# \- Private instances without public IPv4 addresses

# \- Controlled ping failure

# \- Connectivity recovery after restoring the ICMP rule

# 

# \## 9. Security Considerations

# 

# \- Keep SSH access restricted to the current trusted public IP address.

# \- Access private EC2 instances through the bastion host.

# \- Do not expose private application instances directly to the internet.

# \- Use narrowly scoped security group rules.

# \- Never commit private keys, AWS credentials, or other secrets to GitHub.

# 

# \## 10. Project Status

# 

# \*\*Implementation and connectivity testing completed.\*\* Documentation, screenshots, repository publication, and AWS resource cleanup remain to be finalized.

# 

# \## 11. Conclusion

# 

# This project demonstrates how AWS VPC Peering enables private communication between isolated VPC networks without requiring public IP addresses on application instances. It also illustrates the importance of routing, security group configuration, bastion-based administration, and controlled testing for network troubleshooting.

