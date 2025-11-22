# Virtual Network Setup
**Date Created:** November 22, 2025  
**Last Updated:** November 22, 2025

## **Skills Demonstrated:**
- Creating and managing virtual networks in a hypervisor
- Configuring internal, NAT, bridged, and host-only adapters
- Assigning IP addresses and validating connectivity between VMs
- Understanding network segmentation and isolation
- Testing communication using ping, ipconfig/ifconfig, and routing tools
- Basic subnetting and network planning

## **Objective:**
- To build a functional virtual network environment that simulates a small real-world infrastructure
- To understand how different virtual network adapters behave (NAT, bridged, host-only, internal)
- To verify network connectivity between systems and ensure proper segmentation and routing
- To prepare a foundation for later labs involving DHCP, DNS, firewalls, packet capture, and troubleshooting

## **Tools & Environment:**
- Virtualization Platform: VirtualBox 
- Operating Systems: Windows 11 VM, Linux Mint VM
- Network Tools: ping, ipconfig, ifconfig, nmcli, VirtualBox network manager
- Topology:
	- Two VMs connected to the same virtual network
	- Optional: A third VM for testing isolation or acting as a service host

## **Steps:**
1. Created new Host-Only network: vboxnet0 (default name)
	- Assigned IP: 192.168.50.1
	- Netmask: 255.255.255.0
	- DHCP disabled
	- Purpose: Internal lab LAN, isolated from the internet
2. Isolated both VM's from internet by setting Windows 11 and Linux Mint VM's Adapter 1 to Host-Only and selected the newly created Network. This should allow them to communicate with each other over Network IP 192.168.50.0/24.
3. Set Windows IP to 192.168.50.10/24 and set Linux Mint IP to 192.168.50.11/24. No gateway or DNS yet, just internal network only. Verified Windows config using ipconfig and Linux config using ip addr + nmcli.
4. Ping tests between VMs were attempted, but only one VM could run at a time due to hardware limitations. Failure was expected, the network configuration was verified as correct and functional.
	- Win11 → Linux: Failed, due to hardware limitations
	- Linux → Win11: Failed, due to hardware limitations

5. Ping 8.8.8.8 from each VM
	- Win 11: Failed, as expected. I created an isolated Network, there is no gateway
	- Linux: Failed, as expected. I created an isolated Network, there is no gateway
6. Added 2nd network adapter attached via NAT
7. Verified both network adapters are active, one to communicate with the other VM in isolation, adapter 2 allows connection to internet and is not isolated.

## **Notes:**
- Host-Only networks seem to be a great idea for controlled labs
- NAT + Host-Only gives best of both worlds. I can have an isolated LAN + internet access at the same time.
 
## **Issues & Fixes:**
- No issues or fixes in this lab.

## **Outcome:**
- Built a fully functional internal LAN in VirtualBox
- Assigned static IPs and confirmed communication
- Validated segmentation + routing behaviors
- Created foundation for DHCP, DNS, firewall, packet capture labs
- Learned adapter behaviors (Host-Only vs NAT) in a real environment
