<p align="center">
<img src="https://i.imgur.com/Pe5GT2M.png" height="40%" width="60%"/>
</p>

<h1>Inspecting Network Protocols (ICMP, SSH, DHCP, DNS, and RDP) with Wireshark</h1>
Join me in this project as I analyze network traffic to and from Azure Virtual Machines using Wireshark and experiment with the configuration and functionality of Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Network Security Groups)
- Remote Desktop
- PowerShell/Various Command-Line Tools
- Various Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create 2 virtual machines, one with Windows 10 and the other with Ubuntu
- Connect to the Windows 10 VM using Remote Desktop
- Install Wireshark within the Windows 10 VM
- Initiate a non-stop ping to the Ubuntu VM, add an inbound rule in the Ubuntu VM’s Network Security Group to deny ICMP traffic, and observe the effect in Wireshark
- Re-enable ICMP traffic in the Ubuntu VM’s Network Security Group, observe the effect in Wireshark, then end the ping activity
- SSH into the Ubuntu VM and observe the effect in Wireshark, then exit SSH
- In the Windows 10 VM, attempt to issue the VM a new IP address using ipconfig /renew and observe DHCP traffic in Wireshark
- Use the nslookup command and observe DNS traffic in Wireshark
- Observe ongoing RDP traffic in Wireshark
- Delete Resource Groups in Azure


<h2>Synopsis</h2>

<p>
<img src="https://i.imgur.com/PuaPt3D.png" height="70%" width="70%"/>
</p>
<p>
To complete this lab, I began by setting up two virtual machines: one running Windows 10 (VM1) and the other running Ubuntu (VM2). After creating the virtual machines, I used Network Watcher to verify that both were configured within the same virtual network (vnet) and subnet.
</p>
<br />

<p>
<img src="https://i.imgur.com/aZ5e3QF.png" height="70%" width="70%"/>
</p>
<p>
In the top right corner, I found the public IP address, which I copied and pasted into Remote Desktop to connect to VM1 and use it for the lab.
</p>
<br />

<p>
<img src="https://i.imgur.com/qZAaOKM.png" height="70%" width="70%"/>
</p>
<p>
Once logged into VM1, I used Microsoft Edge to install Wireshark, then opened both Wireshark and Windows PowerShell. In Wireshark, I applied an ICMP filter (highlighted in the green line) and initiated a continuous ping to VM2 using its private IP address. For each ping, Wireshark displayed the request sent from VM1 to VM2, along with the reply from VM2 back to VM1.
</p>
<br />

<p>
<img src="https://i.imgur.com/r2sTBjV.png" height="70%" width="70%"/>
</p>
<p>
After confirming that I could successfully ping VM2, I accessed the Network Security Group for VM2 and added an inbound security rule to block ICMP traffic, preventing VM2 from receiving any pings.
</p>
<br />

<p>
<img src="https://i.imgur.com/hqFeBvp.png" height="70%" width="70%"/>
</p>
<p>
Sure enough, after blocking ICMP traffic to VM2, the ping requests from VM1 are now timing out. In Wireshark, under 'Info,' I can still see the requests being sent by VM1, but there are no replies from VM2, along with a 'no response found!' message.
</p>
<br />

<p>
<img src="https://i.imgur.com/jDPXYbg.png" height="15%" width="15%"/>
</p>
<p>
I then returned to VM2’s Network Security Group, selected the rule I had created, and changed the 'Action' to 'Allow' to permit incoming ICMP traffic to VM2 once again.
</p>
<br />

<p>
<img src="https://i.imgur.com/rSfEOeL.png" height="70%" width="70%"/>
</p>
<p>
Once ICMP traffic was allowed again, both the PowerShell terminal and Wireshark confirmed that replies were once more being sent by VM2. Having completed my observation of ICMP traffic in Wireshark, I ended the ping activity in PowerShell.
</p>
<br />

<p>
<img src="https://i.imgur.com/0gWpLSn.png" height="70%" width="70%"/>
</p>
<p>
Next, I filtered for SSH traffic in Wireshark and used the PowerShell terminal to SSH into VM2. As I connected to VM2 and executed commands, SSH packets were captured and displayed in Wireshark. Once finished, I used the 'exit' command to end the SSH session and return to the original terminal.
</p>
<br />

<p>
<img src="https://i.imgur.com/4jzRJ1P.png" height="70%" width="70%"/>
</p>
<p>
To observe DHCP traffic, I applied a filter for DHCP in Wireshark and used the 'ipconfig /renew' command to attempt to issue a new IP address to VM1. While the private IP address remained unchanged, Wireshark displayed both a request and acknowledgment, confirming that DHCP traffic was generated.
</p>
<br />

<p>
<img src="https://i.imgur.com/wrRxGbT.png" height="70%" width="70%"/>
</p>
<p>
Next, I filtered for DNS traffic in Wireshark and used the 'nslookup' command to query google.com. In the terminal, the command returned both IPv4 and IPv6 addresses, while in Wireshark under 'Info,' I observed A and AAAA records, which correspond to the respective types of IP addresses.
</p>
<br />

<p>
<img src="https://i.imgur.com/mFVXr7c.png" height="70%" width="70%"/>
</p>
<p>
Lastly, I used Wireshark to observe RDP traffic, which didn’t require the PowerShell terminal, as RDP traffic was already being generated by my physical computer’s connection to the VM. Since this was an active connection, RDP traffic was continuously generated, and Wireshark displayed each packet in real-time. For the previous protocols, I filtered them in Wireshark using their names (e.g., 'DNS'), but for RDP traffic, I chose to filter by its port number (tcp.port == 3389). Either method works, as long as you know the port!
</p>
<br />

<p>
💡 All done! This lab marked my first time using Wireshark, and it was a lot of fun. I definitely recommend it to anyone new to IT—it's fascinating to see all the packets being captured in real-time! 💡
</p>
<br />
