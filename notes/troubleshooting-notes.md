# Investigation Notes

## Scenario

A Windows Service was intentionally created to simulate a persistence technique commonly used by attackers.

The objective was to verify whether Windows Event Logs, Sysmon, and Wazuh successfully detected the activity.

---

## Initial Verification

Verified:

- Sysmon service running
- Wazuh Agent connected

---

## Activity Performed

Created a Windows Service:

Service Name:

WazuhLabSvc

Binary:

C:\Windows\System32\notepad.exe

Startup Type:

Demand Start

---

## Detection

Windows generated:

Event ID 7045

"A service was installed in the system."

Sysmon generated:

Event ID 1

Process Creation

Process:

sc.exe

---

## Wazuh Investigation

Threat Hunting successfully detected:

- Service installation
- Service name
- Executable path
- User account
- Process responsible
- Timestamp
- Agent

---

## Evidence Collected

- Windows Event ID 7045
- Sysmon Event ID 1
- Service query results
- Wazuh Threat Hunting event
- Event Details
- Service deletion

---

## Indicators of Compromise (IOCs)

| Indicator | Value |
|-----------|-------|
| Service Name | WazuhLabSvc |
| Executable | notepad.exe |
| Tool Used | sc.exe |
| Windows Event | 7045 |
| Sysmon Event | 1 |

---

## MITRE ATT&CK

Technique:

T1543.003

Create or Modify System Process: Windows Service

---

## Severity

Medium

Windows Service creation is a common persistence mechanism used by attackers to maintain long-term access.

---

## Conclusion

The simulated persistence activity was successfully detected through Windows Event Logs and Sysmon, forwarded to Wazuh, investigated in Threat Hunting, and removed after validation.
