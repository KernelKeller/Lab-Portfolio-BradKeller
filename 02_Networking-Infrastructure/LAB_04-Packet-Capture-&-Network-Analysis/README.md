# Packet Capture & Network Analysis
**Date Created:** December 08, 2025  
**Last Updated:** December 08, 2025

## **Skills Demonstrated:**
- Capturing network traffic using Wireshark
- Filtering packets using display filters (tcp, udp, dns, http, icmp, etc.)
- Understanding packet structure (Ethernet, IP, TCP/UDP headers)
- Analyzing network events (handshakes, DNS lookups, ARP requests)
- Identifying suspicious or unusual traffic patterns
- Saving, exporting, and documenting packet captures
- Monitoring live traffic from a VM

## **Objective:**
- Learn how to perform packet captures inside a VM and interpret the results.
- Understand how different protocols behave at the packet level.
- Practice applying Wireshark filters to isolate useful traffic.
- Capture events such as:
	- DNS lookups
	- HTTP/HTTPS requests
	- ICMP pings
	- ARP broadcasts
- Build foundational analysis skills useful for cybersecurity, networking, and SOC roles.

## **Tools & Environment:**
- Windows 11 VM
- Wireshark (installed inside the VM)
- Virtual Machine Manager NAT
- Basic networking tools:
- ping
- ipconfig
- Web browser for generating HTTP/HTTPS traffic

## **Steps:**
1. Verified VM network functionality:
    - Ran "ipconfig" to confirm NAT IP address was assigned.
    - Successfully pinged 8.8.8.8 to test connectivity.
    - Successfully pinged google.com to confirm DNS resolution.
    - Loaded google.com in the browser to confirm HTTP/HTTPS reachability.
2. Installed and prepared Wireshark inside the Windows 11 VM:
    - Downloaded Wireshark from wireshark.org.
    - Installed using default settings.
    - Enabled Npcap option
    - USBPcap was left uninstalled (shouldn't be required for network traffic capture).
    - Prompted to select Installation Options during installation. Select options as seen in image Step-2e.
    - Launched Wireshark and verified the NAT network adapter appeared in the interface list.
3. Performed a baseline packet capture:
   - Via Wireshark, started a live capture on the active Ethernet interface.
   - Allowed the capture to run for ~10 seconds to record background network noise.
   - Observed ARP, DNS, Windows system traffic, and general broadcast traffic.
   - Saved capture as LAB04-Baseline-Capture.pcapng.
4. Captured traffic, applied filters and observed packet details. 
    - DNS Lookup:
        - Opened browser and visited example.com.
        - Applied filter in Wireshark: "dns".
        - Observed DNS query and response packets, noting query type and resolved IP addresses.
    - HTTP/HTTPS Requests:
        - Visited https://google.com.
        - Applied filters in Wireshark: "http" and "tls".
        - Observed HTTP GET requests and TLS handshake messages for HTTPS.
    - ICMP Ping Test:
        - Pinging 8.8.8.8 from Windows VM.
        - Applied filter in Wireshark: "icmp".
        - Observed Echo Requests and Echo Replies, confirming connectivity.
    - ARP Broadcasts:
        - Cleared ARP cache and pinged local network host.
        - Applied filter in Wireshark: "arp".
        - Observed ARP requests and replies, including source/destination MAC addresses.
5. Ran a capture for 10 seconds and stopped it. Applied common display filters in Wireshark for observation tests.
    - ICMP Filter:
        - Applied filter in Wireshark: "icmp"
        - No packets observed because no ping traffic was generated at this point.
    - DNS Filter:
        - Applied filter in Wireshark: "dns"
        - Observed standard queries to Google.com and corresponding DNS responses.
    - TCP Filter:
        - Applied filter in Wireshark: "tcp"
        - Observed TCP handshake, PUSH/ACK flags, and TLS handshake packets.
    - UDP Filter:
        - Applied filter in Wireshark: "udp"
        - Observed QUIC (HTTP/3), DNS queries, and SSDP broadcast traffic.
    - Filter by Destination IP:
        - Identified Google IP (24.244.22.19)
        - Applied filter in Wireshark: ip.addr == 24.244.22.19
        - Captured all traffic exchanged with Google.
6. Dissecting packets to break down their layers for understanding
    - DNS filter:
        - Image Step-6a shows a standard DNS query for google.com
        - Source Port = 59779
        - Destination Port = 53
        - Flags = 0x0100 Standard query
        - Query Type = IPv4
    - TCP filter:
        - Image Step-6b shows a standard TCP query for google.com
        - Destination Port = 52707
        - Source Port = 443
        - Flags = SYN/ACK
    - UDP filter:
        - Image Step-6c shows a standard UDP query for google.com
        - Ethernet shows source MAC and destination MAC addresses
        - IPv4 provides information on source and desination IP's
        - UDP provides information on source and destination ports
        - QUIC IETF is the encrypted payload. Demonstates modern, encrypted, connection-oriented behavior over UDP. No payload content is visible due to encryption. Only headers and packet structure.

## **Notes:**
- Linux's default Virtual Machine Manager doesn't seem to have as many options available, like shared clip board and drag and drop. Will consider switching back to VirtualBox for convenience.
 
## **Issues & Fixes:**
- Wasn't able to transfer files from Virtual Machine Manager to my host like I could when using VirtualBox. 
    - Fix: Inserted a USB drive and selected "redirect USB device". Copied the wireshark files onto a USB drive an transferred to host from there. 

## **Outcome:**
- Captured and analyzed live network traffic from a Windows 11 VM using Wireshark.
- Applied filters to isolate DNS, HTTP/HTTPS, ICMP, ARP, TCP, UDP, and QUIC traffic.
- Dissected packets to examine Ethernet, IP, TCP/UDP headers, and application-layer details, including encrypted QUIC/TLS payloads.
- Confirmed normal traffic patterns and built foundational skills for network analysis and cybersecurity tasks.
