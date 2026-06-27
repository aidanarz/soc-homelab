# SOC HomeLab

## Overview
This repository contains a comprehensive Security Operations Center (SOC) lab demonstrating 
real-world threat detection across two layers:
- **Phase 1:** Endpoint Detection (Sysmon + Active Directory + Splunk)
- **Phase 2:** Network IDS (Suricata + Splunk correlation)


## Architecture
<img width="763" height="959" alt="image" src="https://github.com/user-attachments/assets/1a08ac6e-db49-4d83-8a40-660aebdf336d" />



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
| Technique ID | Technique Name | Phase | Detection Type | Category |
|---|---|---|---|---|
| T1003.003 | NTDS Dump | 1 | Sysmon Process | Credential Access |
| T1046 | Network Service Discovery | 1, 2 | Sysmon + Suricata | Discovery/Reconnaissance |
| ... | ... | ... | ... | ... |

[Full table in MITRE-ATT&CK-Mapping.md]

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
