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
<strong> SUMMARY (in my own words):</strong>  <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   <em> Instructions on how to get DC's private IP </em> <br>
<strong> Create 2 VMs (1 Window 2022 [DC-1], 1 Window 10 [Client-1]) (Use the same Resource Group and Vnet ) </strong><br>
<strong> Change DC-1 NIC to static </strong><br> 
&nbsp;&nbsp;&nbsp;&nbsp;  DC-1 > In the sidebar under "Networking" click "Network settings" > click IP configurations > scroll down and click ipconfig > click static > Save <br> 
<strong> Login to DC-1's firewall (hint: type) and enable ICMPv4 traffic  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  Enable - Start menu > type firewall > click option with 'Advanced Security' > Inbound Rules > widen screen so you can see Protocol tab > 
&nbsp;&nbsp;&nbsp;&nbsp;  right click ICMPv4EchoRequests > Enable rule (there's two enable both of them in turn) <br>
<strong> Login to Client-1 and ping DC-1 to see if it worked  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   <em> Instructions on how to get DC's private IP </em>
<img width="960" alt="ping worked" src="https://github.com/jaysixco/configure-ad/assets/160427311/59817a5c-d136-4890-886b-a99891dec9b4">


<strong> DC-1 </strong>  
<strong> Install ADDS + setup forest </strong><br>
<strong>&nbsp;&nbsp;&nbsp;&nbsp;   Install ADDS </strong> = Service Manager > 'Add roles and features' > Only two buttons you should be clicking are 'Next' and 'Install' <br>
<strong>&nbsp;&nbsp;&nbsp;&nbsp;   Set up new forest </strong> = Service manager > look at upper right on the left side of the word 'manage'; should see what looks like a <br> &nbsp;&nbsp;&nbsp;&nbsp;  flag and a triangle with an exclamation point in it, click it > Promote > Add a new forest > mydomain.com > J~S~2 <br>
<strong> Log back in as mydomain.com\labuser (because we have no jane_admin yet)  </strong><br>
<strong> Create an Admin account and a place to store all the users we'll create later  </strong><br>
<strong> In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  Right click mydomain.com > New > Organizational Unit > (Underscore not mandatory in '_EMPLOYEES', but done for the lab) <br>
<strong> Create a new OU named “_ADMINS”  </strong><br>
<strong> Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”  </strong><br>
<strong> DON'T FORGET to make jane_admin a “Domain Admin” (just because her name is in the Admin folder doesn't mean she's actually an Admin yet)   </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  Right click '_ADMINS' > New > User
<strong> Add jane_admin to the “Domain Admins” Security Group  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  Right click jane_admin > Properties > Member of tab > type domain > Check names > Domain Admins > OK > Apply > OK 
<strong> Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”  </strong><br>
<strong> User jane_admin as your admin account from now on  </strong><br>
<br>
<strong> Now we'll be dealing with Client-1  </strong><br>
<br>
<strong> CLIENT-1 </strong> <br>
<strong> Starting in Azure, go to DNS server and make it DC-1's private IP </strong> <br>
&nbsp;&nbsp;&nbsp;&nbsp;   Get DC's Private IP address first (should be 10.0.0.x) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   <em> Instructions on how to get DC's private IP </em>   
&nbsp;&nbsp;&nbsp;&nbsp;   Go to Client-1 > Networking > Network Interface > DNS servers > Custom > Paste DC-1's Private IP > Save <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Hit restart </strong> so it logs you out of Client-1 remote desktop <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Log back in as labuser </strong> (remember, we haven't joined it to any domain yet) <br>
<strong> Rename the PC (hint: Start > System) as mydomain.com\jane_admin </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;   Right click the start button > Systems > Rename this pc (advanced) > Change > Domain > type mydomain.com > then, username:mydomain.com\jane_admin + password:J~S~2 <br>
<br>
<strong> Remote Desktop for non-administrative users on Client-1 </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Log into </strong> Client-1 as mydomain.com\jane_admin and open system properties <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Click </strong> “Remote Desktop” <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Allow </strong> “domain users” access to remote desktop </strong><br>
<br>
<strong> Create a bunch of additional users and attempt to log into client-1 with one of the users </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Login </strong> to DC-1 as jane_admin <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Open </strong> PowerShell_ise as an administrator <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Create </strong> a new File and <strong> paste </strong> the contents of the script into it <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   <strong>script:</strong> https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1 <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Run </strong> the script and observe the accounts being created <br>
&nbsp;&nbsp;&nbsp;&nbsp;   When finished, <strong> open </strong> ADUC and <strong> observe </strong> the accounts in the appropriate OU <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Log into Client-1 with one of the accounts </strong> (take note of the password in the script) <br>
<br>
<strong> Finish. </strong>
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















