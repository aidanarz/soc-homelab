# Lab Setup Guide - Step by Step

## Prerequisites
- VirtualBox 6.1+
- 16GB RAM
- 100GB free disk space

## VMs to Create

### VM 1: Windows 10 (Endpoint)
```bash
OS: Windows 10 21H2
RAM: 4GB
Storage: 30GB
Network: Bridged

# After install:
1. Install Sysmon
2. Install Splunk Universal Forwarder
3. Install Atomic Red Team
```

### VM 2: Ubuntu 22.04 (SIEM)
```bash
RAM: 8GB
Storage: 50GB
Network: Bridged

# After install:
1. Install Splunk Enterprise
2. Install Suricata
3. Configure log forwarding
```

### VM 3: Windows Server 2022 (AD)
```bash
RAM: 2GB
Storage: 20GB
Network: Bridged

# After install:
1. Setup Active Directory
2. Create domain: corp.local
3. Create 2 domain users
```

### VM 4: Kali Linux (Attacker)
```bash
RAM: 2GB
Storage: Existing Kali installation

# Tools needed:
- Nmap (for T1046)
- Hydra (for T1110)
- Rubeus (for T1558)
- Atomic Red Team tests
```

## Network Configuration
[Diagram showing IP ranges and connectivity]

All VMs on same internal network: 192.168.10.0/24
- Kali: 192.168.10.10
- Windows 10: 192.168.10.100
- Windows Server: 192.168.10.200
- Ubuntu: 192.168.10.50

## Step 1: Sysmon Installation (Windows 10)

```powershell
# Download Sysmon
Invoke-WebRequest -Uri "https://download.sysinternals.com/files/Sysmon.zip" -OutFile Sysmon.zip
Expand-Archive Sysmon.zip

# Copy provided config
Copy-Item sysmon-config.xml .\Sysmon\

# Install Sysmon
cd Sysmon
.\Sysmon64.exe -i sysmon-config.xml -accepteula
```

## Step 2: Splunk Forwarder (Windows 10)

```bash
[See /documentation/vm-setup/windows10-setup.md]
```

## Step 3: Splunk Enterprise (Ubuntu)

```bash
[See /documentation/vm-setup/ubuntu-setup.md]
```

## Step 4: Suricata Installation (Ubuntu)

```bash
[See /documentation/vm-setup/ubuntu-setup.md - Suricata section]
```

## Step 5: Import Detection Rules

### Sysmon Rules
1. Go to Splunk web UI: http://192.168.10.50:8000
2. Settings → Searches, Reports, and Alerts
3. Import: `/phase1-endpoint-detection/sysmon-rules/*.spl`

### Suricata Rules
```bash
sudo cp phase2-network-detection/suricata-rules/local.rules /etc/suricata/rules/
sudo systemctl restart suricata
```

## Step 6: Test Detection
```bash
# From Kali, run basic test
nmap -sS -p 1-100 192.168.10.100

# Check Splunk for alerts
# Check Suricata for alerts
```

## Verification Checklist
- [ ] Sysmon running on Windows 10
- [ ] Splunk receiving Sysmon events
- [ ] Suricata running on Ubuntu
- [ ] Splunk receiving EVE JSON from Suricata
- [ ] First nmap test generates alerts in both sources

See troubleshooting in TROUBLESHOOTING.md if issues arise.