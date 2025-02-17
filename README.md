
# Setup Wazuh Open Source SIEM

## Table of Contents
- [Prerequisites](#prerequisites)
- [Network Topology](#network-topology)
- [Wazuh Overview](#wazuh-overview)
  - [Overview](#overview)
  - [Wazuh Agent Deployment](#wazuh-agent-deployment)
  - [Security Implications](#security-implications)
- [Setup Wazuh Indexer + Server](#setup-wazuh-indexer--server)
  - [Step 1](#step-1)
  - [Step 2](#step-2)
- [Deploy Wazuh Agents](#deploy-wazuh-agents)
  - [Windows [project-x-win-client]](#windows-project-x-win-client)
  - [Domain Controller [project-x-dc]](#domain-controller-project-x-dc)
  - [Linux [project-x-linux-client]](#linux-project-x-linux-client)
- [Create Agent Groups](#create-agent-groups)
- [Onboard Custom Logs](#onboard-custom-logs)
  - [Windows Group](#windows-group)
  - [Linux Group](#linux-group)

## Prerequisites
- Virtualbox installed.
- Virtual Machine [project-x-security-svr] has been provisioned, configured, and fully set up.
- Virtual Machines Installed & Configured:
  - [project-x-win-client]
  - [project-x-linux-client]
  - [project-x-sec-work]
- Windows Server 2025 with Active Directory Domain Services (ADDS) configured and running in the background.

## Network Topology

## Wazuh Overview

### Overview
Wazuh is an open-source platform that provides extended detection response (XDR) and System Information and Event Management (SIEM) to protect cloud, container, and server workloads. Wazuh comes with an array of capabilities including log data analytics, intrusion and malware detection, file integrity monitoring, configuration assessment, vulnerability detection, and support for regulatory compliance.

- **Extended Detection Response (XDR)**: XDR is a defensive approach that integrates data and insights from multiple security layers. Data is collected and aggregated into a unified platform from data sources such as workstations, servers, cloud environments, and network traffic. XDR provides improved detection, investigation, and response to threats by centralizing all data to identify patterns, trends, and analyze malicious activity.

- **System Information and Event Management (SIEM)**: Refers to a system that combines log management, threat detection, and incident response to help organizations monitor and secure their IT environments. Wazuh acts as a SIEM solution by collecting and analyzing security data from multiple sources, detecting threats in real time, and facilitating efficient incident response.

Wazuh relies on an agent-based ecosystem. Software agents are deployed to workstations, servers, containers, and virtual machines which send data to Wazuh’s server for processing, aggregation, and visualization of security-relevant information.

There are three main components that make up the Wazuh ecosystem:
- **Wazuh Indexer**: A highly scalable, full-text search and analytics engine. This central component indexes and stores alerts generated by the Wazuh server.
- **Wazuh Server**: Analyzes data received from the agents. It processes it through decoders and rules, using threat intelligence to look for well-known indicators of compromise (IOCs).
- **Wazuh Dashboard**: The web user interface for data visualization and analysis. It includes out-of-the-box dashboards for threat hunting, regulatory compliance, detected vulnerable applications, file integrity monitoring data, and more.

We will be using Wazuh as our central hub for security logging, analysis, defense, and remediation while we conduct cyber-attack/defend exercises.

### Wazuh Agent Deployment
In Wazuh, there are two primary ways to manage and configure agents: centralized configuration via agent.conf and local configuration on each agent via ossec.conf.

- **Centralized Configuration (agent.conf)**: Configuration changes and centralized management are applied to all agents via the Wazuh manager.
- **Local Configuration (ossec.conf)**: Allows individual agents to have unique configurations. This offers flexibility for agents with specific requirements.

### Security Implications
Running an XDR and SIEM service provides significant advantages for monitoring, detecting, preventing, and responding to security-related activity. Wazuh is free and provides a suite of capabilities for security monitoring.

Some key benefits:
1. **Threat Detection**: Wazuh correlates log data from various sources to detect malicious activities such as brute-force attacks, privilege escalation, and suspicious login attempts.
2. **Proactive Defense**: Wazuh acts as a host-based intrusion detection system (HIDS), monitoring file integrity, log integrity, and detecting unauthorized changes.
3. **Incident Response and Investigation**: Wazuh provides automated responses and forensic data collection to accelerate incident response.

## Setup Wazuh Indexer + Server

### Step 1
Sign into sec-user@secbox:

```
sudo su sec-user
```

Install cURL if it isn’t installed already:

```
sudo apt install curl
```

Start the Wazuh installation wizard:

```
curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i
```

Allow the Wazuh Indexer and Server to install, this may take a few minutes. 

Take note of the Admin password. This will be different for every installation.

### Step 2
Login into the Wazuh dashboard by entering https://localhost in a new browser tab. Use the credentials generated to log in.

## Deploy Wazuh Agents

### Windows [project-x-win-client]
**Method 1:**
1. Power on the [project-x-win-client] VM.
2. Install the Wazuh Windows Installer.
3. After installation, configure the Wazuh agent with the appropriate IP and key.
4. Start the Wazuh agent.

**Method 2:**
1. Use PowerShell to run a deployment command to install the Wazuh agent.

### Domain Controller [project-x-dc]
Repeat the steps for the Domain Controller.

### Linux [project-x-linux-client]
1. Install the Wazuh agent on the Linux client.
2. Start the Wazuh agent with systemctl.

## Create Agent Groups
Create groups for Windows and Linux agents in the Wazuh dashboard.

## Onboard Custom Logs

### Windows Group
Add custom log sources like the Windows Security and Application Event logs to the Wazuh configuration.

### Linux Group
Add custom log sources like /var/log/auth.log and /var/log/secure to the Wazuh configuration.
