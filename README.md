# SOC Lab: Multi-Layer Threat Detection System

## Overview
This repository contains a comprehensive Security Operations Center (SOC) lab demonstrating 
real-world threat detection across two layers:
- **Phase 1:** Endpoint Detection (Sysmon + Splunk)
- **Phase 2:** Network IDS (Suricata + Splunk correlation)

## Key Metrics
- **Detection Rules Created:** 21 (14 Sysmon + 7 Suricata)
- **MITRE ATT&CK Techniques Detected:** 14 techniques across 6 phases
- **Attack Simulations:** 14+ real scenarios with 100% detection rate
- **Lab VMs:** 4 (Windows 10, Windows Server 2022 AD, Ubuntu 22.04, Kali Linux)


## Architecture
[Embed simple ASCII diagram or image]

## Phases

### Phase 1: Endpoint Detection
- Sysmon telemetry collection (process, file, registry, network events)
- 6 SPL detection rules in Splunk
- 8 custom dashboards for visualization
- Coverage: Reconnaissance, Execution, Persistence, Privilege Escalation, Credential Access, Lateral Movement

### Phase 2: Network IDS
- Suricata network-based detection
- 7 custom detection rules with MITRE ATT&CK mapping
- Cross-source correlation (Suricata + Sysmon events)
- Real-time alert generation and investigation workflows

## MITRE ATT&CK Coverage
| Technique | ID | Detection Method | Dashboard |
|-----------|----|--------------------|-----------|
| Network Service Discovery | T1046 | Suricata (nmap SYN) | Port Scan Monitor |
| PowerShell | T1059.001 | Sysmon + Suricata | Process Execution Timeline |
| ... | ... | ... | ... |

[Full table in MITRE-MAPPING.md]

## Features
✅ Production-grade SIEM (Splunk Enterprise)
✅ Real-time network monitoring (Suricata IDS)
✅ Windows Active Directory integration
✅ 21 custom detection rules
✅ Automated attack simulations
✅ Complete documentation + runbooks
✅ Evidence screenshots for all detections

## Lab Requirements
- Virtualization: VirtualBox / Hyper-V
- Storage: ~100GB free space
- RAM: 16GB+ recommended
- Network: Internal bridged network

## Files
- `SETUP.md` - Step-by-step lab setup
- `ARCHITECTURE.md` - Detailed system design
- `MITRE-MAPPING.md` - Rule to technique mapping
- `/phase1-endpoint-detection/` - Sysmon rules + dashboards
- `/phase2-network-detection/` - Suricata rules + config
- `/documentation/` - Comprehensive guides

## Acknowledgments
- MITRE ATT&CK Framework
- Atomic Red Team
- Splunk Community Edition
- Suricata IDS Project