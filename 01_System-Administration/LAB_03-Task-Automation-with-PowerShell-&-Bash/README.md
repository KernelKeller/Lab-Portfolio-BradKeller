# Task Automation with PowerShell & Bash
**Date Created:** November 13, 2025 
**Last Updated:** November 13, 2025 

## **Skills Demonstrated:**
- Writing and executing PowerShell scripts for task automation
- Writing and executing Bash scripts in Linux
- Automating repetitive administrative tasks (e.g., user creation, log cleanup)
- Using variables, loops, and conditional statements in scripts
- Cross-platform automation concepts (Windows vs. Linux)
- Scheduling scripts using Task Scheduler and cron
- Testing, debugging, and documenting automation scripts
- Applying security best practices for scripts (execution policy, file permissions)

## **Objective:**
- Learn to create and run automation scripts using PowerShell (Windows) and Bash (Linux).
- Practice automating real-world administrative tasks such as user account creation, directory setup, and log maintenance.
- Compare syntax, structure, and execution between PowerShell and Bash scripting.
- Understand how to schedule scripts for periodic execution on both operating systems.
- Demonstrate the ability to document, test, and debug automation scripts.

## **Tools & Environment:**
- Operating Systems:
	- Windows 11 Enterprise Evaluation (PowerShell 5+ or 7)
	- Linux Mint 21.3 (Bash 5+)
- Virtualization: VirtualBox VMs (NAT networking enabled)
- Script Editors:
	- Windows: Visual Studio Code
	- Linux: Nano
- Command-Line Tools:
	- PowerShell: New-Item, Get-Content, ForEach-Object, Write-Output, Start-Sleep
	- Bash: echo, for, if, cat, touch, chmod, sleep
- Scheduling Tools:
	- Windows Task Scheduler
	- Linux cron
- Snapshot: Pre-lab clean state

## **Steps:**
Windows 11
1. Created automation.ps1 in VS Code on Windows 11 VM.
2. Pasted script logic to automatically create a main "AutomationLab"
3. Executed script in VS Code and verified folder creation.
4. Confirmed folders and log entries were create/written correctly.

Linux
5. Created automation.sh in Linux VM using Nano.
6. Pasted Bash script with variables for main directory and log file and made script executable using "chmod +x automation.sh".
7. Executed script using "./automation.sh"
8. Verified folder creation and log entries were correct.
9. Opened crontab using crontab -e.
10. Added cron expression to execute Bash automation script every 5 minutes.
11. Saved cron configuration and verified scheduled runs with crontab -l.
12. Confirmed logs updated correctly from automated executions. Removed cron entry after testing.

Windows 11
13. Opened Task Scheduler and created a new task. Set trigger (daily or custom schedule). Set action to run powershell.exe with -ExecutionPolicy Bypass -File "C:\Users\vboxuser\Desktop\automation.ps1". Enabled Run with highest privileges.
14. Manually ran the task to confirm log updates correctly.
15. Confirmed logs updated correctly from automated executions. Removed Scheduled task after testing.

## **Notes:**
- Verified the script created the main folder C:\AutomationLab.
- Confirmed subfolders for users (alice, bob, charlie) were created inside the main folder.
- Script output Automation completed. indicates successful execution.
- Observed folder creation order and timestamps match expected results.

## **Issues & Fixes:**
- In Linux, the scheduled Logging updates were not working due to a pathing error. Cron was searching to run the automation script in the newly created "AutomationLab" folder. The script was located in the home directory.
	- FIX: Adjusted script to create an error log. Read error log and determined moving the automation.sh file into the "AutomationLab" folder would fix the issue. Fix confirmed.

## **Outcome:**
- Successfully automated directory creation and logging across Windows and Linux.
- Demonstrated use of variables, loops, and conditionals in both PowerShell and Bash.
- Implemented scheduled automation using Task Scheduler and cron.
- Validated script behavior through testing and log review.
- Surprised myself with my ability to diagnose and resolve execution and pathing issues in automated workflows, especially since this was my first time doing it.
- Gained first hand view of basic automation workflows.
