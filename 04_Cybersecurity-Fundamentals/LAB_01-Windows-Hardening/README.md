# Windows Hardening
**Date Created:** December 17, 2025 
**Last Updated:** December 18, 2025

## **Skills Demonstrated:**
- Applying baseline security configurations to Windows 11
- Disabling unnecessary services and tightening system settings
- Hardening local accounts, passwords, and authentication policies
- Configuring Windows Firewall rules and allowed applications
- Managing User Account Control (UAC)
- Using Group Policy Editor (gpedit.msc) for security settings
- Configuring audit policies for logging and monitoring
- Hardening the browser (Edge/Chrome) and controlling extensions
- Applying Windows Update and patch-management best practices
- Reviewing system logs for suspicious activity
- Reducing attack surface exposure using built-in Windows tools

## **Objective:**
- To learn and practice the essential steps required to harden a Windows 11 system against common attacks
- To develop repeatable, documented procedures for securing a fresh Windows installation
- To understand the security impact of each hardening step and how attackers typically exploit misconfigurations
- To create a defensible Windows baseline suitable for entry-level cybersecurity roles and home lab environments

## **Tools & Environment:**
- Linux Mint host running Virtual Machine Manager
- Windows 11 Pro VM (primary system being hardened)
- Tools and utilities used:
    - Local Security Policy (secpol.msc)
    - Group Policy Editor (gpedit.msc)
    - Windows Firewall
    - PowerShell security cmdlets (e.g., Get-Service, Set-ExecutionPolicy, Get-LocalUser)
    - Sysinternals Suite (Process Explorer, Autoruns, TCPView)
    - Windows Security / Defender AV
    - Event Viewer (Security, System, Application logs)
- Optional third-party checkers (not required):
    - CIS Benchmark checklist
    - Hardening scripts (review only, not running)

## **Steps:**

**Phase 1: Baseline & Initial Exposure Reduction**
1. Take a pre-hardened snapshot of the Win 11 VM before beginning any hardening process. Provides a safe rollback point in case of issues.
2. Verify Windows 11 Pro version and current OS build, as well as pending updates, to establish the system’s baseline prior to hardening.
    - Press Win + R -> type "winver" and hit enter.
        - Windows Edition: Windows 11 Enterprise
        - Windows Version: 24H2
        - OS build: 261.7171
    - Open Settings -> Windows Update (update if needed)
3. Confirm that automatic sign in is disabled and that local account authentication requires a password. This is to reduce physical and credential based attack risk.
    - Go to Settings -> Accounts -> Sign in options
        - Password required: Yes
        - Pin logon: Disabled
        - Picture Password: Disabled
        - Auto sign-in: Disabled
4. Review all local user accounts using PowerShell to identify unnecessary or risky accounts that could expand an attack surface.
    - In PowerShell, run "Get-Localuser" 
        - Number of users: 5
        - Enabled users: 1
        - Disabled users: 4
        - Any unnecessary accounts: None (disabled users are built in and default users needed for Win 11)
5. Review and disable unnecessary startup programs to reduce persistence opportunities and minimize system attack surface.
    - Open Task Manager -> Startup Apps
    - Disable anything non essential
    - Leave security related items enabled.
6. Verify Microsoft Defender Antivirus is enabled with real time and tamper protection active. Perform a quick scan to confirm baseline protection
    - Open Windows Security
    - Check the following under "Manage settings":
        - Virus & Threat Protection: Enabled
        - Real-time Protection: On
        - Tamper Protection: On
    - Run a quick scan by clicking "Quick scan" in the Virus & threat protection tab.
7. Verify Defender cloud delivered protection and automatic sample submission to improve detection of emerging and unknown threats.
    - Open Windows Security
    - Check the following under "Manage settings":
        - Cloud-delivered protection: Enabled
        - Automatic sample submission: Enabled

**Phase 2: Authentication and UAC Hardening**
8. Configure local password policy to enforce strong, rotating credentials and prevent password reuse. Weak or non expiring passwords risk brute force or credential reuse.
    - Win + R -> gpedit.msc
    - Navigate to Computer Configuration -> Windows Settings -> Security Settings -> Account Policies -> Password Policy
    - Set the following
        - Enforce password history: 24
        - Maximum password age: 60 days
        - Minimum password age: 1 day
        - Minimum password length: 14 characters
        - Password must meet complexity requirements: Enabled
        - Store passwords using reversible encryption: Disabled
    - See Issues & Fixes. Review effective password policy settings using "net accounts" in PowerShell to identify and confirm enforced credential requirements.
9. Verify account lockout policies are active to limit brute force authentication attempts and increase attacker detection.
    - Win + R -> gpedit.msc
    - Navigate to Computer Configuration -> Windows Settings -> Security Settings -> Account Policies -> Password Policy
10. Verify UAC Admin Approval Mode is enabled to ensure administrative actions require explicit user consent.
    - Win + R -> gpedit.msc
    - Navigate to Computer Configuration -> Windows Settings -> Security Settings -> Local Policies -> Security Options
11. Verify UAC elevation behaviour to require secure desktop consent for administrative privilege escalation.
    - User Account Control: Behaviour of the elevation prompt for administrators:
        - Prompt for consent on the secure desktop
    - User Account Control: Switch to the secure desktop when prompting for elevation:
        - Enabled
    - User Account Control: Allow UIAccess applications to prompt for elevation without using the secure desktop:
        - Disabled
12. Disable storage of Legacy LAN Manager password hashes to reduce credential theft and offline cracking risk.
    - This shouldn't even be an option anymore, but if it is, make sure that system is not storing LAN Manager password hashes.
13. Restrict anonymous access to local accounts and network resources to prevent unauthorized enumeration.
    - Network access: Let Everyone permissions apply to anonymous users: Disabled
    - Network access: Do not allow anonymous enumeration of SAM accounts: Enabled
    - Network access: Do not allow anonymous enumeration of SAM accounts and shares: Enabled
    - See Notes about Anonymous enumeration of SAM accounts and shares.
14. Validate UAC hardening by confirming administrative commands require secure desktop elevation.
    - In non admin PowerShell, run net session and confirm the following
    - UAC prompt appears: Yes
    - Secure desktop is used: Yes
    - Access is denied without elevation: Yes
    - Or a flat out denial of access.

**Phase 3: Local Security Policy & Group Policy Hardening**
15. Restrict LAN Manager authentication to NTLMv2 only to reduce exposure to credential relay and pass-the-hash attack.
    - Navigate to Computer Configuration -> Windows Settings -> Security Settings -> Local Policies -> Security Options
        - Network security: LAN Manager authentication level: Send NTLMv2 response only. Refuse LM & NTLM.
16. Disable insecure guest based access by enforcing classic authentication for local accounts.
    - Configure the following:
        - Network access: Sharing and security model for local accounts: Classic - local users authenticate as themselves
17. Enable SMB client signing to protect network communications from tampering and man-in-the-middle attacks
    - Configure the following:  
        - Microsoft network client: Digitally sign communications (always): Enabled
        - Microsoft network client: Digitally sign communications (if server agrees): Enabled
18. Enforce SMB server signing to ensure integrity of file-sharing communications
    - Configure the following:
        - Microsoft network server: Digitally sign communications (always): Enabled
        - Microsoft network server: Digitally sign communications (if client agrees): Enabled
19. Disable anonymous SID and name translation to prevent unauthenticated account discovery
    - Configure the following:
        - Network access: Allow anonymous SID/Name translation: Disabled
20. Restrict inbound NTLM authentication to reduce credential relay and lateral movement risk.
    - Configure the following:
        - Network security: Restrict NTLM: Incoming NTLM traffic: Deny all accounts
21. Apply and validate security policy changes using a forced Group Policy update
    - Apply policy to group: gpupdate /force
    - Validate: reg query HKLM\SYSTEM\CurrentControlSet\Control\Lsa

**Phase 4: Firewall, Services & Attack Surface Reduction**
22. Verify Windows Firewall was enabled across all network profiles with inbound connections blocked by default
    - Open Windows Defender Firewall with Advanced Security
    - Click Windows Defender Firewall Properties
    - Confirm the following:
        - Firewall state: On
        - Inbound connections: Block
        - Outbound connections: Allow
23. Disable unnecessary inbound firewall rules to reduce exposed network services and attack surface (Do not delete rules, just disable them)
    - Go to Inbound Rules
    - Sort by Enabled
    - Disable the following:
        - Remote Assistance
        - Remote Desktop (if not needed)
        - File and Printer Sharing (if not needed)
        - Any unused vendor services
24. Review running Windows services and disable non essential services to reduce background attack surface
    - Open PowerShell Admin and run: Get-Service | Where-Object {$_.Status -eq "Running"}
    - Identify services that can be disabled. Eg. Xbox services, Remote registry, Print Spooler (if you don't print), Fax etc.
    - Disable services using: Set-Service -Name "Service Name" -StartupType Disabled
25. Reboot the system to confirm applied firewall and service hardening changes persisted without impacting system stability.
    - Reboot system
    - Confirm the following:
        - No boot issues: Confirmed
        - Firewall still enabled: Confirmed
        - Disabled services remain stopped: Confirmed

**Phase 5: Logging, Auditing & Validation**
26. Enable advanced audit policies to ensure authentication, account activity, privilege use, and system integrity events are logged.
    - Win + R -> secpol.msc
    - Navigate to Advanced Audit Policy Configuration -> System Audit Policies - Local Group Policy Object
    - Double click the category listed and make the following adjustments by right clicking the correct subcategory and selecting properties:
        - Account Logon
            - Audit Credential Validation: Success, Failure
        - Account Management
            - Audit User Account Management: Success, Failure
        - Logon/Logoff
            - Audit Logon: Success, Failure
            - Audit Logoff: Success
        - Privilege Use
            - Audit Sensitive Privilege Use: Failure
        - System
            - Audit Security System Extension: Success
            - Audit System Integrity: Success, Failure
27. Force a Group Policy refresh to ensure audit policy changes get applied immediately
    - In PowerShell, run gpupdate /force
28. Attempt to create logs using a failed log on event and a privilege event.
    - Lock the workstation and attempt to log in with an incorrect password. Then log in correctly
    - Open PowerShell (non admin) and run net session. This should fail and should now be logged
29. Review Windows Security Event Logs to confirm authentication failure, successful logon, and privilege related events were recorded.
    - Open Event Viewer and navigate to Windows Logs -> Security 
    - Filter or look for the following Event ID's
        - 4625 - Failed Logon
        - 4624 - Successful logon
        - 4672 - Special privileges assigned
30. Validate that Microsoft Defender and Windows Firewall were actively logging security related events as well
    - Open Windows Security and navigate to Virus & threat protection -> Protection history.  Confirm logging is active even if there's no threats.
31. Wrap up with a post hardening validation to confirm system stability and persistence of applied security controls
    - Confirm the Following
        - System boots normally
        - UAC prompts appear when expected
        - Network works
        - Firewall is still enabled
        - Disabled services remain disabled

## **Notes:**
- Created a structured local directory to store screenshots, logs, and policy exports as evidence of applied hardening controls.
- Checked the current PowerShell execution policy (Get-ExecutionPolicy) prior to making security changes.
- Anonymous enumeration of SAM accounts and shares was disabled by default to maintain legacy compatibility. This setting can be hardened to reduce unauthenticated information disclosure.

## **Issues & Fixes:**
- Not able to adjust account policy settings in either secpol.msc or gpedit.msc when logged into local Administrator account. Password policy settings are enforced by the base Windows system image and are not editable through Local Security Policy or Group Policy Editor.
    - No Fix: Do not attempt to hack registry, import security templates, use third party tools or "Force" a new policy with scripts. Password policy settings can not be modified due to enforcement at the system level. Rather than bypassing enforced controls, the effective policy is validated using authoritative and documented.

## **Outcome:**
- Successfully hardened a Windows 11 system by reducing attack surface, enforcing secure authentication behaviour, restricting legacy protocols, tightening firewall and service exposure, and enabling comprehensive security logging. Validated controls through command-line testing and event log review.
