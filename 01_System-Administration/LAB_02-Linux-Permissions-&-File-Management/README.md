# Linux Permissions & File Management
**Date Created:** November 12, 2025  
**Last Updated:** November 12, 2025

## **Skills Demonstrated:**
- File and directory creation and navigation using Linux commands
- Understanding and modifying file permissions with chmod
- Managing file ownership with chown and chgrp
- Exploring default permission values (umask)
- Applying symbolic and octal permission notation
- Creating and managing groups and assigning group permissions
- Testing access and restrictions under different users
- Using sudo and understanding privilege escalation
- Documentation and verification of permission configurations

## **Objective:**
- Learn how Linux manages permissions and ownership for files and directories.
- Practice modifying read, write, and execute permissions using both symbolic and numeric modes.
- Understand user, group, and other permission categories.
- Test and verify how permissions affect file access between different users.
- Demonstrate knowledge of Linux file security and proper permission management.
- Document the commands, results, and troubleshooting process for repeatability.

## **Tools & Environment:**
- Operating System: Linux Mint 21.3 (Cinnamon Edition)
- Virtualization: VirtualBox VM (NAT networking enabled)
- User Accounts:
	- Root / sudo-enabled admin user
	- Two standard users for permission testing (e.g., user1, user2)
- Command-Line Tools:
	- ls, chmod, chown, chgrp, umask, touch, mkdir, cat, sudo
- Snapshot: Pre-lab clean state

## **Steps:**
1. Created two new standard user accounts: user1 and user2 using "sudo adduser"
2. Created /lab_permissions directory for testing file permissions.
3. Assigned ownership of the directory to user1 using "sudo chown user1:user1 /lab_permissions".
4. Logged in as user1 and created 3 test files: file1.txt, file2.txt, and script.sh.
5. Used "chmod +x script.sh" to add execute permission.
6. Used "chmod 600 file1.txt" to restrict access to the owner only.
7. Logged in as user2 and attempted to access user1’s files.
8. Created a shared group named "labgroup" and added user1 and user2.
9. Changed group ownership of /lab_permissions to labgroup.
10. Set directory permissions to 770 (rwx for owner and group, none for others).
11. Verified that both users in "labgroup" could read, write, and execute files in the shared directory.


## **Notes:**
- Linux automatically created home directories for each user under /home/.
- Each user owns their own home directory by default with 700 permissions. Can not access these directories unless through Admin or corresponding user account.
- After making user1 owner of lab_permissions, only user1 currently has full access. Others are restricted unless modified
- Noticed when checking permissions, Linux does not use file extensions to determine executability. Newly created .sh files are plain text and not executable by default.
- Must manually add execute permission using `chmod +x script.sh`.
- Alternatively, scripts can be run using `bash script.sh` even without execute permissions.
- New files are created with read/write but not execute permissions for safety.
- Got curious and checked Directories. Directories get execute permission because it’s required to enter or list contents.
- Noted chmod symbolic mode (+, -, =) modifies permissions directly.
- Noted numeric mode (e.g., 600) defines rwx for owner(6)/group(0)/others(0).
- Access to file1.txt was denied due to restrictive permissions (600).
- Access to script.sh succeeded.

 
## **Issues & Fixes:**
- Issue: Permission denied for file1.txt when accessed by user2.
	- Fix: Changed permissions with "chmod 644 file1.txt" to allow group/others read access.

## **Outcome:**
- Confirmed understanding of Linux permission structure.
- Practiced ability to set ownership, modify permissions symbolically and numerically, and verify access behavior.
- Ended up liking the Linux workflow even though it is relatively new to me at this point.

