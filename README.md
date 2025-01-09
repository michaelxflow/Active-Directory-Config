<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying Active Directory in Azure!</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

- Create Virtual Network and Virtual Machines in Microsoft Azure
- setup of domain controller and a client desktop vitual machine
- Configure DNS settings to connect both desktops for remote access
- ping to the dns server with powershell to ensure connection
- 

<h2>Azure Virtual Machine Steps!</h2>

<p>
<img src="https://github.com/user-attachments/assets/bf50a935-1aeb-45cd-95c0-407279e1b9be" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
To begin this lab, create your "Resource Group" in Microsoft Azure and name it "Active-Directory-Lab"

!Remember your Region you picked for the recourse group, this will have to stay the same for your Virtual Network and Virtual Machine for the next steps!
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/3798b29c-2e36-47f2-9deb-649486f109a1" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go back to home in Azure and create your Virtual Network

Select your Resource Group you just created and name this Virtual Network "Active-Directory-VNet", then hit review and create! Now on to making your VM!
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/1865d7c9-e832-4826-a7b2-5ddb6fbbd9a7" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now lets make your Virtual Machine! Choose your Recourse Group with the same Region selected, then name your VM "DC-1"
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/3e5fa800-65af-4516-a2cf-b9aae4e0bc80" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
For your image select "Windows Server 2022 Datacenter x64 Gen2" this is going to be your OS for the VM.

For the size I picked "Standard_E2_v3-2 vcpus,16GB memory" you can pick any that has at least 2 vcpus for this vm and 8GB or more for memory, this will ensure your VM to handle the loads of this lab!

As seen obove, make your username and password for the login to this vm. I used "labuser" and "Password123!" as my password. You can make up your own, just make sure you dont forget it!

<img src="https://github.com/user-attachments/assets/a18c8b50-2257-405d-a64e-d8cf35e80a6f" height="50%" width="50%" alt="Disk Sanitization Steps"/>

Make sure you check the box below for your licensing before you continue!
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/69b7ef8c-2da7-4613-bc19-bbc2175163f5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to the "Networking" part of this VM and make sure you have selected your Virtual Machine you created! Then hit "Review+Create"
</p>
<br />
Now lets create our Client's Virtual Machine
<p>
<img src="https://github.com/user-attachments/assets/61acb944-e6d8-4796-ad33-398864f6f743" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Repeat the same process as you did with the first VM! Name this VM "Client-1" Select your image as "Window 10 Pro, version 22H2-x64 Gen2" for the OS.

Put in your username and password! You can use the same one as the first VM, then select the Licensing box, select your Virtual Network and "Create"!
</p>
<br />
Before we continue, we need to go back to our VM "DC-1" and set its Private IP Address as "Static". Azure creates a "Dynamic" IP Address, we need to make it "Static" so there's no possibility it will change!

This will ensure we can Remote Desktop Connect with "Client-1" VM using the IP Address of "DC-1" with no hiccups!
<p>
<img src="https://github.com/user-attachments/assets/3db0fcc2-969f-4b99-9d66-9f9d471d7ffb" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to the "Networking" section in "DC-1", drop down to "Network Settings" and click on the "Network Interface Card(NIC)". 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/69c75203-4b62-4e42-a961-98d33f4b601b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to the the "IP Configurations Setting" and click on "ipconfig1".
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/674101fe-aedc-44e8-8760-9193f7dba8f0" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Under "Allocation" change "Dynamic" to "Static". Keeping your IP Address "10.0.0.4" the same! You might have a different IP Address assigned, just keep that one the same! Then hit "Save"!
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/e76b2936-2daf-497c-8098-06b7e8b148c5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to "DC-1" to copy its Public IP Address, we are going to load up uor desktops "Remote Desktop Connection" and paste it to connect to the VM!
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/738afe62-47c9-4a3d-8ca6-352e294efe0b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Enter your Login Credentials to connectto the VM!
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/9da1cac9-253c-4b86-864f-075e3f2015b0" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once your Domain Controller loads in, you're going to right click the Windows start button, click "Run" and type "wf.msc" for Windows Firewall!
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/8ec3046f-7a7b-42b1-97af-a6a35ad0f205" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click on "Windows Defender Firewall Properties" to turn off Firewall Satus on all three "Domain, Private, and Public Profile" settings, hit "Apply" then "Okay". Firewall is now off!
</p>
<br />
We need to then set "Client-1" DNS settings to "DC-1" Private IP Address! Go back into Azure and Copy "DC-1" Address "10.0.0.4".

</p>
<p>
<img src="https://github.com/user-attachments/assets/bcffa1f6-129d-490d-9221-ed7d6320f2f6" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go into "Client-1" under "Networking" click "Network Settings" and go into the "NIC Settings" to configurate.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/44fd55d3-dd88-4569-a9f7-4c9d9cc0aa93" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Under "DNS Servers" change from "Inherit from Virtual Network" to "Custom", then paste "DC-1" DNS IP Address and Save!
</p>
<br />
Go back to the VM tab in Azure and by "client-1" click restart so changes are applied!

Once its finished restarting copy "Client-1" Public IP Address like before and login into the VM using your desktops "Remote Desktop Connections".
<p>
</p>
<br />

<h2>Pinging your Desktops!</h2>

Next we are going to PING DC-1's Private IP Address to ensure there's an astablished connection to both Virtual Machines.
<p>
</p>

Taking DC-1's IP Address "10.0.0.4", open up "Client-1" VM. Search "up Powershell" in the Windows Search command bar and open as Administrator.
<p>
<img src="https://github.com/user-attachments/assets/017f65e7-5a19-4531-aecf-76fa691bd8d4" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the command prompt, type "ping 10.0.0.4". This will ping to "DC-1" ensuring there is connection to the VM. Sending 4 packets and receiving 4 back as a test! 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/547eb654-7e00-4eb2-908a-a08081c14be4" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next type in "ipconfig /all" into powershell, then we can see that the DNS Server is in fact connected to DC-1's DNS Server "10.0.0.4"
</p>
<br />


<h2>Installing Active Directory!</h2>


Now that both of our Virtual Desktops are ready and have connection. We are finally able to install Active Directory!
<p>
</p>
Login to "DC-1", click on Windows Start button and pull up "Server Manager".

<img src="https://github.com/user-attachments/assets/f1730762-6800-4d40-a802-05bb34a187cf" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
You are now on the Server Manager Dashboard! Go to "Add roles and features. Click next until you get to "Server Roles". Click on the box to install "Active Directory Domain Services", "Add features" and continue to "Next" to install.

Now that Active Directory is installed, we then need to configure it to be an actual Domain Controller called "a new forest"! 
</p>
<br />

</p>
<p>
Go back to the Server manager, in the upper right corner, click go to the yellow "Flag" and click on "Promote this server to a Domain" under "Post-deployment configuration". 
</p>
<br />

<img src="https://github.com/user-attachments/assets/99e4b415-c1bc-41b6-9981-d34a2647ac88" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
Select "Add a new Forest", name the "Root Domain Name" as "mydomain.com. Click next and enter a password "EX: Password1". Next uncheck "Create DNS delegation" Then Install!
</p>
<P>
<img src="https://github.com/user-attachments/assets/c15452a6-d413-4fbd-858c-d6dd47ce8255" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
This will automatically restart, once its done log back in on Remote Desktop Connection to "DC-1". We need to specify that we are logging in as a Domain Controller Admin!"
</p>
<br />

<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

</p>
<br />

<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

</p>
<br />


