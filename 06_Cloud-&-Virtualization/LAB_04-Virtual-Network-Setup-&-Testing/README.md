# Virtual Network Setup & Testing
**Date Created:** February 1, 2026  
**Last Updated:** February 1, 2026

## **Skills Demonstrated:**
- Designing and configuring virtual networks within Virtual Machine Manager
- Assigning IP addresses, subnets, and gateways to VM's
- Testing connectivity between VM's and host
- Configuring network isolation and routing rules
- Using network troubleshooting tools to verify connectivity
- Understanding virtual networking concepts (bridged, host-only)

## **Objective:**
- Learn how to create and configure virtual networks for multiple VM's
- Ensure VMs can communicate according to intended network topology
- Test connectivity and validate routing/firewall configurations
- Practice troubleshooting network issues in a virtualized environment

## **Tools & Environment:**
- Linux Mint Host
- Windows 11 Pro VM
- Linux Mint VM
- Virtual Machine Manager
- Networking utilities:
    - ping, tracepath, ip
    - Windows equivalents: ping, tracert, ipconfig
- Virtual network types:
- Bridged, host only

## **Steps:**
1. Verify both Linux and Win 11 VM's are connected to the same bridged virtual network and collect private IP addresses from the same subnet via DHCP
    - Boot up both Linux and Win 11 VM's
    - Linux VM, open a terminal and run:
        - 'ip a'
        - 'ip route'    
        - IP = 10.0.0.45
    - Win 11 VM, open PowerShell and run:
        - 'ipconfig'
        - IP = 10.0.0.21
2. Test bidirectional network connectivity between the Linux and Win 11 VM's using ICMP ping. Confirm successful communication within the bridged virtual network.
    - Linux VM, open a terminal and run:
        - 'ping 10.0.0.21'
    - Win 11 VM, open PowerShell and run:
        - 'ping 10.0.0.45'
3. Verify connectivity between both virtual machines and the Linux host via the bridged gateway, confirming proper host to VM and VM to host routing.
    - Linux VM, in terminal, run:
        - 'ping 10.0.0.239'
    - Win 11 VM, in PowerShell, run:
        - 'ping 10.0.0.239'
    - From host, open a terminal and run:
        - 'ping 10.0.0.21 (Win 11 VM)'
        - 'ping 10.0.0.45 (Linux VM)'
4. Create a host only virtual network in Virtual Machine Manager to simulate an isolated environment with no external or bridged based network access.
    - Go to Virtual Machine Manager and enter the Linux VM hardware options. 
    - Disable the bridged network adapter and enable an isolated network adapter. DHCP can remain enabled.
    - Only change the Linux VM adapter, leave Win 11 adapter as bridged.
5. Test connectivity on the host only network and confirm the VM is now isolated from external networks and unable to communicate with VM's on the bridged network.
    - On Linux VM, open terminal and run:
        - 'ip a'
        - 'ping 8.8.8.8'
        - 'ping 10.0.0.21'
        - Both pings should fail, no internet and cannot reach the Win 11 VM due to it still being on bridged network.
6. Use Linux VM as a primary test system to compare routing tables and traceroute behaviour across bridged and isolated network configurations. Maintain the Win 11 VM on the bridged network as a control system to validate normal external connectivity during testing.
    - On Linux VM, open terminal and run:
        - 'ip route'
        - 'tracepath 8.8.8.8'
        - Compare to what happens on Win 11 VM
    - On Win 11 VM, open PowerShell and run:
        - 'route print'
        - 'tracert 8.8.8.8'
        - Compare to what happens on Linux VM.
7. Restore the Linux VM network back to the bridged virtual network to return the lab environment to a standard working state.
    - Power off the Linux VM
    - Go to Linux VM hardware option in Virtual Machine Manager
    - Deselect the isolated network adapter
    - Re-enable the Bridged adapter.

## **Notes:**
- Bridged networking exposed VM's directly to the LAN, assigning IPs from the same subnet as the host.
- The isolated (host only) network removed the bridged route, preventing external connectivity while maintaining VM-to-host access.


## **Outcome:**
- Successfully configured and tested two different virtual network types, validated VM-to-VM and VM-to-host connectivity, and demonstrated network isolation using a host only virtual network. This lab reinforced practical understanding of bridged, host only, routing, and virtual network segmentation.
