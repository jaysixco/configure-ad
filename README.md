# configure-ad

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
<strong>Setup Resources in Azure</strong>
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
Set Domain Controller’s NIC Private IP address to be static
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a

  
<strong> Ensure Connectivity between the client and Domain Controller </strong>
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping) don't ping through powershell
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall - There's 2 ICMPv4's. Enable rule for both to work.
Check back at Client-1 to see the ping succeed

<strong> Install Active Directory </strong>
Login to DC-1 and install Active Directory Domain Services
Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
Restart and then log back into DC-1 as user: mydomain.com\labuser (the black slash matters!!! If you use a forward slash (/), it will not work!)

<strong> Create an Admin and Normal User Account in AD </strong>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
Add jane_admin to the “Domain Admins” Security Group
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
User jane_admin as your admin account from now on


<strong> Join Client-1 to your domain (mydomain.com) </strong>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
From the Azure Portal, restart Client-1
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)


<strong> Setup Remote Desktop for non-administrative users on Client-1 </strong>
Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”
Allow “domain users” access to remote desktop
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)

<strong> Create a bunch of additional users and attempt to log into client-1 with one of the users </strong>
Login to DC-1 as jane_admin
Open PowerShell_ise as an administrator
Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
Run the script and observe the accounts being created
When finished, open ADUC and observe the accounts in the appropriate OU
attempt to log into Client-1 with one of the accounts (take note of the password in the script)

Finish.

<strong> SUMMARY (in my own words):</strong>  <br>
Create 2 VMs (1 Window 2022 [DC-1], 1 Window 10 [Client-1]) <br>
▪︎Can do <br> 
Change DC-1 NIC to static, <br> 
▪︎DC-1 > Networking > Network Interface > IP configurations > scroll down and click ipconfig > click static > Save <br> 
Login to DC-1's firewall (hint: type) and enable ICMPv4 traffic <br>
▪︎Enable - Start menu > type firewall > click option with 'Advanced Security' > Inbound Rules > widen screen so you can see Protocol tab > right click ICMPv4EchoRequests > Enable rule (there's two enable both of them in turn) <br>
Login to Client-1 and ping DC-1 to see if it worked <br>


<strong> DC-1 </strong>  
Install ADDS + setup forest <br>
Install ADDS - Service Manager > 'Add roles and features' > Only two buttons you should be clicking are 'Next' and 'Install' <br>
Set up new forest = Service manager > look at upper right on the left side of the word 'manage'; should see what looks like a flag and a triangle with an exclamation point in it, click it > Promote > Add a new forest > mydomain.com > J~S~2 <br>
Log back in as mydomain.com\labuser (because we have no jane_admin yet) <br>
Create an Admin account and a place to store all the users we'll create later <br>
  In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” <br>
&nbsp;&nbsp; Right click mydomain.com > New > Organizational Unit
(Underscore not mandatory in '_EMPLOYEES', but done for the lab)
  Create a new OU named “_ADMINS” <br>
&nbsp;&nbsp; 
  Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” <br>
  DON'T FORGET to make jane_admin a “Domain Admin” (just because her name is in the Admin folder doesn't mean she's actually an Admin yet)  <br>
&nbsp;&nbsp; Right click '_ADMINS' > New > User
  Add jane_admin to the “Domain Admins” Security Group <br>
&nbsp;&nbsp; Right click jane_admin > Properties > Member of tab > type domain > Check names > Domain Admins > OK > Apply > OK 
  Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin” <br>
&nbsp;&nbsp; 
  User jane_admin as your admin account from now on <br>
<br>
  Now we'll be dealing with Client-1 <br>
<br>
<strong> CLIENT-1 </strong> <br>
&nbsp; Starting in Azure, go to DNS server and make it DC-1's private IP <br>
&nbsp; Get DC's Private IP address first (should be 10.0.0.x) <br>
&nbsp; Go to Client-1 > Networking > Network Interface > DNS servers > Custom > Paste DC-1's Private IP > Save <br>
&nbsp; Hit restart so it logs you out of Client-1 remote desktop <br>
&nbsp; Log back in as labuser (remember, we haven't joined it to any domain yet) <br>
&nbsp; Rename the PC (hint: Start > System) as mydomain.com\jane_admin <br>
&nbsp; Right click the start button > Systems > Rename this pc (advanced) > Change > Domain > type mydomain.com > then, username:mydomain.com\jane_admin + password:J~S~2 <br>
<br>
<strong> Remote Desktop for non-administrative users on Client-1 </strong>
&nbsp; Log into Client-1 as mydomain.com\jane_admin and open system properties <br>
&nbsp; Click “Remote Desktop” <br>
&nbsp; Allow “domain users” access to remote desktop <br>
&nbsp; You can now log into Client-1 as a normal, non-administrative user now <br>
&nbsp; Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab) <br>
<br>
<strong> Create a bunch of additional users and attempt to log into client-1 with one of the users </strong><br>
&nbsp; Login to DC-1 as jane_admin <br>
&nbsp; Open PowerShell_ise as an administrator <br>
&nbsp; Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) <br>
&nbsp; Run the script and observe the accounts being created <br>
&nbsp; When finished, open ADUC and observe the accounts in the appropriate OU <br>
&nbsp; attempt to log into Client-1 with one of the accounts (take note of the password in the script) <br>
<br>
Finish.


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















