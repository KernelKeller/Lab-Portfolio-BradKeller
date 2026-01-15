# Windows Event Log Analysis
**Date Created:** January 10, 2026  
**Last Updated:** January 10, 2026 

## **Skills Demonstrated:**
- Windows Event Log navigation and filtering
- Security event analysis (logon, privilege use, failures)
- Identifying successful vs failed authentication attempts
- Correlating events across time to identify suspicious behavior
- Basic incident analysis and documentation

## **Objective:**
- Learn how Windows logs system, security, and application events
- Identify normal vs suspicious authentication activity
- Practice filtering and interpreting Security Event IDs commonly used in SOC and IT roles
- Build confidence analyzing logs during troubleshooting or incident response

## **Tools & Environment:**
- Windows 11 Pro VM
    - Event Viewer (eventvwr.msc)
    - Local user accounts
- Key Event IDs
    - 4624 – Successful logon
    - 4625 – Failed logon
    - 4634 – Logoff
    - 4672 – Special privileges assigned to logon
    - 4688 – Process creation

## **Steps:**
1. Boot up Win 11 VM and open Windows Event Viewer using eventvwr.msc. Review the primary Windows log categories (Application, Security, System) to understand where authentication and security related events are recorded
    - Boot Win 11 VM
    - Press Win+R
    - type eventvwr.msc and hit enter
    - Expand Windows logs
        - Application
        - Security 
        - System
2. Clear the Windows Security event log to establish a clean baseline before generating new authentication and system activity.
    - Right click "Security"
    - Select "Clear Log"
    - Choose "Clear"
3. Generate a successful local logon by locking and unlocking the Windows system to create normal authentication events for analysis.
    - Lock Windows using Win+L
    - Log back in with a typical user account.
        - This generates 4642 - Successful logon (4672 if admin)
4. Filter the Security log for event ID 4624 to identify successful logon events and review logon type, user account, and source details.
    - Click Security
    - On the right options bar, select "Filter Current Log"
    - In the Event ID section, input 4624
    - Apply the filter
    - Open one event and inspect the following:
        - Logon Type: 5
        - Account Name: DESKTOP-QLDFVO2$
        - Source Network Address: -
5. Generate failed authentication attempts by intentionally entering incorrect credentials to create Event ID 4625 entries for analysis.
    - Lock the machine using Win+L
    - Enter a wrong password 2 times
    - Log in correctly afterwards
6. Filter the Security log for Event ID 4625 and analyze failed logon attempts, including failure reason, account targeted, and logon type.
    - Filter Security log for Event ID 4625 in the same way as in step 4.
    - Select an event and review:
        - Failure Reason: 
        - Account Name: KernelKeller
        - Logon Type: 2
7. Identify privileged logon activity by filtering for Event ID 4672 and confirming when administrative privileges were assigned during authentication.
    - Clear filters and create a new filter for Event ID 4672 in the same way as in step 4 and 6.
    - Inspect events tied to the admin account
8. Generate process creation events by executing command line tools and analyze Event ID 4688 to observe how Windows logs new process execution.
    - Open PowerShell
    - run the following commands:
        - whoami
        - ipconfig
        - net user
    - Go back to event viewer and filer Security log for 4688
    - See Issues & Fixes below.

## **Notes:**
- Multiple failed logon attempts followed by a successful logon and privileged session could indicate brute force or credential misuse activity if occurring from an unexpected source.
- We can verify that auditing is working even if 4688 ID's are missing. Event ID 5379 entries confirm credential related activity was logged, indicating auditing was functional even though process creation events (4688) were unavailable.

## **Issues & Fixes:**
- By default, Win 11 does not always log process creation unless Advanced Audit Policy is enabled AND Process Creation auditing is turned on. 
    - Fix: Unsuccessful
        - Access Local Security Policy and navigate to Security Settings -> Advanced Audit Policy Configuration -> System Audit Policies -> Detailed Tracking -> Audit Process Creation.
        - Double Click Audit Process Creation
        - Check Success (Can also check Failure as an option)
        - Click Apply -> OK 
        - In PowerShell, run gpupdate /force.  Domain controller may fail but the local policy will push through successfully
        - Back into Local Security Policy tool, navigate to Local Policies -> Security Options
        - Find the policy "Audit: Force audit policy subcategory settings (Windows Vista or later) to override audit policy category settings" and set it to Enabled and apply it.
        - Navigate to Local Policies -> Audit Policy
        - Fine Audit process tracking and double click it. Select Success and apply.
        - Reboot Win 11 VM to implement the changes
        - Attempted to enable process creation auditing (Event ID 4688) using both legacy audit policy, advanced audit subcategory configuration, and direct auditpol enforcement. Unfortunately, verification consistently showed process creation auditing remained disabled. This indicates a platform level limitation where process creation auditing is unavailable without domain based Group Policy or an Enterprise SKU.       
    
## **Outcome:**
- Successfully analyzed Windows Security Event Logs, identified authentication and privilege related activity, and correlated events to simulate real-world incident response investigation.
