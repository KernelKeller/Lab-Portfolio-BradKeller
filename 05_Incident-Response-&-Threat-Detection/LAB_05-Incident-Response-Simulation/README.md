# Incident Response Simulation
**Date Created:** January 15, 2026  
**Last Updated:** January 15, 2026

## **Skills Demonstrated:**
- Incident response life cycle understanding (Prepare, Detect, Contain, Eradicate, Recover, Lessons Learned)
- Security event triage and prioritization
- Log-based incident investigation
- Evidence collection and preservation (basic)
- Root cause analysis
- Cross-platform incident handling (Windows & Linux)
- Documentation and incident reporting

## **Objective:**
- Simulate a real-world security incident in a controlled lab environment
- Practice responding to suspicious or malicious activity end-to-end
- Identify indicators of compromise (IOCs) using logs and system data
- Contain and remediate the incident
- Document findings and response actions as an incident report

## **Tools & Environment:**
- Linux Mint Host (analysis / coordination system)
- Windows 11 Pro VM (primary incident target)
- Linux Mint VM (secondary system / attacker simulation or log source)

Log & Investigation Tools:
- Windows Event Viewer
- PowerShell (process, user, and event inspection)
- Linux logs:
    - /var/log/auth.log
    - /var/log/syslog
- journalctl

System & Process Analysis:
- Task Manager / Resource Monitor
- ps, top, htop
- netstat, ss
- who, last, lastlog

Incident Simulation:
- Failed and successful authentication attempts
- Suspicious PowerShell or Bash commands
- Privilege escalation attempts
- Unauthorized user or group changes

Network Setup:
- Isolated Virtual Machine Manager network

## **Steps:**
1. Prepare the incident response environment by validating log visibility and administrative access on both Win 11 and Linux VM's. Confirm Event Viewer, PowerShell, and Linux system logs are accessible prior to incident simulation
    - On Win 11 VM:
        - Confirm Event Viewer Access
        - Open PowerShell as Administrator
        - Ensure logging is functioning
    - On Linux VM:
        - Confirm access to /var/log/auth.log
        - Confirm journalctl works
        - Open a terminal and keep it ready
2. Simulate a suspicious authentication behaviour by generating multiple failed SSH login attempts followed by a successful authentication to establish a detectable pattern in Linux authentication logs.
    - Create a test user: 'sudo adduser irtest' apply a simple password for now, "FunkyBanana"
    - Force failed logins by typing the incorrect password to the newly created test user: 'ssh -p 2222 irtest@localhost'
    - Verify the logs: 'sudo grep "Failed password" /var/log/auth.log | tail -n 5'
    - Successfully log into the newly created test user using the correct password: 'ssh -p 2222 irtest@localhost'
    - Exit the ssh session: 'exit'
    - Verify the logs: 'sudo grep "Accepted password" /var/log/auth.log | tail -n 5'
    - Can delete new test user: 'sudo deluser irtest'
3. Run a PowerShell command using an execution policy bypass flag to generate a realistic process execution event without making system changes. 
    - Open PowerShell as Admin and run: 
        - 'Get-LocalUser'
        - 'Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
    - Verify creation of logs in event viewer by navigating to Applications and Services Logs -> Microsoft -> Windows -> PowerShell -> Operational
    - Look for warnings or Event ID's 4104.
4. Act as an analyst identifying and responding to alerts. Identify suspicious activity by analyzing Windows Security Event Logs (Event ID's 4624, 4625, etc.) and Linux authentication logs, confirming abnormal authentication patterns and suspicious PowerShell execution aligned with the simulated incident timeline.
    - Win 11 VM
        - In PowerShell, run: 'Get-WinEvent -LogName Security -MaxEvents 20'
        - Look for successful logon's and failed logon's
    - Linux VM
        - In terminal, run: 'sudo cat /var/log/auth.log | tail -n 50
        - Or run: 'sudo journalctl -xe'
5. Perform initial triage to scope the incident, determine the primary affected system. 
    - Win 11 VM:
        - What systems are affected: Windows 11 VM
        - What type of incident: Suspected unauthorized access due to multiple failed logon's
        - Is it ongoing? No, it is lab contained.
    - Linux VM:
        - What systems are affected: Linux VM
        - What type of incident: Suspected unauthorized access due to multiple failed logon's
        - Is it ongoing? No, it is lab contained
6. Contain the incident by isolating the Windows virtual machine from the network and verify no unauthorized user accounts or malicious process were active on either system. 
    - Win 11 VM:
        - Through the Virtual Machine Manager, disconnect the Win 11 VM from the network, temporarily. 
        - Verify no new users were created: 'Get-LocalUser'
        - Check for any abnormal processes: 'Get-Process
    - Linux VM
        - Identify logged in users: 'who' and 'last'
7. Perform eradication checks by validating the absence of persistence mechanisms such as unauthorized user accounts, scheduled tasks, or startup scripts on both Win 11 and Linux VM's.
    - Win 11 VM:
        - In PowerShell: 'Get-ScheduledTask'
    - Linux VM:
        - In terminal: 'crontab -l'
8. Recover affected systems by restoring normal network connectivity and confirming stable system operation with no further indicators of compromise observed.
    - Enable the network adapter in VMM
9. Conduct a lessons learned review. Note that authentication and process creation logs are critical for detection and that enhanced PowerShell logging and centralized log aggregation would improve future incident visibility and response speed.
    - What detected this? 
        - Activity was detected through manual review of system logs during the investigation phase. On Windows, PowerShell Operational logs revealed suspicious execution flags, and on Linux, authentication logs showed failed and successful login attempts that aligned with the simulated incident timeline.
    - What could detect it earlier?
        - Earlier detection could be achieved with centralized log monitoring or alerting on suspicious authentication activity and PowerShell execution flags, rather than relying on manual log review after the fact. Automated alerts on repeated failed logins or execution policy bypass usage would be great, too.
    - What logging would help? 
        - Having all authentication and PowerShell logs in one place would make patterns easier to spot without manually checking each system.

## **Notes:**
- PowerShell activity was not visible in standard Windows Security logs on the standalone system, but was successfully identified using the PowerShell Operational log (Event ID 4104)
- Linux authentication logs clearly showed failed and successful login attempts, making them reliable indicators during the investigation.
- Verifying activity first before documenting reduced frustration and made the investigation more accurate.
 
## **Issues & Fixes:**
- PowerShell process creation events (Event ID 4688) were not generated on the Windows system despite auditing being enabled. This was resolved by using PowerShell Operational logs instead.
- SSH connection attempts initially failed due to the service running on a non default port, which was identified by checking active listening services.

## **Outcome:**
- Successfully simulated and investigated a incidents involving suspicious authentication activity and PowerShell execution across Windows and Linux systems.
- Identified relevant indicators of compromise using system logs and validated the incident timeline.
- Practised the full incident response life cycle, including detection, containment, eradication, and lessons learned.
- Improved lab workflow by validating behaviour before documenting, resulting in clearer findings and less friction during investigation.
