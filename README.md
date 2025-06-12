# Active-Directory-prereqs



Configuring Active Directory (On-Premises)       Within Azure
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

Environments and Technologies Used

•	Microsoft Azure (Vitual Machine/Computer)
•	Remote Desktop
•	Acitive Directory Domain Services
•	Powershell


Operating System Used

•	Windows Server 2022
•	Windows  11


High-Level Deployment and Configuration Steps

•	Create Resources
•	Ensure Connectivity between the client and Domain Controller
•	Install Active Directory
•	Create an Admin and Normal User Account in AD
•	Join Client-1 to your domain (myadproject.com)
•	Setup Remote Desktop for non-administrative users on Client-1
•	Create additional users and attempt to log into client-1 with one of the users


Deployment and Configuration Steps


Setup Resources in Azure

Create the Domain Controller VM (Windows Server 2022) named “DC-1”:




























Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in previous step:


Set Domain Controller’s NIC Private IP address to be static:



















Ensure Connectivity between the client and Domain Controller
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t (perpetual ping):














Install Active Director

Login to DC-1 and install Active Directory Domain Services:

Promote as a Domain Controller:



Setup a new forest as mydomain.com (can be anything, just remember what it is)

 

Restart and then log back into DC-1 as user: mydomain.com\labuser:

























Create an Admin and Normal User Account in AD
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and another one called "_ADMINS":






















Create a new employee named “Jane Doe” with the username of “jane_admin”:

























Add jane_admin to the “Domain Admins” Security Group:





















Log out/close the Remote Desktop connection to DC-1 and log back in as “myadproject.com\jane_admin”. Use jane_admin as your admin account from now on:

























Join Client-1 to your domain (mydomain.com)
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address:

















From the Azure Portal, restart Client-1.
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart):

























Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.

Create a new OU named “_CLIENTS” and drag Client-1 into there:


















Setup Remote Desktop for non-administrative users on Client-1
Log into Client-1 as mydomain.com\jane_admin and open system properties.
Click “Remote Desktop”.
Allow “domain users” access to remote desktop.
You can now log into Client-1 as a normal, non-administrative user now.
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab):




















Create a bunch of additional users and attempt to log into client-1 with one of the users
Login to DC-1 as jane_admin
Open PowerShell_ise as an administrator.
Create a new File and paste the contents of this script (https://github.com/Xinloiazn/configure-ad/blob/main/adscript.ps1) into it:




















Run the script and observe the accounts being created:






















When finished, open ADUC and observe the accounts in the appropriate OU and attempt to log into Client-1 with one of the accounts (take note of the password in the script):














