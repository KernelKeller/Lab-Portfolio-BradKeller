# Remote Support Simulation
**Date Created:** December 16, 2025 
**Last Updated:** December 17, 2025

## **Skills Demonstrated:**
- Providing remote technical support using screen-sharing tools
- Communicating clear instructions to a non-technical user
- Navigating a system remotely using RDP/VNC tools
- Verifying user identity and following security protocols
- Diagnosing issues without physical access to the machine
- Using PowerShell or CMD remotely for troubleshooting
- Understanding remote session performance limitations
- Using logs to validate actions taken during a session
- Practicing professional helpdesk communication workflows

## **Objective:**
- To simulate a real-world helpdesk remote-support session
- To practice troubleshooting on a system you cannot physically access
- To document clear, repeatable remote support procedures
- To improve your ability to diagnose issues through user descriptions
- To build confidence using remote-access tools commonly used in IT support roles

## **Tools & Environment:**
- Linux Mint host running Virtual Machine Manager (KVM/QEMU)
- Windows 11 Pro VM (acting as the “user’s machine”)
- Linux Mint VM (optional: can act as remote-support agent system)
- Remote access tools and methods:
    - RDP (Remote Desktop Protocol)
    - VNC (TigerVNC / Remmina)
    - Windows Quick Assist (if testing Windows → Windows remote support)
    - PowerShell Remoting (Enter-PSSession, Test-WSMan)
    - Event Viewer for remote session logs
- Networking: VMM NAT/bridge virtual network
- Optional simulation tools:
    - Fake “user-reported issue” scenarios
    - Screen recording to review workflow

## **Steps:**
1. Confirm connectivity between both VM's by using ipconfig, ifconfig, and pinging their respective IPv4 addresses. 
2. Enable Remote Desktop on the Win 11 "user machine".  Confirm the system's IP address to allow remote connection from the "support technician" VM. 
    - Go to Settings -> System -> Remote Desktop -> ON and select confirm. See notes if this fails.
    - Take note of Win 11 VM IP address: 192.168.122.153
3. Read simulated client ticket to begin process.
    - Simulated ticket Message: "My computer is very slow and Outlook sometimes freezes. I'm working from home"
    - Assumptions: User is non-technical, I do not have physical access, I must verify identity and request permission.
4. Reach out to user via chat or call to verify the user's identity. Ask them to restart their machine and to verify the issue is still affecting them. Confirm their username, device hostname and obtain a verbal or written consent to initiate a remote support session before connecting to the system.
    - Full Name: Cool Guy
    - Username: gcool@bestjobever.com
    - Workstation Name (a.k.a. hostname): DESKTOP-QLDFVO2.lab.local
    - Consent: Received
5. Initiate a remote desktop session from the Linux Mint support VM using RDP (via Remmina) and successfully connect to the user's Win 11 system.
    - Open Remmina create a new connection profile by hitting the + symbol in the top right corner and input the following information.
        - Name: Win 11 VM
        - Server: User IP (hidden in doc's and images for security purposes now that we've switched to Bridged adapter)
        - Username: "The user name of the account profile"
        - Password: "The Users account profile password"
        - Select save and connect.
6. Inform user that I am now connected to their system. Explain that they may see onscreen activity during troubleshooting and advise that slight performance lag is normal during remote sessions.
7. Perform an initial system assessment using task manager to review CPU, memory, and disk utilization in order to identify potential performance bottlenecks.
    - Open Task manager
    - Check CPU, Memory, and Disk usage.
    - Go to start up apps and adjust any that aren't necessary or resource hogs.
8. Use PowerShell within the remote session to gather system information and identify high-resource processes contributing to the reported performance issues.
    - Open PowerShell and run "systeminfo"
    - follow by running "Get-Process | Sort-Object CPU -Descending | Select-Object -First 10"
9. Identified unnecessary startup applications impacting system performance and safely disabled them with the user's awareness to improve system responsiveness.
10. Verify with the user that system performance had improved and confirmed that Outlook was functioning normally following the applied changes
    - Ask client "Does the system feel more responsive?" or "Is Outlook opening faster now?"
    - If client still has issues, clear outlook cache and restart outlook. Then re-verify.
11. Review relevant system logs in Event Viewer to ensure no critical errors were present during or after the troubleshooting session. This step would be best to first. 
12. Notify the user that troubleshooting is complete, properly end the remote session and ensure the system was left in a stable and secure state.

## **Notes:**
- RDP requires user accounts to have a password for security purposes. If the user account doesn't require a password to log in, you must set one in order to activate RDP.
  
 
## **Issues & Fixes:**
- RDP from Linux to Win 11 VM's would not connect even though pings were successful. This implies it's not a networking issue, probably a setting issue on the Win 11 VM. After further troubleshooting, the root cause was the network. Switching from NAT to Bridged network adapters allowed for RDP to connect.
    - FIX: Set up a bridged network adapter on Linux host and connected both VM's through the Bridged NIC.
    - FIX: Troubleshooting RDP connectivity required switching the VMs from NAT to bridged networking and changing the Windows 11 NIC to e1000 due to missing virtio drivers.

## **Outcome:**
- Successfully verified user identity and obtained consent for remote support.
- Established a stable RDP session from the Linux Mint support VM to the Windows 11 VM.
- Performed system assessment and identified high-resource processes and unnecessary startup applications.
- Safely disabled unneeded startup apps, improving system responsiveness.
- Confirmed with the user that system performance and Outlook functionality were restored.
- Reviewed Event Viewer logs and verified no critical errors were present during or after troubleshooting.
- Completed remote support session following professional helpdesk protocols, leaving the system in a stable and secure state.
- Biggest takeaway: troubleshooting RDP connection issues and discovering/learning how to create a bridged adapter using my PC's wired connection was very interesting.
