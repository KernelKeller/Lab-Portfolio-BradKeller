# Email Migration from Proton to Microsoft 365
**Date:** June 2, 2026 
**Author:** Brad Keller

## **Introduction:**
- Email migration is the process of moving a user's email data from one platform to another while maintaining access to historical messages, folders, and contacts. In this scenario, email data is migrated from Proton Mail to Microsoft 365. This type of migration may be required when a business adopts Microsoft 365 for centralized email, integration with Office applications, and improved collaboration features.

## **Core Components:**
- Proton Mail
    - Secure email provider focused on privacy and encryption
    - Supports IMAP access through Proton Mail Bridge for paid accounts
    - Stores email, folders, and contacts

- Microsoft 365
    - Cloud productivity platform from Microsoft
    - Exchange Online provides hosted email services
    - Supports Outlook desktop, Outlook Web App, and mobile access

- Outlook Desktop
    - Often used as an intermediary migration tool
    - Can connect to both Proton Mail (via Proton Bridge) and Microsoft 365 simultaneously
    - Allows mailbox contents to be copied between accounts

- Proton Mail Bridge
    - Desktop application that creates a local IMAP/SMTP connection for email clients
    - Required when accessing Proton Mail through Outlook or other mail clients
    - Available for paid Proton Mail plans

## **Microsoft 365 Configuration:**
- Before migration:
    - Create user account
    - Assign Exchange Online license
    - Verify mailbox provisioning
    - Confirm Outlook profile creation
    - Verify DNS records if domain migration is included
        - Common DNS records: MX, Autodiscover, SPF, DKIM, DMARC

## **Basic Migration Flow:**
- Pre-Migration Preparation
    - Verify Proton account credentials
    - Perform a full backup of the Proton Mail mailbox
    - Verify backup integrity and accessibility
    - Verify Microsoft 365 tenant is operational
    - Create Microsoft 365 user account in Microsoft Admin Centre
    - Assign Exchange Online license in Microsoft Admin Centre
    - Confirm mailbox can send and receive email
    - Install Proton Mail Bridge if required

- Migration Method 1: Outlook Drag-and-Drop Migration
    - Process:
        - Install Outlook desktop.
        - Configure Proton Mail through Proton Mail Bridge.
        - Configure Microsoft 365 account in Outlook.
        - Allow both mailboxes to synchronize.
        - Migration Initiation: Copy folders and email contents from Proton mailbox to Microsoft 365 mailbox.
        - Verify all mail appears within Outlook and Exchange Online.
        - Confirm mailbox accessibility through Outlook Web App.
    - Advantages:
        - Simple for small mailboxes
        - No specialized migration tools required
    - Disadvantages:
        - Time consuming for large mailboxes
        - Manual process

- Migration Method 2: PST Export and Import
    - This method can be used if client doesn't have a paid Proton Mail plan, making Proton Mail Bridge inaccessible 
    - Process:    
        - Connect Outlook to Proton Mail through Proton Mail Bridge.
        - Export Proton mailbox to PST file.
        - Configure Microsoft 365 mailbox.
        - Migration Initiation: Import PST file into Outlook connected to Microsoft 365
        - Verify folder structure and message counts.
    - Advantages:
        - Useful for archive migrations
        - Provides backup during migration
    - Disadvantages:
        - Additional export/import steps
        - PST corruption risks if files become very large

- Migration Method 3: Third-Party Migration Tools
    - This method can be used as last resort if method 1 or 2 doesn't work   
    - Examples: BitTitan MigrationWiz, CodeTwo, Kernel Migration Tools
    - Process:
        - Connect source Proton mailbox.
        - Connect destination Microsoft 365 tenant
        - Configure migration batch
        - Migration Initiation: Run migration
        - Validate migrated content
    - Advantages:
        - Scalable
        - Suitable for multiple users
    - Disadvantages:
        - Licensing costs
        - Additional configuration requirements

## **Validation and Testing:**
- After migration, test the following:
    - Confirm mailbox login
    - Verify message counts
    - Verify folder structure
    - Test sending email
    - Test receiving email
    - Verify calendar items (if migrated)
    - Verify contacts (if migrated)
    - Confirm mobile device access

## **Considerations:**
- Proton encryption may affect migration options
- Proton Bridge is typically required for Outlook-based migrations
- Large mailboxes may have a significant upload time
- Internet bandwidth can significantly impact migration time
- DNS cutovers should be planned to minimize downtime
- Maintain backups before migration begins
- Timing of migration needs consideration, typically done in business off hours
 
## **Common Issues:**
- Authentication Problems
    - Incorrect Proton credentials
    - Bridge not running
    - MFA configuration issues

- Missing Emails
    - Synchronization not complete
    - Outlook cached mode delays
    - Folder mapping issues

- Slow Migration
    - Large mailbox size
    - Upload bandwidth limitations
    - Microsoft 365 throttling

- Folder Structure Differences
    - Custom Proton folders may require manual verification after migration

- DNS Issues
    - Incorrect MX records
    - Delayed DNS propagation
    - Mail interruptions after cutover

## **Summary:**
- Migrating email from Proton Mail to Microsoft 365 can involve connecting Proton Mail through Proton Mail Bridge and transferring mailbox contents into Exchange Online using Outlook, PST exports, or third-party migration tools. Successful migrations require proper Microsoft 365 mailbox preparation, validation testing, DNS planning, and post-migration verification to ensure users retain access to all of their email data.
- For a 1-10 user small business migration, I would create the Microsoft 365 mailbox, assign licensing, connect Proton Mail to Outlook using Proton Bridge, copy mailbox contents into Exchange Online, verify message counts, cutover DNS, and perform send/receive testing.

## **References:**
The following resources were used as reference/research:
- ChatGPT & Google search. Various sites used for concept understanding and clarification.
- Install ProtonMail Bridge
    - https://www.youtube.com/watch?v=0C-kVNOXLlI
- Migrate ProtonMail to Office 365
    - https://www.youtube.com/watch?v=tleEN4e4r_U

