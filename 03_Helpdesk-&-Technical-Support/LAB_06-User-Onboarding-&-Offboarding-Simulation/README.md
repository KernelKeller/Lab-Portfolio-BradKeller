# User Onboarding & Offboarding Simulation
**Date Created:** February 21, 2026  
**Last Updated:** February 22, 2026

## **Skills Demonstrated:**
- Windows user account management
- Linux user account creation and permission management
- NTFS and Linux file permission configuration
- Shared folder access configuration
- User access verification
- Account disabling and access revocation
- Basic onboarding and offboarding workflow simulation
- Documentation and process standardization

## **Objective:**
- To simulate a real-world IT onboarding and offboarding process by creating, configuring, verifying, and removing user access across Windows and Linux systems in a controlled lab environment.

## **Tools & Environment:**
- Host OS: Linux Mint
- Hypervisor: Virtual Machine Manager (KVM/QEMU)
- Windows 11 Pro VM
- Linux Mint VM (acting as internal file server)
- Windows Computer Management (lusrmgr.msc)
- Linux CLI (useradd, chmod, chown, groups)

## **Steps:**
1. Open Local Users and Groups (lusrmgr.msc) in Win 11 VM. Then Create a new local user account named jcarter. Assign jcarter an initial password and confirm the account creation
    - Press Win+R and type lusrmgr.msc
    - Go to Users
    - Right click -> New User
    - Create the following:
        - Username: jcarter
        - Full Name: John Carter
        - Password: TreeFrog99!
        - Ensure "User must change password" is checked.
2. Create a new local security group named Finance. Add user jcarter to the Finance group.
    - Go to Groups
    - New Group
    - Name: Finance
    - Add jcarter
3. Create a shared folder at C:\Finance. Modify the NTFS permissions to remove general user access and grant "Modify" permissions to the Finance group only.
    - Create folder C:\Finance using PowerShell: 'New-Item -Path "C:\Finance" -ItemType Directory'
    - Navigate to C:\ and right click the Finance directory -> Properties -> Security
    - Remove "Users" and "Authenticated Users" groups. You may need to select the "Advanced" option to remove inherited permissions. Click "Edit Permissions", select "Disable Inheritance" and then select "Convert inherited permissions into explicit permissions on this object" and finally hit "Apply"
    - Add "Finance" group
    - Give Modify permission to Finance
4. Log out of admin account and log into jcarter to verify access. Then navigate to and successfully access C:\Finance and create a test file to confirm permissions.
    - Log out
    - Log in as jcarter by selecting "Other User" and typing DESKTOP-QLDFVO2\jcarter and the password chosen when creating the user.
    - A new password is needed when signing in jcarter for the first time, input a new password: FrogTree99!
    - Navigate to and access C:\Finance
    - Create a test file named "I_Made_It_To_Finance"
5. Now go into your Linux VM and create a finance group on the Linux file server. Create a user account jcarter and assign it to the finance group. Create a shared directory at /srv/finance then configure ownership and permissions so only finance group members have access.
    - Log into your Linux VM and open a terminal
    - Create a new group called finance and user jcarter. jcarter will be added to the finance group.
        - 'sudo groupadd finance'
        - 'sudo useradd -m -G finance jcarter'
        - 'sudo passwd jcarter' and set password to TreeFrog99!
    - Create a finance shared directory and add the finance group to it's permissions.
        - 'sudo mkdir /srv/finance'
        - 'sudo chown root:finance /srv/finance'
        - 'sudo chmod 770 /srv/finance'
    - Switch to jcarter account to verify directory access and create a test file inside /srv/finance to confirm correct permissions
        - 'su - jcarter'
        - 'cd /srv/finance'
        - 'pwd' and see that you are in the /srv/finance directory
        - 'touch testfile.txt'
        - 'ls' to see if the test file is created.
6. Windows off boarding: disable the jcarter account in Local Users and Groups and then remove jcarter from the Finance group. Perform a final cleanup by removing jcarter account and Finance group from the system
    - In Win 11 VM, log into the admin account and go back to lusrmgr.msc and right click user jcarter. 
        - Select properties
        - Check "Account is disabled"
    - In lusrmgr.msc, go to groups and remove jcarter from the Finance group.
    - To go back to a clean slate, delete jcarter and the Finance group.
7. Linux off boarding: Lock the jcarter account on the Linux server using 'usermod -L'. Then remove jcarter from the finance group to revoke shared directory access. Perform a final cleanup by removing jcarter account and Finance group from the system
    - In terminal, run the following:
        - 'sudo usermod -L jcarter'
        - 'sudo gpasswd -d jcarter finance'
    - To go back to a clean slate, in terminal, run the following:
        - 'sudo userdel -r jcarter'
        - Verify no other users are in the finance group by running 'getent group finance'. If the return is something like finance:x:1002: and no usernames are listed, it's empty.
        - 'sudo groupdel finance'
        - Verify jcarter is deleted by running 'id jcarter'. It should return "id: 'jcarter': no such user"
        - Verify group finance is deleted by running 'getent group finance'. It should return nothing.

## **Notes:**
- When removing "Users" and "Authenticated Users" you will temporarily lose access to the folder. Simply try to enter the folder and select to give permanent permissions to yourself, you are in the admin account after all. Typically this can be avoided by having an Admin group which you would make your account part of, you can add the admin group and then remove the other groups afterwards.

## **Issues & Fixes**
- In Linux, this method caused a new user to be created but shell was not configured properly. This can be resolved by exiting jcarter shell and running 'sudo usermod -s /bin/bash jcarter'. See image Step-5d.png.

## **Outcome:**
Successfully simulated a cross platform user life cycle management workflow, including account provisioning, group based access control, permission hardening, access validation, account disabling, and secure deprovisioning across Windows and Linux systems.
