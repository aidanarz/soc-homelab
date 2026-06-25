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
Network: NAT

# After install:
1. Install Sysmon
2. Install Splunk Universal Forwarder
3. Install Atomic Red Team
```

### VM 2: Ubuntu 22.04 (SIEM)
```bash
RAM: 8GB
Storage: 50GB
Network: NAT

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
2. Create domain: yourdomain.local
3. Create 2 or more domain users in different distribution groups
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

All VMs on same internal network: 192.168.10.0/24
- Kali: 192.168.110.133
- Windows 10: 192.168.110.100
- Windows Server: 192.168.110.132
- Ubuntu: 192.168.110.131

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
tba
```

## Step 3: Splunk Enterprise (Ubuntu)

```bash
tba
```

## Step 4: Suricata Installation (Ubuntu)

```bash
tba
```

## Step 5: Import Detection Rules

### Sysmon Rules
1. Go to Splunk web UI: http://192.168.110.131:8000
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
