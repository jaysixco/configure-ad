# configure-ad

<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />
In this tutorial, you are going to play 3 roles:  administrator, agent, user <br>
In this tutorial, you/we are going to be creating and delegating tickets <br>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
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
<strong> Create 2 VMs (1 Window 2022 [DC-1], 1 Window 10 [Client-1]) (Use the same Resource Group and Vnet )</strong><br>
Windows 2022 - note: don't check the box under Licensing <br>
Windows 10 - DO check the box under Licensing

Create 2 Virtual Machines (VMs)

<strong> First we create a Windows 10 Virtual Machine </strong>

1) Type "portal.azure.com" in the url search bar, which should bring you to the Azure homepage and then click "Virtual Machines" (see screenshot)
![Screenshot 2024-05-19 120803](https://github.com/jaysixco/monitoring-traffic-rd/assets/160427311/3e38d2a9-6b4a-4191-8d7b-371ced1ae53d)

2) On the Virtual Machines page, click "+ Create" (in the left hand corner) and then click "Azure virtual machine" in the drop down menu (see screenshot)
![Screenshot 2024-05-19 121645](https://github.com/jaysixco/monitoring-traffic-rd/assets/160427311/1478bc17-d2e7-4b9e-9da5-d3f1a560becd)


4) For "Resource group *" - click "Create new" and then type whatever name you want <br>
   For "Virtual machine name *" - type whatever name you want <br>
   For "Region *" - Click any option that starts with ("US") <br>
   For "Availability Zone * - Leave it as "Zone 1"<br>
   For "Image *" - click "Windows 10 Pro, version 22H2 - x64 Gen2" <br>
   For "Size *" - Any option that has "2 vcpus" or "4 vcpus" <br>
   For "Username *" - "Windows-10-VM" (for example/whatever you want) <br>
   For "Password *" - "Password1" (for example/whatever you want) <br>
   For "Confirm password *" - whatever you typed in previous step <br>
   For "Public inbound ports *" - "Allow selected ports" <br>
   For "Select inbound ports *" - "RDP (3389)" <br>
   Under "Licensing" check the box next to "I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights." <br>
   When you're done, cLick the blue button at the bottom that says "Review + create"
   An example of how the page should look like when done: <br>
  ![w10vmpg1](https://github.com/jaysixco/configure-ad/assets/160427311/db59fffb-0e55-46a1-aaa2-6492eecb811a) <br>
  <img width="302" alt="w10vmpg2" src="https://github.com/jaysixco/configure-ad/assets/160427311/3efbd4d7-62fd-4eac-853d-588b7785787f">


   
6) Clicking "Review + create" from the previous step will bring you to this page. All you have to do is click the blue button that says "Create".
![Screenshot 2024-05-19 130702](https://github.com/jaysixco/monitoring-traffic-rd/assets/160427311/48b6672d-5ec0-46e2-b79e-4cdf45514bc8)


<strong> Now we create a Windows 2022 Virtual Machine </strong>

1) After you click "Create" in previous step, you will see this page <br>
<img width="956" alt="deployment in progress" src="https://github.com/jaysixco/configure-ad/assets/160427311/11a468e8-9380-41cf-849c-7ccada54790d">
<br>
2) Wait until it turns into this page (see screenshot), then click "Create another VM" <br>
![Screenshot 2024-06-10 074023](https://github.com/jaysixco/configure-ad/assets/160427311/08685797-a9c7-43db-abe0-88f0671d09e6) <br>
3) Click the "Networking" tab <br>
4) Click the drop down menu for "Virtual network" <br>
5) If you don't see any virtual networks, like this (see screenshot), refresh the page <br>
![no virtual network](https://github.com/jaysixco/monitoring-traffic-rd/assets/160427311/739fe2e2-d91b-4bc3-8f9c-d20f1595ab2d) <br>
6) Repeat steps 2 and 3, and now you should see this (see screenshot). Click whichever option ends in "-vnet", then click the "Basics" tab. <br>
<img width="593" alt="image" src="https://github.com/jaysixco/configure-ad/assets/160427311/059c839e-b18d-43d1-ab3d-f0a4a9aa021d">
<br>
7) For "Resource group *" - click the drop down menu and click the name of the resource group you created for Windows 10 <br>
   For "Virtual machine name *" - type whatever you want <br>
   For "Region *" - Same region as the one you chose for Windows 10 <br>
   For "Availability Zone * - Leave it as "Zone 1"<br>
   For "Image *" - "Ubuntu Server 22.04 LTS - x64 Gen2" <br>
   For "Size *" - Any option that has "2 vcpus" or "4 vcpus" <br>
   For "Authentication type" - click "Password" <br>
   For "Username *" - "Windows-2022-VM" (for example/whatever you want) <br>
   For "Password *" - "Password1" (for example/whatever you want) <br>
   For "Confirm password *" - whatever you typed in previous step <br>
   For "Public inbound ports *" - "Allow selected ports" <br>
   For "Select inbound ports *" - "SSH (22)" <br>
   Under "Licensing" **DO NOT** check the box next to "I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights." <br>
   When you're done, click the blue button at the bottom that says "Review + create" <br>
   An example of how the page should look like when done: <br>
<img width="306" alt="w22p1" src="https://github.com/jaysixco/configure-ad/assets/160427311/b6da2760-463e-4efc-ac4a-887d86203203"> <br>
<img width="302" alt="w22p2" src="https://github.com/jaysixco/configure-ad/assets/160427311/84b94ca5-a8d9-4673-aad0-9ef6718402e1">


8) Clicking "Review + create" from the previous step will bring you to this page. All you have to do is click the blue button that says "Create".
<img width="533" alt="w22create" src="https://github.com/jaysixco/configure-ad/assets/160427311/63908051-93c3-4525-b355-6e2985f9f5bc">

<strong> Waiting for the VMs to be created </strong>

1) Click the Microsoft Search bar (1) and then click "Virtual Machines" (2) <br>
![click search then VM](https://github.com/jaysixco/monitoring-traffic-rd/assets/160427311/7b1c4437-bdda-4f29-bce7-fed665dd1380)

2) You might see that 1 VM is running while the other VM is still being created (see screenshot).
<img width="958" alt="1run" src="https://github.com/jaysixco/configure-ad/assets/160427311/78b00970-db1c-4a2a-9924-9e91b6793184">

3) Refresh the page from time to time until it shows that both VM's are running (see screenshot).
<img width="959" alt="2run" src="https://github.com/jaysixco/configure-ad/assets/160427311/21c8866d-d516-467d-bfb2-0b9452e2e758">

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


<h2>started from here</h2>
<strong> Change DC-1 NIC to static </strong><br>
1) Go to the Virtual Machine's page and right click the name of your Windows 2022 DC-1 and open it in a new tab <br><br>
<img width="572" alt="new tab" src="https://github.com/jaysixco/configure-ad/assets/160427311/52d39049-764d-402b-a154-e82dfd2c8b97">

2) In the sidebar under "Networking" click "Network settings" (1) and then Click "IP configurations" (2) <br>
<img width="806" alt="ipconfig" src="https://github.com/jaysixco/configure-ad/assets/160427311/3b7d6193-4724-48cf-b791-9f183f0f7395">
<br>
3) Scroll down and click "ipconfig1" (1), then click "Static" (2), scroll down and then click "Save" (3) <br>
<img width="864" alt="static" src="https://github.com/jaysixco/configure-ad/assets/160427311/29453887-4abb-41da-a15a-1df8880755ee">

4)

<strong> Log in to DC-1's firewall (hint: type) and enable ICMPv4 traffic  </strong><br>
1) Log in to Windows 2022 VM through Remote Desktop <br>
1) Starting from the Virtual Machines homepage (see screenshot), right click the name of your Windows 2022 VM and open it in a new tab <br>
<img width="574" alt="vmhomepage" src="https://github.com/jaysixco/configure-ad/assets/160427311/af994a61-d2c6-4a33-8f41-18fd79c57135"> <br>
<img width="572" alt="new tab" src="https://github.com/jaysixco/configure-ad/assets/160427311/32b3a4d4-0e0c-43d1-9945-55d98ad69f01"> <br>
2) In the Window 2022 VM homepage, look for "Public IP address" and click white space next to it to copy that number to your clipboard <br>
<img width="958" alt="coptoclip" src="https://github.com/jaysixco/configure-ad/assets/160427311/c8a4d375-a65e-49c9-8da0-73110aff4782"> <br>
3) Click the Search button at the bottom of your screen (1), type "Remote Desktop Connection" (2) and then click "Open" (3) <br>
![Remote Desktop Login](https://github.com/jaysixco/monitoring-traffic-rd/assets/160427311/171f3b17-5895-4a7f-9591-1d7b359c3191)
4) Press the "Ctrl" and "V" button on your keyboard at the same time to paste the number you copied in step 2 and then click "Connect" <br>
5) On the page that says "Enter Your Credentials" click "More choices" and then click "Use a different account" <br>
6) Type the username and password you created for Windows 2022 VM, then click the blue button that says "Ok"
7) If this pops up (see screenshot), click "Yes". <br>
![if this pops up](https://github.com/jaysixco/monitoring-traffic-rd/assets/160427311/a3f8403a-d15e-4eef-809d-c961677f0596) <br>
8) As it logs you into the Virtual Machine, there will be a blue page that says "Choose privacy settings for your device". Turn them all off. Then click the blue button at the bottom that says "Accept". <br>
9) Type "firewall" in the start menu search bar <br>
10) Click option with 'Advanced Security' <br>
<img width="960" alt="1" src="https://github.com/jaysixco/configure-ad/assets/160427311/6da63887-66cf-4881-a468-91e719fd54ea"> <br>
11) Click "Inbound Rules" (1), then scroll right until you are able see and click the "Protocol" tab (2) <br>
<img width="785" alt="2" src="https://github.com/jaysixco/configure-ad/assets/160427311/1d8d84fc-7469-443f-942b-c57e986095c2"> <br>
12) Scroll back to the left and, while holding down the Shift key on your keyboard, click these two "Core Networking Diagnostics - ICMP Echo" (1), then click "Enable Rule" (2) <br>
<img width="785" alt="3" src="https://github.com/jaysixco/configure-ad/assets/160427311/519678c6-983a-4b4e-b652-9f4303db229e"> <br>


<strong> Log in to Client-1 and ping DC-1's private IP address to see if it worked  </strong><br>
</strong> Find DC-1's (Window 2022 VM's) private IP address
1) Start at Virtual Machines homepage and right click name of Window's 2022 VM and open it in a new tab <br>
<img width="572" alt="new tab" src="https://github.com/jaysixco/configure-ad/assets/160427311/143bc443-5399-4417-ac4d-0278396edfc4">
3) Scroll down <br>
4) Under header called "Networking" you will see a number next to "Private IP address". Take note of this number. You will need it later on. <br>
<br>

</strong> Remote Desktop your way into Client-1 (Windows 10 VM) </strong> <br>
1) Start at the Virtual Machines homepage <br> <img width="574" alt="vmhomepage" src="https://github.com/jaysixco/configure-ad/assets/160427311/40b3a24c-2806-4cb3-a21b-225bd3b6f8b3">

2) Right click the name of the Windows 10 VM and then click "Open in a new tab" <br> <img width="959" alt="w10newtab" src="https://github.com/jaysixco/configure-ad/assets/160427311/c6aa765f-c40c-463c-a712-2d88d62e363e">

3) In the Window 10 VM homepage, look for "Public IP address" and click white space next to it to copy that number to your clipboard <br>
<img width="959" alt="w10coptoclip" src="https://github.com/jaysixco/configure-ad/assets/160427311/6db1a7b3-1751-4589-8f4b-12905b659715">
<br>
4) Click the Search button at the bottom of your screen (1), type "Remote Desktop Connection" (2) and then click "Open" (3) <br>
![Remote Desktop Login](https://github.com/jaysixco/monitoring-traffic-rd/assets/160427311/171f3b17-5895-4a7f-9591-1d7b359c3191)
5) Press the "Ctrl" and "V" button on your keyboard at the same time to paste the number you copied in step 2 and then click "Connect" <br>
6) On the page that says "Enter Your Credentials" click "More choices" and then click "Use a different account" <br>
7) Type the username and password you created for Windows 10 VM, then click the blue button that says "Ok" <br> 
8) If this pops up (see screenshot), click "Yes". <br>
![if this pops up](https://github.com/jaysixco/monitoring-traffic-rd/assets/160427311/a3f8403a-d15e-4eef-809d-c961677f0596) <br>
9) As it logs you into the Virtual Machine, there will be a blue page that says "Choose privacy settings for your device". Turn them all off. Then click the blue button at the bottom that says "Accept". <br>
<img width="960" alt="1" src="https://github.com/jaysixco/configure-ad/assets/160427311/c86005b3-e364-4ee7-bd05-afaedf71e551">
<img width="960" alt="2" src="https://github.com/jaysixco/configure-ad/assets/160427311/ffbdca21-e6f8-476e-b6db-66decc2e0e99">
<img width="960" alt="3" src="https://github.com/jaysixco/configure-ad/assets/160427311/a4c60e0d-336a-4519-af7d-ff9badce8736">
<img width="960" alt="4" src="https://github.com/jaysixco/configure-ad/assets/160427311/ea450435-4eb5-411b-9d26-beea770d4212">
<img width="960" alt="5" src="https://github.com/jaysixco/configure-ad/assets/160427311/87883c66-8baa-44e1-82d4-611e8ed7bb65">

10) Open command prompt <br>
11) Type "ping" and then paste the Private IP address you copied/memorized <br>
12) If it worked, you should see the word "Reply" repeated a few times like this: </em> <br>
<img width="960" alt="ping worked" src="https://github.com/jaysixco/configure-ad/assets/160427311/59817a5c-d136-4890-886b-a99891dec9b4">

Exit Window 10 VM (add pics from phone)
Click Remote Desktop Icon and then click into Window 2022 VM

<strong> DC-1 (Windows 2022 VM) </strong>  
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
<br>
1) Start at DC-1 (Window 2022 VM) homepage <br>
2) Copy the Public IP address <br>
3) Open Remote Desktop Login page <br>
4) Paste the Public IP address, then click enter.
5) Click "More choices", then click "Use a different account"<br>
5) For the username, type "mydomain.com\labuser and type the same password you created for the VM <br>
<br>
<strong> Create an Admin account and a place to store all the users we'll create later  </strong><br>
1) Now that you're in to DC-1 (Window 2022 VM), type "Active Directory"in Start Menu search box (1) and then cllick "Active Directory Users and Computers (ADUC)" (2) <br>
<img width="960" alt="Capture - ADUC" src="https://github.com/jaysixco/configure-ad/assets/160427311/b947408d-dde2-4fdd-9b40-57cb426ec615">
<br>

<strong> Create an Organizational Unit (OU) called “_EMPLOYEES”  </strong><br>
1) Right click "mydomain.com" <br>
2) Hover mouse over "New" <br>
3) Click "Organizational Unit" <br>
<img width="565" alt="Capture - OU" src="https://github.com/jaysixco/configure-ad/assets/160427311/d7c7cb8d-4d7c-40f7-bdd2-12d5f3374e75">
<br>
4) Type "_EMPLOYEES" (Underscore not mandatory in '_EMPLOYEES') <br>
<br>
<img width="328" alt="employees" src="https://github.com/jaysixco/configure-ad/assets/160427311/3c5df8a7-1190-4748-a8d9-42ef6183977f">
<br>

<strong> Create a new OU named “_ADMINS”  </strong><br>
1) Right click "mydomain.com" <br>
2) New > Organizational Unit <br>
3) type "_ADMINS" <br>
<img width="328" alt="ou admins" src="https://github.com/jaysixco/configure-ad/assets/160427311/327fcff9-e170-471e-a0a5-f12ae69c543b">
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
<img width="328" alt="pg after this, click finish" src="https://github.com/jaysixco/configure-ad/assets/160427311/a762db89-e8f1-40a4-b76b-4193ff24ca93">
<br>

<strong> DON'T FORGET to make jane_admin a “Domain Admin” (just because her name is in the Admin folder doesn't mean she's actually an Admin yet). To do this, follow steps below.  </strong><br>
<br>

<strong> Add jane_admin to the “Domain Admins” Security Group  </strong><br>
1) Double click "Admins" (1) Right click "jane_admin" (2), Click "Properties" (3) <br>
<img width="565" alt="#1" src="https://github.com/jaysixco/configure-ad/assets/160427311/000b1d3d-790c-4715-b83a-72dd8822fa42">
4) Click "Member Of" tab (1), Click "Add" (2), Type "domain" (3), Click "Check names" (4) <br>
<img width="486" alt="#2" src="https://github.com/jaysixco/configure-ad/assets/160427311/f74f48b4-4930-47ea-9c50-f18f113193b4">
8) Click "Domain Admins, then click "Ok" <br>
<img width="429" alt="click domain admins, then ok (1)" src="https://github.com/jaysixco/configure-ad/assets/160427311/41d54a04-1d71-4e72-abcc-51bc7da73ed3">
9) Click "Ok" <br>
<img width="343" alt="click Ok (2)" src="https://github.com/jaysixco/configure-ad/assets/160427311/3bbd5254-8d27-4137-9c77-590b77d0225f">
10) Click "Apply", then click "Ok" <br>
<br>
<img width="308" alt="click Apply, click Ok (3)" src="https://github.com/jaysixco/configure-ad/assets/160427311/16e8dfff-7dc1-4ba2-a839-4fac662d1996">
<br>



<strong> Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”  </strong>
1) Open command prompt (type "cmd" in Start menu) <br>
<img width="623" alt="type cmd" src="https://github.com/jaysixco/configure-ad/assets/160427311/f7e90b86-eec5-419e-afdc-147915196b66"> <br>
2) Type "logoff" and then press enter to log out <br>
<img width="674" alt="type logoff" src="https://github.com/jaysixco/configure-ad/assets/160427311/8ab751f6-7f82-4733-8e44-06fcd9189129"> <br>
3) Copy DC-1's Public IP address, if you don't have it already <br>
4) Open Remote Desktop Login page (type "Remote Desktop" in Start menu <br>
5) Paste the Public IP address, then click enter.
6) Click "More choices", then click "Use a different account"<br>
5) For the username, type "mydomain.com\jane_admin" and type the same password you created for the VM <br>

<strong> Use jane_admin as your admin account from now on  </strong>

<strong> Now we'll be dealing with Client-1  </strong><br>

<strong> CLIENT-1 (Windows 10 VM) </strong> <br>
<strong> Starting in Azure, go to DNS server and make it DC-1's private IP </strong> <br>
&nbsp;&nbsp;&nbsp;&nbsp;   Get DC's Private IP address first <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    Click DC-1 > Scroll down until you see "Private IP address" <br>   
1) Go to Azure's Virtual machine page
2) Right click Client-1 (Windows 10 Vm) and open it in a new tab
3) Under "Networking" on left hand side, click "Network settings" (1) and then Click "Network Interface" (2) <br>
![2](https://github.com/jaysixco/configure-ad/assets/160427311/3ea657a0-0262-47fc-932f-be8243511f63)
5) Click "DNS servers" (1), click "Custom" (2), paste DC-1's (2022 VM) private IP address in the box (3), and then click "Save"<br>
![3](https://github.com/jaysixco/configure-ad/assets/160427311/c71b4a54-869e-4571-9a36-6ef2d729992b)

&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Go to Client-1 (Windows 10 VM) page and hit restart. Wait until it says the VM has been successfully restarted. </strong> <br>
(A.C, #1)

&nbsp;&nbsp;&nbsp;&nbsp;   <strong> Now, log back in through Remote Desktop as labuser </strong> (remember, we haven't joined it to any domain yet)

<strong> In Client-1 (Window 10 VM) rename the PC as mydomain.com\jane_admin </strong><br>
1) Right click the start button, then click "Systems" <br>
<img width="960" alt="right click start, click system" src="https://github.com/jaysixco/configure-ad/assets/160427311/e975a35c-af0d-4aa6-aa6d-627f7ab50fe5"><br>
2) Scroll down <br>
3) Click "Rename this PC (advanced)" <br>
<img width="601" alt="rename this pc" src="https://github.com/jaysixco/configure-ad/assets/160427311/daf013c7-5136-474f-a4d5-64d40361b0e2"><br>
4) Click "Change" <br>
<img width="307" alt="click change" src="https://github.com/jaysixco/configure-ad/assets/160427311/794905cf-860e-451b-becb-031589a41a33"><br>
5) Click circle next to "Domain", type "mydomain.com" and click "Ok" <br>
<img width="242" alt="click circle, type mydomain, click Ok" src="https://github.com/jaysixco/configure-ad/assets/160427311/d2c750e1-d659-44e8-9ad5-5e72d487c384"><br>
6) Then in the page that appears type for username:"mydomain.com\jane_admin" and whatever password you want (should probably use the same password you've been using for other parts) <br>
<img width="342" alt="page that appears type" src="https://github.com/jaysixco/configure-ad/assets/160427311/86749a8a-34ee-4c08-829c-7200b0baf244"><br>
7) A box will pop up telling you that you must restart Client-1 Vm in order to apply the changes. Just click "Ok".<br>
<img width="265" alt="this box will pop up telling you that you must restart, click OK" src="https://github.com/jaysixco/configure-ad/assets/160427311/7ed3a83b-d09f-4b37-8da2-513a3f2d546d"> <br>
8) There should also be a pop-up (you might have to minimize other screens to see it) that asks if you want to Restart Now or Later. Click the button that says "Restart Now"
<br>
<img width="265" alt="click Restart Now" src="https://github.com/jaysixco/configure-ad/assets/160427311/0c179d3a-64a6-46a8-abd2-33b970b5f8ee">
<br>

<strong> Remote Desktop for non-administrative users on Client-1 </strong> <br>
1) Remote Desktop your way back in to Client-1 as mydomain.com\jane_admin and open system properties (right click Start button > Click "System") <br>
2) On the "About" screen that pops up, scroll down and under "Related Settings" heading, click “Remote Desktop” <br>
<img width="601" alt="#1" src="https://github.com/jaysixco/configure-ad/assets/160427311/0e69552e-7acb-4934-9a75-7e5c62e2457c">
3) Scroll down and click “Select users that can remotely access this PC” <br>
<img width="601" alt="#2" src="https://github.com/jaysixco/configure-ad/assets/160427311/55af10b1-6d0a-4286-ae47-39ba468214d9">
4) Click “Add” <br>
<img width="282" alt="remo for nonad users - click Add" src="https://github.com/jaysixco/configure-ad/assets/160427311/60782dae-e5b6-4d39-b258-50cc44a12ea3"> <br>
5) Type “domain users”, click </strong> "Check Names" <br>
<img width="342" alt="type dom users, click check names" src="https://github.com/jaysixco/configure-ad/assets/160427311/ab30e1f2-6b86-40ba-a386-1a1e82b4c6a7"> <br>
6) There might be a popup asking for credentials. Just type "mydomain.com\jane_admin" for the username and whatever password you created then click "Ok". On the page after that, click "OK" as well. <br>
<img width="282" alt="crem for nonad users - on the pg after that click Ok (3)" src="https://github.com/jaysixco/configure-ad/assets/160427311/29e2b2b2-d65c-4870-a6d4-5c24532d2910"> <br>


<strong> Create a bunch of additional users and attempt to log into Client-1 (Window 10 VM) with one of the users </strong><br>
1) Exit Window 10 VM and then switch over to WIndow 2022 VM
2) Open PowerShell_ise as an administrator (type Powershell in start menu search bar, right click "Windows Powershell ISE"(1) > Click "Run as administrator"(2) <br>
 <img width="960" alt="1" src="https://github.com/jaysixco/configure-ad/assets/160427311/c6ac6161-d01a-4ac0-91fe-92fa37c89912"> <br>
2a) If you're asked whether you want to allow this app to make changes to your device, click "Yes'
<br>
3) Open this link (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) in a new tab then click "Raw" (screenshot below) 
<br>
<img width="960" alt="Capture - Click Raw" src="https://github.com/jaysixco/configure-ad/assets/160427311/0891ba73-964d-4479-bc91-6e08c6055411">
<br>
4) Copy all the "Raw" content (ctrl + A, then ctrl + C) <br>
5) Go back to the Powershell Ise homepage (see screenshot below). <br>
6) Click "New File" (screenshot below, letter A). <br>
7) Click anywhere in the white section and press "ctrl + V" to Paste. <br>
8) Click the green play button to run the script (screenshot below, letter B)
<br>
<img width="854" alt="Capture - ctrl + V, New Script, Run Script" src="https://github.com/jaysixco/configure-ad/assets/160427311/31f27fbd-6c3b-47b7-8751-682adbb25135">
<br>
9) After you click the play button (screenshot above), a bunch of accounts will start generating <br>
10) Type "Active Directory" in the start menu and click "Active Directory Users and Computers" <br>
11) Click "mydomain.com" in the sidebar and then double click "_EMPLOYEES". You will see that all the accounts being generated are held here. <em>Example below </em>
<br>
<img width="565" alt="Capture - Users created" src="https://github.com/jaysixco/configure-ad/assets/160427311/352e9fef-cf56-4b6e-8eac-8956c6b9d500">
<br>

<strong> Log in to Client-1 with one of the accounts </strong><br>
<em> In the screenshot above, we can see that one of the account names is "bapa.mop" so we will use it for our example. </em><br>
1) Minimize the Window 2022 VM and then switch over to Window 10 VM
2) Log out of Window 10 VM (open command prompt, type "logoff", then press enter)
3) Log back in with WIndow 10 VM's public IP address through Remote Desktop.
4) Click "More choices" then "Use a different account"
5) Type in the username you chose (ex. "bapa.mop" (no "mydomain.com" required)).
6) If you noticed, because of the script all the accounts have the same password as "Password1" (see screenshot above)<br>
<br>
<strong> Finish. </strong>

<p>
Done. Everything correct, just needs a screenshot run.</em>
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

































































