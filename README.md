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

<h2>High-Level Deployment and Configuration Steps</h2>

<strong>Step 1: Overview<strong>

<p>
<strong>Setting up Active Directory infrastructure, I will configure and interconnect two virtual machines. One acting as a domain controller with a static IP address for the DNS and one acting as a client machine with a dynamic IP address.</strong>
</p>
<img <img width="829" height="708" alt="Screenshot 2025-09-03 140121" src="https://github.com/user-attachments/assets/ae6e2329-5d49-4af7-921f-ae28db986c90"/>
<p>
</p>
<br />

<p>

<strong>Step 2. Create two VMs (Azure)in the same VNET. One will be a Domain Controller, the other will be a Client machine.<strong>

  - Create a virtual machine (Domain Controller) on and another virtual machine (Client-1) on Azure.
  - Name them DC-1 for the Domain Controller and Client-1 for the client machine
  - For the Domain Controller: Select Windows Server 2022: Azure Edition - x64 Gen2 as the image
  - size (Standard_D2s_v3 - 2 vcpus, 8 GiB memory)
  - For ther Client machine: Select Windows 10 pro edition
  - size (Standard_D2s_v3 - 2 vcpus, 8 GiB memory)
</p>
<p>
<img width="2226" height="677" alt="Screenshot 2025-09-04 141758" src="https://github.com/user-attachments/assets/08d89516-e295-4263-84f7-a372e549d4f9"/>

</p>
<br />




<p>
 <strong>Step 3. Set the Domain Controller's Private IP to static</strong>
</p>
- We will change the DC to a static IP because its offering Active Directory services to the client machine (we do not want the private IP address to change so we will put it as static). Client machine will be joined to the domain. we're gonna tell client one to use DC one as the DNS server. And we need to manually configure the DNS settings for client one to use this private IP address. And if this private IP address changes this DNS server, will no longer be valid.
<p>
- Once the VM has been deployed, proceed to the VM overview page and select "Networking" -> network setting on the left side.
</p>
<img width="771" height="444" alt="Screenshot 2025-09-04 142451" src="https://github.com/user-attachments/assets/b8473d58-6d43-4ec4-b93e-ac88125e17c9"/>
<br />



</p>
- Select Network Interface Card -> IP configurations -> ipconfig1 and set Private IP address allocation to static.
<p>
</p>
<img width="769" height="442" alt="Screenshot 2025-09-04 142507" src="https://github.com/user-attachments/assets/00575e62-2987-4f8b-ace7-14fb9ac51c77"/>







<p>

</p>
<p>
<strong>Step 4. Log into DC-1 and go turn off these firewall settings.</strong> 
</p>
- Go to the search bar at the bottom left and type in run.  >  Then type in wf.msc 
This will open Windows defender firewall.
<p>
<img width="411" height="232" alt="Screenshot 2025-09-04 143238" src="https://github.com/user-attachments/assets/c945f46e-c550-43c0-911a-67a6c789633d"/>

</p>
<img width="405" height="250" alt="Screenshot 2025-09-04 143310" src="https://github.com/user-attachments/assets/ae199777-c7f9-4ec9-bb91-6661dbc8bd32" />

<p>
- Make sure to turn off the Domain Profile and Private Profile's Fire wall off.
- Leaving the public profile firewall on.
- Turn the Inbound and Outbound connections rules to allow.
  
</p>
<img width="396" height="449" alt="Screenshot 2025-09-04 143329" src="https://github.com/user-attachments/assets/41f373d8-b2a4-4f70-8fc0-5ccae531a175"/>



<br />



<p>
  <strong>Step 5. Client-1 will connect to DC-1 to ensure connectivity.</strong>
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

</p>



<br />

