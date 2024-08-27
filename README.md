

# Azure Honeynet & SOC Project: Cyber Attacks in Real Time

## Introduction

This project simulates a small-scale honeynet in Microsoft Azure, attracting real-world traffic from attackers worldwide. The objective is to demonstrate best security practices, incident response tactics, and the impact of hardening an environment. I deployed virtual machines exposed to the public internet to attract attackers, then I used Microsoft Sentinel to analyze incidents and create attack maps based on ingested logs. The metrics provided are based on a 24-hour observation period.

## Azure Components Utilized

 **Azure Virtual Network (VNet)**
 **Azure Network Security Group (NSG)**
 **Virtual Machines** (2x Windows, 1x Linux)
 **Log Analytics Workspace** with Kusto Query Language (KQL) Queries
 **Azure Key Vault**
 **Azure Storage Account**
 **Microsoft Sentinel**
 **Microsoft Defender for Cloud**

## Architecture Before Hardening

### Overview

In the initial stage, all resources were deployed allowing all inbound traffic from the internet in order to attract malicious actors.  This configuration was left unchanged for 24 hours to observe the generated metrics for log analysis and to generate our global attack map.

### Attack Maps Before Hardening

- **NSG Allowed Inbound Malicious Flows**  
  ![NSG Malicious Flows](path_to_image)

- **Linux Syslog Authentication Failures**  
  ![Linux Syslog Auth Failures](path_to_image)

- **Windows RDP/SMB Authentication Failures**  
  ![Windows RDP/SMB Failures](path_to_image)

- **MSSQL Authentication Failures**  
  ![MSSQL Auth Failures](path_to_image)

### Metrics Before Hardening

| Metric                          | Count |
|---------------------------------|-------|
| SecurityEvent (Windows VM)      | 4,358 |
| Syslog (Linux VM)               | 2,345 |
| SecurityAlert (Microsoft Defender for Cloud) | 6     |
| SecurityIncident (Sentinel Incidents)       | 73    |
| NSG Inbound Malicious Flows Allowed         | 103   |

## Architecture After Hardening

### Hardening Measures and Security Controls

Based on the incidents detected in the "BEFORE" stage, the following security enhancements were implemented:

- **Network Security Groups (NSGs):** Hardened by allowing only my public IP address; all other traffic is blocked.
- **Built-in Firewalls:** Configured to deny access from unauthorized users on the VMs.
- **Private Endpoints:** Replaced public endpoints with Private Endpoints for resources such as storage accounts and databases, limiting access to the virtual network only.

### Attack Maps After Hardening

All map queries returned no results due to the absence of malicious activity during the 24-hour period following the implementation of hardening measures.

### Metrics After Hardening

| Metric                          | Count |
|---------------------------------|-------|
| SecurityEvent (Windows VM)      | 2,364 |
| Syslog (Linux VM)               | 24    |
| SecurityAlert (Microsoft Defender for Cloud) | 0     |
| SecurityIncident (Sentinel Incidents)       | 0     |
| NSG Inbound Malicious Flows Allowed         | 0     |

## Incident Response Using NIST 800.61r2

For each simulated attack, I followed incident response practices based on the NIST SP 800-61r2 guidelines.

### Preparation

- Set up Azure lab to ingest logs into Log Analytics Workspace.
- Configured Sentinel and Defender, with alert rules in place.

### Detection & Analysis

- Detected malware on a workstation.
- Assigned alert to an owner, set severity to "High" and status to "Active".
- Identified affected systems and users.
- Conducted a full system scan with up-to-date antivirus software.
- Verified the alert as a "True Positive".
- Notified appropriate personnel as per organizational policies.

### Containment, Eradication & Recovery

- Quarantined the infected systems.
- Used antivirus solutions or clean system images to restore affected systems.

### Post-Incident Activity

- Analyzed the root cause, extent of damage, and effectiveness of the response.
- Disseminated reports to stakeholders.
- Implemented corrective actions and conducted a lessons-learned review.

## Conclusion

This project successfully demonstrates the construction of a honeynet in Microsoft Azure, the integration of log sources into a Log Analytics workspace, and the effectiveness of Microsoft Sentinel in generating alerts and incidents based on the ingested logs. The significant reduction in security events and incidents after applying security controls underscores their importance in protecting the environment.

