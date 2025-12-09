# Troubleshooting Connectivity Issues
**Date Created:** December 8, 2025 
**Last Updated:** December 9, 2025

## **Skills Demonstrated:**
- Systematic network troubleshooting using layered methodology
- Using tools like ping, tracert, traceroute, ipconfig, ifconfig, netstat, and nslookup
- Diagnosing DNS misconfigurations
- Identifying IP addressing and subnet issues
- Verifying routing and gateway configurations
- Testing open/closed ports and firewall blocks
- Interpreting packet loss, latency, and unreachable messages
- Applying real-world troubleshooting processes (isolation, testing, verification)

## **Objective:**
- Learn how to identify and resolve common local and network connectivity problems.
- Troubleshoot issues such as:
	- No internet access
	- Incorrect IP addressing
	- DNS failures
	- Gateway misconfigurations
	- Firewall blocking connectivity
- Practice building a structured workflow for diagnosing issues logically.
- Develop confidence using standard troubleshooting tools and commands.
- Simulate real helpdesk and entry-level IT support scenarios.

## **Tools & Environment:**
- Windows 11 VM and Linux Mint VM
- Virtual Machine Manager networking (NAT, Host-only, or misconfigured settings intentionally)
- Command-line tools:
	- ping
	- ipconfig / ifconfig
	- nslookup
	- tracert / traceroute
	- Test-NetConnection
	- netstat
    - dig

## **Steps:**
1. Perform a baseline test on both Win11 and Linux VM's to ensure the adapters are set to NAT and are functioning.
    - Verified both VM's are running NAT network
    - Ran ipconfig on Win11 VM and ifconfig on Linux VM to collect IP information
    - Tested connectivity from Win11 VM to Linux VM using ping <Linux_IP>
    - Tested connectivity from Linux VM to Win 11 VM using ping <win11_IP>
    - Tested external connectivity on both machines using ping 8.8.8.8
    - Verified DNS resolution using nslookup google.com on Win11 and nslookup google.com / dig google.com on Linux

2. Issue #1: Incorrect IP addressing. Break, Troubleshoot, and repair in Win11 VM
    - Break Win11 VM's IP addressing by changing the adapter IP information by navigating to the adapter, right clicking and selecting properties, then select the IPv4 connection and going into that connections properties. Select to use a manually designated IP.
    - Expect all tests to fail and for ipconfig to report the new incorrect IP information, manually designated from the previous step. Test connectivity in powershell using:
        - ipconfig = displays the IP addressing that was manually entered into the adapter
        - Ping 8.8.8.8 = failed
        - tracert 8.8.8.8 = failed
        - nslookup google.com = failed
    - Repair connectivity by navigating back to the network adapter, going into properties and setting the IPv4 connection to obtain IP and DNS automatically. Then go to powershell and run ipconfig /renew. 
    - Test by using ping 8.8.8.8 and nslookup google.ca in powershell.

3. Issue #2: Default Gateway Misconfiguration. Break, Troubleshoot, and repair in Win11 VM. This simulates users who set static IP's incorrectly on corp networks.
    - Break Win11 VM's default gateway by changing the adapter default gateway to something incorrect but providing the correct IP, subnet mask, and DNS.
    - Test connectivity in powershell using:
        - ipconfig = displays the IP addressing that was manually entered into the adapter
        - ping 8.8.8.8 = failed
        - ping the incorrect default gateway = failed
        - ping Linux VM IP = success
        - nslookup google.com = failed
        - tracert 8.8.8.8 = failed
    - Repair connectivity by navigating back to the network adapter, and setting the IPv4 connection to obtain IP and DNS automatically. Then run the following in powershell to get a new IP and test connectivity:
        - ipconfig /renew: Obtains DHCP provided IP addressing
        - Ping 8.8.8.8 = success
        - Ping Linux VM = success
        - tracert 8.8.8.8 = success

4. Issue #3: DNS failure. Break, Troubleshoot, and repair in Win11 VM. This simulates Typo's in DNS, Downed DNS servers, Wrong static config, Misconfigured DHCP servers.
    - Break Win 11 VM's DNS by changing the DNS in the adapters IPv4 properties to a dead or non existent DNS server. Leave IP addressing to automatic
    - Test connectivity in powershell using:
        - ipconfig /all = Shows the new and incorrect DNS addresses have been appliedping 
        - ping 8.8.8.8 = Tests raw connectivity, because default gateway is correct, pinging 8.8.8.8 succeeds
        - ping google.com = fails due to incorrect DNS address
        - nslookup google.com = fails due to incorrect DNS address
        - open web browser and attempt to load any webpage = Fails due to incorrect DNS address
    - Repair connectivity by navigating back to the network adapter, and setting the IPv4 connection to obtain IP and DNS automatically. Then run the following in powershell to get a new DNS and IP address and test connectivity:
        - ipconfig /flushdns = flushed
        - ipconfig /renew = renewed
        - ping google.com = success
        - nslookup google.com = success
        - web browser test = success

5. Issue #4: Firewall Blocking Connectivity. Breaking connectivity by blocking outbound traffic to simulate when corporate policies misapply, updates break rules, security software breaks everything, users click "block" accidentally on a prompt. This should produce symptoms like unable to browse, ping fails, apps can't reach internet, dns might still work, local pings could work.
    - In the Win11 search bar, search and open Windows Defender Firewall with Advanced Security. Select outbound rules and add a new rule. Choose ports 80, 443 and select tcp. Block the connection and apply to all profiles. Name the rule "Block Web Traffic"
    - Create a second "custom" rule to block ICMPv4 and name it "Block ICMP"
    - Test connectivity in powershell using:
        - ping 8.8.8.8 = failed
        - ping Linux VM = failed
        - nslookup google.com = success
        - tracert google.com = failed
        - web browser test = failed
    - Repair connectivity by navigating to Windows Defender Firewall with Advanced Security and right clicking the two rules to select disable or delete. Then test connectivity in powershell using:
        - ping 8.8.8.8 = success
        - ping Linux VM = success
        - tracert google.com = success
        - web browser test = success

6. Issue #5: Bad Static Route (Routing Failure). Break routing by sending traffic to the Linux VM.
    - In powershell, run "route add 8.8.8.0 mask 255.255.255.0 <Linux IP>"
    - Test connectivity in powershell using:
        - ping 8.8.8.8 = failed
        - tracert 8.8.8.8 = failed
        - ping google.com = success
        - route print = Can see the bad route under IPv4 Route Table
        - ping 1.1.1.1 = success
    - Repair connectivity by running "route delete 8.8.8.0" in Powershell. Test connectivity in powershell using ping, tracert, etc.

## **Notes:**
- Linux VM labels its gateway as broadcast, Win 11 labels it as default gateway.
- When pinging from Linux to Win11, I want to ping the default gateway IP in order to test connectivity
 
## **Issues & Fixes:**
- Issue #1 (Incorrect IP Addressing):
    - Win11 VM was assigned invalid static IP information, breaking all connectivity.
    - Fix: Restored adapter settings to automatic (DHCP), then ran ipconfig /renew to obtain valid IP and DNS settings.

- Issue #2 (Default Gateway Misconfiguration):
    - Incorrect gateway caused loss of external connectivity while still allowing local VM pings.
    - Fix: Reset IPv4 settings to automatic and ran ipconfig /renew, restoring default gateway and internet access.

- Issue #3 (DNS Failure):
    - Manually setting a dead DNS server caused symptoms where raw IP pings worked but domain names failed.
    - Fix: Reverted DNS settings to automatic, flushed DNS cache, renewed IP configuration, confirmed DNS resolution worked.

- Issue #4 (Firewall Blocking Connectivity):
    - Custom outbound rules blocked web traffic and ICMP, causing failed pings and unreachable websites even though DNS was still functional.
    - Fix: Disabled/deleted the custom firewall rules and retested connectivity to confirm normal ICMP and web traffic restored.

- Issue #5 (Bad Static Route):
    - A static route incorrectly directed traffic for the 8.8.8.0/24 network to the Linux VM, causing routing failures for Google DNS and related hosts.
    - Fix: Removed the static route using route delete, verified the routing table, and confirmed external reachability.

## **Outcome:**
- Successfully simulated and resolved multiple real-world connectivity failures including incorrect IP addressing, gateway issues, DNS failures, firewall blocks, and routing misconfigurations.
- Practiced systematic troubleshooting using command-line tools such as ping, tracert, nslookup, route, ipconfig, and ifconfig.
- Verified ability to isolate problems logically, test hypotheses, identify root causes, and restore functionality.
- Built a complete troubleshooting workflow that mirrors real helpdesk scenarios and demonstrates competence in diagnosing network issues at multiple layers.
- Confirmed full connectivity restored on both Win11 and Linux VMs after each scenario, with proper documentation and screenshots captured.
