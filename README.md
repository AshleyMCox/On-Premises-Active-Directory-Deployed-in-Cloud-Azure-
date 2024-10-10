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
![68747470733a2f2f692e696d6775722e636f6d2f52707a6e6752692e706e67](https://github.com/user-attachments/assets/b921ac47-02db-47be-a3ce-bf826ea0d609)
At the top right of the Server Manager Dashboard, click on the flag

Select "Promote This Server to a Domain Controller"
![68747470733a2f2f692e696d6775722e636f6d2f474f59695446652e706e67](https://github.com/user-attachments/assets/0a896d02-3c7f-4707-b193-a637a1928e0e)

Select "Add a New Forest"

Root domain name: mydomain.com

Select Next

Create a password
Select Next and follow the prompts
Select Install to complete the installation
![68747470733a2f2f692e696d6775722e636f6d2f496a66555a30612e706e67](https://github.com/user-attachments/assets/04fac6ca-d503-42f9-b217-91d6d4621b2f)

DC-1 will automatically restart
Log back into DC-1 as user: mydomain.com\labuser
![image](https://github.com/user-attachments/assets/29bae90c-1788-4a2b-a5f4-b75ea7e9b8c7)
# Step 4: Create an Admin and Normal User Account in Active Directory v1.15.8

On DC-1, open Server Manager

Click Tools at the top-right of the screen

Select Active Directory Users and Computers
![68747470733a2f2f692e696d6775722e636f6d2f756447486247732e706e67](https://github.com/user-attachments/assets/af87a4f0-38fe-468a-afae-c5ce24a22c8c)

Right-click mydomain.com > New > Select Oranizational Unit (OU)

Create two OUs

Name the first "_EMPLOYEES"
Name the second "_ADMINS"
![68747470733a2f2f692e696d6775722e636f6d2f3577535a7541342e706e67](https://github.com/user-attachments/assets/fb77d5dd-2b26-492d-934b-1c14ef0f581a)

Right-click mydomain.com and click Referesh to sort the new organizational units to the top

Go to the _ADMINS OU

Right-click the name of the OU > New > User

First/Last name: Jane Doe

User login name: jane_admin

Select Next

Create a password

Uncheck all boxes
Select Next and then select Finish
![68747470733a2f2f692e696d6775722e636f6d2f6e76366a6339702e706e67](https://github.com/user-attachments/assets/4aa1bbaf-8a3b-465a-9bfb-25c1c132d485)
![68747470733a2f2f692e696d6775722e636f6d2f754c6f7051545a2e706e67](https://github.com/user-attachments/assets/ff439d07-e860-498e-b81a-74956cfa6c39)

Go to the _ADMINS OU

Right-click Jane Doe > select Properties

Click the tab named "Member of" > select Add

Type in the names of your domain administrators

Select "Check Names" > OK > Apply
Log out of DC-1 as "labuser" and log back in as “mydomain.com\jane_admin”
![68747470733a2f2f692e696d6775722e636f6d2f4561704d6842732e706e67](https://github.com/user-attachments/assets/03b3fcd2-4d04-45e5-a246-dbf5fc495b1a)
# Step 5: Join Client-1 to your domain (mydomain.com)

Go back to the Azure portal

Navigate to the Client-1 Virtual Machine

On the left-hand side of the screen select Networking

Select the link next to the NIC > select DNS Server > Custom

Type in DC-1's private IP address

Click Save
After it is done updating, select Restart and select Yes
![68747470733a2f2f692e696d6775722e636f6d2f7a365565734f372e706e67](https://github.com/user-attachments/assets/9c25f4db-bcad-4598-8816-116a2b6f8060)
![68747470733a2f2f692e696d6775722e636f6d2f627430794b31372e706e67](https://github.com/user-attachments/assets/ad5dae19-839f-4c71-a4dd-f667560e6a76)
![68747470733a2f2f692e696d6775722e636f6d2f734235656448352e706e67](https://github.com/user-attachments/assets/1869a150-848a-45af-a9f7-b7b68b5a48d8)

Log back into Client-1 using Microsoft Remote Desktop as the original local admin (labuser)

Right-click the Start menu and select System

On right-hand side of the screen, select Rename This PC (Advanced) > Change

Under "Member of," select Domain

Type "mydomain.com" and select OK

Username: mydomain.com\jane_admin
Type in password and press OK
Restart the computer

![68747470733a2f2f692e696d6775722e636f6d2f3348784a4c70652e706e67](https://github.com/user-attachments/assets/05410306-41be-4447-8bdb-f9259331b28e)
![68747470733a2f2f692e696d6775722e636f6d2f3348784a4c70652e706e67](https://github.com/user-attachments/assets/949a7f5d-b51b-475c-b537-8541a80089e9)

# Step 6: Setup Remote Desktop for non-administrative users on Client-1

Log back into Client-1

Use mydomain.com\jane_admin

Right-click the Start menu and select System

On the right-hand side of the screen, select Remote Desktop

Under User Accounts, click "Select Users That Can Remotely Access This PC > select Add

Type in the name of your domain users
Select "Check Names" > OK > OK
![68747470733a2f2f692e696d6775722e636f6d2f48674158564d582e706e67](https://github.com/user-attachments/assets/40236283-beb9-4880-9c9c-b97947c38fe4)
![68747470733a2f2f692e696d6775722e636f6d2f305144556b356c2e706e67](https://github.com/user-attachments/assets/38c3f3d9-6960-405a-ae07-d5e091b9dc2b)

# Step 7: Create as many additional users as you would like and attempt to log into Client-1 with one of the users' profiles

Log back into DC-1 as jane_admin

Search for "Powershell_ise,"

Right-click on Powershell_ise and open it as an administrator

At the top-left of the screen select New Script and paste the contents of the following script into it
You can find the script here
![68747470733a2f2f692e696d6775722e636f6d2f4d70764c4962422e706e67](https://github.com/user-attachments/assets/ee2f07eb-e861-4ebd-9b78-6de40c4eb0b0)
![68747470733a2f2f692e696d6775722e636f6d2f563476497672652e706e67](https://github.com/user-attachments/assets/c7f86918-8ac0-44b1-ad4f-7a8708c59f5f)

Click the green arrow button near the top-middle of the screen

This will run the script

Once the users have been created, go back to Active Directory Users and Computers > mydomain.com > _EMPLOYEES - You will see all the accounts that were created

You can now log into Client-1 with one of the accounts that were created
Try logging into Client-1 as user "base.milu" using the password "Password1"
![68747470733a2f2f692e696d6775722e636f6d2f33484e314e66342e706e67](https://github.com/user-attachments/assets/7f279957-7a49-446b-a3a6-22f84bff4ee4)
![image](https://github.com/user-attachments/assets/7548137c-643c-49a0-bdf6-f02679c7f87b)
![68747470733a2f2f692e696d6775722e636f6d2f375a56427038612e706e67](https://github.com/user-attachments/assets/005f9c96-d257-4613-a66f-0a501aa2120f)
![68747470733a2f2f692e696d6775722e636f6d2f457a67485752732e706e67](https://github.com/user-attachments/assets/a5bc2f63-5a7e-4682-80a8-4797c5e6a354)
![68747470733a2f2f692e696d6775722e636f6d2f6859466f6478752e706e67](https://github.com/user-attachments/assets/9597e352-eb3b-4679-b6a3-0cf62f338507)






















