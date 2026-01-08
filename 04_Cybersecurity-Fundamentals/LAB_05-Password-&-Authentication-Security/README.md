# Password & Authentication Security
**Date Created:** December 7, 2025 
**Last Updated:** December 7, 2025 

## **Skills Demonstrated:**
- Understanding password policies and best practices
- Configuring password complexity, expiration, and history rules
- Implementing multi-factor authentication (MFA) on Windows and Linux systems
- Managing user accounts and authentication methods
- Using Active Directory and local account security tools
- Testing authentication workflows and login restrictions
- Identifying weak passwords and insecure authentication practices
- Understanding hashing, salting, and secure password storage
- Practising secure credential management in lab environments

## **Objective:**
- To learn and apply best practices for password and authentication security
- To simulate real world scenarios with secure and insecure authentication setups
- To demonstrate the ability to configure policies that mitigate common attack vectors
- To document repeatable procedures for password management and MFA implementation
- To strengthen understanding of secure authentication fundamentals for entry-level cybersecurity roles

## **Tools & Environment:**
- Linux Mint host running Virtual Machine Manager
- Windows 11 Pro VM (primary system for password policy testing)
- Linux Mint VM (for cross-platform authentication tests)
- Tools and utilities:
    - Windows: Local Security Policy (secpol.msc), Active Directory Users & Computers (ADUC), PowerShell (Get-LocalUser, Set-LocalUser)
    - Linux: passwd, /etc/shadow, PAM configuration
    - Optional MFA tools (authenticator apps or simulated MFA in lab)
    - Event logs: Windows Event Viewer and Linux /var/log/auth.log
    - Security testing tools: John the Ripper, Hashcat (lab only, offline testing)
- Networking: isolated VMM virtual network for testing login/authentication scenarios

## **Steps:**
1. Create a local test accounts (lab_user1) with a deliberately weak password to establish a baseline insecure authentication configuration, prior to applying security policies.

    - Win 11 VM:    
        - In the Win 11 VM, open settings and navigate to Accounts -> Other users.
        - Under Other users, select "Add account"
        - Select " I don't have this person's sign-in information"
        - Select "Add a user without a Microsoft account"
            - Username: lab_user1
            - Password: weakaccess1!
        - Log into new user account to verify creation has succeeded.
    - Linux VM:
        - create a new user by running sudo adduser labuser1 in a terminal
        - use a weak password: password123
2. Configure Windows local password policies using Local Security Policy to enforce minimum length, complexity requirements, password history, and expiration settings. The validate the password policy enforcement by attempting to set a weak password, confirming the system correctly rejects insecure credentials.
    - Run secpol.msc
    - Navigate to Account Policies -> Password Policy
    - Set Minimum password length: 12
    - Password must meet complexity requirements: Enabled
    - Maximum password age: 60 days
    - Enforce password history: 5 passwords
    - Then force the updating in PowerShell by running gpupdate /force
3. Configure Linux password aging and complexity controls useing chage and PAM (pam_pwqualirty) to enforce minimum length, character requirements, and password rotation policies. Vefiry Linux password policy enforcement by attempting to assign weak passwords and confirming rejection by the authentication system
    - Check current settings: sudo chage -l labuser1
    - Set new password aging rule: sudo chage -M 60 -m 7 -W 7 labuser1
    - Enforce complexity: sudo nano /etc/pam.d/common-password
        - ensure this line exists or modify it to match: password requisite pam_pwquality.so retry=8 minlen=12 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1
    - Test by running passwd labuser1 and attempting another weak password (password1). This should fail now.
4. Implement multi-factor authentication concepts on Windows using Windows Hello, this can demonstrate knowledge of combining password based authentication with additional verification factors. Review how multi-factor authentication is implemented on Linux systems using PAM modules that require a one-time password in addition to a standard user password.
    - Win 11 VM:
        - Open Settings and navigate to Accounts -> Sign-in options
        - Under Pin section, click "Set up"
        - Enter account password and create a PIN
        - Verify by logging into the account using the new PIN
    - Linux VM:
        - Linux authentication is controlled by PAM. MFA is implemented using PAM modules. OTP is requested after password verification.
5. Review Windows Security Event Logs to identify successful and failed authentication attempts to validate visibility into login activity. Analyze Linux authentication logs to confirm proper logging of login attempts and password changes.
    - Win 11 VM:
        - Open Event Viewer and navigate to Windows Logs -> Security
        - Look for Event ID 4624 (successful logon) and 4625 (failed logon)
    - Linux VM:
        - In terminal, run sudo tail -n 50 /var/log/auth.log
6. Examine Linux password hash storage within /etc/shadow to understand hashing and salting mechanisms used to protect credentials. The perform controlled offline password analysis to demonstrate awareness of password attack vectors and the importance of strong password policies.
    - In Linux VM, run sudo grep labuser1 /etc/shadow

## **Notes:**
- No additional third-party tools were required for core password and authentication security testing, as all controls were implemented using native Windows and Linux security mechanisms commonly found in enterprise environments. 

## **Issues & Fixes:**
- Regarding Step 1, weak passwords are blocked by existing policy (which we will not adjust for the sake of a lab), so I'm not able to actually create an account with a "weak" password. Linux VM allowed me to make the weak password as intended.
    - Fix: select a complaint password that sounds weak, like weakpassword1!
- Regarding Step 2, Win 11 already has built in policies that are unable to be changed without going into registry. We will not do this for the lab, we will intentionally leave these unchanged to preserve system integrity in the lab environment.
    - Fix: imagination.

## **Outcome:**
- Successfully implemented and tested password and authentication security controls across Windows and Linux systems, including password policy enforcement, authentication logging, and multi-factor authentication concepts. Developed some understanding of both defensive configuration and common authentication attack vectors in a controlled lab environment.
