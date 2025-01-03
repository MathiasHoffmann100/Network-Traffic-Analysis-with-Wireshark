# Network-Traffic-Analysis-with-Wireshark
A project focused on analyzing and filtering network traffic using Wireshark to troubleshoot and optimize network performance.

The project explores the detailed process of capturing packets on an Ethernet port, demonstrating how to apply advanced filters to effectively isolate HTTPS traffic. It also covers methods for identifying IP addresses within the captured data and techniques to exclude specific IP addresses from the capture, ensuring a more precise and efficient traffic analysis.

This is a straightforward yet essential project for understanding traffic interpretation, providing a foundational skill set that can be expanded upon for advanced network diagnostics and security analysis.

First, we need to run Wireshark in the terminal as a SUDO user to ensure we have all the necessary privileges.

sudo usermod -aG wireshark $USER

![image](https://github.com/user-attachments/assets/629ab948-a3c0-4ffd-928e-f447b08b653e)

Second, we start a packet capture in Wireshark on a specific port.
1.	Select the Network Interface: Open Wireshark and choose the network interface you want to monitor (e.g., eth0 or wlan0).
2.	Set Capture Filters (Optional): To focus only on traffic for a specific port, use a capture filter like:
tcp.port==443
This will capture only traffic on port 443 (HTTPS).
3.	Start the Capture: Click the green shark fin button to begin capturing traffic.
4.	Monitor and Stop: Let the capture run for a while to collect sufficient data, then click the red stop button to halt the capture.
5.	Save the Capture (Optional): Save the captured data to a .pcap file for further analysis if needed.

![image](https://github.com/user-attachments/assets/9feedfeb-b684-49f5-a9fd-be1ba063cc16)
![image](https://github.com/user-attachments/assets/0bd6a810-a192-412b-b56c-6c0168b95cf4)

Later, we need to locate the Client Hello and copy the Destination IP. In this case: 40.114.177.156.

![image](https://github.com/user-attachments/assets/09bcfc1e-84e7-4956-b6b8-21f2c0e49f3e)

After this, we search for the IP in a search engine, and we will find out who it belongs to (in this case, DuckDuckGo). Page that was already open at the time of the traffic capture.

![image](https://github.com/user-attachments/assets/92b2c12d-d026-4e43-ac3b-2651c5f0aedc)

![image](https://github.com/user-attachments/assets/d13ccb44-6983-4903-aa24-fa7d631676cb)

Now a page will be detected using a display filter, thus discovering its IP. For that, we will filter the traffic that uses a TLS handshake.

tls.handshake.type==1

We find the Client Hello in the traffic from the search and copy the destination IP address. (In this case, 109.176.239.70).

![image](https://github.com/user-attachments/assets/a429e58f-839f-4a86-b84f-3403c72a5143)

A specific IP address is used as a filter to capture packet data related to a particular website:

ip.addr == 109.176.239.70

![image](https://github.com/user-attachments/assets/94bec1c9-f508-4151-9733-4bad90695d8c)

To have greater precision in the search, we can also filter our IP. This way, we can analyze in detail the traffic between the host and the server we access. (In the current case, the VM's IP is 192.168.2.205).
ip.addr == 109.176.239.70 and ip.src == 192.168.2.205

![image](https://github.com/user-attachments/assets/ca764c04-9de6-4a00-baf6-31e1a5db631c)

If greater precision is needed in the search, both the destination IP (server) and source IP (host) can be filtered.

ip.dst == 109.176.239.70 and ip.src == 192.168.2.205

Finally, conditionals can be implemented to exclude or include packets in the capture. This way, only specific results will be searched for, making subsequent analysis easier. For this, the use of parentheses is very important to avoid excluding, for example, IPs that we actually need.

In this example, I excluded the IP address of hackthebox.com (109.176.239.70). While capturing packets, I searched different pages. However, as a result, we find that the destination address of hackthebox.com does not appear.

!(ip.addr == 109.176.239.70) and (tcp.port == 80 or tcp.port == 443)

![image](https://github.com/user-attachments/assets/46a09f37-bfa9-4703-88fa-18ce1603adf9)

Finally, I removed the parentheses from ports 80 and 443 so that they are included in the search, thus removing them from the exclusion.
!(ip.addr == 109.176.239.70) and tcp.port == 80 or tcp.port == 443

![image](https://github.com/user-attachments/assets/105c773d-f681-4d57-8ec5-d191a323b4a5)

The implemented commands are basic for using Wireshark and traffic analysis, but Wireshark offers many advanced features. More complex filters can be applied for specific protocols, IPs, and ports, along with custom color schemes for traffic visualization. The tool also provides detailed statistics for network performance, such as bandwidth usage and response times, and supports wireless network capture. By mastering both basic and advanced features, Wireshark becomes a powerful tool for troubleshooting, security analysis, and network optimization.
