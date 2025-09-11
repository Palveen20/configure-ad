<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial demonstrates how to deploy an on-premises style Active Directory Domain Services (AD DS) infrastructure in the cloud using Microsoft Azure Virtual Machines. The setup includes a Domain Controller (DC) and a client machine, both connected within the same Virtual Network (VNet).

By following these steps, you will configure Active Directory in Azure, establish proper DNS functionality, and verify connectivity between the domain controller and the client machine.<br />


<h2>Environments and Technologies Used</h2>
Microsoft Azure – Virtual Machines (Compute)
Remote Desktop (RDP) – to connect to VMs
Active Directory Domain Services (AD DS)
PowerShell – for management and verification
Windows Firewall – configuration for connectivity


<h2>Operating Systems Used </h2>

- Windows Server 2022 (Domain Controller)
- Windows 10 (Client)

<h2>High-Level Deployment and Configuration Steps</h2>

<strong>Step 1: Overview<strong>

<p>
<strong>We will deploy two virtual machines inside the same Azure VNet:
DC-1 – Windows Server 2022, acting as the Domain Controller with a static private IP for DNS services.
Client-1 – Windows 10, acting as the client machine with a dynamic IP but configured to use DC-1 as its DNS server.
The static IP ensures that the DC always has a consistent address, which is required for reliable DNS resolution and domain joins.</strong>
</p>
<img <img width="829" height="708" alt="Screenshot 2025-09-03 140121" src="https://github.com/user-attachments/assets/ae6e2329-5d49-4af7-921f-ae28db986c90"/>
<p>
</p>
<br />

<p>

<strong>Step 2. Create Two Virtual Machines in Azure<strong>
In the Azure Portal, create two VMs in the same Resource Group and Virtual Network.
Domain Controller (DC-1)
Image: Windows Server 2022 Azure Edition (x64 Gen2)
Size: Standard_D2s_v3 (2 vCPUs, 8 GiB RAM)
Client Machine (Client-1)
Image: Windows 10 Pro
Size: Standard_D2s_v3 (2 vCPUs, 8 GiB RAM)
  
</p>
<p>
<img width="2226" height="677" alt="Screenshot 2025-09-04 141758" src="https://github.com/user-attachments/assets/08d89516-e295-4263-84f7-a372e549d4f9"/>

</p>
<br />




<p>
 <strong>Step 3. Assign a Static Private IP to DC-1 </strong>
</p>
Since the DC provides DNS services, its IP must remain constant.
Open Azure Portal > DC-1 > Networking.
Select the Network Interface Card (NIC).
Go to IP Configurations > ipconfig1.
Change Private IP address allocation from Dynamic to Static.
</p>
<img width="771" height="444" alt="Screenshot 2025-09-04 142451" src="https://github.com/user-attachments/assets/b8473d58-6d43-4ec4-b93e-ac88125e17c9"/>
<br />



</p>

<img width="769" height="442" alt="Screenshot 2025-09-04 142507" src="https://github.com/user-attachments/assets/00575e62-2987-4f8b-ace7-14fb9ac51c77"/>







<p>

</p>
<p>
<strong>Step 4. Configure Firewall on DC-1 </strong> 
</p>
Log in to DC-1 using RDP.
Press Win + R, type wf.msc, and press Enter to open Windows Defender Firewall.
<p>
<img width="411" height="232" alt="Screenshot 2025-09-04 143238" src="https://github.com/user-attachments/assets/c945f46e-c550-43c0-911a-67a6c789633d"/>

</p>
<img width="405" height="250" alt="Screenshot 2025-09-04 143310" src="https://github.com/user-attachments/assets/ae199777-c7f9-4ec9-bb91-6661dbc8bd32" />

<p>
Modify firewall profiles:
Turn off: Domain Profile, Private Profile
Keep enabled: Public Profile
Set inbound and outbound rules to Allow connections.
  
</p>
<img width="396" height="449" alt="Screenshot 2025-09-04 143329" src="https://github.com/user-attachments/assets/41f373d8-b2a4-4f70-8fc0-5ccae531a175"/>



<br />



<p>
  <strong>Step 5.Test Connectivity from Client-1</strong>
</p>
  - To ensure connectivity between the two VM's, we will ping the domain controller from the client.
<p>


  - Login to VM client-1 using remote desktop (for mac user use windows app)

  - Find DC-1's private IP address in the Azure Portal and copy it. Proceed to Client-1 and open the terminal and type "ping (DC-01 private ip address)"

  - Now we can ping DC-1 successfully from Client-1
  - Then we can type ipconfig /all to see that the DNS server is DC-1 private IP address.


</p>
<img width="1996" height="1249" alt="Screenshot 2025-09-03 203420" src="https://github.com/user-attachments/assets/367d3abf-89e2-4d1b-93e3-a277d50ab847"/>

<p>
You have now deployed a basic Active Directory environment in Azure that mimics an on-premises setup. The Domain Controller (DC-1) has a static private IP, ensuring DNS consistency, while the client (Client-1) can communicate with the DC over the Azure VNet.
This foundational setup prepares the environment for the next steps:
Installing Active Directory Domain Services (AD DS) on DC-1
Promoting DC-1 to a Domain Controller
Joining Client-1 to the domain
</p>



<br />

