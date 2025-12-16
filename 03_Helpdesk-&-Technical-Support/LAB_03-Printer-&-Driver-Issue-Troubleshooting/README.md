# Printer & Driver Issue Troubleshooting
**Date Created:** December 15, 2025  
**Last Updated:** December 16, 2025

## **Skills Demonstrated:**
- Installing and updating printer drivers
- Troubleshooting printer detection issues (USB, network, shared printers)
- Diagnosing spooler service failures
- Using Windows 11 Print Management tools
- Clearing stalled print jobs and resetting the print spooler
- Identifying compatibility issues with printer models and drivers
- Testing network-based printing via IP address or hostname
- Verifying permissions for shared printers on a domain or workgroup
- Understanding how Windows handles printer queues and driver packages

## **Objective:**
- To practice diagnosing and resolving common printer issues encountered in helpdesk roles
- To identify root causes such as missing drivers, network problems, spooler failures, or incorrect configurations
- To demonstrate the correct steps for installing, removing, or updating printer drivers
- To simulate user-reported issues like “printer not printing,” “offline,” or “job stuck in queue”
- To create clear, repeatable documentation following real IT support workflows

## **Tools & Environment:**
- Linux Mint host running Virtual Machine Manager
- Windows 11 Pro VM (primary system tested)
- Linux Mint VM (optional for cross-OS printing tests)
- Tools used:
    - Windows Print Management
    - Services.msc (Print Spooler)
    - Windows Settings → Bluetooth & Devices → Printers & scanners
    - Event Viewer (Application/System logs for spooler errors)
    - CLI commands: net stop spooler, net start spooler, clearing C:\Windows\System32\spool\PRINTERS
    - Optional: Network printer simulator or shared folder acting as a fake print share
- Virtual networking through VMM (NAT/bridged depending on test)

## **Steps:**
1. Install a printer using manual settings and intentionally select an incorrect or generic driver to simulate a common user-reported issue, where print jobs fail or remain stuck in queue.
    - Go into settings -> Bluetooth and Devices -> Printers & Scanners
    - Select "Add device" and then when available, select "Add a new device manually"
    - Select "Add a local printer or network printer with manual settings"
    - Select existing port "LPT1: (Printer Port)" and then select "Generic/Text Only".
    - Name the printer and select install.
2. Generate a test print job to reproduce the issue and confirm the job remains stuck in the print queue, indicating a printer or spooler related problem.
    - Open Notepad and type some text, like "Test print job - printer troubleshooting lab"
    - Go to File -> Print -> Select the broken printer -> Print
    - Open the printer queue and confirm the job is stuck/error/paused. 
3. Check the Print Spooler service using Services.msc. Confirm it's status and restart the service as part of initial printer troubleshooting.
    - Press Win + R -> type "services.msc" and hit enter
    - Locate "Print Spooler"
    - Note its current state: 
    - Right click spooler and select "Restart"
    - Open Event Viewer -> Windows Logs -> System and filter for PrintService errors.
        - Reviewed Event Viewer PrintService logs and confirmed no relevant PrintService errors were generated, indicating the issue was not caused by a spooler crash or system level failure.
4. Manually clear stalled print jobs by stopping the Print Spooler service, deleting queued files from the spool directory and restarting the device
    - Open PowerShell as admin and run "net stop spooler"
    - Navigate to spooler queued files by running "cd C:\Windows\System32\spool\PRINTERS"
    - Delete all files in this folder by running "remove-item * -Force"
    - restart the spooler by running "net start spooler"
5. Identify and remove the faulty printer driver package using Print Management to prevent Windows from reusing a corrupted or incompatible driver.
    - Open Print Management by pressing Win + R -> type "printmanagement.msc" and hit enter
    - Go to "All Drivers" and right click on the faulty driver to select "Remove Driver Package"
    - See Issues and Fixes section if unable to remove driver package.
6. Reinstall the printer using a TCP/IP address instead of automatic detection to simulate troubleshooting a network printer that does not auto-discover. 
    - Go into settings -> Bluetooth and Devices -> Printers & Scanners and add a device manually.
    - Select "Add a printer using an IP address or hostname"
    - Select "TCP/IP Device" and insert the printers static IP address
7. Verify the printer works by successfully sending a test print job and confirming the queue cleared without errors.

## **Notes:**
- Not all printer failures generate Event Viewer errors. Issues such as incorrect ports, offline devices, or incompatible drivers may prevent jobs from reaching the spooler error handling stage.
- After removing a printer and restarting the spooler, the printer will reactivate automatically. Must keep spooler running in order to remove printer and its driver package.
 
## **Issues & Fixes:**
- Was unable to remove the faulty spooler package due to Windows claiming it was "in use" even though it wasn't. 
    - FIX: Go into settings -> Bluetooth and Devices -> Printers & Scanners and select the faulty printer and click remove.
    - FIX: Go back to Print Management to remove the faulty driver.

## **Outcome:**
- Successfully diagnosed and resolved a simulated printer failure by identifying spooler issues, removing corrupted drivers, and reinstalling the printer using manual configuration methods.
