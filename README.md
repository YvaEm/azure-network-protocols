<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1 : Setup The Environment
- Step 2: RDP into VM and download WireShark
- Step 3: Setup FireWall (Network Security Group)
- Step 4: Observe Network Protocols

<h2>Actions and Observations</h2>

Part 1: Setup the Environment 

Create a Resource Group in Azure
![Screenshot 2024-11-28 093140](https://github.com/user-attachments/assets/b85e0a11-b208-4813-8cc6-80ad3b6bd837)


Create a Windows 10 VM
```
  Select the resource group previously created 
  Allow it to create a new Virtual Network and Subnet
```
![Screenshot 2024-11-28 094259](https://github.com/user-attachments/assets/4f76cbf0-6cd2-40a3-90f5-6396994edbb0)


Create a Linux VM
```
  Select the resource group previously created 
  Select the Virtual Network that was made with the Windows 10 VM
```
![Screenshot 2024-11-28 095102](https://github.com/user-attachments/assets/1d1f4952-212a-478c-97b0-ce920288ea64)
![Screenshot 2024-11-28 095234](https://github.com/user-attachments/assets/813c6544-67dc-43b0-bef8-5404ac219bb4)




 Part 2: RDP into VM and Download WireShark 

Use RDP to connect to the Windows 10 VM
![Screenshot 2024-11-28 100122](https://github.com/user-attachments/assets/1732a255-9beb-45e7-ad95-c29f3537c41d)
![Screenshot 2024-11-28 100640](https://github.com/user-attachments/assets/9294ff34-d977-4d9a-af46-6b8e72b74a37)


Download Wireshark on Windows 10 VM
```
  Open Wireshark and start packet capture
  Filter for ICMP traffic only
```
![Screenshot 2024-11-28 100930](https://github.com/user-attachments/assets/437a3ea7-14f5-4338-baf3-147fc50ef5aa)
![Screenshot 2024-11-28 101648](https://github.com/user-attachments/assets/81c5c11e-57ad-4acb-bef1-10bcb709de31)


  
Open Powershell as Admin and ping the Linux VM 
![Screenshot 2024-11-28 102133](https://github.com/user-attachments/assets/930b1a57-338c-4cf9-8ad3-55817244dd30)


Observe ping request from Wireshark
![Screenshot 2024-11-28 102145](https://github.com/user-attachments/assets/795afa3b-0294-49eb-a3e0-7f7cb3a84e6c)



Part 3: Setup Firewall(Network Security Group)

Start a nonstop ping to Linux VM from Windows VM
![Screenshot 2024-11-28 103210](https://github.com/user-attachments/assets/a7311d1a-3cc5-4d10-bb9a-6d7132769dd2)


Open the Network Security group on the Linux VM
```
  Press on the "Network Interface / Ip Configuration"
  Disable Inbound ICMP traffic
```
![Screenshot 2024-11-28 103321](https://github.com/user-attachments/assets/b1b0e206-ba4c-410e-873e-e95c76dda9ac)
![Screenshot 2024-11-28 103341](https://github.com/user-attachments/assets/ec9b0770-c6a9-465f-af8f-e5b33a5fe862)
![Screenshot 2024-11-28 103456](https://github.com/user-attachments/assets/0c57767b-3118-41cb-b046-7aa9a3c73ee0)



Back in the Windows VM Observe ICMP traffic in Wireshark and CMD ping activity 
```
You should start seeing the replies turn into 'Request timedout."
```
![Screenshot 2024-11-28 103638](https://github.com/user-attachments/assets/316c01a0-da4c-46ce-9053-af6ee78e2681)



Open the Network Security group on the Linux VM
```
  Re-enable Inbound ICMP traffic (Delete the Security Rule"
  You should start seeing replies again on powershell
```
![Screenshot 2024-11-28 103850](https://github.com/user-attachments/assets/922e1f36-6bbd-4f5c-a9da-0bcdb79f67c9)
![Screenshot 2024-11-28 103941](https://github.com/user-attachments/assets/2842845e-86e8-4f6c-875c-9274f9d82ae7)


Stop Ping traffic


<p>
Part 4: Observe Network Protocol Traffic

Observe SSH Traffic
```
  Open Wireshark and filter for SSH traffic only
  SSH into Linux VM using its private IP (ssh Yvaboe@PrivateIpAddress)
  Type some commands
  Observe SSH traffic in Wireshark
```
![Screenshot 2024-11-29 065246](https://github.com/user-attachments/assets/856231e8-b84b-4ca5-8d9e-5a84fd8dd8f7)
![Screenshot 2024-11-29 065425](https://github.com/user-attachments/assets/a103d70d-44bb-42fe-9dae-e4b7dc37ea7d)
![Screenshot 2024-11-29 065311](https://github.com/user-attachments/assets/7480ceab-0930-480b-941a-0bbac71a0853)


Observe DHCP Traffic 
```
  Open Wireshark and filter for DHCP traffic only
  Open PowerShell as an Admin 
  Issue your VM an new IP address from the command line: ipconfig /renew
  Observe DHCP traffic in Wireshark
```
![Screenshot 2024-11-29 070517](https://github.com/user-attachments/assets/d3962bee-a498-4e0a-96cc-a03782147814)
![Screenshot 2024-11-29 070528](https://github.com/user-attachments/assets/cbd9e551-b7d5-4ea5-8b43-03f56462fbff)


Observe DNS Traffic
```
  Open Wireshark and filter for DNS traffic only ]
  Open CMD Line and use nslookup to see what google.com and Disney.com IP addresses are 
  Observe DNS traffic in Wireshark
```
![Screenshot 2024-11-29 071616](https://github.com/user-attachments/assets/68579837-8158-45a3-924c-2079a4751d60)
![image](https://github.com/user-attachments/assets/d4205455-cfde-4035-a98c-1c44106832a7)



Observe RDP Traffic 
```
  Open Wireshark and filter for RDP traffic only (tcp.port == 3389)
  Observe immediate spam of traffic
```
![Screenshot 2024-11-29 072216](https://github.com/user-attachments/assets/33ea6fe6-1432-4475-bc71-4c24528e7e61)


</p>
<br />
