# Firewall Setup & Rule Testing
**Date Created:** December 22, 2025 
**Last Updated:** December 23, 2025

## **Skills Demonstrated:**
- Configuring firewall rules on Linux and Windows systems
- Understanding inbound vs. outbound traffic rules
- Testing open/closed ports using tools like netcat, or Test-NetConnection
- Applying persistent firewall rules using ufw or iptables on Linux
- Managing Windows Defender Firewall with Advanced Security
- Allowing/blocking specific applications, services, and ports
- Verifying firewall rules and monitoring traffic logs
- Understanding the role of firewall policies in network security
- Troubleshooting connectivity issues caused by firewall misconfigurations

## **Objective:**
- To practice configuring firewalls on both Linux and Windows systems
- To understand how different firewall rules affect network connectivity
- To simulate real world scenarios where security policies restrict or allow traffic
- To validate firewall configurations by testing connectivity and observing logs
- To build repeatable, documented procedures for firewall setup and testing

## **Tools & Environment:**
- Linux Mint host running Virtual Machine Manager
- Linux Mint VM
- Windows 11 Pro VM
- Tools and utilities:
    - Linux: ufw, iptables, netcat
    - Windows: Windows Defender Firewall with Advanced Security, Test-NetConnection
    - Event Viewer
    - Optional monitoring: Wireshark for traffic verification
- Networking: VMM bridged virtual networks for simulated LAN testing

## **Steps:**
1. Verify network connectivity between all virtual machines by checking IP addresses and confirming successful ICMP ping responses prior to applying any firewall rules.
    - Linux: ip a and then ping <win11-vm-ip> 10.0.0.45
    - Win 11: ipconfig then ping <linux-vm-ip> 10.0.0.21
2. Reset existing UFW firewall rules to ensure a clean default state. Configure UFW to deny all incoming traffic by default and allow all outgoing traffic. Then enable the UFW firewall and verify that the default policies are active
    - Check current firewall status with sudo ufw status verbose
    - Reset the rules to default using sudo ufw reset
    - Set firewall rules using:
        - sudo ufw default deny incoming
        - sudo ufw default allow outgoing
    - Enable the firewall using sudo ufw enable
3. Test inbound connectivity to Linux firewall system from the Win11 VM using PowerShell Test-NetConnection, confirming that default firewall rules blocked access to common service ports.
    - From Win 11 VM, run Test-NetConnection -ComputerName <linux-vm-ip> -Port 22
    - From Win 11 VM, run Test-NetConnection -ComputerName <linux-vm-ip> -Port 80
    - From Win 11 VM, run Test-NetConnection -ComputerName <linux-vm-ip> -Port 443
4. Create a firewall rule to allow inbound SSH traffic to the Linux system. Then Verify that the only approved SSH port was accessible while all other ports remained blocked.
    - On Linux VM, run the following:
        - sudo ufw allow ssh
        - sudo ufw status numbered
    - On Win 11 VM, run the following to test connectivity as an "attacker". This port should be open now.
        - Test-NetConnection -ComputerName <linux-vm-ip> -Port 22
5. Allow a temporary custom TCP port to test application level traffic using netcat. Then confirm successful connectivity prior to blocking the port. Then deny the custom TCP port and verify that the firewall correctly blocks connection attempts from others.
    - On Linux, run sudo ufw allow 9001/tcp
    - On Linux, run nc -lvnp 9001
    - On Win 11, run Test-NetConnection -ComputerName <linux-vm-ip> -Port 9001 and connection should succeed
    - Block the port on Linux VM by running sudo ufw deny 9001/tcp.
    - Retry connection from Win 11 VM, it should fail now.
6. Create a custom inbound firewall rule in Windows Defender firewall with Advanced Security to block a specific TCP port. Verify the blocked connectivity from an external system (in this case, the Linux VM) using network scanning.
    - Open Windows Defender Firewall and Advanced Security
    - Create a new inbound rule
        - Port -> TCP
        - specific port 3389
        - Block the connection
        - Apply to all profiles
    - Test from Linux VM using nc -zv -w 3 <win11-vm-ip> 3389 
7. Test outbound network connectivity using from Windows using Test-NetConnection. Then apply an outbound firewall restriction and confirm traffic is successfully blocked.
    - Test default configuration by running Test-NetConnection -ComputerName <linux-vm-ip> -Port 2222. This should succeed by default.
    - Open Windows Defender Firewall and Advanced Security
    - Create a new outbound rule
        - Port -> TCP
        - specific port 2222
        - Block the connection
        - Apply to all profiles
    - Retest connectivity using Test-NetConnection -ComputerName <linux-vm-ip> -Port 2222. This should fail now.
8. Review firewall logs on both Linux and Windows systems to confirm administrative events were being recorded. Logging of traffic needs to be enabled for packet level events, which we did not do for this lab.
    - In Win 11 VM, open Event Viewer
        - Navigate to Applications and Services Logs -> Microsoft -> Windows -> Windows Defender firewall with Advanced Security
        - Click "Firewall" in the main panel.
        - Observe blocked connections

## **Notes:**
- This lab brought light to the importance of verifying actual service configurations before assuming default ports. Firewalls enforce security policies, and connectivity failures can come from misaligned service configurations. A Firewall reset to default does not change service configurations. Lesson learned!
 
## **Issues & Fixes:**
- Connecting to Linux machine from Win 11 VM didn't work over port 22. Dug around a bit and identified from a previous lab we moved the ssh port to 2222.
    - Fix: Create a new rule allowing ssh over port 2222, then run Test-NetConnection -ComputerName <linux-vm-ip> -Port 2222 from the Win 11 machine. Connection should work now.

## **Outcome:**
- Successfully configured and tested firewall rules on both Linux and Windows systems. Confirmed default deny security posture, validated selective service access, and verified firewall behaviour through active scanning, connection testing, and log analysis.
