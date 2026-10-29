\# AWS VPC Peering Implementation and Testing



\## 1. Objective



To establish private communication between two Amazon VPCs using VPC Peering and validate connectivity, access control, and recovery from a controlled network failure.



\## 2. Implementation Steps



\### Step 1: Create VPC A



Created `ewb-vpc-a` with CIDR `10.10.0.0/16`.



Configured:

\- Public subnet: `ewb-a-public` (`10.10.1.0/24`)

\- Private subnet: `ewb-a-private` (`10.10.2.0/24`)

\- Internet Gateway: `ewb-igw-a`

\- Route tables: `ewb-rt-a-public` and `ewb-rt-a-private`



\### Step 2: Create VPC B



Created `ewb-vpc-b` with CIDR `10.20.0.0/16`.



Configured:

\- Private subnet: `ewb-b-private` (`10.20.1.0/24`)

\- Route table: `ewb-rt-b-private`



\### Step 3: Launch EC2 Instances



Launched three EC2 instances:



| Instance | Private IP | Purpose |

|---|---|---|

| `ewb-bastion` | `10.10.1.10` | Administrative SSH entry point |

| `ewb-app-a` | `10.10.2.10` | Private application host in VPC A |

| `ewb-app-b` | `10.20.1.10` | Private application host in VPC B |



App-A and App-B do not have public IPv4 addresses.



\### Step 4: Configure Security Groups



Configured security groups to control SSH and ICMP traffic.



\- Bastion: SSH from the administrator's current public IP `/32`.

\- App-A: SSH from the bastion private IP and ICMP from App-B.

\- App-B: SSH from the bastion private IP and ICMP from App-A.



\### Step 5: Establish VPC Peering



Created the VPC Peering connection between `ewb-vpc-a` and `ewb-vpc-b`.



Peering connection ID: `pcx-0ac60bde27e3c0562`



Verified that the connection reached the active state.



\### Step 6: Configure Routes



Added a route to `10.20.0.0/16` through the peering connection in VPC A's route tables.



Added a route to `10.10.0.0/16` through the peering connection in VPC B's private route table.



These routes provide paths in both directions between the VPCs.



\### Step 7: Connect Through the Bastion



Used Windows OpenSSH to connect to the private instances through the bastion host. The private key was stored locally and was not intended for publication.



\### Step 8: Test Connectivity



From App-A, ran:



```bash

ping -c 4 10.20.1.10

```



From App-B, ran:



```bash

ping -c 4 10.10.2.10

```



Successful replies verified bidirectional ICMP connectivity.



\### Step 9: Perform a Controlled Failure Test



Temporarily removed the inbound ICMP rule in `ewb-sg-app-b` with source `10.10.2.10/32`.



From App-A, ran:



```bash

ping -c 4 10.20.1.10

```



The test returned 100% packet loss, demonstrating that the security group rule controlled the ICMP traffic.



\### Step 10: Restore Connectivity



Restored the ICMP inbound rule allowing traffic from `10.10.2.10/32`.



Repeated the ping test from App-A and verified that connectivity recovered.



\## 3. Results



The lab demonstrated:

\- An active VPC Peering connection.

\- Correct routing between two non-overlapping VPCs.

\- Private instance access through a bastion host.

\- Successful bidirectional ping tests.

\- A controlled ICMP failure caused by a security group rule change.

\- Recovery after restoring the rule.



\## 4. Troubleshooting Notes



During SSH troubleshooting, the bastion security group's allowed SSH source became outdated when the administrator's public IP changed. Updating the inbound rule to the current public IP `/32` restored TCP connectivity to port 22.



Windows OpenSSH also required restrictive permissions on the private key and a correctly configured SSH config file.



\## 5. Security and Cost Considerations



\- Keep SSH restricted to trusted administrator IP addresses.

\- Keep private application instances without public IPv4 addresses.

\- Never publish private keys or AWS credentials.

\- Avoid creating a NAT Gateway unless required.

\- After capturing the required evidence, clean up unused AWS resources and review billing.



\## 6. Conclusion



The implementation demonstrated private VPC-to-VPC communication, bastion-based administration, route configuration, security group enforcement, and controlled connectivity recovery.

