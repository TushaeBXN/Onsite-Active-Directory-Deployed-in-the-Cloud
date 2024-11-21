<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>
<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />
<h2>Environments and Technologies Used</h2>
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell
<h2>Operating Systems Used </h2>
- Windows Server 2022
- Windows 10 (21H2)
<h2>Deployment and Configuration Steps</h2>
<p>
</p>
<p>
In this lab, we will set up two VMs within the same VNET: one as a Domain Controller (DC) and the other as a Client machine. The DC will be assigned a static IP to provide Active Directory services. The Client machine will join the domain and configure its DNS settings to use the DC as its DNS server.
</p>
<br />
<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
DC-1 will be assigned a static private IP address. Client-1 will connect to DC-1, and we will verify connectivity by attempting to ping DC-1 from Client-1. Initially, the ping will fail due to ICMPv4 being blocked by the firewall on DC-1. After enabling ICMPv4 on DC-1's firewall, the ping from Client-1 will succeed.
</p>
<br />
<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Next, log in to DC-1 to install Active Directory Users & Computers. Promote the VM to a Domain Controller and create a new forest with the domain "mydomain.com." Restart the VM and log back in as "mydomain.com\labuser." If the steps were completed correctly, you should be able to access and run Active Directory Users & Computers as demonstrated below.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
Awesome, now let’s get to organizing! First, we’re creating some Organizational Units (OUs)—fancy folders for your domain. Start with one named _EMPLOYEES (because every great domain has employees). Then, create another called _ADMINS (gotta keep the bosses happy). To make an OU, just right-click the domain area, hit New -> Organizational Unit, and fill in the name.

Next, hop into your shiny new _ADMINS OU, right-click, select New -> User, and fill in the details for your star admin: Jane Doe. She’s stepping up as an Admin, so her username will be Jane_admin. Finally, give Jane the VIP pass by adding her to the Domain Admins security group. Boom—admin squad assembled!
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
From here on, you can use Jane_admin as the administrator account. Next, we’ll join Client-1 to the domain (mydomain.com). Start by updating Client-1's DNS settings in the Azure portal, setting the DNS server to the DC's private IP address. Once updated, restart Client-1 directly from the Azure portal. The image below confirms that Client-1 is now using DC-1 as its DNS server.
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
Time to bring Client-1 into the club! Head over to System Settings, and under About, look to the right and click Rename this PC (Advanced). In the pop-up, select Change and switch to the domain option. Type in mydomain.com, then enter the credentials for mydomain.com\labuser. Once you hit OK, your computer will restart, and voilà—Client-1 is now officially part of mydomain.com. Welcome to the domain life!
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Fantastic! Client-1 is now part of the domain. Next up, let’s set up Remote Desktop for non-administrative users on Client-1. Log in to Client-1 as an admin and open System Properties. Navigate to the Remote Desktop tab, then grant access to Domain Users for Remote Desktop. Once that’s done, you’ll be able to log in to Client-1 as a regular user. Easy peasy!
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, let’s put this setup to the test! To verify that regular users can RDP into Client-1, we’ll unleash a PowerShell script to generate thousands of users in the domain—because why not go big? Once the users are created, pick one lucky user and try RDP-ing into Client-1. If it works, mission accomplished!
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Boom! The PowerShell script did its thing and created a user named "bab.hubo." We successfully logged into Client-1 using his credentials, proving that normal users can RDP like pros. 🖥️🎉
</p>
