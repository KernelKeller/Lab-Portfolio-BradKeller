# Software Installation & Package Management
**Date Created:** November 16, 2025  
**Last Updated:** November 16, 2025 

## **Skills Demonstrated:**
- Installing, updating, and removing software packages on Linux using apt, dpkg
- Managing repositories and verifying package integrity
- Troubleshooting broken dependencies
- Comparing Linux package managers with Windows tools like Chocolatey or PowerShell’s winget
- Understanding how software is distributed, verified, and sandboxed

## **Objective:**
- Learn how to manage software installations efficiently using package managers
- Understand repository sources, digital signatures, and dependency management
- Practice installing tools from both package repositories and manual .deb installations
- Gain confidence in updating and maintaining software on Linux systems

## **Tools & Environment:**
- Operating System: Linux Mint
- Commands Used:
	- sudo apt update && sudo apt upgrade
	- sudo apt install
	- sudo apt remove
	- sudo apt autoremove
	- dpkg -i
	- apt-cache search
- Tools Installed During Lab:
	- htop (system monitor, installed and uninstalled)
	- nmap (network scanner)
	- curl (data transfer utility)
	- git
	- tree
	- google chrome (.deb manual install)

## **Steps:**
1. Updated local package index using "sudo apt update" and applied system upgrades with "sudo apt upgrade -y."
2. Searched for available packages using apt-cache search to discover tools related to system monitoring and network scanning.
3. Installed three different tools (htop, nmap, and curl) using sudo apt install and verified installation by checking their versions.
4. Removed a package using sudo apt remove and cleaned unused dependencies with sudo apt autoremove.
5. Installed git and tree system tools using apt install to demonstrate package installation and management.
6. Installed a .deb file manually using dpkg -i and resolved dependencies with apt --fix-broken install.
7. Verified software installed across different package systems using apt list --installed


## **Notes:**
- dpkg installs do not resolve dependencies automatically, unlike apt.
 
## **Issues & Fixes:**
- Issue: dpkg returned "No such file or directory" when installing chrome.
	- Fix: Downloaded the .deb file using the default browser, then reran dpkg from ~/Downloads.
- Issue: Large system updates took hours and stopped when the VM slept.
	- Fix: Adjusted VM power settings to avoid sleep; reran sudo apt update && sudo apt upgrade.

## **Outcome:**
- Successfully practiced installing, removing, and verifying packages with apt.
- Installed a .deb package manually using dpkg and resolved dependency issues.
- Gained confidence navigating Linux directories and managing software.

