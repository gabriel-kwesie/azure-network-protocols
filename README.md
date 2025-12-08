<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 11 (25H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1: Identify Required Traffic
- Step 2: Create NSG Rules
- Step 3: Associate NSGs with Resources
- Step 4: Monitor & Maintain

<h2>Actions and Observations</h2>

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/edea4458-27b6-4457-917e-6d7ab64baa0e" />
</p>
<p>
Go to Azure, create a resource group, set the name, and region > Review and create
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/3aad7789-12ff-4982-8f29-8bf2bbbf84e3" />
</p>
<p>
Create a Virtual machine, select the Create resource group > name virtual machine > select the region >  change the image to windows 11 Pro > choose an appropriate size (2 vcpus, 8 GiB for tutorial) > set username and password > check the box under licensing > go to the networking tab and click create a new virtual network and rename > review and create
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/1cfd618e-089a-44b0-95bc-763c1499192a" />
</p>
<p>
Once previous Virtual machine is created, create a new virtual machine > name virtual machine > put in the same resource group and region > for the image select ubuntu server 22 or 24 > choose an appropriate size (2 vcpus, 8 GiB for tutorial) > set username and password > check the box under licensing > go to the networking tab and select the previously created virtual network (if it doesn’t appear refresh your page and repeat these steps) > once its been assigned to the virtual network review and create
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/b7350476-4703-44d2-85df-f97409ff340d" />
</p>
<p>
To verify that both VMs are in the correct Virtual Network, go to the virtual machines tab within Azure> click the VM> next to Virtual network/subnet, it will show the create network for both machines
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/a6cff554-afdd-4517-9c7e-11cc13a9e82c" />
</p>
<p>
Copy the public IP address of the Windows VM from Azure > Open Remote Desktop > paste the IP > then log in with the created username and password
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/e9a9e8b6-2aa2-4c73-b812-cd7a78b52bee" />
</p>
<p>
If there is a login issue within Azure, go to virtual machines > select the virtual machine > scroll down to help > click reset password > set username or password if needed
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/9e48f8be-0135-45db-96b6-f576f1cae6e3" />
</p>
<p>
Within the Windows VM, open a web browser and search www.wireshark.org > download the Windows x64 installer > open the download, and an installation wizard will pop up> click next and finish the installation 
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/3f4e7092-afd9-4105-83d3-ffea50ce7b0c" />
</p>
<p>
Click the start icon > search Wireshark and launch the application > select Ethernet> click the shark fin icon in the top left to start a packet capture (if the fin is grey, then it has already started) > type icmp in the “Apply a display filter” bar (there should be no activity displayed)
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/1d9102f4-903e-4fb8-8d69-3fc20ca74f50" />
</p>
<p>
In Azure, find the Ubuntu/Linux VM’s private IP addresses by clicking the VM’s name > go back to the Windows VM in Remote Desktop and open PowerShell > type “ping (private IP of the Linux VM)” (Within Wireshark, you will see an update)
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/5c1c5987-47cd-48bc-bfdc-dcd8ce2fae82" />
</p>
<p>
Within the Windows VM, in PowerShell, type “ping (private IP) -t” (this will start a perpetual ping) > now in Azure, go to > Virtual machines > the Ubuntu/Linux VM > Networking > Network Settings > now under click under Network security group >  setting > inbound security rules > click add > source will be any, destination will be any, put an * in destination port ranges, Protocol will be ICMPv4, action will be deny, and priority will be 290 > name the rule (DenyAnyCustomAnyInbound) > add (this will start to time out requests within PowerShell and Wireshark in the VM) 
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/4f2c7669-5008-4487-8edb-ce2c70ad8bb0" />
</p>
<p>
Back in Azure, delete the rule previously created > go back to the Windows VM (It will start to show replies like before the rule was in place) > press control-c in PowerShell to stop the ping > click the stop icon to stop Wireshark
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/5824a236-3850-42e2-956b-367178e57245" />
</p>
<p>
Within the Windows VM, close and reopen Wireshark > select Ethernet and press the shark fin icon to start a new packet capture (if not already started) > type ssh in the “Apply a display filter (there should be no traffic once applied)
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/f040c0d1-aa37-491e-a37d-89ecb57381f5" />
</p>
<p>
Close and reopen PowerShell within the Windows VM > type ssh (username of the Linux VM)@(Private IP Address) and press enter > it will ask if you want to connect, type yes > now type in the password of the Linux VM (Text will not be shown but is being imputed) and press enter (Now you are connected to the Linux VM) > Observe the traffic in wireshark > to exit the SSH connection type exit and press enter
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/ca92e264-5d22-4fc1-9e7b-ff5a52cd6f84" />
</p>
<p>
Within Wireshark, clear the filter and type in DHCP > Within Powershell, run ipconfig /renew (Releases current IP address and gives you a new one)
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/7380e975-aeaf-4406-b5a2-47f84da64d28" />
</p>
<p>
Within Wireshark, clear the filter and type dns > restart the capture (should have no traffic) > in PowerShell type nslookup (Example: disney.com) (giving us the website's IP address) (When searched, you won't be able to access the usual site domain)
</p>
<br />

<p>
<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/e2b0ada6-64ef-438d-8763-4b0db41f0adc" />
</p>
<p>
Within Wireshark, clear the filter and type (tcp.port==3389) RDP (Remote Desktop Protocol) > (it will immediately start  spam due to it displaying remote desktop activity)
</p>
<br />
