# 🛡️ Investigation: PowerShell-Related Alert Validation

## 📌 Overview
This project documents the investigation of a **high-severity, multi-stage incident** flagged by Microsoft Defender involving suspected PowerShell activity and lateral movement.  
The goal was to validate the alert, identify affected assets, and determine whether the activity represented a real security threat or a benign administrative action.

---

## 🚨 Incident Details
- **Incident ID:** 22987
- **Severity:** High
- **Category:** Initial Access, Execution, Lateral Movement
- **Status at Time of Review:** Active
- **Detection Source:** Microsoft Defender for Endpoint

---

## 🖥️ Affected Asset
- **Device Name:** `sandboxvm`
- **Environment:** Azure-hosted virtual machine
- **User Context Observed:** SYSTEM / local administrative processes
- **Alert Timestamp Reviewed:** January 14, 2026 (15:25–15:50 UTC)
---


<img width="1542" height="859" alt="Screenshot 2026-01-15 at 2 20 48 PM" src="https://github.com/user-attachments/assets/f968cedc-5d25-4bc9-95f4-d4ec9bad1639" />


---

## 🔍 Investigation Methodology

### Data Source
- **Log Table:** `DeviceProcessEvents`
- **Purpose:** Identify process execution during the alert window and validate presence of suspicious tooling.

### Scope Applied
- Single affected endpoint (`sandboxvm`)
- Narrow time window aligned to alert generation
- Focus on execution context and parent-child process relationships
---

<img width="1562" height="321" alt="image" src="https://github.com/user-attachments/assets/ac4c701d-ed4f-4d0e-9583-deb857bcd9a5" />


---

## 📊 Key Findings

- No direct execution of:
  - `powershell.exe`
  - `pwsh.exe`
  - `cmd.exe`

- Observed process activity included:
  - `svchost.exe`
  - `rundll32.exe`
  - `backgroundTaskHost.exe`
  - `RuntimeBroker.exe`

- All activity executed under **SYSTEM-level context**
- No evidence of interactive or attacker-driven command execution

---

## 🧠 Analysis & Interpretation

Although the alert referenced PowerShell-related behavior, endpoint telemetry showed **no direct invocation of PowerShell binaries** during the relevant timeframe.

The observed activity is consistent with:
- Automated system tasks
- Administrative or orchestration-driven actions
- Cloud or extension-based operations

There was **no evidence of hands-on-keyboard attacker behavior**, credential abuse, or malicious payload execution.

---

## ✅ Incident Disposition

- **Classification:** Likely Administrative / Benign Activity
- **Risk Level After Validation:** Low
- **Action Taken:** Documented and monitored
- **Outcome:** No containment or remediation required

This investigation demonstrates the importance of validating alert context and behavior rather than relying solely on alert titles or severity labels.

---

## 🎯 Skills Demonstrated
- Alert triage and validation
- Endpoint process analysis
- Log scoping and noise reduction
- False positive identification
- Clear technical documentation

---

## 🧭 MITRE ATT&CK Context

Although the investigation did not confirm malicious activity, the observed behavior aligned with known adversary techniques in the MITRE ATT&CK framework.

- **T1059 – Command and Scripting Interpreter**  
  PowerShell-related behavior was detected through system-level processes. While no direct execution of `powershell.exe` was observed, the alert was triggered by behavior commonly associated with scripted command execution.

- **T1021 – Remote Services**  
  Activity patterns were consistent with remote administrative execution mechanisms. Investigation confirmed the activity originated from legitimate administrative or automated processes rather than an external threat actor.

MITRE ATT&CK was used as a **behavioral reference framework**, not as confirmation of malicious intent.


