# Incident Report: RDP Brute-Force Activity

## Incident Summary

Microsoft Sentinel generated an alert after detecting repeated failed
Windows authentication attempts against an Azure-hosted Windows virtual
machine.

The activity originated from an external IP address and targeted several
common local account names. Investigation of Windows Security Events found
more than 1,000 failed authentication attempts from the source during the
investigation period.

No successful Windows authentication from the investigated source IP was
identified in the reviewed telemetry.

---

## Incident Details

| Field | Details |
|---|---|
| Detection Source | Microsoft Sentinel |
| Data Source | Windows Security Events |
| Primary Event ID | 4625 - Failed Logon |
| Severity | Medium |
| Affected Asset | Azure Windows VM |
| Attack Type | Password Guessing / Brute Force |
| Status | Contained |

> The source IP has been omitted from the public report.

---

## Detection

A custom Microsoft Sentinel scheduled analytics rule monitored Windows
Security Event ID 4625.

The rule grouped failed authentication attempts by source IP and host and
generated an alert when the configured threshold was exceeded within the
detection window.

The analytics rule included entity mappings for:

- Source IP address
- Affected host

Microsoft Sentinel then generated an incident for investigation.

---

## Investigation

### 1. Authentication Analysis

Windows authentication events associated with the source IP were reviewed.

The investigation identified more than 1,000 Event ID 4625 failed
authentication events.

A separate search for Event ID 4624 was performed to determine whether the
same source IP successfully authenticated.

No Event ID 4624 successful authentication from the investigated source IP
was identified during the reviewed 24-hour period.

### 2. Targeted Accounts

The activity targeted several common local account names, including:

- Administrator
- admin
- user

The attempts were distributed almost evenly across the account names,
consistent with automated password-guessing behavior.

### 3. Attack Duration

Authentication attempts continued for approximately 4.8 hours.

The repeated and highly regular authentication attempts across multiple
common usernames were consistent with automated activity rather than
ordinary user authentication failures.

### 4. Authentication Failure Analysis

Windows reported the following authentication status information:

- Status: `0xC000006D`
- SubStatus: `0xC0000064`

These events indicated failed authentication attempts involving invalid
credentials/account names.

### 5. Scope Analysis

Additional SecurityEvent activity associated with the source IP was
reviewed.

Only Event ID 4625 authentication failures were identified in the reviewed
SecurityEvent telemetry. No successful authentication from the investigated
source IP was observed.

---

## Incident Assessment

The activity was assessed as a **true-positive brute-force/password-guessing
attempt**.

The available telemetry showed repeated unsuccessful authentication attempts
against multiple common local account names.

No evidence of successful authentication from the investigated source was
identified within the reviewed telemetry and investigation period.

---

## Containment

The source IP was blocked at the Azure network layer using an inbound
Network Security Group (NSG) rule.

The containment rule:

- Denied traffic from the identified source IP
- Targeted TCP port 3389 (RDP)
- Used a higher-priority deny rule than the existing RDP allow rule

This prevented additional RDP traffic from the identified source from
reaching the virtual machine.

---

## Recommendations

For a production environment, additional defensive measures would include:

- Avoid exposing RDP directly to the public internet
- Restrict management access to trusted source networks
- Use Azure Bastion or another secure administrative access method
- Enforce strong authentication and MFA where applicable
- Monitor repeated authentication failures
- Maintain SIEM detection and alerting for suspicious authentication activity
- Regularly review NSG and firewall rules

---

## Lessons Learned

This incident demonstrated the complete SOC incident-response workflow:

**Detection → Triage → Investigation → Scope Analysis → Containment → Verification**

The lab also demonstrated how Windows authentication telemetry, KQL,
Microsoft Sentinel analytics rules, entity mapping, and Azure network
controls can be combined to detect and respond to suspicious authentication
activity.
