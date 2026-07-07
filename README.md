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







































