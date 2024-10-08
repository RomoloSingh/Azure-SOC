# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I set up a mini honeynet in Azure and collected log data from various sources into a Log Analytics workspace. Microsoft Sentinel then utilized this data to create attack maps, trigger alerts, and generate incidents. I first measured security metrics in the unsecured environment for 24 hours, implemented security controls to strengthen the environment, and then measured the metrics again for another 24 hours. The results, which are detailed below, include the following metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![Microsoft SQL Server (MSSQL) Auth Failures](https://i.imgur.com/BdObjli.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/9id3zaT.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/z8Hk7Id.png)<br>
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/Nogy9EP.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 2024-08-3 20:52:57

Stop Time 2024-08-4 20:52:57

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 29609
| Syslog                   | 4756
| SecurityAlert            | 2
| SecurityIncident         | 97
| AzureNetworkAnalytics_CL | 114

## Attack Maps Before Hardening / Security Controls

![Linux Syslog Auth Failures](https://i.imgur.com/dWhUgO2.png)<br>

```All other map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:

Start Time 2024-08-06 13:52

Stop Time	2024-08-07 13:52

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 16
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0
## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
