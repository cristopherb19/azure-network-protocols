<p align="center">
<img src="https://imgur.com/Bv6XcRR.png" height="70%" width="70%" alt="network traffic"/>
</p>



<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
Understanding the movement of data within a network is important because it helps IT professionals troubleshoot problems, spot security threats, and more. In this tutorial, we’ll use Wireshark to examine network traffic going to and from Azure Virtual Machines, and we will also experiment with Network Security Groups. 
<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, DHCP, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 22.04

<h2>Key Steps</h2>

- Create a resource group
- Create a Windows 10 virtual machine
- Create a Linux(Ubuntu) virtual machine
- Make sure both virtual machines are in the same virtual network/subnet
- Connect to Windows 10 virtual machine
- Install and open Wireshark on Windows 10 virtual machine
- Observe various protocol traffic (SSH, DHCP, DNS, RDP) using Wireshark
- Clean up Azure environment

<h2>Actions and Observations</h2>

<h3>Create a Resource Group</h3>

<p>
  
- Within Azure, navigate to "Resource Groups"
- Click "Create" and enter a name for your Resource Group and select a region
- Review + create your resource group
</p>
<p>
<img src="https://imgur.com/yAlz9Lu.png" height="80%" width="80%" alt="Resource Group Page"/>
</p>
<br />

<h3>Create a Windows 10 Virtual Machine</h3>

<p>

- Navigate to "Virtual Machines"
- Click "Create" then select "Azure Virtual Machine"
- Under "Resource group", select the resource group you previously created. I named mine, "RG-Network-Activities"
<img src="https://imgur.com/yksNwAr.png" height="80%" width="80%" alt="Virtual machine resource group"/>

- Under "Image", select the Windows 10 Pro option
<img src="https://imgur.com/4o8YoPY.png" height="80%" width="80%" alt="Virtual machine image selection"/>

- Create username and password
- Go to the Networking section, then create a new virtual network titled "Lab2-vnet"
- Review + create your virtual machine
- <img src="https://imgur.com/H0pAAQP.png" height="80%" width="80%" alt="Virtual machine image selection"/>
</p>
<br />

<h3>Create a Linux(Ubunutu) Virtual Machine</h3>

<p>
  
- Navigate to "Virtual Machines"
- Click "Create" then select "Azure Virtual Machine"
- Select the previous Resource Group and Virtual Network used for the Windows 10 VM, ("RG-Network-Activities")
- Under "Image", select the Ubuntu Server 22.04 LTS option
<img src="https://imgur.com/PJCXi37.png" height="80%" width="80%" alt="Virtual machine image selection 2"/>

- Under "Authenication type", select "password" and fill out with username and password of your choice
<img src="https://imgur.com/lcrignK.png" height="80%" width="80%" alt="Virtual machine authentication"/>

- Navigate to the "Networking" tab, and under "Virtual network", select the virtual network used when creating the Windows 10 virtual machine("Lab2-vnet")
<img src="https://imgur.com/19Rqd4S.png" height="80%" width="80%" alt="Virtual network"/>

- Review + create your virtual machine
  
</p>
<br />

<h3>Make sure both virtual machines are in the same virtual network/subnet</h3>

<p>

  - Navigate to "Virtual Machines"
  - Click on your Windows virtual machine and check the name of the "Virtual network/subnet"
  - Click on your Linux virtual machine and make sure the "Virtual network/subnet" is the same as in your Windows virtual machine("Lab2-vnet/default")
  <img src="https://imgur.com/gSH7Je2.png" height="80%" width="80%" alt="Windows 10 Virtual Network"/>
  <img src="https://imgur.com/onCf406.png" height="80%" width="80%" alt="Windows 10 Virtual Network"/>
</p>
<br />

<h3>Connect to Windows 10 Virtual Machine</h3>

<p>

- Open up Microsoft Remote Desktop. If using Mac, install "Windows App" from the App Store and open it
- To connect to the Windows 10 virtual machine, locate the Public IP Address to fill in the "PC name" field of Windows App
- To retrieve the public IP Address of the Windows 10 virtual machine, go to Azure, navigate to your Windows virtual machine and look for its "Public IP Address"
- Copy the public IP address and paste it into the "PC name" field in your Remote Desktop Application and connect
<img src="https://imgur.com/9BfaXtK.png" height="80%" width="80%" alt="Remote desktop IP"/>
<img src="https://imgur.com/YNmPZCs.png" height="80%" width="80%" alt="Remote desktop IP"/>

</p>
<br />

<h3>Install and open Wireshark on Windows 10 virtual machine</h3>

<p>

  - Within your Windows 10 virtual machine, open up Microsoft Edge, and download and install Wireshark from https://www.wireshark.org/ 
  - Open Wireshark

</p>
<br />

<h3>Observe network traffic</h3>

<p>

  - Open Microsoft Windows PowerShell 
  - Within Wireshark, start packet capture by clicking the blue fin icon at the top left(under the "File" button)
  <img src="https://i.imgur.com/vCQosqd.png" height="80%" width="80%" alt="Wireshark packet capture"/>

</p>

<h3>ICMP Traffic</h3>

<p>

  - While in Wireshark, type "ICMP" in the search bar and hit [Enter], this applies a filter that will only display ICMP traffic
  - Head back to Azure, retrieve the private IP address for the Linux(Ubuntu) virtual machine you created. This is done in the same fahsion you retrieved the Windows 10 public IP address
  - After retrieving the private IP address, we are going to attempt to ping from within the Windows 10 virtual machine
  - go to PowerShell and type "ping" followed by the Linux private IP address ("ping 10.0.0.5")
  - Observe ping requests and replies in Wireshark
  <img src="https://imgur.com/oITe2Rv.png" height="80%" width="80%" alt="Wireshark ICMP Traffic"/>
  <img src="https://imgur.com/d43sQjG.png" height="80%" width="80%" alt="Wireshark ICMP Traffic"/>
  - Attempt to ping a public website within PowerShell and observe traffic (e.g. "ping www.google.com")
  <img src="https://imgur.com/uYATxSx.png" height="80%" width="80%" alt="Wireshark Ping Website"/>

  - Initiate a continuous ping from your Windows virtual machine to your Linux(Ubuntu) virtual machine by typing "-t" at the end of the ping ("ping 10.0.0.5 -t") and observe traffic
  <img src="https://imgur.com/VfOtQpe.png" height="80%" width="80%" alt="Wireshark perpetual ping"/>

  - Open the Network Security Group(NSG) associated with the Linux(Ubuntu) virtual machine and disable incoming ICMP traffic by creating a port rule that denies ICMP traffic in Azure
  <img src="https://imgur.com/3w3ww6r.png" height="80%" width="80%" alt="Port rule"/>

  -Observe the ICMP traffic in Wireshark and the command line Ping activity requests should continue to timeout
  <img src="https://imgur.com/NT4NxDl.png" height="80%" width="80%" alt="Ping timeout"/>

  - Re-enable ICMP traffic in the Network Security Group
  - Observe traffic, and if done correctly, the ping activity should resume
  - Stop the ping activity by hitting "Ctrl + C" while in PowerShell
</p>

<h3>SSH Traffic</h3>

<p>

  - In Wireshark, type "SSH" in the search bar and hit [Enter] to apply a filter that will only display SSH traffic
  - From the Windows 10 VM, "SSH into" your Linux(Ubuntu) virtual machine through its private IP address by typing "ssh (username)@(private IP address)" within PowerShell ("ssh labuser@10.0.0.5")
  - In PowerShell, type commands (hostname, id, pwd, etc) into the Linux(ubuntu) SSH connection and observe the SSH traffic in Wireshark
  <img src="https://imgur.com/ewSvGsM.png" height="80%" width="80%" alt="SSH traffic"/>

  - While in PowerShell, exit the SSH session by typing "exit" and pressing [Enter]
</p>

<h3>DHCP Traffic</h3>

<p>

- Head to Wireshark, type "DHCP" in the search bar and hit [Enter] to apply a filter that will only display DHCP traffic
- From your Windows 10 virtual machine, attempt to issue your virtual machine a new IP address with "ipconfig /renew"
- Observe the DHCP traffic in Wireshark.
- In this case our virtual machine maintains the same IP. If we were to release our IP address with ("ipconfig /release") then renew it ("ipconfig /renew") we would see the complete DHCP cycle in Wireshark.
<img src="https://imgur.com/f5ZoYKR.png" height="80%" width="80%" alt="DHCP"/>
</p>

<h3>DNS Traffic</h3>

<p>

- In Wireshark, type "DNS" in the search bar and hit [Enter] to apply a filter that will only display DNS traffic
- Using PowerShell from the Windows 10 virtual machine, use "nslookup" to retrieve the IP addresses for websites (Example: "nslookup google.com" or "ns lookup disney.com")
- Observe the DNS traffic in Wireshark
<img src="https://imgur.com/UQ748Ux.png" height="80%" width="80%" alt="DNS traffic"/>
  
</p>

<h3>RDP Traffic</h3>

<p>

- In Wireshark, filter for RDP traffic: type in "tcp.port == 3389" in the search bar and hit [Enter] to apply a filter that will only display RDP traffic
- Observe the continous RDP traffic between the Windows 10 VM and your local machine
- Notice that traffic is nonstop because the RDP protocol is constantly showing a live stream from one computer to another, so traffic is always being transmitted
<img src="https://imgur.com/elDr3yU.png" height="80%" width="80%" alt="RDP traffic"/>
</p>

<h3>Clean Up Azure Environment</h3>

<p>

  - Now that we have finished inpecting traffic between virtual machines, we will clean up our Azure environment to prevent being charged by Microsoft Azure for allocating resources
  - Close remote desktop connection
  - Delete resource group(s) created in the beginning of the lab
  - Verify that the resource group(s) have been deleted
  <img src="https://imgur.com/swBpCLO.png" height="80%" width="80%" alt="RDP traffic"/>
</p>

<h3>Conclusion</h3>

<p>

Congratulations on completing this project!  I hope this tutorial has taught you how to create and connect to virtual machines within Azure, and given you better insight on how data flows across a network!
  
</p>
