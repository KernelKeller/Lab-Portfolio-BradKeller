# Simple Web App Deployment
**Date Created:** January 31, 2026  
**Last Updated:** January 31, 2026

## **Skills Demonstrated:**
- Deploying a basic web application on a VM
- Configuring web servers
- Assigning network settings and ports for accessibility
- Testing connectivity from host and VM-to-VM
- Understanding virtual network isolation and routing
- Basic troubleshooting for application access issues

## **Objective:**
- Learn how to deploy and serve a simple web application on a VM
- Ensure the application is accessible from other VMs and/or host
- Practice configuration of network interfaces and firewall rules
- Test basic functionality and connectivity of deployed services

## **Tools & Environment:**
- Linux Mint Host
- Linux Mint VM (acting as web server)
- Windows 11 Pro VM (client/test machine)
- Virtual Machine Manager
- Web server options:
    - Python http.server module
- Networking utilities:
    - ping, curl, ss
- Firewall configuration tools:
    - ufw

## **Steps:**
1. Identify the Linux VM web server VM's IP address using ip a to confirm network connectivity within the virtual network. Then verify outbound network connectivity from the Linux VM by successfully pinging an external host.
    - On Linux VM, open a terminal and run 'ip a'
        - Look for something like inet 10.0.0.x: 10.0.0.45
    - Verify connectivity using 'ping -c 3 google.com'
2. Create a simple HTML web application (index.html) to serve as a test web page for deployment validation.
    - Create a project directory: 'mkdir ~/simple-web-app'
    - Navigate to the new web app directory: 'cd ~/simple-web-app'
    - Create an index.html file: 'nano index.html' and paste the following:

<!DOCTYPE html>
<html>
<head>
    <title>Simple Web App Lab</title>
</head>
<body>
    <h1>Simple Web App Deployment</h1>
    <p>If you can see this page, the web server is working.</p>
    <p>Served from Linux Mint VM.</p>
</body>
</html>

    - Save and exit.
3. Start a lightweight Python HTTP server on port 8000 to serve the web application from the Linux VM.
    - Using Pythons built in web server on port 8000, in Terminal: 'python3 -m http.server 8000'
        - We should see something like "Serving HTTP on 0.0.0.0 port 8000'
4. Verify local access to the web application by successfully loading the page from http://localhost:8000 on the Linux VM.
    - In google chrome or Firefox, go to http://localhost:8000
5. Access the web application from the Windows 11 VM using the Linux VM's IP address to confirm successful VM to VM connectivity.
    - Open Win 11 VM and access Edge or Chrome (Chrome is gooderer)
    - Navigate to http://10.0.0.45:8000, using the Linux VM IP address found in step 1.
    - If it loads, that confirms networking + service exposure.
6. Confirm the web service is actively listening on port 8000 using ss and validate HTTP responses using curl
    - In Linux VM terminal:
        - ss -tulnp | grep 8000
        - curl http://localhost:8000
7. End Web service and delete simple-web-app directory
    - ctrl + c in terminal hosting web app
    - Remove web app directory: 'rm -r ~/simple-web-app'
    - Remove ufw rule allowing 8000/tcp:
        - 'sudo ufw status numbered
        - 'sudo ufw delete <NUMBER>'

## **Notes:**
- May need to activate/allow port 8000/tcp in UFW firewall before other VM's can connect successfully.
 
## **Issues & Fixes:**
- Attempting to access the web service from Win 11 was timing out and failing. After troubleshooting I discovered that port 8000 was not allowed in Linux UFW
    - FIX: Create a rule allowing port 8000 through 'sudo ufw allow 8000/tcp'

## **Outcome:**
- Successfully deployed a simple web application on a Linux Mint VM and confirmed accessibility from both the host and a separate Windows VM, demonstrating basic web service deployment, networking, and troubleshooting skills.
