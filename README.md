# SOC Lab: Multi-Layer Threat Detection System

## Overview
This repository contains a comprehensive Security Operations Center (SOC) lab demonstrating 
real-world threat detection across two layers:
- **Phase 1:** Endpoint Detection (Sysmon + Active Directory + Splunk)
- **Phase 2:** Network IDS (Suricata + Splunk correlation)


## Architecture
<img width="763" height="1029" alt="WhatsApp Image 2026-06-21 at 17 56 11" src="https://github.com/user-attachments/assets/d23f8a4c-53cb-4c05-98ed-f8e8f4fbe4a4" />


## Phases

### Phase 1: Endpoint Detection
- Sysmon telemetry collection (process, file, registry, network events)
- 6 SPL detection rules in Splunk
- 6 custom dashboards for visualization
- Coverage: Execution, Persistence, Privilege Escalation, Credential Access

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

[Full table in MITRE-ATT&CK-Mapping.xlsx]

## Features
✅ Production-grade SIEM (Splunk Enterprise)
✅ Real-time network monitoring (Suricata IDS)
✅ Windows Active Directory integration
✅ Evidence screenshots for all detections

## Acknowledgments
- MITRE ATT&CK Framework
- Atomic Red Team
- Splunk Community Edition
- Suricata IDS Project
