# Building a Honeynet + SOC in Azure (Live Traffic)
<img width="3732" alt="Azure Architecture for Lab" src="https://github.com/user-attachments/assets/8ee6fa61-2c6b-4886-8526-0311d72bf2c2">

## Introduction

Here's what I did in this project:
- **Create a honeynet:** I started by setting up a decoy network within Azure and exposing it to the public internet. This enticed real hackers to attack my resources.
- **Build a cloud Security Operations Center (SOC):** Next, I built a monitoring and response center—a kind of watchtower where I can observe threat actors target my honeynet resources.
- **Trigger and respond to security incidents:** The honeynet generated security alerts. And I responded to them by following [NIST SP 800-61](https://csrc.nist.gov/pubs/sp/800/61/r2/final) guidelines.
- **Implement security controls:** As part of NIST’s guidelines, I bolstered my honeynet's security posture by implementing security controls.

I ran the insecure honeynet for 24 hours and collected metrics. Based on the incidents being investigated, I applied security controls to harden the environment. I waited another 24 hours and collected metrics for the hardened environment.

Here are the metrics I collected before and after implementing security controls:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
<img width="1766" alt="architecture before hardening" src="https://github.com/user-attachments/assets/d2d49933-9d42-4ee8-b917-684070e95b64">

## Architecture After Hardening / Security Controls
<img width="2302" alt="Architecture after hardening" src="https://github.com/user-attachments/assets/2517516e-ca89-4550-8542-f9225944f3d5">


The architecture of the honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Groups (NSGs)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel (SIEM)

Additional technologies, components and regulations used:

- Microsoft Defender for Cloud (MDC)
- NIST SP 800-53 Revision 5
- NIST SP 800-61 Revision 2
- Windows Event Viewer
- Kusto Query Language (KQL)
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management

Before hardening the honeynet, all the resources were deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open. And all other resources were deployed with public endpoints visible to the public internet.

After hardening the honeynet, the Network Security Groups blocked all traffic with the exception of my admin workstation. I added an NSG at the subnet level for additional network protection. I also enabled the built-in Windows firewall and implemented private endpoints for the other resources (Key Vault and Blob Storage Account).

## Attack Maps Before Hardening / Security Controls
### Failed Windows RDP Logon Attempts
![windows failed rdp map](https://github.com/user-attachments/assets/5fc91792-5264-49bb-9247-87a8caae6780)
### Failed Linux Logon Attempts
![linux syslog auth failures](https://github.com/user-attachments/assets/10a27811-21a7-481d-abe6-6a1c73e5b0e3)
### External Traffic the NSGs Allowed Into the Network
![nsg malicious allowed in](https://github.com/user-attachments/assets/56cf2a88-941c-4b9d-9dfb-c35b42a4c798)
### Microsoft SQL Server Logon Attempts
![MS SQL Maps](https://github.com/user-attachments/assets/1343ff54-7bef-4b6f-97c6-7632674faab7)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

**Start Time:** 8/26/2024, 2:21:24.593 PM PST

**Stop Time:** 8/27/2024, 2:21:24.593 PM PST

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 42754
| Syslog                   | 3636
| SecurityIncident         | 117
| AzureNetworkAnalytics_CL | 35805

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:

**Start Time:** 8/28/2024, 2:33:27.078 PM PST

**Stop Time:**	8/29/2024, 2:33:27.078 PM PST

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12159
| Syslog                   | 1
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Impact of Security Controls

| Metric                   | Percentage Change
| ------------------------ | -----
| SecurityEvent            | -71.56%
| Syslog                   | -99.97%
| SecurityIncident         | -100%
| AzureNetworkAnalytics_CL | -100%

## Conclusion

I built a honeynet and cloud SOC in Azure cloud. This setup triggered security incidents, which I responded to by following NIST's guidelines and hardening my honeynet environment. This project lies at the heart of defensive cybersecurity, while also providing a solid introduction to Azure cloud. It sharpened my incident response skills and deepened my understanding of how threat actors operate.

## Read the In-Depth Article Breakdown
If you'd like to check out the in-depth project breakdown, check out my article:
How I Used Azure Cloud to Build a Honeynet, Detect Live Threats, and Respond to SOC Incidents
