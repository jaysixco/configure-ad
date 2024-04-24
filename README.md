# configure-ad

# ticket-lifecycle

<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />
In this tutorial, you are going to play 3 roles:  administrator, agent, user <br>
In this tutorial, you/we are going to be creating and delegating tickets <br>

<h2>Video Demonstration</h2>

- ### [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

- Item 1
- Item 2
- Item 3
- Item 4
- Item 5

<h2>Installation Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Setup Resources in Azure
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
Set Domain Controller’s NIC Private IP address to be static
Question to self: what is the starting point to change DC-1 to static?
Serious question: Also, why can't I just change the DNS server at this step, too, while I'm at it?
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
Question: what is the trick to make Client-1 connect to DC's v-net (at least on my computer)?
Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

Ensure Connectivity between the client and Domain Controller
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping) don't ping through powershell
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall - There's 2 ICMPv4's. Enable rule for both to work.
Question to self: what is the start point to enable ICMPv4?
Check back at Client-1 to see the ping succeed

Install Active Directory
Login to DC-1 and install Active Directory Domain Services
Question to self: What is the start point to install Active Directory Domain Services (ADDS)
Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
Question to self: What is the start point to setup a new forest?
Restart and then log back into DC-1 as user: mydomain.com\labuser (the black slash matters!!! If you use a forward slash (/), it will not work!)

Create an Admin and Normal User Account in AD
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
Add jane_admin to the “Domain Admins” Security Group
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
User jane_admin as your admin account from now on


Join Client-1 to your domain (mydomain.com)
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
Question to self: What is the first two start points to get to DNS settings?
From the Azure Portal, restart Client-1
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
Question to self: what is the start point to join a domain?
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)


Setup Remote Desktop for non-administrative users on Client-1
Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”
Allow “domain users” access to remote desktop
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)

Create a bunch of additional users and attempt to log into client-1 with one of the users
Login to DC-1 as jane_admin
Open PowerShell_ise as an administrator
Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
Run the script and observe the accounts being created
When finished, open ADUC and observe the accounts in the appropriate OU
attempt to log into Client-1 with one of the accounts (take note of the password in the script)

Finish.

SUMMARY (in my own words):
Create 2 VMs (1 Window 2022 [DC-1], 1 Window 10 [Client-1])
Change DC-1 NIC to static, 
Login to DC-1's firewall (hint: type) and enable ICMPv4 traffic
Login to Client-1 and ping DC-1 to see if it worked


DC-1
Install ADDS + setup forest
Log back in as mydomain.com\labuser (because we have no jane_admin yet)
Create an Admin account and a place to store all the users we'll create later (hint: starts with “_E”)
DON'T FORGET to make jane_admin a “Domain Admin” (just because her name is in the Admin folder doesn't mean she's actually an Admin yet)
Now we'll be dealing with Client-1


CLIENT-1
Starting in Azure, go to DNS server and make it DC-1's private IP
Hit restart so it logs you out of Client-1 remote desktop
Log back in as labuser (remember, we haven't joined it to any domain yet)
Rename the PC (hint: Start > System) as mydomain.com\jane_admin
Now you can log back in as mydomain.com\jane_admin
Then allow users to access this computer through Start>Systems>Remote Desktop>Select users… (this will allow all the users we're about to create access to this computer)

Use the simple list for the last part


</p>
<br />
<p>

</p>
<p>

</p>


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<img width="960" alt="Ticket LIfecycle - Karen's Complaint" src="https://github.com/jaysixco/ticket-lifecycle/assets/160427311/b80253e6-9fff-4e21-9a45-53b605d68663">
<img width="959" alt="Ticket LIfecycle - Reassign Button" src="https://github.com/jaysixco/ticket-lifecycle/assets/160427311/beeacf9e-dbd6-45dd-a792-345745867684">













