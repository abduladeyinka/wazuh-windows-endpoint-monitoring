# 🛡️ Windows Endpoint Monitoring with Wazuh, Sysmon & File Integrity Monitoring (FIM)

![Wazuh](https://img.shields.io/badge/Wazuh-Endpoint_Monitoring-blue)
![Sysmon](https://img.shields.io/badge/Sysmon-Process_Logging-purple)
![Windows](https://img.shields.io/badge/Windows-10_Home-0078D6)
![FIM](https://img.shields.io/badge/FIM-Real_Time-green)
![Blue Team](https://img.shields.io/badge/Blue_Team-Detection_Engineering-red)

---

## 📌 Project Overview
This homelab demonstrates how to monitor a Windows 10 endpoint using:

- **Wazuh Agent** for log collection and security monitoring  
- **Sysmon** for detailed process, network, and registry telemetry  
- **File Integrity Monitoring (FIM)** for real‑time detection of file changes  
- **Wazuh Manager** as the SIEM backend  

This setup replicates how a SOC monitors Windows endpoints in real enterprise environments.

---

## 🧩 Architecture
                 ┌──────────────────────────────┐
                 │        Windows 10 Host        │
                 │  ───────────────────────────  │
                 │  Sysmon → Telemetry           │
                 │  Wazuh Agent → Logs           │
                 │  FIM → File Monitoring        │
                 └───────────────┬──────────────┘
                                 │
                                 ▼
                 ┌──────────────────────────────┐
                 │         Wazuh Manager         │
                 │  Parses + Correlates Logs     │
                 │  Displays Alerts in Dashboard │
                 └──────────────────────────────┘

---
## ⚙️ Environment Setup

### **Windows 10 Endpoint**
- Installed Wazuh Agent v4.7.5  
- Installed Sysmon with SwiftOnSecurity config  
- Enabled File Integrity Monitoring (FIM)  
- Connected to Wazuh Manager at `10.0.0.193`

### **Wazuh Manager**
- Receives logs from Windows agent  
- Parses Sysmon events  
- Displays alerts in the Wazuh Dashboard  

---

## 🔍 Sysmon Integration
Sysmon was installed with the SwiftOnSecurity configuration to provide high‑quality telemetry for:

## 🧠 Detection Use Cases Demonstrated
This homelab successfully detects:
- Process creation (Sysmon Event ID 1)
- Network connections (Event ID 3)
- Registry modifications (Event ID 13)
- File creation/modification/deletion (FIM)
- Startup folder persistence attempts
- Suspicious PowerShell activity
- Unauthorized file drops

### **Wazuh Sysmon configuration**
The following block was added to ossec.conf to ingest Sysmon logs

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

## 🔐 File Integrity Monitoring (FIM)

### **FIM Configuration**
Monitoring was enabled for the Desktop directory:

```xml
<directories realtime="yes">%USERPROFILE%\Desktop</directories>
```

Agent log confirmation:
Real-time file integrity monitoring started.

### **FIM Test Procedure**
A 3‑step test was performed:

```powershell
New-Item "$env:USERPROFILE\Desktop\fim-test.txt" -ItemType File
Set-Content "$env:USERPROFILE\Desktop\fim-test.txt" "test content"
Remove-Item "$env:USERPROFILE\Desktop\fim-test.txt"
```

### **Expected Alerts**
- File created  
- File modified  
- File deleted

## 📸 Screenshots

### 🔐 FIM Alerts  
Real-time detection of file creation, modification, and deletion.

![FIM Alerts](screenshots/fim-alerts-dashboard.png)

---

### 🛡️ Sysmon Event Logging  
Sysmon Event ID 1 (Process Create) captured and forwarded to Wazuh.

![Sysmon Event](screenshots/sysmon-event-process-create.png)

---

### 🖥️ Wazuh Agent Status  
Agent installed, active, and communicating with Wazuh Manager.

![Agent Status](screenshots/wazuh-agent-status-dashboard.png)

## 🧠 Detection Use Cases Demonstrated
This homelab successfully detects:
• 	Process creation (Sysmon Event ID 1)
• 	Network connections (Event ID 3)
• 	Registry modifications (Event ID 13)
• 	File creation/modification/deletion (FIM)
• 	Startup folder persistence attempts
• 	Suspicious PowerShell activity
• 	Unauthorized file drops

## 📈 What I Learned
• 	How to configure Wazuh Agent on Windows
• 	How Sysmon enhances endpoint visibility
• 	How to debug XML configuration issues
• 	How to validate log ingestion end‑to‑end
• 	How to test FIM with real file operations
• 	How SIEMs correlate events from multiple sources

## 🏁 Conclusion
This project demonstrates a complete Windows endpoint monitoring pipeline using Wazuh, Sysmon, and FIM. It replicates real SOC workflows and highlights practical detection engineering skills, including log collection, event analysis, and real‑time alerting.