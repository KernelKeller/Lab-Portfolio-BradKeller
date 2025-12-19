# Ticket Lifecycle Simulation
**Date Created:** December 17, 2025  
**Last Updated:** December 18, 2025

## **Skills Demonstrated:**
- Creating, categorizing, and prioritizing IT support tickets
- Following ITIL-style ticket management workflows
- Documenting troubleshooting steps clearly and professionally
- Communicating with users through ticket updates
- Managing ticket statuses: New → In Progress → On Hold → Resolved → Closed

## **Objective:**
- To simulate the full life cycle of a helpdesk support ticket from creation to closure
- To practice documenting technical issues in a way real employers expect
- To build habits for clear communication, accountability, and follow through
- To demonstrate the ability to manage multiple tickets with varying priorities

## **Tools & Environment:**
- Linux Mint host running Virtual Machine Manager
- Windows 11 Pro VM
- Linux Mint VM

## **Steps:**
1. Power on Win11 VM and Linux Mint VM. Win 11 represents the end user and Linux represents the helpdesk technician environment.
2. Create three simulated support tickets with varying priorities (Low, Medium, High) to represent common helpdesk scenarios. Including account access issues, application failures, and network connectivity problems.
3. Review tickets and prioritize them based on business impact and urgency.
    - Ticket #001
        - Issue: User forgot log in password
        - Priority: Medium-High
        - Reason: No access to workstation is a complete production blocker, 
    - Ticket #002
        - Issue: Application wont launch
        - Priority: Medium
        - Reason: User still has access to the rest of the system, not a complete productivity blocker. Prio Medium in case the app is critical in order to work
    - Ticket #003
        - Issue: No network connection
        - Priority: High
        - Reason: This is almost a full productivity blocker but it could also affect multiple users. Consider this top prio. 
4. Set the high priority network ticket to "In Progress", verify loss of connectivity using ipconfig and ping, identify a disabled network adapter, restore connectivity, and confirm successful internet access before marking the ticket as Resolved.
    - Ticket status flow: New -> In Progress -> Resolved -> Closed
5. Address the Medium-High priority password access request by following account recovery procedures. Confirm successful user login, and close the ticket after user confirmation.
    - Ticket status flow: New -> In Progress -> Resolved -> Closed
6. Begin work on the medium priority application issue, review Windows Event Viewer application logs to identify errors. Place the ticket On Hold while awaiting a system restart, then confirm the application launched successfully. Mark the ticket as Resolved.
    - Ticket status flow: New -> In Progress -> On Hold -> Resolved -> Closed

## **Notes:**
- This lab focuses on ticket workflow, prioritization, and documentation rather than deep technical troubleshooting. It aims to reflect real-world helpdesk operations.
- Ticket activities were simulated and documented without a dedicated ticketing platform to focus on workflow and decision-making rather than tooling.
- Network issues were prioritized first due to immediate business impact and resolved quickly once the root cause was identified.
- This lab was designed to reinforce the importance of clear documentation, user communication, and structured workflows when managing multiple concurrent support requests.
 
## **Issues & Fixes:**
- Application failure was resolved after reviewing logs and performing a system restart

## **Outcome:**
- Simulated the full life cycle of multiple helpdesk tickets, demonstrating proper prioritization, troubleshooting, communication, and ticket closure.
- Managed three concurrent support tickets from intake to closure, balancing urgency and impact while maintaining clear and professional communication throughout the ticket life cycle.
