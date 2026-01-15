# 🛡️ Multi-Stage Incident Investigation (SOC Tier 2)

## 📌 Incident Overview

A high-severity security incident triggered by multiple correlated alerts involving authentication anomalies and privileged PowerShell execution within an Azure-hosted Windows virtual machine.  

The incident was escalated due to indicators commonly associated with initial access and potential lateral movement, requiring deeper investigation to determine whether the activity represented a true compromise or legitimate administrative behavior.

---

<img width="1542" height="859" alt="Screenshot 2026-01-15 at 2 20 48 PM" src="https://github.com/user-attachments/assets/a7c38377-6bd6-43b5-84b1-6c5580ce2463" />



---

## 🧭 Investigation Scope

- **Incident ID:** 22987  
- **Severity:** High  
- **Time Range Investigated:** January 14–15, 2026  
- **Environment:** Azure Windows VM (cloud lab)  
- **Tools Used:**  
  - Microsoft Defender for Endpoint  
  - Azure Activity Logs (Log Analytics)

---

## 🔎 Alert Prioritization

The incident contained hundreds of alerts across multiple categories.  
Rather than reviewing each alert individually, the investigation focused on high-signal indicators most likely to represent attacker behavior:

- Privileged PowerShell execution  
- Unusual failed sign-in attempts  
- File-based security test detection (EICAR)

These alerts were prioritized because they represented different stages of a potential attack chain, including access pressure, execution, and confirmation behavior.

---

## ⚙️ Execution Analysis (PowerShell Activity)

Analysis of the PowerShell alert revealed that execution was initiated by:

- **Process:** `RunCommandExtension.exe`  
- **User Context:** `NT AUTHORITY\SYSTEM`

Key findings:
- PowerShell was **not launched by an interactive user**
- Execution originated from an **Azure VM extension**
- Commands ran with **SYSTEM-level privileges**

This confirmed that the activity was initiated through Azure management tooling rather than direct user access such as RDP.

![PowerShell Execution](screenshots/powershell-runcommand.png)

---

## ☁️ Cloud Control-Plane Correlation

To identify the origin of the execution, Azure Activity Logs were reviewed for Microsoft.Compute operations targeting the affected virtual machine.

Findings included:
- Compute-related activity within the same timeframe as the PowerShell execution
- Behavior consistent with **Azure-initiated administrative actions**
- No direct one-to-one mapping between endpoint telemetry and a single AzureActivity event

Correlation was established using timing, resource scope, and execution context, which is expected in cloud environments where control-plane and endpoint telemetry are decoupled.

![Azure Activity Logs](screenshots/azure-activity.png)

---

## 🧠 Analyst Assessment

Based on the available evidence, the activity was assessed as **administrative rather than malicious**.

Supporting factors:
- Execution originated from Azure management-plane tooling  
- No persistence mechanisms were identified  
- No malicious payloads or attacker tooling were observed  
- No confirmed lateral movement occurred  
- No follow-on suspicious activity was detected  

While the behavior triggered valid security alerts due to its elevated privileges and execution method, there was insufficient evidence to confirm a security breach.

---

## 🏷️ Final Classification

**Suspicious administrative activity — no confirmed compromise**

The alerts warranted investigation, but the evidence supported expected administrative behavior within a cloud lab environment.

---

## 📈 Key Lessons Learned

- Elevated administrative actions can closely resemble attacker tradecraft  
- Cloud investigations require correlating control-plane and endpoint telemetry  
- Not all high-severity alerts result in confirmed incidents  
- Knowing when to close an investigation is a critical SOC analyst skill  

---

## 🔧 Recommendations

- Improve correlation between Azure control-plane logs and endpoint telemetry  
- Ensure Azure Activity Logs are consistently ingested and retained  
- Reduce alert noise for known administrative workflows where appropriate  

---

## 🧩 Framework Reference

This investigation aligns with the following MITRE ATT&CK tactics:

- **Initial Access** – Authentication anomalies  
- **Execution** – PowerShell execution via management tooling  

Frameworks were used to guide analysis, not to force classification.

---

## ✅ Key Takeaway

This project demonstrates real-world SOC Tier 2 investigation skills, including alert triage, cloud-aware analysis, disciplined investigation methodology, and the ability to correctly distinguish between malicious activity and legitimate administrative operations.
