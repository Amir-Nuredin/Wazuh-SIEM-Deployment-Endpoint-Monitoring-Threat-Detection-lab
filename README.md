# Wazuh SIEM Deployment Endpoint Monitoring Threat Detection lab

## Objective

This lab/project aims to develop foundational skills in Security Information and Event Management (SIEM), endpoint monitoring, log analysis, and threat detection using Wazuh. In this lab, I deployed a complete Wazuh SIEM environment on an Ubuntu Server virtual machine, including the Wazuh Manager, Indexer, and Dashboard, and configured Windows and Ubuntu endpoints with Wazuh agents to enable centralized log collection and real-time security monitoring. To gain hands-on experience with security monitoring, I simulated multiple attack scenarios by generating failed SSH authentication attempts, failed Windows logins, and Nmap reconnaissance scans from a Kali Linux attack machine. I analyzed the resulting security events within the Wazuh Dashboard and used its Threat Hunting capabilities to investigate logs by source IP, username, event type, and timeline. This project provided hands-on experience with Wazuh deployment, endpoint monitoring, centralized log management, attack simulation, threat hunting, and security event analysis while demonstrating how SIEM platforms help security teams detect and investigate suspicious activity across enterprise environments.

### Skills Learned

This project provided hands-on experience with the following SIEM, security monitoring, and threat detection skills:
- Wazuh SIEM deployment and configuration
- Security event collection and centralized log management
- Windows and Linux endpoint agent deployment
- Attack simulation using SSH brute-force attempts and Windows failed logins
- Security alert analysis and incident investigation
- SIEM-based monitoring and Security Operations Center (SOC) workflows

### Tools Used

- Ubuntu Server 24.04 LTS
- Wazuh Manager
- Wazuh Dashboard
- Wazuh Indexer
- Wazuh Agents (Windows & Ubuntu)
- VirtualBox
- Kali Linux
- Windows 10

## Current Lab Environment Architecture/Setup

Below are screenshots of my current lab Architecture (IM1). This week I added a new Ubuntu VM that would serve as a SIEM server for Wazuh and later Splunk. 

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/054871ec-7ded-4fbb-b281-09eb56fac2a2" />

IM1: Current Lab Environment/Architecture After this Week's Project

## Steps/Procedure

### Part 1: Building Wazuh/SIEM Server

The first step in this project was to actually build the VM that would be used for Wazuh and other SIEMs. First, I went to the Ubuntu Server Downloads page (https://ubuntu.com/download/server) and downloaded the 24.04 iso version of Ubuntu. I then went into VirtualBox and created the VM. I named the VM "Wazuh-SIEM." I gave it 4GB of RAM and 2 vCPUs. I also gave it 70 GB of storage as well. I gave it two network adapters, one NAT and one internal network, like the rest of my VMs. I then booted up the VM and went through the entire installation process, which took about 15 minutes. After installation was done, I logged in with the credentials I created, and it worked (IM2). One last thing I had to do was assign a static IP. This was important because, many times for VMs/machines that are used for servers or critical functions, it's a good idea not to use DHCP so the IP address doesn't change. 

<img width="531" height="529" alt="Screenshot 2026-07-06 085547" src="https://github.com/user-attachments/assets/fe152656-bd1c-4da1-93d3-714b7f4e8da3" />

IM2: Ubuntu SIEM Server Configured and Set Up

### Part 2: Initial Server Configuration

Next was to update the server and install any tools I needed. I used the command "sudo apt update" to check for any updates on the server, and "sudo apt upgrade -y" to automatically make all those updates. I then used the command "sudo apt install curl wget unzip net-tools htop -y" to install those five tools. To confirm they installed correctly, I checked the version of the tools, and they were all up to date (IM3). 

<img width="1282" height="736" alt="Screenshot 2026-07-06 085654" src="https://github.com/user-attachments/assets/5216fedc-6217-4638-b306-e29659af5cf5" />

IM3: Confirming Tools were Installed Correctly

### Part 3: Verifying Network Connectivity

Now that the SIEM VM was set up and updated, I wanted to ensure that it could access other VMs and the internet as well. I performed a couple of pings (IM4), one for google.com to confirm the VM had internet access and DNS was working, as well as pinging the VM itself to confirm internal connectivity, and they both worked. 

<img width="711" height="272" alt="Screenshot 2026-07-06 090013" src="https://github.com/user-attachments/assets/ae5a88fa-c60e-49ef-8fa9-89a7f26ac333" />

IM4: Using the Ping Command to confirm Network Connectivity

### Part 4: Installing Wazuh

Now I was ready to install Wazuh. I was installing the all-in-one deployment, which included the Wazuh Manager, Indexer, and Dashboard. I first ran the command "curl -sO https://packages.wazuh.com/4.13/wazuh-install.sh" to download the Installation Assistant. I then used the command "chmod +x wazuh-install.sh" to make that assistant executable. I then ran that executable with the command "sudo ./wazuh-install.sh -a." This started the installer and went through the entire installation process (IM5). This took about 15 minutes. Once it completed, at the end it told me how to access the Wazuh Dashboard (https://IP of VM:443) and gave me the login credentials as well. To confirm the installation worked, I ran the command "sudo systemctl status wazuh-manager" to check if the service was running, and it was (IM6). 

<img width="961" height="775" alt="Screenshot 2026-07-02 172309" src="https://github.com/user-attachments/assets/d9fd92bb-a5d9-4ab2-8999-c11fd27470ea" />

IM5: Installation Process of Wazuh 

<img width="850" height="370" alt="Screenshot 2026-07-06 090202" src="https://github.com/user-attachments/assets/649f029d-c23b-4566-87f1-776081885291" />

IM6: Confirming Wazuh Manager is up and running

### Part 5: Accessing Wazuh Dashboard

Now that the full setup was complete, I could access the Wazuh Dashboard. To do this, I went on my Windows 10 VM and put in the search bar "https://192.168.1.30:443." I was then asked for my login credentials; I put them in, and I was in the Wazuh dashboard (IM7). I explored around a little to see the different features in Wazuh, like Security Events, Agents, and Vulnerabilities. 

<img width="1890" height="822" alt="Screenshot 2026-07-06 092227" src="https://github.com/user-attachments/assets/ba75205b-80ea-457a-b621-27a8e64d8431" />

IM7: Wazuh Dashboard

### Part 6: Installing Windows Agent and Connecting it to Wazuh Manager

For Wazuh to get any logs/activity from the VMs, they needed to be connected to the manager. This is done by installing an agent onto the VM that you want Wazuh to get logs from. To do this, I clicked on the "Deploy new agent" button on the dashboard (IM8). Just a side note: before I go over the process of deploying the agent, I was trying to deploy this agent on the Windows Server VM, but accidentally named it "Windows10." Just wanted to point that out to avoid any confusion. It then took me to the Deployment Wizard. I put in information like the OS of the agent, the Server address (of the server hosting Wazuh, not of the agent I was configuring), and the name of the agent. At the bottom, it generated a command that would be used to install that agent onto the VM (IM9). I then went into PowerShell and typed in the command given. It ran for about a minute, and then I used the command "NET START Wazuh" To start the service. I then used the command "Get-Service WazuhSvc" to confirm the service was running (IM10). After doing all of this, I went back in Wazuh and saw my Windows agent, and it was active, confirming successful deployment (IM11). 

<img width="532" height="266" alt="Screenshot 2026-07-02 173242" src="https://github.com/user-attachments/assets/136bb007-a667-4c3c-a1fb-5a5dce363c77" />

IM8: Deploying a New Agent in Wazuh

<img width="1835" height="750" alt="Screenshot 2026-07-06 092439" src="https://github.com/user-attachments/assets/2924c82b-4323-48b1-94d3-a60e7816fcc6" />

IM9: Configuring Windows Agent in Wazuh

<img width="843" height="251" alt="Screenshot 2026-07-06 094007" src="https://github.com/user-attachments/assets/b5e72b1c-213c-4295-9aa3-01dc376166f9" />

IM10: Installing Wazuh Agent on Windows Server VM

<img width="1885" height="518" alt="Screenshot 2026-07-06 093639" src="https://github.com/user-attachments/assets/724c78e8-12a0-41fa-b6e1-5462537b0b15" />

IM11: Confirming Windows Agent is Active

### Part 7: Installing Ubuntu Agent and Connecting it to Wazuh Manager

Now I am going to do a similar process to install a Wazuh agent on my Ubuntu VM. I went into the agents section and again configured the OS of the agent, the server IP address, and gave the agent a name (Ubuntu-Endpoint). I then went into the Ubuntu VM and wrote the command given to install the agent onto the VM. Once that was complete, I used the commands "sudo systemctl enable wazuh-agent" and "sudo systemctl start wazuh-agent" to enable and start the service (IM12). I then went back to Wazuh and confirmed that the Ubuntu VM agent was active and configured, and it was (IM13). 

<img width="958" height="365" alt="Screenshot 2026-07-06 095602" src="https://github.com/user-attachments/assets/b286a894-5539-47f1-beda-f297f734f31a" />

IM12: Installing Wazuh Agent on Ubuntu VM and Starting the Service

<img width="1882" height="283" alt="Screenshot 2026-07-06 095752" src="https://github.com/user-attachments/assets/2e86c7de-448c-4f8e-b3a5-087d86886f1e" />

IM13: Confirming Ubuntu Agent is Active



































