# Building a SOC + Honeynet in Azure (Live Traffic)
![DALL·E 2025-01-09 18 28 17 - A simplified 'Cloud Honeynet + SOC' diagram with only essential text  The layout includes a central 'Log Analytics Workspace' icon  On the left, Azure](https://github.com/user-attachments/assets/2a8d4ad9-4ffe-4e1a-988a-d68a3465a23b)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![DALL·E 2025-01-09 18 13 47 - A clean and straightforward network architecture diagram based on the given image  The diagram features a large cloud labeled _Public Internet_ in bol](https://github.com/user-attachments/assets/1d315afd-6830-4c54-834c-ecee106a33cf)


## Architecture After Hardening / Security Controls
![DALL·E 2025-01-09 17 59 39 - A straightforward and minimal virtual network architecture diagram  A dashed-line box labeled _Subnet_ contains two rectangular icons labeled _VM_ for](https://github.com/user-attachments/assets/7098a509-4c2a-4993-aeb1-cfa246754437)



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
![linux-ssh-auth-fail](https://github.com/user-attachments/assets/d530691f-6776-4d2a-8934-69793c87b391)<br>
![nsg-malicious-allowed-in](https://github.com/user-attachments/assets/26219fc2-9c65-4235-ab91-ba32292d6e78)<br>
![windows-rdp-auth-fail](https://github.com/user-attachments/assets/ebb33c87-c68c-403e-8142-2a90793a4986)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2025-01-08:35:29
Stop Time 2025-01-09 08:54:31

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 43583
| Syslog                   | 6326
| SecurityAlert            | 3
| SecurityIncident         | 150
| AzureNetworkAnalytics_CL | 2510

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1151
| Syslog                   | 4
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
