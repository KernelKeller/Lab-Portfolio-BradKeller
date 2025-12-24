# Linux Hardening
**Date Created:** December 20, 2025 
**Last Updated:** December 21, 2025

## **Skills Demonstrated:**
- Applying baseline security configurations to Linux Mint
- Managing user accounts and permissions
- Configuring sudo rules and password policies
- Securing SSH (disabling root login, key-based authentication, changing default port)
- Hardening firewall settings using ufw or iptables
- Reviewing and hardening services, daemons, and system startup items
- Managing file permissions and ownership on sensitive directories
- Installing and configuring security updates and patches
- Monitoring logs for suspicious activity using journalctl or /var/log
- Implementing basic intrusion detection with fail2ban
- Understanding Linux security best practices according to CIS Benchmarks

## **Objective:**
- To practice securing a Linux Mint VM against common threats
- To understand the impact of each hardening step on system usability and security
- To create a repeatable Linux hardening procedure suitable for entry-level cybersecurity labs
- To simulate real world scenarios where Linux systems must be hardened before deployment

## **Tools & Environment:**
- Linux Mint host running Virtual Machine Manager
- Linux Mint VM (primary system being hardened)
- Windows 11 Pro VM (optional for cross-platform testing)
- Tools and commands used:
    - Terminal commands: ufw, chmod, chown, passwd, ssh, fail2ban-client
    - apt for updates and security patching
    - Log monitoring: journalctl, /var/log/syslog, /var/log/auth.log
    - Text editors: nano or vim for configuration file editing
    - Optional: CIS Benchmark checklist for Linux

## **Steps:**
1. Create a clean baseline snapshot of the Linux Mint VM prior to applying any hardening changes, to allow for safe rollback if needed.
2. Update all system packages and apply the latest security patches using apt to reduce exposure to known vulnerabilities. Enable unattended security updates to ensure critical patches are automatically applied.
    - Run the following commands:
        - sudo apt update
        - sudo apt upgrade -y
        - sudo apt autoremove -y
        - sudo apt install unattended-upgrades -y
        - sudo dpkg-reconfigure unattended-upgrades and select yes
    - Verify by running:
        - systemctl status unattended-upgrades
3. Review all local user accounts and verify that only required users exist on the system. Confirm sudo access was restricted to authorized users only and disabled direct root login.
    - List users using: cut -d: -f1 /etc/passwd
    - Check who has sudo: getent group sudo
    - Lock unused accounts: sudo passwd -l "username"
    - Lock direct root login: sudo passwd -l root
4. Enforce strong password policies using PAM to require longer, more complex passwords. Configure password aging rules to reduce the risk of credential compromise over time.
    - Install a password quality tool by running sudo apt install libpam-pwquality -y
    - Edit by running sudo nano /etc/security/pwquality.conf
    - Adjust the following:
        - minlen = 14
        - ucredit = -1
        - lcredit = -1
        - dcredit = -1
        - ocredit = -1
        - retry = 3
    - Force password aging by running sudo nano /etc/login.defs
    - Adjust the following:
        - PASS_MAX_DAYS 90
        - PASS_MIN_DAYS 7
        - PASS_WARN_AGE 14
5. Harden SSH by disabling root login, enforcing key based authentication, and changing the default SSH port. Then verify SSH access using public key authentication prior to disabling password based logins.
    - Install SSH if it's not already present by running sudo apt install openssh-server -y
    - Edit the SSH configurations by running sudo nano /etc/ssh/sshd_config
    - Adjust the following information:
        - Port 22
        - PermitRootLogin no
        - PasswordAuthentication no
        - PubkeyAuthentication yes
        - MaxAuthTries 3
    - Restart SSH by running sudo systemctl restart ssh
    - Test connection from host to VM by copying the public key
        - Run ssh-copy-id user@vm-ip (User being the VM username and the VM's IP address. This will prompt to input the VM's user password, but after the initial input, you will be able to ssh without having to use the PW in the future).
6. Configure the UFW firewall to deny all incoming traffic by default while allowing outbound connections.  Explicitly allow SSH traffic on a non standard port to reduce automated attack.
    - Run the following commands:
        - sudo ufw default deny incoming
        - sudo ufw default allow outgoing
        - sudo ufw allow 2222/TCP
        - sudo ufw enable
        - sudo ufw status verbose
7. Review all running services and disable unnecessary daemons to minimize the system attack surface. Verify that only essential services were enabled at boot.
    - List running services using systemctl list-unit-files --type=service
    - Disable unused services (example sudo systemctl disable cups && sudo systemctl stop cups)
    - Check start up services using systemctl list-unit-files | grep enabled
8. Audit file and directory permissions on sensitive system locations to prevent unauthorized access. Afterwards, correct insecure permissions on critical authentication and configuration files to enforce least privilege, protect credential storage etc.
    - Check current permissions using ls -ld /etc /var/log /home
    - Secure critical files using   
        - sudo chmod 640 /etc/shadow
        - sudo chmod 644 /etc/passwd
        - sudo chown root:root /etc/shadow
    - Verify no unsafe world writable system files were present. Identify expected writable files in temporary and user specific directories, watch for potential issues in other directories like /etc or /bin etc.
        - sudo find / -xdev -type f -perm -0002
9. Review authentication and system logs to identify potential security events. Then confirm persistent logging was enabled to support incident investigation.
    - Review auth logs using sudo cat /var/log/auth.log
    - Review high severity system logs from current boot to identify any errors or misconfigs introduced during hardening process. sudo journalctl -p 3 -xb
    - Verify persistent logging is enabled using ls /var/log/journal. If there is a directory that looks similar to 6b5c9e7d8f2a4a4e9c6b7f0e12345678, then it's enabled.
10. Install and configure Fail2Ban to automatically block repeated failed authentication attempts. Then verify active SSH protection and ban enforcement using fail2ban-client
    - run sudo apt install fail2ban -y
    - Create local configs using:
        - sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
        - sudo nano /etc/fail2ban/jail.local
        - Adjust the following under sshd:
            - enabled = true
            - port = 22
            - maxretry = 3
            - bantime = 3600
    - Restart fail2ban using sudo systemctl restart fail2ban
    - Verify using sudo fail2ban-client status sshd
11. Validate system accessibility and security controls after hardening changes were applied. Then create a post hardening snapshot to preserve the secured system state.

## **Notes:**
- We want to lock direct access to root to limit the attack surface by eliminating a known high value target, enforce least privilege, improve audit ability through sudo logging and require attackers to compromise multiple security layers instead of gaining full system control with a single credential.
- When locking root, the systems reports "password changed" which roughly translates to "password authentication disabled" in this case. Locking an account mean the password has is made invalid so authentication can never succeed. In this case, Linux places a ! character to the hash. That leading ! means authentication using a password is impossible, even if the correct password is used.
 
## **Issues & Fixes:**
- Wasn't able to configure ssh to port 2222.
    - Fix: revert it back to port 22 for this lab, but note that switching ssh to a random port other than the expected port 22 will make it harder for a nefarious entity to gain entrance through ssh.

## **Outcome:**
-Successfully hardened a Linux Mint system using industry security best practices, reducing attack surface through account controls, SSH hardening, firewall enforcement, permission auditing, persistent logging, and automated intrusion prevention.
- Demonstrated the ability to assess, implement, validate, and document Linux security controls in a repeatable manner suitable for real world system deployment.
