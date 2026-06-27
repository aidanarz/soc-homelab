# MITRE ATT&CK Mapping

## Phase 1: Sysmon-Based Detections

### T1003.003 - Credential Dumping: NTDS Dump
- **Category:** Credential Access
- **Description:** Extraction of password hashes from Active Directory NTDS.dit database
- **Detection Method:** Sysmon process monitoring
- **Detection Artifacts:**
  - Process: `vssadmin.exe`, `copy` commands targeting NTDS.dit
  - Parent Process: Suspicious PowerShell or cmd.exe execution
- **Query:** `ntds_exfiltration_alert.spl`

### T1046 - Network Service Discovery
- **Category:** Discovery
- **Description:** Enumeration of network services and open ports
- **Detection Method:** Network traffic analysis + Sysmon
- **Detection Artifacts:**
  - Process: Nmap, Nessus, or port scanning tools
  - Network: Multiple connection attempts across ports
- **Query:** `network_service_timeline.spl`

### T1059.001 - Command and Scripting Interpreter: PowerShell
- **Category:** Execution
- **Description:** PowerShell command execution for system administration or malicious purposes
- **Detection Method:** Sysmon event log (Event ID 1 - Process Creation)
- **Detection Artifacts:**
  - Process Name: `powershell.exe`
  - Command Line: Suspicious scripts or obfuscated commands
  - Parent Process: Explorer.exe, Outlook.exe (user-initiated)
- **Query:** `powershell_activity_timeline.spl`

### T1112 - Modify Registry
- **Category:** Defense Evasion
- **Description:** Modification of Windows registry for persistence or disabling security features
- **Detection Method:** Sysmon registry events
- **Detection Artifacts:**
  - Registry Paths: `HKLM\Software\Microsoft\Windows\`, `HKCU\Software\Microsoft\`
  - Registry Keys: Run, RunOnce, Services
  - Process: Suspicious processes modifying registry
- **Query:** `registry_persistence_map.spl`

### T1204.002 - User Execution: Malicious File
- **Category:** Execution
- **Description:** User interaction with malicious files (downloads, attachments)
- **Detection Method:** Sysmon file creation and execution monitoring
- **Detection Artifacts:**
  - File Extensions: .exe, .dll, .bat, .ps1, .scr
  - File Locations: Temp folders, Downloads, Desktop
  - Process Chains: Explorer → Malicious executable
- **Query:** `suspicious_file_creation.spl`

### T1547.001 - Boot or Logon Autostart Execution: Registry Run Keys
- **Category:** Persistence
- **Description:** Malware configured to execute at boot/logon via registry Run keys
- **Detection Method:** Sysmon registry event monitoring
- **Detection Artifacts:**
  - Registry Keys: 
    - `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
    - `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
    - `HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce`
  - Registry Values: Suspicious executable paths
- **Query:** `image_registry_map.spl`

---

## Phase 2: Network-Based Detections (Suricata)

### T1046 - Network Service Discovery
- **Category:** Discovery
- **Description:** Nmap SYN scans for network reconnaissance
- **Detection Method:** Suricata network IDS signatures
- **Detection Rule:** 
  ```
  alert tcp any any -> $HOME_NET any (msg:"CUSTOM Nmap SYN Scan Detected"; 
    flags:S, 12; flow:to_server; threshold:type both, track by_src, count 20, seconds 5; 
    sid:20260601; rev:2;)
  ```
- **SID:** 20260601

### T1110 - Brute Force
- **Category:** Credential Access
- **Description:** Multiple connection attempts to FTP service for credential guessing
- **Detection Method:** Suricata threshold-based detection
- **Detection Rule:**
  ```
  alert tcp any any -> $HOME_NET 21 (msg:"CUSTOM FTP Brute Force Attempt"; 
    flags:S; threshold:type both, track by_src, count 10, seconds 30; 
    classtype:attempted-admin; sid:20260704; rev:1;)
  ```
- **SID:** 20260704

### T1595.002 - Active Scanning: Vulnerability Scanning
- **Category:** Reconnaissance
- **Description:** HTTP scanning for vulnerabilities and admin panels
- **Detection Method:** Suricata HTTP inspection
- **Detection Rules:**
  - Oversized GET requests: SID 20260603
  - Admin panel scanning: SID 20260606
  - Uncommon HTTP methods: SID 20260608
  - Suspicious User-Agent: SID 20260609

### T1190 - Exploit Public-Facing Application
- **Category:** Initial Access
- **Description:** SQL injection and web application attacks
- **Detection Method:** Suricata HTTP payload analysis
- **Detection Rule:**
  ```
  alert http any any -> $HOME_NET any (msg:"CUSTOM SQL Injection Pattern Detected"; 
    flow:to_server,established; http.uri; content:"' OR "; nocase; 
    classtype:web-application-attack; sid:20260607; rev:1;)
  ```
- **SID:** 20260607

### T1021 - Remote Services (Implicit via Telnet)
- **Category:** Lateral Movement
- **Description:** Telnet connection attempts (legacy remote access protocol)
- **Detection Method:** Suricata port monitoring
- **Detection Rule:**
  ```
  alert tcp any any -> $HOME_NET 23 (msg:"CUSTOM Telnet Connection Attempt"; 
    flags:S; classtype:attempted-admin; sid:20260605; rev:1;)
  ```
- **SID:** 20260605

### T1071 - Application Layer Protocol: DNS
- **Category:** Command and Control / Exfiltration
- **Description:** DNS queries to suspicious/malicious domains
- **Detection Method:** Suricata DNS inspection
- **Detection Rule:**
  ```
  alert dns $HOME_NET any -> any 53 (msg:"CUSTOM Suspicious DNS Query - Test Domain"; 
    dns_query; content:"malware.test"; nocase; classtype:bad-unknown; sid:20260610; rev:1;)
  ```
- **SID:** 20260610

### T1498 - Network Denial of Service
- **Category:** Impact
- **Description:** ICMP ping flood attacks
- **Detection Method:** Suricata threshold-based ICMP monitoring
- **Detection Rule:**
  ```
  alert icmp any any -> $HOME_NET any (msg:"CUSTOM Ping Flood Detected"; 
    itype:8; threshold:type both, track by_src, count 20, seconds 10; 
    classtype:attempted-dos; sid:20260602; rev:1;)
  ```
- **SID:** 20260602

---

## Summary Matrix

| Technique ID | Technique Name | Phase | Detection Type | Category |
|---|---|---|---|---|
| T1003.003 | NTDS Dump | 1 | Sysmon Process | Credential Access |
| T1046 | Network Service Discovery | 1, 2 | Sysmon + Suricata | Discovery/Reconnaissance |
| T1059.001 | PowerShell Execution | 1 | Sysmon Process | Execution |
| T1071 | Application Layer Protocol | 2 | Suricata DNS | C&C/Exfiltration |
| T1110 | Brute Force | 2 | Suricata Threshold | Credential Access |
| T1112 | Modify Registry | 1 | Sysmon Registry | Defense Evasion |
| T1190 | Exploit Public-Facing Application | 2 | Suricata HTTP | Initial Access |
| T1204.002 | User Execution: Malicious File | 1 | Sysmon File | Execution |
| T1498 | Network Denial of Service | 2 | Suricata ICMP | Impact |
| T1547.001 | Registry Run Keys | 1 | Sysmon Registry | Persistence |
| T1595.002 | Vulnerability Scanning | 2 | Suricata HTTP | Reconnaissance |

---

## Detection Coverage by MITRE Framework

### Initial Access
- T1190 - Exploit Public-Facing Application (SQL Injection, Admin Panel Scanning)

### Execution
- T1059.001 - PowerShell Execution
- T1204.002 - User Execution: Malicious File

### Persistence
- T1547.001 - Registry Run Keys

### Defense Evasion
- T1112 - Modify Registry

### Credential Access
- T1003.003 - NTDS Dump
- T1110 - Brute Force (FTP)

### Discovery
- T1046 - Network Service Discovery

### Reconnaissance
- T1595.002 - Vulnerability Scanning

### Command and Control / Exfiltration
- T1071 - Application Layer Protocol (DNS)

### Impact
- T1498 - Network Denial of Service

---

## Notes
- **Phase 1** focuses on host-based detection using Sysmon event logs
- **Phase 2** extends coverage with network-based detection using Suricata IDS
- Combined detection enables multi-layer attack chain analysis
