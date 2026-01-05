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
Windows 10 Endpoint
   ├── Sysmon (Telemetry)
   ├── Wazuh Agent
   │       ├── Sysmon Logs
   │       ├── Windows Event Logs
   │       └── File Integrity Monitoring (FIM)
   └──→ Sends logs to Wazuh Manager → Dashboard (Security Events)

---

## ⚙️ Environment Setup

### **Windows 10 Endpoint**
- Installed Wazuh Agent v4.7.5  
- Installed Sysmon with SwiftOnSecurity config  
- Enabled File Integrity Monitoring (FIM)  
- Connected to Wazuh Manager at `10.0.0.193`

### **Wazuh Manager**
- Running on Linux  
- Receives logs from Windows agent  
- Parses Sysmon events  
- Displays alerts in the Wazuh Dashboard  

---

## 🔍 Sysmon Integration

### **Sysmon Installation**
Installed Sysmon using:


Using the **SwiftOnSecurity** configuration for high‑quality telemetry.

### **Wazuh Sysmon Configuration**

Added this block to `ossec.conf`:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>

Analyzing event log: 'Microsoft-Windows-Sysmon/Operational'

screeshot

FIM Configuration
Inside :<stscheck>

<directories realtime="yes">%USERPROFILE%\Desktop</directories>

validation
Agent log:
Real-time file integrity monitoring started.

FIM Test:
performed a 3-step test:

New-Item "$env:USERPROFILE\Desktop\fim-test.txt" -ItemType File
Set-Content "$env:USERPROFILE\Desktop\fim-test.txt" "test content"
Remove-Item "$env:USERPROFILE\Desktop\fim-test.txt"

Expected Alerts
• 	File created
• 	File modified
• 	File deleted
Screenshot Placeholders


🧠 Detection Use Cases Demonstrated
This homelab successfully detects:
• 	Process creation (Sysmon Event ID 1)
• 	Network connections (Event ID 3)
• 	Registry modifications (Event ID 13)
• 	File creation/modification/deletion (FIM)
• 	Startup folder persistence attempts
• 	Suspicious PowerShell activity
• 	Unauthorized file drops

📈 What I Learned
• 	How to configure Wazuh Agent on Windows
• 	How Sysmon enhances endpoint visibility
• 	How to debug XML configuration issues
• 	How to validate log ingestion end‑to‑end
• 	How to test FIM with real file operations
• 	How SIEMs correlate events from multiple sources