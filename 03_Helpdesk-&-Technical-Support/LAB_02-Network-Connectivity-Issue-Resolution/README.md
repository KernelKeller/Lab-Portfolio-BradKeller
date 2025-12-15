# Network Connectivity Issue Resolution
**Date Created:** December 11, 2025  
**Last Updated:** December 12, 2025

## **Skills Demonstrated:**
- Diagnosing Layer 1–3 connectivity issues
- Using network troubleshooting tools (ping, ipconfig, tracert, arp, nslookup)
- Checking Windows network adapters and configurations
- Identifying DNS issues vs. gateway issues
- Reviewing routing tables
- Resetting network stacks (Windows + Linux)
- Understanding DHCP assignment, static IPs, and subnetting
- Validating connectivity inside a virtualized environment

## **Objective:**
- Simulate real-world helpdesk scenarios where users cannot access the network or Internet
- Identify root causes (DHCP failure, DNS misconfiguration, gateway issues, NIC problems)
- Practice using both GUI and CLI tools to diagnose and resolve connectivity issues
= Build repeatable troubleshooting workflows matching Tier 1–2 helpdesk expectations

## **Tools & Environment:**
- Linux Host running Virtual Machine Manager
- Windows 11 Client VM
- Windows Server (Domain Controller / DHCP / DNS) VM
- Tools: ping, ipconfig, arp, nslookup, tracert, PowerShell networking commands, Windows Network Adapter settings, Server Manager (DHCP & DNS roles)
- Virtual networking via VMM NAT or isolated virtual network

## **Steps:**
**Scenario 1: DHCP FAILURE**
1. Simulate a DHCP outage by stopping the DHCP Server service on the Windows Server VM.
    - Server was originally set up as a DNS server without DHCP. The DHCP server in services is missing. Will need to migrate DHCP responsibility from virtual network to Windows Server. Let's change the lab to this instead of a failed DHCP.  
2. Install DHCP service to the local DNS server.
    - Open Server Manager and launch the Add Roles and Features wizard to install the DHCP Server role on the existing DNS server.
    - Select role-based installation and confirm the local server as the installation target.
    - Install the DHCP Server role, including required management tools.
    - Click next through features, DHCP overview, etc. Select install and let it install.
3. Complete post-install DHCP configuration, authorizing the DHCP server for use on the network.
    - In to the top right of the Server Manager Window, select the Flag with a warning symbol and select "Complete DHCP Configuration"
    - Select "Use the following user's credentials" and commit.
4. Open the DHCP management console and verify the DHCP Server role is active.
    - In server manager, go to Tools -> DHCP
5. Create and activate a new DHCP IPv4 scope to provide IP address assignments to client systems. Configure scope options including default gateway and DNS server.
    - Right click the IPv4 and select "new scope"
    - Name the scope "LAB_Scope"
    - Enter IP range:
        - Start: 192.168.122.100
        - End: 192.168.122.200
        - No exclusions
        - Lease duration can be left at default
        - Default gateway should be the gateway the client is connected to through the NAT adapter.
        - Domain name and DNS Servers should be the Win Server host name and IP address.
        - Next through WINS Servers and activate the scope
6. Verify on client that DHCP and DNS servers are working and assigning a new IP info.
    - Get original IP using ipconfig /all: 192.168.122.153
    - Run ipconfig /release; ipconfig /renew
    - Verified DHCP is working. In a fully controlled scenario where the server is the only DHCP provider, releasing and renewing the client IP would assign an APIPA address if the DHCP server were unavailable.

**Scenario 2: DNS Failure** 
1. Verify Normal DNS functionality on the client using nslookup and ping. Confirm DNS server IP configured via DHCP was correctly set to the Windows Server VM. 
    - ping 8.8.8.8 = fails
    - nslookup google.com = fails
    - ping and nslookup to server IP succeeds
    - ping and nslookup to server host name fails
2. Check Network IPv4 adapter settings in Server VM.
    - IP = Assigned
    - subnet Mask = Assigned
    - Default Gateway = Blank
    - DNS server: Preferred set to local, alternate is blank.
3. Check Network IPv4 adapter settings in Client VM
    - IP = Assigned
    - subnet Mask = Assigned
    - Default Gateway = Assigned
    - DNS server: Preferred set to local, alternate set to 8.8.8.8
3. Missing Default Gateway likely the issue for internet connection, alternate DNS missing would leave the DNS server to only solve local DNS issues, also a problem. Set default gateway in Server VM to match the Client VM default gateway and alternate DNS server to 8.8.8.8.
4. Confirm connectivity and DNS resolution is fixed
    - ping 8.8.8.8 = success
    - nslookup google.com = success
    - ping and nslookup to server IP = success
    - ping and nslookup to server host name = success

## **Notes:**
- Identified that the Windows Server VM was configured only with the DNS role, and DHCP services were being provided by the virtual network rather than the server.
 
## **Issues & Fixes:**
- Initial environment used virtual network–provided DHCP.
    - FIX: DHCP Server role was installed on Windows Server.
- Client could not access Internet; external DNS failed
    - Fix: Added default gateway to server, configured DNS forwarders, restarted DNS service, flushed DNS caches
 
## **Outcome:**

**Scenario 1:**
- Installed and verified DHCP Server role on Windows Server.
- Created an IPv4 scope with proper gateway and DNS assignments.
- Verified DHCP lease assignment on client and documented APIPA behaviour conceptually.
- Demonstrated understanding of DHCP troubleshooting workflow and lab documentation standards.

**Scenario 2:**
- Successfully identified that the server’s missing default gateway was preventing Internet access.
- Configured server default gateway and DNS forwarders to restore external name resolution while preserving internal lab DNS.
- Verified that both server and client VMs can now resolve internal lab hostnames and external Internet domains.
- Confirmed full network functionality within the lab: internal communication, DHCP-assigned IPs, and Internet access.
- Demonstrated systematic troubleshooting of network connectivity and DNS issues in a virtualized lab environment.
