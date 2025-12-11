# Account Lockout & Password Reset
**Date Created:** December 9, 2025
**Last Updated:** December 11, 2025

## **Skills Demonstrated:**
- Diagnosing account lockout causes
- Performing secure password resets
- Using Active Directory Users & Computers (ADUC)
- Understanding account lockout policies
- Verifying user identity following support protocols
- Auditing account events in Event Viewer
- Managing user accounts in Windows environments
- Troubleshooting login failures

## **Objective:**
- To practice identifying and resolving account lockouts in a help-desk environment
- To demonstrate the correct procedure for securely resetting user passwords
- To simulate real-world support scenarios involving authentication issues
- To document repeatable troubleshooting methods that match industry expectations

## **Tools & Environment:**
- Windows Server (Domain Controller) with AD DS installed
- Windows 11 Client VM joined to the domain
- Active Directory Users & Computers (ADUC)
- Event Viewer (Security logs, Account Lockout events)
- Local Group Policy / Domain Group Policy (for lockout policy verification)
- PowerShell
- Virtual Machine Manager

## **Steps:**
1. Download Windows Server 2022 found at this website:
    - https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022
2. Create a VM and boot using the Windows Server 2022 ISO file.
3. Log into the Domain Controller and open ADUC. Verify the "Lab User" account exists under the Users container and confirm it is enabled.
4. Simulated an account lockout by entering incorrect credentials repeatedly on the Windows 11 client until Windows displayed an account lockout message. 
5. Verify the lockout status in ADUC under the Account tab, confirming the account was locked out. Click the "Unlock account" check box and select "Apply" to unlock the frozen account on the Win 11 client.
6. Reset the "Lab User" password and enable “User must change password at next logon” to follow secure password reset procedures.
    - In ADUC, right click Lab User and select "Reset Password"
    - Input new password information and check "User must change password at next logon"
7. Tested the new password by logging in as "Lab User" on the Windows 11 client. Successfully completed the forced password change and accessed the desktop.
8. In the Server VM, review the Security logs in Event Viewer and locate Event ID 4740 to confirm the account lockout was recorded and traceable.
    - Open Event Viewer and go to Windows Logs -> Security
    - Filter by event ID 4740 (An account lockout event)
    - Verify that it was for Lab User
9. Identify the root cause of the issue. Event ID 4740 indicates a user account lockout originating from the client computer. In this lab, the root cause was repeated incorrect password entries by the user.

## **Notes:**
- After downloading and installing the Windows Server 2022 ISO and setting up the domain and adding my win 11 client VM to the domain, I realized.... That was a lab in itself! I should have documented that process as I went along. I'll add that to the list of future labs to do.

 
## **Issues & Fixes:**
- ISO file was not booting up when creating the VM, after a few rounds of troubleshooting, I realized I downloaded the wrong ISO file.
    - Fix: go back to the website and download the proper file you dummy.
- After 5 failed password attempts, a "delaying next attempt" message was displayed instead of a lockout message. I needed to update the Account Lockout Threshold in Group Policy Management.
    - Fix: Updated the lockout options in Group Policy Management to cause a lockout instead of a delay.

## **Outcome:**
- Successfully simulated, identified, unlocked, and reset a domain user account following standard Helpdesk procedures. Verified authentication and reviewed event logs to confirm proper auditing.
