# Ubiquiti Deployment and UniFi Overview
**Date:** June 1, 2026 
**Author:** Brad Keller

## **Introduction:**
- This document summarizes my understanding of deploying and managing Ubiquiti UniFi networking equipment in an MSP environment serving multiple clients. The focus is on basic deployment flow, cloud management, and operational considerations.

## **Core Components:**
- UniFi Cloud Gateway: Handles routing, firewall, DHCP, and central network management.
- UniFi Dream Router: All in one gateway, switch, and access point for small deployments.
- UniFi Switches: Provide wired network distribution and VLAN segmentation support.
- UniFi Access Points: Provide wireless coverage and SSID broadcasting.
- UniFi Portal: Cloud based management layer for monitoring and administering multiple sites.

## **Basic Deployment Flow:**
- Physical Setup:
    - Connect ISP modem to UniFi Gateway WAN port
    - Connect switches and access points to UniFi Gateway as needed
    - Power on devices and verify link connectivity

- Initial Configuration:
    - Access UniFi setup interface
    - Configure basic internet connectivity 
    - Create LAN network and DHCP settings
    - Adopt devices into the UniFi Network

- Wireless Setup:
    - Create SSIDs like "main" or "guest" etc.
    - Apply security settings (WPA2/WPA3)
    - Assign SSIDs to appropriate network segments

## **UniFi Cloud / Portal Management:**
- Devices are managed through the UniFi Portal or Network Controller
- Customers/Clients are separated into Sites
- Remote access allows monitoring, configuration, and updates
- Devices can be adopted remotely once connected to the internet

## **Considerations:**
- Firmware updates should be controlled to avoid service disruption
- Configuration backups are important for recovery scenarios
- Multi-site management allows isolation between clients
- Role-based access helps limit technician permissions
 
## **Common Issues:**
- Device adoption failures can be network or connectivity related
- WAN connectivity issues from ISP/modem misconfigurations
- WiFi instability can be caused by interference or AP placement
- Cloud sync delays or offline reporting mismatches
- Potential Double NAT issues from not switching ISP modem/router to bridged

## **Summary:**
- This overview aimed to demonstrate a foundational understanding of UniFi deployment and management within an MSP environment, including basic setup flow, cloud management structure, and operational considerations. All things considered, I now want a Dream Router in my house.

## **References:**
The following resources were used to help my understanding of UniFi deployment and MSP style management workflows:

- ChatGPT & Google search. Various sites used for concept validation and clarification.
- UniFi Dream Router setup (small business context)  
    - https://www.youtube.com/watch?v=EUh2522iGHk
- UniFi Cloud Gateway Ultra setup walkthrough  
    - https://www.youtube.com/watch?v=Vw3KmpMlWto
- UniFi controller multi-site management explaination 
    - https://www.youtube.com/watch?v=SHKUXMBbRpU
- UniFi small business network setup (VLANs, guest WiFi concepts)  
    - https://www.youtube.com/watch?v=cgLr9VZu_Zg
- UniFi firewall rules basics in an MSP context  
    - https://www.youtube.com/watch?v=v0B2IDEfnjA
