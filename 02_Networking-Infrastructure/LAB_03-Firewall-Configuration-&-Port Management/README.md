# Firewall Configuration & Port Management
**Date Created:** November 30, 2025  
**Last Updated:** November 30, 2025

## **Skills Demonstrated:**
- Managing Windows Firewall (or Linux UFW/firewalld)
- Creating inbound and outbound firewall rules
- Allowing and blocking ports and applications
- Understanding TCP/UDP distinctions
- Testing open/closed ports with tools like netstat, Test-NetConnection, and telnet
- Verifying rule behavior and troubleshooting blocked traffic
- Basic network security hardening concepts

## **Objective:**
- Learn how to view, create, edit, and remove firewall rules in both GUI and command-line.
- Understand how ports map to services and why port control is crucial for security.
- Practice allowing and blocking specific ports (e.g., 22, 80, 3389).
- Build a repeatable workflow for firewall configuration and troubleshooting.

## **Tools & Environment:**
- Windows 11 VM & Linux Mint VM
- Windows Defender Firewall with Advanced Security, Linux UFW
- PowerShell (Windows), Terminal (Linux)
- Apache2 (Linux)
- Tools for testing ports:
	- Test-NetConnection
	- netstat
	- curl (Linux/Windows)
- VirtualBox NAT network

## **Steps:**
1. Document Host, Windows 11 VM, and Linux VM IP's and network modes using ipconfig, ipconfig /all, ip a, and ip route:
	(a) Host:
	- IP: (Host IP Hidden for security)	
	(b) Win 11 VM:
	- IP: 10.0.2.15 (Due to hardware restrictions, only one VM is able to be open at once, so 	the IP's ended up being the same)
	- Gateway: 10.0.2.2
	- Network Mode: NAT
	(c) Linux VM:	
	- IP: 10.0.2.15 (Due to hardware restrictions, only one VM is able to be open at once, so 	the IP's ended up being the same)
	- Gateway: 10.0.2.2
	- Network Mode: NAT

2. Test connectivity from each VM to external hosts and localhost:
	(a) Win 11 VM:
	- Baseline test results for port 80: Failed (expected)
	- Baseline test results for port 22: Failed (expected)
	(b) Linux VM:
	- Baseline http result: Worked
	- Baseline Random TCP Port result: Worked

3. In Win 11 VM, use GUI and Powershell to create a rule to block outbound http traffic on port 80, allow inbound RDP traffic on port 3389
	(a) GUI pathway to create an outbound rule -> See image "Step-3a"
	(b) Powershell command to create an outbound rule -> See image "Step-3b"
	(c) Tested outbound HTTP traffic by curling, it should fail -> See image "Step-3c"
	(d) GUI pathway to create an inbound rule to allow 3389 -> See image "Step-3d"
	(e) Powershell command to create an inbound rule to allow 3389 -> See image "Step-3e"
	(f) Test RDP rule -> see Image "Step-3f"
	(g) Remove firewall rules to end Win 11 portion of lab.

4. In Linux VM, create a rule to allow ssh on port 22 and a rule to block http traffic on port 80.
	(a) Enable UFW on Linux
	(b) Allow ssh on port 22, sudo ufw status to verify rule was created
	(c) Block http on port 80, sudo ufw status to verify rule was created
	(d) Remove recently created rules to end Linux portion of lab	

## **Notes:**
- Linux VM can host a web server by installing apache2. This allows port 80 to open
- Inbound tests failed on Windows because no service was listening on ports 22 or 80.
- Inbound port 80/443 won't matter when a web server isn’t running.
- Outbound is what controls client browsing.
- Learned that UFW creates both IPv4 + IPv6 rules automatically.

 
## **Issues & Fixes:**
- Wanted to have connection to port 80, Linux isn't a webserver so downloading apache2 allowed to host a webserver and allow connection to port 80.
- Checked UFW default policy.
- Tested results using curl, Test-NetConnection, and ping.

## **Outcome:**
- Successfully created, tested, and validated inbound/outbound firewall rules on both Windows and Linux.
- Learned how port control impacts real connectivity.
- Confirmed rule behavior using curl, Test-NetConnection, ufw status.
- Learned the difference between inbound and outbound rules in practical use.
- Learned blocking outbound ports is the correct way to control client traffic.
- Learned you must run a service before testing inbound connectivity.
- This lab reinforced foundational firewall concepts and validated that I can confidently configure, test, and troubleshoot Windows and Linux firewall rules in a real environment.


