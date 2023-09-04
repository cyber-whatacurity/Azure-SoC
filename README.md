# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](![image](https://github.com/cyber-whatacurity/Azure-SoC/assets/143567437/2e24a052-f6d0-4a55-84ef-bf1214993d08)

## Introduction

in this project, I constructed a compact honeynet within the Azure platform. I collected log data from diverse origins and directed it towards a Log Analytics workspace. Microsoft Sentinel then leveraged this data to construct attack visualizations, initiate alert notifications, and generate incident reports. Over a 24-hour period, I assessed security metrics in the initially vulnerable setting, implemented security measures to fortify the environment, conducted another 24-hour measurement phase, and present the subsequent outcomes. The metrics to be presented encompass:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](![image](https://github.com/cyber-whatacurity/Azure-SoC/assets/143567437/0e24ab8c-601e-45c8-bbf2-ca011f467610)

## Architecture After Hardening / Security Controls
![Architecture Diagram](![image](https://github.com/cyber-whatacurity/Azure-SoC/assets/143567437/04b8addf-2356-40d9-b5d4-bbf604d490e7)

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
![NSG Allowed Inbound Malicious Flows](![image](https://github.com/cyber-whatacurity/Azure-SoC/assets/143567437/5563b100-bdc6-4518-a242-94419d02b635)<br>
![Linux Syslog Auth Failures](![image](https://github.com/cyber-whatacurity/Azure-SoC/assets/143567437/9f8ed67f-cea0-4266-b882-f7e389afe5b3)<br>
![Windows RDP/SMB Auth Failures](![image](https://github.com/cyber-whatacurity/Azure-SoC/assets/143567437/40c433f6-0d63-4b85-83f9-0643746d1a03)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-08-15T00:40:17
Stop Time 2023-08-16T00:40:17

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 30843
| Syslog                   | 18870
| SecurityAlert            | 8
| SecurityIncident         | 377
| AzureNetworkAnalytics_CL | 5756

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 8/23/2023 20:04:30
Stop Time	8/24/2023 20:04:30

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9426
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, we established a small-scale honeynet within the Microsoft Azure framework and integrated log sources into a Log Analytics workspace. Microsoft Sentinel played a pivotal role in initiating alerts and generating incident reports based on the ingested log data. Furthermore, we conducted an initial assessment of metrics in the vulnerable environment before implementing security measures, followed by a subsequent measurement phase post-security enhancements. Notably, the implementation of these security controls led to a significant reduction in both security events and incidents, underscoring their efficacy.

It's important to mention that if the network's resources had been heavily utilized by regular users, it is conceivable that a greater number of security events and alerts could have been triggered during the 24-hour period following the implementation of security controls.
