# DHCP & DNS Configuration
**Date Created:** November 24, 2025  
**Last Updated:** November 24, 2025

## **Skills Demonstrated:**
- Configuring DHCP settings (scope, exclusions, leases)
- Understanding how IP addressing is assigned dynamically
- Setting static IPs vs. DHCP assignments
- Configuring local DNS settings in Windows or Linux
- Testing DNS resolution with command-line tools
- Troubleshooting hostname/IP issues
- Observing DHCP lease behavior inside a VM environment

## **Objective:**
- Understand how DHCP automatically assigns IP addresses to devices on a network.
- Learn how DNS translates hostnames into IP addresses and why it’s essential.
- Configure and test DHCP behavior within a VirtualBox NAT or Host-Only network.
- Practice setting static DNS servers (Google DNS, local DNS, etc.).
- Verify DHCP leases and DNS queries using command-line tools like ipconfig, nslookup, and ping.

## **Tools & Environment:**
- VirtualBox (NAT or Host-Only network)
- Windows 11 VM (or Linux Mint if preferred for DNS tests)
- Command Prompt / PowerShell / Terminal
- Built-in VirtualBox DHCP server (for host-only)
- External DNS servers (Google 8.8.8.8, Cloudflare 1.1.1.1)

## **Steps:**
1. Collect Initial DHCP-provided IP addresses
	Windows 11 VM:
	- Run ipconfig /all
	- Note IP, subnet mask, gateway, and DNS server
	- IP and DNS were automatically assigned via DHCP

	Linux VM:
	- Run ifconfig or ip a and cat /etc/resolv.conf
	- Run sudo dhclient -r && sudo dhclient to observe DHCP lease changes
	- Note IP, subnet mask, gateway, and DNS server. Verify DHCP and DNS are working correctly 	by using ping and ipconfig/ifconfig. Noting any changes to IP and DNS servers.
2. Verify DHCP and DNS functionality
	Windows 11 VM:
	- Run ping 8.8.8.8 → confirms internet connectivity
	- Run ping google.com to confirm DNS resolution works

	Linux VM:
	- Run ping 8.8.8.8 to confirm internet connectivity
	- Run ping google.com to confirm DNS resolution works
	- DHCP lease renewals (sudo dhclient -r and sudo dhclient) successfully changed IP, DNS 	remained same	
3. Attempt to configure a static IP
	Windows 11 VM
	- Attempted to manually set IP, subnet mask, default gateway, and DNS server
	- Connectivity failed because VirtualBox NAT adapter requires DHCP or careful route 	configuration
	
	Linux VM
	- Attempted to set static IP using netplan or ifconfig commands
	- IP changed as expected, but connectivity failed with NAT adapter
	- If using Host-Only adapter, i think static IP would have worked

## **Notes:**
- After doing this lab, I get why there's a lot of DNS and DHCP jokes on the web.
- Static IP configuration failed on NAT adapter because it seems VirtualBox NAT enforces DHCP for routing and DNS. Host-Only adapters can allow static IPs for internal network communication.
 
## **Issues & Fixes:**
- Setting static IP to the adapter in Win 11 Required me to disable and re-enable the adapter in order for the changes to push through and allow connectivity.

## **Outcome:**
- Observed DHCP automatic IP assignment and lease behavior
- Attempted configuration of static IP. Connectivity did not work due to using a NAT adapter instead of Host Only adapter
- Tested DNS resolution and confirmed manual DNS override works
- Learned how to adjust IP's and DNS settings
- Learned about NAT and Host-Only adapters in VM settings. How they may differ and could potentially allow for static IP's in one but not the other.

