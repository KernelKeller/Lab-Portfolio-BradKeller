# Linux Log Investigation
**Date Created:** January 12, 2026 
**Last Updated:** January 12, 2026

## **Skills Demonstrated:**
- Linux system log analysis
- Authentication and authorization log review
- Identifying failed and successful login attempts
- Using command-line tools to filter and search logs
- Understanding Linux logging structure and log rotation
- Basic incident investigation methodology

## **Objective:**
- Learn where Linux stores critical system and security logs
- Practice analyzing authentication, system, and service activity
- Identify normal vs suspicious behavior in Linux logs
- Build foundational skills used in SOC, sysadmin, and cybersecurity roles

## **Tools & Environment:**
- Linux Mint VM
- Linux Mint Host
- Log files:
    - /var/log/auth.log
    - /var/log/syslog
    - /var/log/kern.log
    - /var/log/dpkg.log
- Commands:
    - journalctl
    - grep
    - less
    - tail
    - awk
    - who
    - last
    - lastlog

## **Steps:**
1. Verify sudo access on the Linux VM to ensure permission to read system and security log files under /var/log.
    - Open a terminal and run sudo -v
2. List available system log files in /var/log to identify authentication, system, kernel, and package management logs.
    - List log directory contents using ls -lh /var/log
3. Review /var/log/auth.log using less to identify failed and successful authentication attempts, sudo usage, and session activity.
    - Open the auth log by running sudo less /var/log/auth.log
    - Look for failed password, accepted password, sudo, session opened.
4. Filter authentication logs for failed login attempts to assess potential brute force or unauthorized access activity.
    - In terminal, run sudo grep "Failed password" /var/log/auth.log
    - If you want to count attempts, run sudo grep "Failed password" /var/log/auth.log | wc -l
5. Identify successful authentication events in auth.log to establish baseline user login behaviour
    - In terminal, run sudo grep "Accepted" /var/log/auth.log
    - Look for Username, Authentication method, Timestamp. Every successful login should be expected and explainable.
6. Analyze sudo related entries to determine whether privilege escalation activity aligned with expected administrative behaviour.
    - In terminal, run sudo grep "sudo" /var/log/auth.log
    - Look for which user ran sudo, what command used, was this expected?
7. Examine kernel logs to identify low level system events and confirm no abnormal kernel behaviour was present. 
    - In terminal, run sudo less /var/log/kern.log
    - Look for hardware events, network interface changes, security module messages.
8. Review dpkg.log to identify software installation or removal activity that could indicate system modification.
    - In terminal, run sudo less /var/log/dpkg.log
    - Look for /install or /remove
9. Use who, last, and lastlog to correlate current and historical user session activity with authentication logs.
    - In terminal, run the following:
        - who
        - last
        - lastlog
10. Query systemd journal logs using journalctl to supplement traditional log analysis and capture recent system events.
    - In terminal, run journalctl -xe or journalctl --since "today"

## **Notes:**
- A key note is Linux logs are distributed, not centralized like Windows Event Viewer.
- Linux uses log rotation. Older logs may be compressed and file names often imply purpose.
 
## **Issues & Fixes:**
- Large log files were navigated efficiently using less instead of cat. 

## **Outcome:**
- Successfully analyzed Linux authentication, system, kernel, and package logs to validate normal system behaviour and practice foundational incident investigation techniques.
