\# AWS VPC Peering Connectivity Tests



\## 1. SSH Connection Commands



Run these commands from Windows PowerShell using the local SSH configuration:



```powershell

ssh -F C:/ssh/config ewb-bastion

ssh -F C:/ssh/config ewb-app-a

ssh -F C:/ssh/config ewb-app-b

```



The private instances are accessed through the bastion using SSH ProxyJump configuration.



\## 2. App-A to App-B



Run from App-A:



```bash

ping -c 4 10.20.1.10

```



\*\*Expected result:\*\* Successful ICMP replies when the required routes and security group rules are configured.



\## 3. App-B to App-A



Run from App-B:



```bash

ping -c 4 10.10.2.10

```



\*\*Expected result:\*\* Successful ICMP replies.



\## 4. Controlled Failure Test



Temporarily remove the inbound ICMP rule in `ewb-sg-app-b` with source `10.10.2.10/32`.



From App-A, run:



```bash

ping -c 4 10.20.1.10

```



\*\*Observed result:\*\* 4 packets transmitted, 0 received, 100% packet loss.



\## 5. Recovery Test



Restore the ICMP inbound rule in `ewb-sg-app-b` with source `10.10.2.10/32`.



Repeat:



```bash

ping -c 4 10.20.1.10

```



\*\*Expected result:\*\* Successful ping replies after the rule is restored.



\## 6. SSH Troubleshooting



Check TCP port 22 from Windows PowerShell:



```powershell

Test-NetConnection 16.176.8.87 -Port 22

```



The bastion security group must allow SSH from the administrator's current public IP address using `/32`. If the public IP changes, update the inbound rule to match the new address.



\## 7. Security Reminder



Do not publish private keys, AWS credentials, or other secrets. Keep SSH restricted to trusted source addresses.

