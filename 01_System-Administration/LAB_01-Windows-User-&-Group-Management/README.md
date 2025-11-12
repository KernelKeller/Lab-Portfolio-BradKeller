# **Windows User & Group Management**
**Date Created:** November 11, 2025  
**Last Updated:** November 11, 2025

## **Skills Demonstrated:**
- Windows user account creation and management
- Local group creation and management
- Assigning and testing user permissions
- PowerShell scripting for system administration
- Least privilege principle implementation
- GUI vs. command-line administration
- Troubleshooting access issues
- Documentation of system administration tasks

## **Objective:**
- Learn how to create, modify, and delete local user accounts in Windows 11.
- Understand how to create and manage local groups and assign users to them.
- Practice using both the GUI and PowerShell for user and group management.
- Test and verify user permissions, including adding/removing administrative rights.
- Demonstrate the principle of least privilege in a controlled lab environment.
- Document the process for repeatability and portfolio inclusion.

## **Tools & Environment:**
- Operating System: Windows 11 Enterprise Evaluation (64-bit)  
- Virtualization: VirtualBox VM (NAT networking enabled)  
- User Account: Local Administrator  
- GUI Tool: Computer Management (compmgmt.msc)  
- Command-Line Tool: PowerShell (Admin)  
- Snapshots: Pre-lab snapshot for rollback if needed 

## **Steps:**
1. Verified admin access using PowerShell using commands whoami, Get-ComputerInfo
2. Created pre-lab VirtualBox snapshot
3. Access Computer Management GUI via compmgmt.msc
4. Created 3 local users (Tech1, Tech2, Guest1) using GUI via compmgmt.msc
	- Confirmed newly created users appear under Local Users and Groups > Users
5. Created two custom local groups via Powershell 
	- IT_Team and GuestsLimited
6. Added Tech1 and Tech2 to IT_Team. Added Guest1 to GuestsLimited
7. Verified memberships using Get-LocalGroupMember
8. Created VirtualBox snapshot labelled "Pre Permissions" before modifying user permissions.
9. Added Tech1 to local Administrators group for temporary elevation.
10. Logged in as Tech1, confirmed elevated permissions using whoami /groups and attempting folder creation in C:\Program Files
11. Logged in as Tech2, confirmed standard user permissions using whoami /groups and attempting folder creation in C:\Program Files
12. Enabled Guest1 account and confirmed limited access in same method as above
13. Removed Tech1 from Administrators group to restore least privilege.
14. Remove test users and groups after lab completion using:
	- Remove-LocalUser -Name "Tech1","Tech2","Guest1"
	- Remove-LocalGroup -Name "IT_Team","GuestsLimited"

## **Notes:**
- Confirmed new users and groups appear correctly in PowerShell and Computer Management.
- Observed that Guest1 is disabled by default upon creation. Tech1 and Tech2 are enabled by default upon creation.
- whoami /groups output confirmed BUILTIN\Administrators membership for Tech1.
- Successfully created a folder in C:\Program Files, confirming admin rights are active.
- Tech2 was not prompted to change password on first login. During Creation, "User must change password at next logon" was deselected.
- Tech1 could create folders in system directories while elevated.
- Tech2 and Guest1 received Access Denied errors when attempting system modifications.
- Confirmed only the original local Administrator remains in the Administrators group.
- Verified all users remain active and properly assigned to their intended groups.

 
## **Issues & Fixes:**
- PowerShell’s Get-LocalUser cmdlet doesn’t include group memberships. Which is ridiculous if you’re trying to audit users and their groups quickly. Seems it's required to have to query each user individually using Get-LocalGroup or Get-LocalGroupMember. No Fix, but found my first gripe.

## **Outcome:**
- Successfully created local users and groups.
- Added users to groups and verified group memberships.
- Temporarily elevated Tech1 to admin, tested permissions, then reverted back to standard rights, demonstrating least privilege enforcement.
- Documented process for repeatability in a lab or portfolio setting.


