# Snapshot & Backup Lab
**Date Created:** January 30, 2026  
**Last Updated:** January 30, 2026

## **Skills Demonstrated:**
- Creating and managing VM snapshots
- Understanding backup strategies for virtualized environments
- Restoring systems to previous states using snapshots or backups
- Testing backup integrity and recovery procedures
- Basic disaster recovery awareness
- Documentation of backup policies and procedures

## **Objective:**
- Learn how to take and manage snapshots of virtual machines in Virtual Machine Manager
- Practice backing up and restoring VM states
- Understand the difference between snapshots and full backups
- Verify that snapshots and backups can be restored safely and reliably

## **Tools & Environment:**
- Linux Mint VM
- Virtual Machine Manager
- Backup/restore features of VMM:
    - Snapshots (full and incremental)
    - Export/Import VM
- File management commands for verifying backup integrity:
    - cp, rsync, tar, sha256sum
- Optional logging or documentation of backup events

## **Steps:**
1. Boot into the Linux VM and verify system state using basic system commands. Create a baseline marker file to identify the VM state prior to snapshot creation
    - Boot into Linux VM and log in
    - Open a terminal and run:
        - 'uname -a'
        - 'df -h'
    - Create a simple marker file:
        - 'echo "Baseline state before snapshot" > ~/baseline.txt
2. Create a powered off VM snapshot in Virtual Machine Manager labelled Pre_Change_Baseline_Snapshot to preserve a clean system state prior to making changes.
    - Shut down Linux VM
    - In Virtual Machine Manager, select the Linux VM and go to View -> Snapshots
    - At the bottom left of screen, select the + sign to "Create Snapshot"
    - Name it: "Pre_Change_Baseline_Snapshot"
    - Description: Clean baseline snapshot before configuration changes
    - Select Finish to create the snapshot
3. Boot back into the Linux VM after snapshot creation and intentionally modify the system by installing new software and changing files to simulate post-snapshot configuration drift.
    - Boot back into the Linux VM and open terminal to run the following commands:
        - 'sudo apt update'
        - 'sudo apt install -y stress'
        - echo "System modified after snapshot" >> ~/baseline.txt
        - Create junk data using: 'fallocate -l 500M ~/junkfile.bin'
4. Restore the VM to the previously created snapshot and vefiry that installed software and modified files were successfully reverted, confirming snapshot rollback functionality
    - Shut down Linux VM and go back to Snapshots
    - Right click the Pre_Change_Baseline_Snapshot and select "Start Snapshot". You will get a warning that changes since last snapshot was created will be discarded, proceed. That's what we want for this lab.
    - Boot up the Linux VM again and verify the roll back by running the following commands in Terminal:
        - 'cat ~/baseline.txt
        - 'which stress'
        - 'ls ~/junkfile.txt'
    - Expected results:
        - Stress -> NOT FOUND
        - junkfile.bin -> NOT FOUND
        - baseline.txt -> Original content only of "Baseline state before snapshot"
5. Understand that Snapshots do NOT equal back ups. Move onto next steps to explore that concept.
6. Export the Linux VM machine to a standalone disk image file using Virtual Machine Manager to create a full offline backup independent of snapshots.
    - Shut down Linux VM
    - Find the Linux VM's disk image location by going to the details page of the Linux VM, selecting VirtIO Disk 1 and copying the source path: 
    - Navigate to the location of the Linux VM's disk image: /var/lib/libvirt/images/linux2022.qcow2
    - Create a directory and copy the linux2022.qcow2 image to the new directory to store as a VM backup, using terminal:
        - 'sudo mkdir -p ~/vm_backups'
        - 'sudo cp /var/lib/libvirt/images/linux2022.qcow2 ~/vm_backups/'
    - Verify the integrity of the VM backup by generating and validating a SHA-256 checksum of the copied disk
        - Create a fingerprint: 'sha256sum ~/vm_backups/linux2022.qcow2 > ~/vm_backups/linux2022.qcow2.sha256'
        - Check fingerprint for changes in the future: 'sha256sum -c ~/vm_backups/linux2022.qcow2.sha256'
7. Simulate a disaster recovery scenario by deleting the original VM and restoring it from the exported backup image, confirming successful full VM recovery.
    - Go to Virtual Machine Manager
    - Select Create a new virtual machine
    - Select import existing disk image
    - Input the directory of the copied disk image from step 6 and follow the VM creation prompts.
    - Snoop around in the new VM for kicks!
8. Remove the /vm_backups directory + image files and the new VM in Virtual Machine Manager from the host machine.

## **Notes:**
- Creating a directory using sudo caused the directory to be owned by root, affecting the permissions. Can create the directory without using sudo instead.
 
## **Issues & Fixes:**
- Attempting to create a checksum finger print was getting blocked by permissions. 
    - Fix: change ownership from root to user.
        - 'sudo chown -R $USER:$USER ~/vm_backups'

## **Outcome:**
- Successfully demonstrated VM snapshot creation and rollback, full VM disk backup and restoration, and backup integrity verification within a Linux environment. Confirmed the limitations of snapshots compared to true backups, validated recovery procedures without impacting the original VM, and resolved real-world permission issues related to backup handling on a Linux host.
