# **Creating a Windows 11 VM**
**Date Created:** July 4, 2025  
**Last Updated:** July 8, 2025 

## **Objective:**
- Create a Windows 11 Virtual Machine using VirtualBox for the purpose of working on Labs.

## **Tools Used:**
- VirtualBox 7.1.10
- Windows 11 Enterprise 64 bit ISO

## **Steps:**
1. Check system specs to ensure system has capacity to run a Windows 11 VM
- 113gb Free Disk Space
- 8 GB RAM
- Intel Core i5-1135G7 (2.4 GHz)
- confirmed VM will require 4GB of ram and 65 GB of hard drive space
   
2. Download and install VirtualBox
- https://www.virtualbox.org/wiki/Downloads
   
3. Download Windows 11 64 bit ISO
- https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise

4. Create a new VM with the following settings
 - Name/Type: Microsoft Windows 11
-  RAM: 4096 MB (4 GB)
-  CPU: 2 cores
- Disk: 64 GB dynamic + 11 GB for a total space of 75 GB

5. Run New VM
- Press any key when VM window first loads up in order to access the optical disc to boot from ISO file
- Allow Windows to start installation
- Follow Windows setup prompts (see images)
6. Check internet accessiblity
- If no internet, verify that the VM network adapter is set to **NAT**

8. Take Snapshot of fresh install
- Name: Clean Install - July 2025
- Clean Install for easy rollback to clean slate when needed

## **Notes:**
- Evaluation version is valid for 90 days. I can be rearmed or reset by resroting the snapshot
- VM performance may be limited on systems with 8 GB RAM. Close other apps during usage to ensure smoother operation
 
## **Issues & Fixes:**
- Originally tried to install with 30 GB hard drive space and 3 GB of RAM. The installer would not complete.
	- **Fix:** Check Windows 11 specs online and match VM specs to it
- No internet on first boot
	- **Fix:** Switched VM network to **NAT**
