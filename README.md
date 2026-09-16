# Microsoft Sentinel RDP Brute-Force Detection & Incident Response Lab

## Project Overview

This project demonstrates an end-to-end Security Operations Center (SOC)
workflow using Microsoft Azure and Microsoft Sentinel.

I configured a Windows virtual machine to send security telemetry to Microsoft Sentinel, developed a custom KQL analytics rule to detect repeated 
failed authentication attempts, investigated a generated security incident, 
and contained the threat using an Azure Network Security Group (NSG).

The project demonstrates the workflow:

Detection → Alert → Incident → Investigation → Containment → Verification

## Technologies Used

- Microsoft Azure
- Microsoft Sentinel
- Microsoft Defender
- Azure Log Analytics
- Kusto Query Language (KQL)
- Windows Security Events
- Azure Network Security Groups (NSG)

## Skills Demonstrated

- SIEM monitoring
- Security log analysis
- KQL querying
- Detection engineering
- Brute-force detection
- Incident triage
- Incident investigation
- Windows authentication analysis
- Network-level containment
