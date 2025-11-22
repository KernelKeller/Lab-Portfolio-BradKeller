# Local & Remote System Configuration
**Date Created:** November 17, 2025  
**Last Updated:** November 21, 2025

## **Skills Demonstrated:**
- Configuring local system settings (hostname, time zone, network, users, permissions)
- Editing system configuration files using both GUI and CLI
- Setting up and testing remote connections via SSH and RDP
- Managing system services and startup behavior
- Applying security and access control best practices for remote management
- Troubleshooting failed remote connections and authentication issues

## **Objective:**
- To practice configuring a local machine and enabling secure remote management capabilities.
- To understand how system configuration files and tools differ between Windows and Linux environments.
- To verify successful remote connectivity and secure access between two VMs.

## **Tools & Environment:**
- Operating Systems: Windows 11, Linux Mint
- Virtual Machines: Configured in VirtualBox
- Commands/Utilities Used:
	- Windows: SystemPropertiesRemote, net user, services.msc, PowerShell (Get-Service, Set-NetIPAddress)
	- Linux: hostnamectl, timedatectl, ifconfig/ip, systemctl, sshd, scp, ufw
- Networking: Host-only or NAT with port forwarding for SSH/RDP connections
- Optional Tools: PuTTY (Windows), Remmina (Linux), Wireshark (for packet capture verification)

## **Steps:**
1. Windows: Renamed system to LAB-WIN11, set PST timezone, verified network config, rebooted. Linux: Renamed to lab-linux, set timezone to Asia/Manila, checked IP addressing, rebooted.
2. Added admin and standard users on both Windows and Linux, then verified account creation via "net user` and `cat /etc/passwd".
3. Enabled RDP and firewall rules on Windows. Installed and enabled SSH on Linux, configured firewall.
4. Configured NAT port forwarding for RDP and SSH.Host connects using 33891 (Windows) and 2222 (Linux).
5. Successfully established RDP session to Windows VM.
6. Successfully connected via SSH into Linux VM.
7. Viewed and modified service startup on both OSes. Disabled Fax on Windows; disabled Bluetooth on Linux.
8. Exported Windows security policy for review. Disabled SSH root login in Linux.


## **Notes:**
- May seem quite obvious, but I was unable to change computer name in Win11 unless I ran PowerShell as admin
- Hostname updates require reboot on Windows but they are immediate on Linux.
- Windows RDP listens on TCP 3389. SSH typically listens on TCP 22.
- Both use system services that can be verified with "systemctl" or "Get-Service".
- NAT + port-forwarding allows a single VM to simulate a 2-machine environment.
- Host acts as the remote client.
- First SSH connection generates and stores a host key.
- RDP required firewall and registry change before working.
 
## **Issues & Fixes:**
- Needed to manually enable ufw on Linux.

## **Outcome:**
- Successfully configured system identity, users, networking, and services on both Windows and Linux.
- Enabled and tested remote access using NAT port forwarding.
- Implemented basic hardening steps.
- Verified that all objectives of the lab were achieved with only one VM running at a time thanks to host-acting-as-remote setup.

