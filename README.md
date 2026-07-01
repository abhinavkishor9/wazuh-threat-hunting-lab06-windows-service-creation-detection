# wazuh-threat-hunting-lab06-windows-service-creation-detection
## Overview

This lab demonstrates how attackers can establish persistence by creating Windows Services and how this activity can be detected using Windows Event Logs, Sysmon, and Wazuh Threat Hunting.

A test service was created using the Windows `sc.exe` utility. Sysmon captured the process execution, Windows generated Event ID 7045 indicating a new service installation, and Wazuh ingested the event for investigation.

---

## Objectives

- Verify Sysmon is running
- Verify Wazuh Agent connectivity
- Create a Windows Service
- Detect the service creation
- Investigate the event in Wazuh
- Review Event ID 7045
- Remove the service after testing

---

## Lab Environment

- Windows 11
- Sysmon
- Wazuh Agent
- Wazuh Manager
- Wazuh Dashboard

---

## MITRE ATT&CK

**Technique**

T1543.003

**Name**

Create or Modify System Process: Windows Service

---

## Attack Simulation

A Windows Service was created using:

```cmd
sc create WazuhLabSvc binPath= "C:\Windows\System32\notepad.exe" start= demand
```

The service was verified using:

```cmd
sc query WazuhLabSvc
```

---

## Detection Workflow

1. Verify Sysmon service
2. Verify Wazuh Agent
3. Create Windows Service
4. Windows logs Event ID 7045
5. Sysmon logs Process Creation (Event ID 1)
6. Wazuh ingests the logs
7. Investigate in Threat Hunting
8. Remove the service

---

## Commands Used

### Verify Sysmon

```powershell
Get-Service Sysmon64
```

### Verify Wazuh

```powershell
Get-Service WazuhSvc
```

### Create Service

```cmd
sc create WazuhLabSvc binPath= "C:\Windows\System32\notepad.exe" start= demand
```

### Verify Service

```cmd
sc query WazuhLabSvc
```

### View Windows Event ID 7045

```powershell
Get-WinEvent -LogName System -MaxEvents 50 | Where-Object {$_.Id -eq 7045}
```

### View Sysmon Process Creation

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -FilterXPath "*[System[(EventID=1)]]" -MaxEvents 30 | Where-Object {$_.Message -match "sc.exe"}
```

### Remove Service

```cmd
sc delete WazuhLabSvc
```

---

## Detection Results

- Windows Service created successfully
- Windows Event ID 7045 generated
- Sysmon captured service creation process
- Wazuh detected service installation
- Event investigated in Threat Hunting
- Service removed successfully

---

## Indicators Observed

- Service Name
- Service Path
- Process Name
- User Account
- Event ID 7045
- Sysmon Event ID 1
- Timestamp
- Agent Name

---

## Skills Practiced

- Windows Service Analysis
- Persistence Detection
- Windows Event Logs
- Sysmon Investigation
- Wazuh Threat Hunting
- IOC Collection
- MITRE ATT&CK Mapping

---

## MITRE ATT&CK Mapping

| Technique | Description |
|-----------|-------------|
| T1543.003 | Windows Service Persistence |

---

## Outcome

Successfully simulated Windows Service persistence, validated Windows Event ID 7045 generation, correlated Sysmon telemetry, and investigated the activity in Wazuh Threat Hunting.
