  ![68747470733a2f2f692e696d6775722e636f6d2f705535413538532e706e67](https://github.com/user-attachments/assets/377cfda2-40c2-446b-85e0-c67d0e220f2d)
# On-Premises Active Directory Deployed in the Cloud (Azure)
This tutorial outlines the implementation of on-premises Active Directory within two Azure virtual machines.

# Environments and Technologies Used
Microsoft Azure (Virtual Machines/Compute)

Remote Desktop

Active Directory Domain Services

PowerShell

# Operating Systems Used
Windows Server 2022

Windows 10 (21H2)
# Deployment and Configuration Steps
# Step 1: Setup Resources in Azure

Create two virtual machines

The first virtual machine will be the Domain Controller
Name: DC-1
Image: Windows Server 2022
Take note of the virtual network (vNet) that is automatically created
![68747470733a2f2f692e696d6775722e636f6d2f6d72704257744d2e706e67](https://github.com/user-attachments/assets/87f27470-2464-441e-8e72-a1a3cbc486e1)

 Set DC-1's Virtual Network Interface Card (vNIC) private IP address to be static
	- Go to DC-1's network settings
	- Select Networking
	- Select the link next to Network Interface
	- Select IP Configurations > ipconfig1
	- Change the assignment from dynamic to static 
		- This ensures DC-1's IP address will not change
  ![68747470733a2f2f692e696d6775722e636f6d2f7863794c554f472e706e67](https://github.com/user-attachments/assets/ae7a7b72-4cc7-4156-9f51-f579c8ae4a0f)
  ![68747470733a2f2f692e696d6775722e636f6d2f5a6157647a546c2e706e67](https://github.com/user-attachments/assets/60ee0ef1-1b46-4297-a46f-7768ba14a66b)
  ![68747470733a2f2f692e696d6775722e636f6d2f566e305568576d2e706e67 (1)](https://github.com/user-attachments/assets/942ee2a8-9202-44a2-9e48-502b53a3e7de)

   The second virtual machine will be the Client
	- Name: Client-1
	- Image: Windows 10 Pro
	- Use the same resource group and vNet as DC-1
![68747470733a2f2f692e696d6775722e636f6d2f566637796559312e706e67](https://github.com/user-attachments/assets/11eff9a3-b428-4312-99be-6c7c07c4e7fc)
![68747470733a2f2f692e696d6775722e636f6d2f33444b343143722e706e67](https://github.com/user-attachments/assets/4f23aed4-07a5-4803-83c3-8ec508fc891f)

# Step 2: Ensure Connectivity Between the Client and Domain Controller
Login to Client-1 using Microsoft Remote Desktop

Search for Command Prompt and open it
Ping DC-1's private IP Address (for example, 10.1.0.4)
Type "ping -t 10.1.0.4" into the command-line interface
The ping request continually times out due to the firewall settings
To fix this, we need to enable ICMPv4 on DC-1's local Windows firewall
![68747470733a2f2f692e696d6775722e636f6d2f5536554f716a352e706e67](https://github.com/user-attachments/assets/c21d4713-36b2-4d6a-be06-1d4dd9d733e1)

Login to DC-1 using Microsoft Remote Desktop
Start > Windows Administrative Tools > Windows Defender Firewall with Advanced Security > Inbound Rules
Sort the list by protocols
Find "Core Networking Diagnostics" and "ICMPv4" and enable these two inbound rules
![68747470733a2f2f692e696d6775722e636f6d2f627736656f4c682e706e67](https://github.com/user-attachments/assets/03b46029-8a28-442d-bca3-ea8f171c579a)
![68747470733a2f2f692e696d6775722e636f6d2f4259314f6867622e706e67 (1)](https://github.com/user-attachments/assets/38dc2e09-18d9-4424-a2b1-2087f4c4546a)

Log back into Client-1 and the command line will automatically begin pinging DC-1 successfully
![68747470733a2f2f692e696d6775722e636f6d2f38393057494a422e706e67](https://github.com/user-attachments/assets/11be8bff-6b71-4ccc-b913-fd7af0578e0f)

# Step 3: Install Active Directory
Log back into DC-1

Open Server Manager

Select "Add Roles and Features" > Follow the prompts

At Server Roles, check "Active Directory Domain Services."

Ignore how the picture below already says "Installed"
Select Add Features > select Next

Complete the installation
![68747470733a2f2f692e696d6775722e636f6d2f445152564e6e6d2e706e67](https://github.com/user-attachments/assets/6b13a2c1-bffd-4046-8c5c-05ca6757a518)






