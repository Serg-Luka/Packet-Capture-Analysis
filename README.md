# Packet Capture Analysis (Wireshark & tcpdump)
In this project, I performed a network analysis on packet capture files using Wireshark and tcpdump. This lab is based on Qwiklabs and is hosted on Google Cloud.

### Skills Acquired

- Network traffic filtering and analysis
- Packet inspection
- Application of specific filters for focused network traffic investigation

### Tools Used

- Wireshark
- tcpdump
- Qwiklabs


#### The project scenario is outlined below:

## Scenario 1 - Network filtering with Wireshark
_In this scenario, you’re working as a security analyst investigating traffic to a website._

_You will analyse a network packet capture file that contains traffic data from a user connecting to a website. Filtering network traffic using packet sniffers to extract relevant information is a key skill for a security analyst._

_Your task is to filter the data to identify the source and destination IP addresses involved in the web browsing session, examine the protocols used when the user connects to the website, and investigate some of the data packets to understand the type of information exchanged between systems during the network capture._

### Task 1. Exploring data with Wireshark

For this task, I opened a network packet capture file containing data from a system that made web requests to a site. I launched this file with Wireshark to get an overview of how the data was displayed in the application.

I opened the packet capture file by double-clicking the sample `p.cap` file called 'sample' on the Windows desktop, which launched Wireshark. Then, I maximised the Wireshark window by double-clicking the title bar next to the `sample.pcap` filename. The Wireshark window displayed the contents of the `p.cap` file, as shown below:

![Z1](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/623cc9b2-3344-4b79-96cf-873c4d436b6e)

A large amount of network packet traffic is listed. I will apply filters to focus on the information I need.

The key property columns for each packet are as follows:

- **No.** : The packet's index number in this capture file
- **Time:** The timestamp of the packet
- **Source:** The source IP address
- **Destination:** The destination IP address
- **Protocol:** The protocol of the packet
- **Length:** The total size of the packet
- **Info:** Information about the packet data (the payload) as interpreted by Wireshark

As I scroll, I notice that not all data packets are the same colour. Colouring rules provide visual cues to help quickly classify different types of data. Given that network packet capture files can contain vast amounts of data, these colour rules assist in swiftly identifying relevant packets. Our `sample.pcap` includes a group of light blue packets with DNS traffic and green packets with a mix of TCP and HTTP protocol traffic.

On examining the packets, I found that ICMP was the protocol type for the first (and all) packets that contained an 'Echo (ping) request' in the info column.

### Task 2. Applying a basic Wireshark filter and inspecting a packet

In this task, I conducted a detailed analysis of the network packet and applied filters to inspect the network layers and protocols within the packet.

I started by applying the display filter `ip.addr == 142.250.1.139` in the text box and pressing enter.

![Z2](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/c0cdd65b-4e2c-49ec-9ec1-b1b142d45d35)

This filter significantly reduced the list of packets displayed, showing only those where either the source or destination IP address matched the one I entered. At this point, only two packet colours remained: light pink for ICMP protocol packets and light green for TCP (and HTTP, a subset of TCP) packets. 

---
I then double-clicked the first packet that listed TCP as the protocol, opening the packet's detail window as shown below:

![Z3](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/796f5fd2-8d0b-4b0e-baa2-a2539faeee74)

The upper section of the window contains subtrees where Wireshark analyses the different parts of the network packet. The lower section displays the raw packet data in hexadecimal and ASCII text, with placeholder dots `“.”` indicating fields where character data is unavailable.

---
Double-clicking **Frame** - the first subtree - reveals information about the overall network packet, or frame, including the frame length and the packet's arrival time. At this level, I can see details about the entire packet of data. The window pane looks like this:

![Z4](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/d3486505-f079-4a68-8295-d92d972d618f)

---
Collapsing the first subtree and double-clicking **Ethernet II**, the second subtree, I observe packet details at the Ethernet level, such as the source and destination MAC addresses and the type of internal protocol contained within the Ethernet packet. The window pane displays as shown below:

![Z5](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/7dedc356-1e6c-49b9-a7e2-f2ad4ffff0b4)

---
Collapsing the second subtree and double-clicking **Internet Protocol Version 4**, the third subtree, I find packet data about the Internet Protocol (IP) within the Ethernet packet. This includes source and destination IP addresses and the internal protocol (such as TCP or UDP) carried within the IP packet. The source and destination IP addresses here match those shown in the summary display for this packet in the main Wireshark window.
The window pane appears as below:

![Z6](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/2d2e3f84-6fb5-44d5-af9c-83d35c4f609e)

---
Collapsing the third subtree and double-clicking **Transmission Control Protocol**, the fourth subtree, provides details about the TCP packet, such as source and destination TCP ports, sequence numbers, and flags.

![Z7](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/fcd2e3b7-ce35-4c01-a8d7-4a72aa219384)

The source and destination ports here correspond to the ports in the summary display for this packet in the main Wireshark window. I also noticed that Port 80 was the TCP destination port for this packet, indicating the initial web request to an HTTP website, typically listening on TCP port 80.

---
Within the **Transmission Control Protocol** subtree, I scrolled down and double-clicked **Flags**, which provides a detailed view of the TCP flags set in this packet, as shown below:

![Z8](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/e188a8d7-d047-4cfe-8e83-44d0139ef193)


### Task 3. Using filters to select packets

In this task, I used filters to examine specific network packets based on their source or destination. I applied filters to isolate packets using either their Ethernet Media Access Control (MAC) address or their Internet Protocol (IP) address.

I started by filtering traffic from a specific source IP address using the query `ip.src == 142.250.1.139`. This returned a list with fewer packets, containing only those originating from 142.250.1.139.

![Z1](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/82187bd3-14cd-4c3f-8791-6d11b7ee8e73)


Next, I filtered traffic to a specific destination IP address using the query `ip.dst == 142.250.1.139`. This returned a list of packets sent to 142.250.1.139.

![Z2](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/c464cfdd-33b4-411b-afd6-1c3928ae7b76)


To filter traffic by Ethernet MAC address, I used the query `eth.addr == 42:01:ac:15:e0:02`. This filtered traffic related to one MAC address, regardless of the other protocols involved:

I then opened the first packet in the list and expanded the **Ethernet II** subtree. The MAC address specified in the filter appeared as either the source or destination address in the expanded subtree.

I then navigated to the **Internet Protocol Version 4** subtree and checked the fields **Time to Live** and **Protocol**.

The **Protocol** field showed that the internal protocol for the first packet from MAC address `42:01:ac:15:e0:02` was ICMP.

### Task 4. Using filters to explore DNS packets

In this task, I used filters to select and examine DNS traffic. Once filtered, I drilled down into the DNS protocol to analyse how the packet data contained both queries (site names being looked up) and answers (IP addresses returned by the DNS server when a name is resolved).

To filter for UDP port 53 traffic, I used the query `udp.port == 53`. DNS traffic uses UDP port 53, so this query lists only traffic related to DNS queries and responses, as shown below:

![4](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/49b0b86c-12ee-491a-b31d-f39efdd6f9b3)

By double-clicking the first packet in the list, I opened the detailed packet in the window.

Expanding the **Domain Name System (query)** and selecting **Queries**, I observed that the queried website was _opensource.google.com_, as shown below:

![Z5](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/4fbef1a6-1652-4b15-8dac-e49e649cf708)

I also noted that the IP address 142.250.1.139 appeared under the **Answers** section for the DNS query for `opensource.google.com`.

## Task 5. Using filters to explore TCP packets
In this task, I applied additional filters to select and examine TCP packets, specifically searching for text present in the payload data. This allowed me to locate packets based on specific text, such as a name or other significant data.

To filter for TCP port 80 traffic, I used the query `tcp.port == 80`, with results as shown below:

![Z6](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/de8f9aba-8091-4b8d-9958-5e2213264a13)
TCP port 80 is the default for web traffic, and I observed multiple packets created when the user accessed `http://opensource.google.com`.

Selecting the first packet, with a destination IP address of 169.254.169.254, I found that the **Time to Live value** in the Internet Protocol Version 4 subtree was `64`.

![Z7](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/6513bf7c-86a3-4cc0-a7e5-26e80b027e0f)

The Frame Length was `54` bytes, as shown in the Frame subtree:

![Z8](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/23e8b97c-b3b9-411d-83f0-1f1dac430cf8)

By applying the filter `tcp contains "curl"`, I selected packets that contained the specific text "curl" in the payload data, filtering packets related to web requests made using the curl command, as shown below:

![Z9](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/442b409d-8fee-4030-9d4d-f96da1f17cbd)


## Conclusion

In this project, I successfully analysed network traffic using Wireshark and tcpdump. By applying various filters, I efficiently sifted through data packets to identify relevant information. I identified source and destination IP addresses, examined the protocols used during web browsing sessions, and investigated the payload data within packets.

I gained insights into network layers and protocols, specifically DNS traffic for domain name queries and their corresponding IP addresses, as well as TCP packets for web requests and responses.

This project improved my skills in network traffic analysis and solidified my foundation in using Wireshark and tcpdump for real-world network security and troubleshooting scenarios. These skills are essential for any security analyst or network professional, enabling effective monitoring and analysis of network communications.
