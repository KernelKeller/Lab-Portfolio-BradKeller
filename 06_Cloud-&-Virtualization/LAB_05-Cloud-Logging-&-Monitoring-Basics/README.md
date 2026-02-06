# Cloud Logging & Monitoring Basics
**Date Created:** February 1, 2026   
**Last Updated:** February 1, 2026

## **Skills Demonstrated:**
- Understanding logging and monitoring concepts in cloud environments
- Collecting and reviewing system and application logs
- Identifying security and performance-related events
- Basic log aggregation and correlation across systems
- Monitoring resource usage and system health
- Documentation of monitoring and alerting practices

## **Objective:**
- Learn how logging and monitoring function in cloud and virtualized environments
- Practice collecting and reviewing logs from multiple systems
- Identify common events related to authentication, services, and system health
- Understand how monitoring data supports troubleshooting and incident response

## **Tools & Environment:**
- Linux Mint Host
- Windows 11 Pro VM
- Linux Mint VM
- Virtual Machine Manager (KVM/QEMU)
- Logging tools:
    - Windows Event Viewer
    - Linux log files (/var/log/syslog, /var/log/auth.log, journalctl)
- Monitoring tools:
    - top, htop, vmstat, df, free
    - Windows Task Manager / Resource Monitor
- Optional network monitoring:
    - tcpdump, Wireshark

## **Steps:**
1. Start up Win 11 and Linux VM's and verify successful login and network connectivity on both systems.
    - Win 11 VM:
        - Log in and open PowerShell
        - 'ping 8.8.8.8'
    - Linux VM:
        - Log in and open a terminal
        - 'ping 8.8.8.8'
2. Collect baseline system performance metrics on both Linux and Win 11 systems, including CPU usage, memory consumption, disk utilization, and system up time prior to generating any test events.
    - Win 11 VM:
        - Open Task Manager
        - Navigate to Performance tab
        - Observe CPU, Memory, Disk, Network
    - Linux VM:
        - Open a terminal
        - 'uptime'
        - 'free -h'
        - 'df -h'
        - 'top' (exit top with q)
3. Review Linux system activity by examining /var/log/syslog to observe background services, system events and routine operating system messages.
    - Open a terminal
    - 'sudo less /var/log/syslog'
4. Examine Linux Authentication logs in /var/log/auth.log to identify user authentication activity, sudo usage, and session management events.
    - Open a terminal
    - 'sudo less /var/log/auth.log'
5. Use 'journalctl' to view recent systemd-managed log entries and identify centralized system events and error level messages.
    - Open a terminal
    - 'sudo journalctl -n 50'
    - 'sudo journalctl -p err'
6. Open Windows Event Viewer and navigate to System and Security logs to review operating system events, service activity, and authentication related records.
    - Press Win + R and type eventvwr.msc to open Event Viewer
    - Navigate to:
        - Windows Logs -> System
        - Windows Logs -> Security
7. Generate new Windows log entries by performing a user lock and authentication event and confirm the corresponding security events were recorded in Event Viewer.
    - Lock the system using Win +L and log back in.
    - Go to event viewer and refresh the Security logs.
8. Simulate a high CPU usage on the Linux system and monitor real time performance changes using top, confirming increased processor utilization and system load.
    - On Linux VM, open a terminal and run:
        - 'yes > /dev/null' - Let this run
        - Stop the process using Ctrl + C
    - Open another terminal and run:
        - 'top'
        - Stop the process using Ctrl + C
9. Monitor Linux Memory and disk utilization using 'free -h' and 'df -h' to assess available resources and filesystem usage.
    - On Linux VM, open a terminal and run:
        - 'free -h'
        - 'df -h'
10. Observe real time system performance metrics on Windows using Task Manager and Resource Monitor, including CPU, memory, disk, and network activity.
    - Open Task Manager and observe CPU, Memory, Disk
    - Open Resource Monitor
    - Observe changes in resource utilization during normal system operation.

## **Outcome:**
- Successfully collected and analyzed system and authentication logs across Linux and Windows environments, monitored real-time resource usage, and correlated system events with performance data to demonstrate foundational cloud logging and monitoring concepts used in troubleshooting and incident response.
