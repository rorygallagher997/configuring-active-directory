<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://github.com/user-attachments/assets/09f3dbc7-888d-468f-9a07-7d7d9e6b49fb" height="50%" width="50%"/>
</p>
<p>
Create the virtual machine for the domain controller DC-1 using Windows Server 2022 and name the Resource Group to be made at the same time (AD-Lab)
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/48ad118e-83fc-48b0-9a38-614bd0d1f77f" height="50%" width="50%"/>
</p>
<p>
Create the second VM (Client-1).
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/49d66e7d-411b-4187-91e3-e65cc90775c8" height="50%" width="50%"/>
</p>
<p>
Navigate to DC-1's network settings, click on the network interface (dc-1694_z1) and select "IP Configurations"
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/5526f827-7d20-4434-9956-ffc803418909" height="50%" width="50%"/>
</p>
<p>
Change the network interfaces' IP address to Static and save
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/9578f9a1-0e36-496e-9197-077a3ccae2d8" height="50%" width="50%"/>
</p>
<p>
Log into Client-1 with Remote Desktop using Client-1's public IP. Ping DC-1's private IP to test for connectivity. As you can see, ping requests are timed because DC-1 does not allow ICMP traffic yet.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/89b95bde-29a4-4bd3-a55b-8c3ee10a4fa2" height="50%" width="50%"/>
</p>
<p>
Log into DC-1 with Remote Desktop using DC-1's public IP. Open Windows Defender Firewall.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/7720714e-55ee-4ddf-9710-d530c6575b4f" height="50%" width="50%"/>
</p>
<p>
Navigate to Inbound rules in the top left and find ICMP under Protocol. Right click on the rule and select "Enable Rule".
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/b8461d21-f627-4080-b2f1-be6696d99f9f" height="50%" width="50%"/>
</p>
<p>
We are now able to ping DC-1.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/4bb20b28-63eb-4564-a441-8f6483285476" height="50%" width="50%"/>
</p>
<p>
We are now going to install Active Directory on DC-1. Go to Server Manager on DC-1 and select "Add roles and features". 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/8293c71b-ef9e-49b9-9f9b-4e3fd0a78b4f" height="50%" width="50%"/>
</p>
<p>
Select Active Directory Domain Services.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/3ba53000-4bad-4434-98af-f2fc1a7a191d" height="50%" width="50%"/>
</p>
<p>
Click on the "post deployment configuration" exclamation point in the top right and select "Promote this server to a domain controller"
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/4a8fff2a-b5e5-4283-9635-bcd040ff78c5" height="50%" width="50%"/>
</p>
<p>
We now have to specify the domain information. Select "Add a new forest" and create a root domain name. For this example, we will use a generic name and call it "mydomain.com".
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/433e3f07-a386-4fce-8074-5303e4b70bf0" height="50%" width="50%"/>
</p>
<p>
Once installed, you will automatically be logged out. You must now log back in as a mydomain user. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/af555198-26ac-4ad5-9b00-da8987f5a6ad" height="50%" width="50%"/>
</p>
<p>
Once logged in, open Active Directory Users and Computers.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/47134d44-1cee-45e7-8a78-d08b533396d0" height="50%" width="50%"/>
</p>
<p>
We will be creating a new Organizational Unit (OU). Right click on mydomain.com, scroll down to New and select Organizational Unit. We are going to name this OU "_EMPLOYEES". We are also going to create another OU called "_ADMINS".
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/53a66dfc-b360-4404-b4a6-2abc7699f142" height="50%" width="50%"/>
</p>
<p>
We will now create a new Admin account. Right click the _ADMINS OU, scroll to "New" and select "User". Create a new user with any name.
</p>
<br />


<p>
<img src="https://github.com/user-attachments/assets/70ac4e24-90a4-4b89-bf76-67f60d7b8e10" height="50%" width="50%"/>
</p>
<p>
Even though we added John Doe to the _ADMINS OU, he is not yet an admin account. To make him an Admin, right click the user, go to "member of" and then press "Add". Select the group "Domain Admins" and press OK.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/e4fa2be6-447e-4b56-9aee-434541b1fc3f" height="50%" width="50%"/>
</p>
<p>
We will now log out of DC-1 and sign in again as John Doe.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/efab9417-9f02-4810-88e0-a114d45d86e6" height="50%" width="50%"/>
</p>
<p>
We need to set Client-1's DNS to DC-1's private IP now. Navigate to DC-1 on Azure and retrieve the private IP address.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/56df3b7d-2993-4883-a216-18b8b49708ea" height="50%" width="50%"/>
</p>
<p>
Go to Client-1's NIC by going to Network settings.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/3825ce1d-25e1-40a3-a534-558122741253" height="50%" width="50%"/>
</p>
<p>
Click on the Network Interface in bold under "Network interface/IP configuration".
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/0b95ea61-23a5-410c-8890-cfbe07e7aec5" height="50%" width="50%"/>
</p>
<p>
Click on DNS Servers on the left, change from Inherit to Custom and paste DC-1's private IP.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/df291737-be92-4c16-81b2-015c41d3f530" height="50%" width="50%"/>
</p>
<p>
Restart Client-1 and log back in. We still have to log in as labuser because it is not yet joined to the domain. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/b2f3ddaa-2626-44da-bd68-acbcff4d290f" height="50%" width="50%"/>
</p>
<p>
We will now join Client-1 to the domain. Go to Settings->System->About->Rename this PC
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/6327d83f-6f57-4e48-b866-18d103d16f89" height="50%" width="50%"/>
</p>
<p>
Click "Change" and make it a member of mydomain.com
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/e782b7ce-bdfb-4520-9908-2b2ca3ef5a3a" height="50%" width="50%"/>
</p>
<p>
Use your admin account credentials to grant permission.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/34f518a9-74a0-4b4d-abb8-7eb74c3a2d7f" height="50%" width="50%"/>
</p>
<p>
Client-1 will be restarted after changes are made and we can now log in as John Doe
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/65e5890a-f541-419b-8141-cd8e7bbed4a8" height="50%" width="50%"/>
</p>
<p>
Go to system properties on Client-1 and navigate to "Remote Desktop". Click on "Select users that can remotely access this PC"
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/561d06ee-63c6-4451-a9b5-8bffb103c6bc" height="50%" width="50%"/>
</p>
<p>
Click "Add" under Remote Desktop Users and enter the Object Group as "Domain Users". This will allow all users on the domain to access Client-1
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/3f2184af-94bd-41e8-b46d-c075480e5fdc" height="50%" width="50%"/>
</p>
<p>
We are now going to create a bunch of users using Powershell ISE. Open Powershell ISE as an administrator.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/600b9de7-1adb-4893-83a2-ec3e758058a6" height="50%" width="50%"/>
</p>
<p>
Open a new file and write the script shown. Click run script.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/65b1105f-f9b6-49d7-84cb-c4773ceca171" height="50%" width="50%"/>
</p>
<p>
Random users are being created.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/363663d8-24d6-479b-82bb-9c7c5c8e1ef4" height="50%" width="50%"/>
</p>
<p>
Open Active Directory Users and Computers. Click on the _EMPLOYEES Organizational Unit created earlier and notice all the users created. All users will have passwords of Password1.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/a8b174d4-1465-4cf3-a14c-7ee571ee6d60" height="50%" width="50%"/>
</p>
<p>
Pick a user and right click to go to properties. Then go to the account tab and copy the username. Go to Client-1.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/da1b5581-9a37-44cd-af9c-71bf58956429" height="50%" width="50%"/>
</p>
<p>
Logout of Client-1 as John  Doe.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/922f9e14-905f-4d02-b2bf-ebf439799b23" height="50%" width="50%"/>
</p>
<p>
Log back in as the user you chose.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/11cad436-bad6-4c5c-8303-f1313a34ea8a" height="50%" width="50%"/>
</p>
<p>
Login successful.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/b78ede1c-9fe2-451b-ac87-d4cf89e5e3f5" height="50%" width="50%"/>
</p>
<p>
jin.kuna is logged into Client-1 after being created as a user on DC-1.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/06a076e2-8002-49b5-a85d-7480c2378a05" height="50%" width="50%"/>
</p>
<p>
Everytime a new user logs into Client-1, a new folder is created for them.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/360d8219-1abd-4316-a9c1-61d55918e04c" height="50%" width="50%"/>
</p>
<p>
If too many log in attempts are made, you can right click user in DC-1 and go properties. Go to account and select "unlock account".
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/cddb87c4-c27c-4762-9104-becc6ec14ec5" height="50%" width="50%"/>
</p>
<p>
You can also reset their password by right clicking the user and selecting "Reset Password".
</p>
<br />
