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

-Setup Resources in Azure

-Create the Domain Controller VM (Windows Server 2022) named “DC-1”

-Take note of the Resource Group and Virtual Network (Vnet) that get created at this time

-Set Domain Controller’s NIC Private IP address to be static

-Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a

-Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

-Ensure Connectivity between the client and Domain Controller

-Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)

-Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall

-Check back at Client-1 to see the ping succeed


<h2>Deployment and Configuration Steps</h2>

-Install Active Directory

-Login to DC-1 and install Active Directory Domain Services

-Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)

-Restart and then log back into DC-1 as user: mydomain.com\labuser

![Annotation 2023-07-10 145013](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/cba842f6-c882-461f-85f8-9b9d5390e9bc)

![Annotation 2023-07-10 145347](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/0aa3cf5f-4057-46ae-b721-0485a1190f2f)

![Annotation 2023-07-10 145501](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/8fdc64d6-5451-41ff-8045-f001bb3be04e)

-Create an Admin and Normal User Account in AD

-In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”

-Create a new OU named “_ADMINS”

-Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”

-Add jane_admin to the “Domain Admins” Security Group

-Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”

-User jane_admin as your admin account from now on

![Annotation 2023-07-10 150259](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/a470e14d-bf2a-437e-8997-9ca564c0e989)

![Annotation 2023-07-10 150554](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/739e3858-41ee-472d-a094-fb6a0b174727)

![Annotation 2023-07-10 150645](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/d6b47e31-47e9-468b-be8d-91c44002f8f8)

![Annotation 2023-07-10 151219](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/35bd0e39-c93a-4b11-8fd7-8fd9cb390f03)

![Annotation 2023-07-10 152359](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/76868810-7ddb-4e0b-a77e-b24bf883845f)


-Join Client-1 to your domain (mydomain.com) 

-From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address

-From the Azure Portal, restart Client-1

-Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)

-Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers 

- (ADUC) inside the “Computers” container on the root of the domain
- 
-Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational 
 purposes. I guess I skipped this in the lab!)

![Annotation 2023-07-10 152630](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/c6c699dc-bc72-42c1-a22a-90ac8912d460)

![Annotation 2023-07-10 153255](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/c64817f4-d19a-4eec-a28a-019593155bad)

![Annotation 2023-07-10 153734](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/389a4b31-101c-4b33-897e-e3aac51c50bb)


-Setup Remote Desktop for non-administrative users on Client-1

-Log into Client-1 as mydomain.com\jane_admin and open system properties

-Click “Remote Desktop”

-Allow “domain users” access to remote desktop

-You can now log into Client-1 as a normal, non-administrative user now

-Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)

![Annotation 2023-07-10 154217](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/4104a256-6320-4e63-a284-7d16841dbf5d)

![Annotation 2023-07-10 154328](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/8ebd44d6-0ed0-4e3c-b0ab-384def641f36)

![Annotation 2023-07-10 155320](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/fa4cd84f-f340-4d6c-8027-3ffc277f9ac2)

![Annotation 2023-07-10 155342](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/7a576129-06c8-4d74-9b48-ece2b3136a44)


-Create a bunch of additional users and attempt to log into client-1 with one of the users

-Login to DC-1 as jane_admin

-Open PowerShell_ise as an administrator

-Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

-Run the script and observe the accounts being created

-When finished, open ADUC and observe the accounts in the appropriate OU

-attempt to log into Client-1 with one of the accounts (take note of the password in the script)

![Annotation 2023-07-10 155540](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/9c6f8c12-1b1d-4df9-ad91-f04b0f553c62)

![Annotation 2023-07-10 160228](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/37d6e39d-455b-4264-ba9a-7a4ec33eb952)

![Annotation 2023-07-10 160416](https://github.com/isobarh/Active-Directory-Deployed-in-the-Cloud-Azure-/assets/139295370/aa7641fc-2bae-47b3-8200-7275dd05929e)


Finish.

