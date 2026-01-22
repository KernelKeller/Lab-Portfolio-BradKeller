# Mini SIEM Lab
**Date Created:** January 14, 2026  
**Last Updated:** January 14, 2026

## **Skills Demonstrated:**
- Log collection and centralization
- Basic SIEM concepts and workflows
- Correlating events across multiple systems
- Identifying security-relevant log sources
- Writing simple detection logic or filters
- Threat detection and investigation fundamentals
- Understanding signal vs noise in logs

## **Objective:**
- Simulate a basic SIEM environment using local and VM-based systems
- Centralize logs from Windows and Linux systems
- Identify and correlate security events across hosts
- Practice investigating suspicious activity using aggregated logs
- Demonstrate foundational SOC-level detection and analysis skills

## **Tools & Environment:**
- Linux Mint Host (log collector / analysis system)
- Windows 11 Pro VM
- Linux Mint VM

Log Sources:
- Windows Event Logs (Security, System, PowerShell)
- Linux logs:
    - /var/log/auth.log
    - /var/log/syslog
    - /var/log/kern.log

Tools & Utilities (lightweight / lab-scale):
- rsyslog (log forwarding)
- journalctl
- Event Viewer
- PowerShell
- Bash utilities:
    - grep, awk, sed, cut
- Optional visualization:
    - CSV exports or simple dashboards (if implemented)

Simulated Events:
- Failed login attempts
- Successful authentication events
- Suspicious command execution
- Privilege escalation attempts

## **Steps:**
1. Power on all virtual machines and verify network connectivity between the Linux Mint host, Linux Mint VM, and Windows 11 VM. Define system roles by designating the Linux Mint host as the centralized log collection and analysis system, with both VMs acting as independent log sources.
    - On each VM run the following commands to confirm connectivity:
        - ip a = 10.0.0.45
        - ipconfig = 10.0.0.21
        - HOST IP 10.0.0.239 ERASE THIS BEFORE PUSHING TO GITHUB
        - ping <linux VM> from Win 11 VM
        - ping <Windows VM> from Linux VM
2. If the logs don't exist locally, the SIEM will never see it, so we verify baseline log generation on each endpoint by reviewing local Windows Event Logs and Linux System logs to confirm normal logging behaviour prior to centralization.
    - Linux VM:
        - sudo tail -n 20 /var/log/auth.log
        - sudo tail -n 20 /var/log/syslog
    - Windows VM
        - Open Event viewer and navigate to Windows Logs -> Security
        - Windows logs -> System
        - Applications and Services Logs -> PowerShell
3. Configure rsyslog on the Linux Mint VM to forward all system logs to the Linux Mint host, which will be set up to listen for incoming UDP and TCP log traffic on port 514.
    - On Linux VM
        - Create a new file, sudo nano /etc/rsyslog.d/10-forwarder.conf
        - add this to the document:  *.* @@<SIEM_HOST_IP>:514
        - restart rsyslog with sudo systemctl restart rsyslog
    - On Linux Host VM
        - Create a new file, sudo nano /etc/rsyslog.d/00-listener.conf
        - add the following:
            - module(load="imudp")
            - input(type="imudp" port="514")
            - module(load="imtcp")
            - input(type="imtcp" port="514")
        - restart using sudo systemctl restart rsyslog
        - Verify by using sudo ss -tulnp | grep 514 (you should see srsyslogd listening on TCP and/or UDP 514)
4. Validate centralized Linux log ingestion by generating authentication events on the Linux Mint VM and confirming their presence on the SIEM host using log filtering commands
    - On Linux VM
        - run command "logger "SIEM FORWARDING TEST - LINUX VM"
        - Confirm it exists locally by running sudo journalctl -n 20
    - On Linux Host
        - sudo journal ctl -n 20 /var/log/syslog
    
5. 5. Generate and analyze Windows VM events locally to simulate endpoint-level logging and correlate with Linux log activity. 
    - On Windows VM:
        - Open Event Viewer
        - Navigate to Windows Logs -> Security, System, and PowerShell
        - Generate test events manually if desired (e.g., failed login attempts, PowerShell commands)
        - Review logs to confirm entries exist
    - Note: Windows logs were analyzed locally since forwarding was not configured in this lab


## **Notes:**
- Linux VM logs successfully forwarded to the Linux Mint host and could be verified using `journalctl` and `logger` test messages.
- Windows 11 logs could not be centralized in this lab due to the lack of a forwarding mechanism; events were analyzed locally.
- Systemd/journald on Linux Mint required using `journalctl` instead of directly grepping `/var/log/syslog` for reliable results.
- Lab demonstrated basic SIEM concepts, log centralization, and event correlation workflow even without full Windows log forwarding.

 
## **Issues & Fixes:**
- Issue: Original lab instructions assumed Windows logs could be forwarded to the Linux host; this was never configured.
    - Fix: Updated lab scope to analyze Windows logs locally while maintaining centralized Linux logging.
- Issue: Grep commands against `/var/log/syslog` returned no results on Linux Mint due to systemd/journald logging.
    - Fix: Switched to using `journalctl` for log verification and validation.
- Issue: Step ordering in the original lab had host log checks before generating events on the Linux VM.
    - Fix: Corrected order to first generate events on the Linux VM, then verify locally, then confirm ingestion on the host.


## **Outcome:**
- Successfully configured Linux VM to forward all system logs to the centralized Linux Mint host.
- Verified end-to-end log forwarding from Linux VM to host using `logger` test messages.
- Demonstrated local log review on Windows VM to simulate endpoint-level monitoring.
- Practiced SIEM workflow concepts: event generation, collection, filtering, correlation, and investigation of suspicious activity across multiple systems.
- Identified the limitations of the lab environment (Windows forwarding not configured) and documented practical workarounds.

