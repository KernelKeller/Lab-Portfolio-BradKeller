# IAM & Policy Configuration
**Date Created:** January 29, 2026  
**Last Updated:** January 29, 2026

## **Skills Demonstrated:**
- Understanding identity and access management concepts
- Creating and managing user accounts and roles
- Configuring permissions and policies for resources
- Enforcing least privilege and role-based access controls (RBAC)
- Applying security best practices for cloud and virtual environments
- Basic cloud governance and compliance awareness

## **Objective:**
- Learn how to create, assign, and manage user accounts and roles in a virtualized/cloud environment
- Apply access policies to restrict resources based on role or identity
- Understand how IAM controls impact system security and operational workflow
- Practice configuring permissions in a repeatable, structured way

## **Tools & Environment:**
- Linux Mint Host (or your VM management environment)
- Windows 11 Pro VM
- Linux Mint VM
- Virtual Machine Manager (KVM/QEMU)
- Cloud IAM analogues simulated via local policies:
    - Linux user/group permissions
    - Windows Local Users & Groups / Local Security Policy
    - Optional: simulated JSON/YAML IAM policy files
- Commands & utilities:
    - Windows: secpol.msc, PowerShell (Get-LocalUser, Set-LocalUser)
    - Linux: usermod, groups, chmod, chown, /etc/sudoers
    - General: documentation of policy application

## **Steps:**
1. Prepare Linux and Windows VMS, ensuring administrative/root access is available for IAM and policy configuration.
    - Open PowerShell as administrator on Win 11 VM. For further verification use 'whoami /groups'
    - Open Terminal in Linux, run 'sudo -v' and input the admin password for access
2. Create two test users on both Linux and Windows VM's to simulate Identity and Access Management (IAM) accounts.
    - Linux VM:
        - 'sudo adduser iamuser1'
        - 'sudo adduser iamuser2'
        - Use password TreeFork88!
    - Win 11 VM:
        - 'New-LocalUser -Name "iamuser1" -Password (ConvertTo-SecureString "TreeFork88!" -AsPlainText -Force)'
        - 'New-LocalUser -Name "iamuser2" -Password (ConvertTo-SecureString "TreeFork88!" -AsPlainText -Force)'
        - Use password TreeFork88!
3. Define roles/groups and assign users to them to establish role-based access controls.
    - Linux VM:
        - 'sudo groupadd devs'
        - 'sudo groupadd admins'
        - 'sudo usermod -aG devs iamuser1'
        - 'sudo usermod -aG admins iamuser2'
    - Win VM:
        - Open Computer Management -> Local Users and Groups -> Groups
        - Create Devs group
        - Add iamuser1 to Devs and leave iamuser2 unassigned
4. Configure folder permissions to enforce least privilege, only assigned groups to have access.
    - Linux VM:
        - Create a shared directory: 'sudo mkdir /project'
        - Set ownership: 'sudo chown root:devs /project'
        - Set permissions: 'sudo chmod 770 /project' (only owners and group can read/write)
        - Verify: 'ls -ld /project'
    - Windows VM:
        - Create a basic shared folder using PowerShell: 'mkdir C:\Shared\Project' 
        - Right click it: Properties -> Security -> Advanced
        - Add group Devs (This gives modify permissions)
        - Remove Users from write access to simulate least privilege
        - Remove Authenticated users from write access to prevent alternate permission routes.
5. Verify access from each new user and confirm access to the shared folder matches assigned group permissions:
    - Linux VM:
        - 'su - iamuser1'
        - 'cd /project' This should succeed.
        - 'su - iamuser2'
        - 'cd /project' This should fail.
    - Win 11 VM:
        - Log in as iamuser1 -> Access project folder and create a text document, it should work.
        - Log in as iamuser2 -> Acces project folder and create a text document, it should fail.
6. Document all IAM policies, group memberships, and resource permissions to ensure compliance and reproducibility.
    - Linux VM:
        - 'groups username'
        - 'ls -l /project'
    - Win 11 VM:
        - In PowerShell: 'Get-LocalGroupMember -Group "Devs"
7. Clean up test users, groups and resources to reset the environment for future labs.
    - Linux VM:
        - 'sudo deluser iamuser1'
        - 'sudo deluser iamuser2'
        - 'sudo groupdel devs'
        - 'sudo groupdel admins'
        - 'sudo rm -rf /project'
    - Win 11 VM:
        - Delete users and groups through Local Users and Groups
        - Delete Shared folder

## **Notes:**
- Need to disable inheritance in Windows when you want to remove or restrict inherited permissions, otherwise permissions may not reflect what you expect or want.
- Even after removing a user from the Admin group, access may still be granted via other groups.  This is similar to cloud IAM where multiple policies can cumulatively grant access.
 
## **Issues & Fixes:**
- Issue: iamuser2 seemed to reatain write access to C:\Shared\Project even after removing from Admins.
    - Fix: Checked permissions and discovered Authenticated Users had Modify permissions. Changed Authenticated Users to Read & Execute only, then verified expected results, which were restricting write abilities.

## **Outcome:**
- Successfully created and managed users and roles in both Linux and Windows VMs.
- Configured folder permissions to enforce least privilege and role-based access control (RBAC).
- Verified that users could only access resources according to their assigned roles: iamuser1 (Dev) could access the shared project folder and write, while iamuser2 had access but no write permissions
- Documented IAM policies, group memberships, and resource permissions for repeatable lab practices.
- Gained hands-on understanding of how IAM concepts apply in both cloud-analogous environments and traditional OS-level permission management.
- RBAC simplifies permission management and reduces the risk of unauthorized access.
- Learned differences between Linux (user/group/permission bits) and Windows (ACLs) approaches to IAM.
