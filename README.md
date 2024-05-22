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
<strong> SUMMARY</strong>  <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   <em> Instructions on how to get DC's private IP </em> <br>
<strong> Create 2 VMs (1 Window 2022 [DC-1], 1 Window 10 [Client-1]) (Use the same Resource Group and Vnet )</strong><br>
Windows 2022 - note: don't check the box under Licensing <br>
Windows 10 - DO check the box under Licensing

  
<strong> Change DC-1 NIC to static </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  1) Go to the Virtual Machine's page <br>
2) Right click the name of your Windows 2022 DC-1 and open it in a new tab <br>
3) In the sidebar under "Networking" click "Network settings" (1) and then Click "IP configurations" (2) <br>
![1 - put 2 red recs](https://github.com/jaysixco/configure-ad/assets/160427311/cdf031c9-aded-4db5-a705-ea40688a515c)
<br>
4) Scroll down and click "ipconfig1" (1), then click "Static" (2), and then click "Save" (3) <br>
![2 - 3 red recs - ip, static, save](https://github.com/jaysixco/configure-ad/assets/160427311/66dc1505-5820-43f1-8521-77edd06a4d3f) <br>



<strong> Log in to DC-1's firewall (hint: type) and enable ICMPv4 traffic  </strong><br>
1) Log in to Windows 2022 VM through Remote Desktop <br>
- (copy and paste the instructions from monitoring traffic draft)
2) Type "firewall" in the start menu search bar
3) Click option with 'Advanced Security' <br>
<img width="960" alt="1" src="https://github.com/jaysixco/configure-ad/assets/160427311/6da63887-66cf-4881-a468-91e719fd54ea"> <br>
4) Click "Inbound Rules" (1), then scroll right until you are able see and click the "Protocol" tab (2) <br>
<img width="785" alt="2" src="https://github.com/jaysixco/configure-ad/assets/160427311/1d8d84fc-7469-443f-942b-c57e986095c2"> <br>
5) Scroll back to the left and, while holding down the Shift key on your keyboard, click these two "Core Networking Diagnostics" (1), then click "Enable Rule" (2) <br>
<img width="785" alt="3" src="https://github.com/jaysixco/configure-ad/assets/160427311/519678c6-983a-4b4e-b652-9f4303db229e"> <br>


<strong> Log in to Client-1 and ping DC-1's private IP address to see if it worked  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   <em> Instructions on how to get DC's private IP <br>
1) Start at Virtual Machines homepage <br>
2) Right click name of Window's 2022 VM and open it in a new tab <br>
3) Scroll down <br>
4) Under header called "Networking" you will see a number next to "Private IP address" <br>
5) Copy that number <br>
6) Remote Desktop your way into Client-1 <br>
7) Open command prompt <br>
8) Type "ping" and then paste the Private IP address you copied <br>
9) If it worked, you should see the word "Reply" repeated a few times like this: </em> <br>
<img width="960" alt="ping worked" src="https://github.com/jaysixco/configure-ad/assets/160427311/59817a5c-d136-4890-886b-a99891dec9b4">


<strong> DC-1 (windows 2022 VM) </strong>  
<strong> Install ADDS + setup forest </strong><br>
<strong>&nbsp;&nbsp;&nbsp;&nbsp;   Install ADDS </strong> = On the Service Manager page, click "Add roles and features" </strong><br>
<img width="960" alt="Capture" src="https://github.com/jaysixco/configure-ad/assets/160427311/86f64b1b-abfc-435f-a5ee-8e7135ec307e">
<br>
Keep clicking "Next>" button until you get to "Server Roles" tab (following screen). Click the box next to "Active Directory Domain Services" <br>
<img width="588" alt="Capture" src="https://github.com/jaysixco/configure-ad/assets/160427311/828837cc-8ec0-47f0-b7fc-2af4be09d846">
<br>
After you click the box next to "Active Directory Domain Services", this box will pop up (see screenshot below). Just click "Add Features" <br>
<img width="313" alt="Capture - Add Features" src="https://github.com/jaysixco/configure-ad/assets/160427311/5d63572e-eeb2-4df5-8d3f-d7c03914a40a">
<br>
After that, just keep clicking "Next" until you get to the "Confirmation" tab (see screenshot). Click "Install". Then after it installs, click "Close". <br>
<img width="590" alt="1" src="https://github.com/jaysixco/configure-ad/assets/160427311/b01ac33d-db0d-4c71-8f96-71d3caae2362">
<br>
<br>
<strong> Set up new forest </strong> = On the Service manager page, click the flag and triangle with an exclamation point  in it (1), then click "Promote this server to a domain controller"(2)> <br> 
<img width="956" alt="1" src="https://github.com/jaysixco/configure-ad/assets/160427311/02c4c4b6-160d-4a81-813e-83bebf39c861">
<br>


Click "Add a new forest" and type "mydomain.com" ><br>
<img width="572" alt="Capture2-addforest+username" src="https://github.com/jaysixco/configure-ad/assets/160427311/e043bf1e-0909-4b6f-acc0-6b3faf4153cc">


<br>

Create a password >  <br>
<img width="574" alt="Capture3-password" src="https://github.com/jaysixco/configure-ad/assets/160427311/a3c31e70-009d-47b6-b403-d16e0daf85e6">

<br>
<strong> Keep clicking "Next>" button until you get to the "Prerequisites Check" page. Then click "Install" button. After it installs, it will automatically log you out. </strong><br>
<strong> <em>If you try to log back in to DC-1 (Windows 2022 VM) with "labuser" as the username, it won't work. You have to log back in as "mydomain.com\labuser" in the username. You can still log in with the same password you used for "labuser" (ie. if your password was "Abc123" for username "labuser", the password is still "Abc123" for username "mydomain.com\labuser). </em></strong><br>
1) Start at DC-1 (Window 2022 VM) homepage <br>
2) Copy the Public IP address <br>
3) Open Remote Desktop Login page <br>
4) Paste the Public IP address, then click enter.
5) Click "More choices", then click "Use a different account"<br>
5) For the username, type "mydomain.com\labuser and type the same password you created for the VM <br>
<br>
<strong> Create an Admin account and a place to store all the users we'll create later  </strong><br>
1) Now that you're in to DC-1 (window 2022 VM), type "Active Directory"in Start Menu search box (1) and then cllick "Active Directory Users and Computers (ADUC)" (2) <br>
<img width="960" alt="Capture - ADUC" src="https://github.com/jaysixco/configure-ad/assets/160427311/b947408d-dde2-4fdd-9b40-57cb426ec615">
<br>

<strong> Create an Organizational Unit (OU) called “_EMPLOYEES”  </strong><br>
1) Right click "mydomain.com" <br>
2) Hover mouse over "New" <br>
3) Click "Organizational Unit"
4) Type "_EMPLOYEES" (Underscore not mandatory in '_EMPLOYEES') <br>
<img width="565" alt="Capture - OU" src="https://github.com/jaysixco/configure-ad/assets/160427311/d7c7cb8d-4d7c-40f7-bdd2-12d5f3374e75">
<br>

<strong> Create a new OU named “_ADMINS”  </strong><br>
1) Right click "mydomain.com" <br>
2) New > Organizational Unit <br>
3) type "_ADMINS" <br>
<br>

<strong> Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”  </strong><br>
1) Right click '_ADMINS', hover mouse over "New", then click "User" <br>
<img width="565" alt="1" src="https://github.com/jaysixco/configure-ad/assets/160427311/bbf620b3-088d-43f6-ac1c-660895940107"> <br>
2) For "First name:" type "Jane" <br>
   For "Last name:" type "Doe" <br>
   For "User logon name:" Type "jane_admin"
   Then click "Next>" button<br>
<img width="328" alt="2" src="https://github.com/jaysixco/configure-ad/assets/160427311/a4dbca4e-232c-4eff-85d5-a45d56602ab3"> <br>
3) Create a password (you can use the same one that you created for the VMs)
4) Make sure the only box that is checked is "Password never expires", then click "Next>" <br>
<img width="328" alt="3" src="https://github.com/jaysixco/configure-ad/assets/160427311/aecf2c58-f81b-4e18-8193-6616a8bb248c"> <br>
5) On the page after this, click "Finish"<br>
<br>

<strong> DON'T FORGET to make jane_admin a “Domain Admin” (just because her name is in the Admin folder doesn't mean she's actually an Admin yet)   </strong><br>
<br>

<strong> Add jane_admin to the “Domain Admins” Security Group  </strong><br>
1) Double click "Admins" <br>
2) Right click "jane_admin" <br>
3) Click "Properties" <br>
4) Click "Member Of" tab <br>
5) Click "Add"
6) Type "domain" <br>
7) Click "Check names" <br>
8) Click "Domain Admins" <br>
9) Click following button sequence: "Ok","Ok","Apply","Ok" <br>
<br>

<strong> Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”  </strong>
1) Open command prompt (type "cmd" in Start menu)
2) Type "logoff" and then press enter to log out
3) Copy DC-1's Public IP address, if you don't have it already <br>
4) Open Remote Desktop Login page (type "Remote Desktop" in Start menu <br>
5) Paste the Public IP address, then click enter.
6) Click "More choices", then click "Use a different account"<br>
5) For the username, type "mydomain.com\jane_admin" and type the same password you created for the VM <br>

<strong> Use jane_admin as your admin account from now on  </strong>

<strong> Now we'll be dealing with Client-1  </strong><br>

<strong> CLIENT-1 (windows 10 VM) </strong> <br>
<strong> Starting in Azure, go to DNS server and make it DC-1's private IP </strong> <br>
&nbsp;&nbsp;&nbsp;&nbsp;   Get DC's Private IP address first <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    Click DC-1 > Scroll down until you see "Private IP address" <br>   
1) Go to Azure's Virtual machine page
2) Right click Client-1 (window 10 Vm) and open it in a new tab
3) Under "Networking" on left hand side, click "Network settings" (1) and then Click "Network Interface" (2) <br>
![2](https://github.com/jaysixco/configure-ad/assets/160427311/3ea657a0-0262-47fc-932f-be8243511f63)
5) Click "DNS servers" (1), click "Custom" (2), paste DC-1's (2022 VM) private IP address in the box (3), and then click "Save"<br>
![3](https://github.com/jaysixco/configure-ad/assets/160427311/c71b4a54-869e-4571-9a36-6ef2d729992b)

&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Go to Client-1 (Windows 10 VM) page and hit restart. Wait until it says the VM has been successfully restarted. </strong> <br>

&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Now, log back in through Remote Desktop as labuser </strong> (remember, we haven't joined it to any domain yet)

<strong> In Client-1 (Window 10 VM) rename the PC as mydomain.com\jane_admin </strong><br>
1) Right click the start button <br>
2) Click "Systems" <br>
3) Scroll down <br>
4) Click "Rename this PC (advanced)" <br>
5) Click "Change" <br>
6) Click circle next to "Domain" <br>
7) Type "mydomain.com" and click "Ok" <br>
8) Then in the page that appears type for username:"mydomain.com\jane_admin" and whatever password you want (should probably use the same password you've been using for other parts) <br>
9) A box will pop up telling you that you must restart Client-1 Vm in order to apply the changes. Just click "Ok".
10) There should also be a pop-up (you might have to minimize other screens to see it) that asks if you want to Restart Now or Later. Click the button that says "Restart Now"
<br>

<strong> Remote Desktop for non-administrative users on Client-1 </strong> <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Remote Desktop your way back in to </strong> Client-1 as mydomain.com\jane_admin and open system properties (right click Start button > Click "System") (see screenshot) <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Click </strong> “Remote Desktop” <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Click </strong> “Select users that can remotely access this PC” <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Click </strong> “Add” <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Type </strong>  “domain users”, click </strong> "Check Names", then click "Ok" (see first screenshot). On the page after that, click "OK" as well (see second screenshot) 
//Put the screenshots side by side

<strong> Create a bunch of additional users and attempt to log into Client-1 with one of the users </strong><br>
1) Log in to DC-1 as jane_admin (see screenshot)
2) Open PowerShell_ise as an administrator (type Powershell in start menu search bar, right click "Windows Powershell ISE" > Click "Run as administrator" <br>
   //Get a better screenshot <br> 
   //Put a rectangle around start menu search bar, circle with arrow pointing to "Windows Powershell ISE", and have the screenshot capture "Run as administrator" as well
<img width="625" alt="Capture - Powershell ISE admin" src="https://github.com/jaysixco/configure-ad/assets/160427311/e3e2aabe-786f-423d-a26c-1869817dcea5">
<br>
Open link (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) in a new tab then click "Raw" (screenshot below) 
<br>
<img width="960" alt="Capture - Click Raw" src="https://github.com/jaysixco/configure-ad/assets/160427311/0891ba73-964d-4479-bc91-6e08c6055411">
<br>
Copy all the "Raw" content (ctrl + A, then ctrl + C), then go back to the Powershell Ise homepage (see screenshot below). <br>
Click "New File" (screenshot below, letter A). <br>
Click anywhere in the white section and press "ctrl + V" to Paste. <br>
Click the green play button to run the script (screenshot below, letter B)
<br>
<img width="854" alt="Capture - ctrl + V, New Script, Run Script" src="https://github.com/jaysixco/configure-ad/assets/160427311/31f27fbd-6c3b-47b7-8751-682adbb25135">
<br>
After you click the play button (screenshot above), a bunch of accounts will start generating in the "_EMPLOYEES" organization unit (see screenshot below)
<br>
<img width="565" alt="Capture - Users created" src="https://github.com/jaysixco/configure-ad/assets/160427311/352e9fef-cf56-4b6e-8eac-8956c6b9d500">
<br>

<strong> Log in to Client-1 with one of the accounts </strong><br>
In the screenshot above, we can see that one of the account names is "bapa.mop" so we will use it for our example. <br>
Log out of Client-1. Log back in through Remote Desktop. Click //"Use a different account"// (see first screenshot)
The username is "bapa.mop" (no "mydomain.com" required). 
If you noticed, because of the script all the accounts have the same password as password (see screenshot above<br>
<br>
<strong> Finish. </strong>

<p>
  <em>Steps above are accurate. Are able to complete with steps above. All that is left to do is slight formatting.</em>
</p>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<strong> SUMMARY):</strong>  <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   <em> Instructions on how to get DC's private IP </em> <br>
<strong> Create 2 VMs (1 Window 2022 [DC-1], 1 Window 10 [Client-1]) (Use the same Resource Group and Vnet )</strong>

  
<strong> Change DC-1 NIC to static </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  DC-1 > In the sidebar under "Networking" click "Network settings" > click IP configurations > scroll down and click ipconfig > click static > Save


<strong> Login to DC-1's firewall (hint: type) and enable ICMPv4 traffic  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  Enable - Start menu > type firewall > click option with 'Advanced Security' > Inbound Rules > widen screen so you can see Protocol tab > 
&nbsp;&nbsp;&nbsp;&nbsp;  right click ICMPv4EchoRequests > Enable rule (there's two enable both of them in turn)

<strong> Login to Client-1 and ping DC-1 to see if it worked  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   <em> Instructions on how to get DC's private IP </em>
<img width="960" alt="ping worked" src="https://github.com/jaysixco/configure-ad/assets/160427311/59817a5c-d136-4890-886b-a99891dec9b4">


<strong> DC-1 </strong>  
<strong> Install ADDS + setup forest </strong><br>
<strong>&nbsp;&nbsp;&nbsp;&nbsp;   Install ADDS </strong> = Service Manager > 'Add roles and features' </strong><br>
<img width="960" alt="Capture" src="https://github.com/jaysixco/configure-ad/assets/160427311/86f64b1b-abfc-435f-a5ee-8e7135ec307e">
<br>
Keep clicking "Next>" button until you get to "Server Roles" tab (following screen). Click the box next to "Active Directory Domain Services" <br>
<img width="588" alt="Capture" src="https://github.com/jaysixco/configure-ad/assets/160427311/828837cc-8ec0-47f0-b7fc-2af4be09d846">
<br>
After you click the box next to "Active Directory Domain Services", this box will pop up (see screenshot below). Just click "Add Features" <br>
<img width="313" alt="Capture - Add Features" src="https://github.com/jaysixco/configure-ad/assets/160427311/5d63572e-eeb2-4df5-8d3f-d7c03914a40a">
<br>
After that, just keep clicking "Next" until you get to the "Features" tab (<em>add screenshot later</em>) Click Install. Then after it installs, click "Close".
<br>
<br>
<strong>&nbsp;&nbsp;&nbsp;&nbsp;   Set up new forest </strong> = Service manager > look at upper right on the left side of the word 'manage'; should see what looks like a flag and a triangle with an exclamation point in it, click it > <br> 
<img width="960" alt="Capture-flagexclamation" src="https://github.com/jaysixco/configure-ad/assets/160427311/332bade1-9d4a-4ca8-b582-a198b17bfb73">
>

<br>

Promote > <br>
<img width="960" alt="Capture1-promote" src="https://github.com/jaysixco/configure-ad/assets/160427311/781927ea-eb90-4e9b-a39c-d1c089470f88">
<br>


Click "Add a new forest" and type "mydomain.com" ><br>
<img width="572" alt="Capture2-addforest+username" src="https://github.com/jaysixco/configure-ad/assets/160427311/e043bf1e-0909-4b6f-acc0-6b3faf4153cc">


<br>

Create a password >  <br>
<img width="574" alt="Capture3-password" src="https://github.com/jaysixco/configure-ad/assets/160427311/a3c31e70-009d-47b6-b403-d16e0daf85e6">

<br>
<strong> Keep clicking "Next>" button until you can't anymore. Then click "Install" button. Wait. After it installs, it will automatically log you out. </strong><br>
<strong> If you try to log back in with "labuser" as the username, it won't work. You have to log back in as "mydomain.com\labuser" in the username. You can still log in with the same password you used for "labuser" (ie. if your password was "Abc123" for username "labuser", the password is still "Abc123" for username "mydomain.com\labuser).  </strong><br>
<br>

<strong> Create an Admin account and a place to store all the users we'll create later  </strong><br>
<strong> Log in to DC-1. Type "Active Directory"in Start Menu search box (//edit screenshot later, put red rectangle around the start menu search box and Active Dicrectory//) and click "Active Directory Users and Computers (ADUC)" > </strong><br>
<img width="960" alt="Capture - ADUC" src="https://github.com/jaysixco/configure-ad/assets/160427311/b947408d-dde2-4fdd-9b40-57cb426ec615">
<br>

<strong> Create an Organizational Unit (OU) called “_EMPLOYEES”  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  Right click "mydomain.com" > Hover mouse over "New" > Click "Organizational Unit" > (Underscore not mandatory in '_EMPLOYEES', but done for the lab) <br>
<img width="565" alt="Capture - OU" src="https://github.com/jaysixco/configure-ad/assets/160427311/d7c7cb8d-4d7c-40f7-bdd2-12d5f3374e75">
<br>

<strong> Create a new OU named “_ADMINS”  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  Right click "mydomain.com" > New > Organizational Unit > type "_ADMINS" <br>
<br>

<strong> Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  Right click '_ADMINS' > New > User <br>
<br>

<strong> DON'T FORGET to make jane_admin a “Domain Admin” (just because her name is in the Admin folder doesn't mean she's actually an Admin yet)   </strong><br>
Possible screenshot
<br>

<strong> Add jane_admin to the “Domain Admins” Security Group  </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;  Double click "Admins" > Right click "jane_admin" > Click "Properties" > Click "Member Of" tab > Type "domain" > Click "Check names" > Click "Domain Admins" > Click following button sequence: "Ok","Ok","Apply","Ok" <br>
<br>

<strong> Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”  </strong>

<strong> Use jane_admin as your admin account from now on  </strong>

<strong> Now we'll be dealing with Client-1  </strong><br>

<strong> CLIENT-1 </strong> <br>
<strong> Starting in Azure, go to DNS server and make it DC-1's private IP </strong> <br>
&nbsp;&nbsp;&nbsp;&nbsp;   Get DC's Private IP address first <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    Click DC-1 > Scroll down until you see "Private IP address" <br>   
&nbsp;&nbsp;&nbsp;&nbsp;   Go to Client-1 > Networking > Network Interface > DNS servers > Custom > Paste DC-1's Private IP > Save <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Hit restart </strong> so it logs you out of Client-1 remote desktop <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Log back in as labuser </strong> (remember, we haven't joined it to any domain yet)

<strong> Rename the PC as mydomain.com\jane_admin </strong><br>
&nbsp;&nbsp;&nbsp;&nbsp;   Right click the start button >  Click "Systems" > Scroll down > Click "Rename this Pc (advanced)" > Click "Change" > Click circle next to "Domain" > Type "mydomain.com" > then, username:mydomain.com\jane_admin + password:J~S~2 <br>
<br>

<strong> Remote Desktop for non-administrative users on Client-1 </strong> <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Log into </strong> Client-1 as mydomain.com\jane_admin and open system properties (right click Start button > Click "System") (see screenshot) <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Click </strong> “Remote Desktop” <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Click </strong> “Select users that can remotely access this PC” <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Click </strong> “Add” <br>
&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Type </strong>  “domain users”, click </strong> "Check Names", then click "Ok" (see first screenshot). On the page after that, click "OK" as well (see second screenshot) 
//Put the screenshots side by side

<strong> Create a bunch of additional users and attempt to log into Client-1 with one of the users </strong><br>
1) Log in to DC-1 as jane_admin (see screenshot)
2) Open PowerShell_ise as an administrator (type Powershell in start menu search bar, right click "Windows Powershell ISE" > Click "Run as administrator" <br>
   //Get a better screenshot <br> 
   //Put a rectangle around start menu search bar, circle with arrow pointing to "Windows Powershell ISE", and have the screenshot capture "Run as administrator" as well
<img width="625" alt="Capture - Powershell ISE admin" src="https://github.com/jaysixco/configure-ad/assets/160427311/e3e2aabe-786f-423d-a26c-1869817dcea5">
<br>
Open link (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) in a new tab then click "Raw" (screenshot below) 
<br>
<img width="960" alt="Capture - Click Raw" src="https://github.com/jaysixco/configure-ad/assets/160427311/0891ba73-964d-4479-bc91-6e08c6055411">
<br>
Copy all the "Raw" content (ctrl + A, then ctrl + C), then go back to the Powershell Ise homepage (see screenshot below). <br>
Click "New File" (screenshot below, letter A). <br>
Click anywhere in the white section and press "ctrl + V" to Paste. <br>
Click the green play button to run the script (screenshot below, letter B)
<br>
<img width="854" alt="Capture - ctrl + V, New Script, Run Script" src="https://github.com/jaysixco/configure-ad/assets/160427311/31f27fbd-6c3b-47b7-8751-682adbb25135">
<br>
After you click the play button (screenshot above), a bunch of accounts will start generating in the "_EMPLOYEES" organization unit (see screenshot below)
<br>
<img width="565" alt="Capture - Users created" src="https://github.com/jaysixco/configure-ad/assets/160427311/352e9fef-cf56-4b6e-8eac-8956c6b9d500">
<br>

<strong> Log in to Client-1 with one of the accounts </strong><br>
In the screenshot above, we can see that one of the account names is "bapa.mop" so we will use it for our example. <br>
Log out of Client-1. Log back in through Remote Desktop. Click //"Use a different account"// (see first screenshot)
The username is "bapa.mop" (no "mydomain.com" required). 
If you noticed, because of the script all the accounts have the same password as password (see screenshot above<br>
<br>
<strong> Finish. </strong>

<p>
  <em>Steps above are accurate. Are able to complete with steps above. All that is left to do is slight formatting.</em>
