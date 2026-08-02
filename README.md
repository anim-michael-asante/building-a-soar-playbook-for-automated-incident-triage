![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![License](https://img.shields.io/badge/License-MIT-blue)
![Course](https://img.shields.io/badge/Course-CY376-informational)
![Team](https://img.shields.io/badge/Team-Blue%20Team-2E8540)
![Platform](https://img.shields.io/badge/Platform-Splunk%20SOAR%20(Phantom)-black)

# Building a SOAR Playbook for Automated Incident Triage

## Table of Contents

1. [Overview](#overview)
2. [Scope and Objectives](#scope-and-objectives)
3. [Methodology](#methodology)
4. [Deliverable](#deliverable)
5. [Tools and Environment](#tools-and-environment)
6. [Repository Contents](#repository-contents)
7. [Lessons Learned](#lessons-learned)
8. [References](#references)
9. [Author](#author)

---

## Overview

This project designs a complete Security Orchestration, Automation, and Response (SOAR) playbook for automated phishing incident triage, built for Splunk SOAR (Phantom). Manual phishing triage does not scale at enterprise email volume and produces inconsistent analyst decisions. The playbook codifies enrichment, decision logic, containment, and post-incident review into an auditable, standards-referenced workflow, mapped to NIST SP 800-61 Revision 2 and the SANS Incident Handler's Handbook, with human-in-the-loop checkpoints for high-impact actions per NIST SP 800-53 Revision 5.

---

## Scope and Objectives

**In scope**
- Phishing delivered via email (MITRE ATT&CK T1566 and sub-techniques)
- Full incident lifecycle: trigger, enrichment, decision logic, containment, eradication, recovery, closure, post-incident review
- Splunk SOAR (Phantom) as the target automation platform

**Out of scope**
- Non-email initial access vectors (drive-by compromise, removable media)
- Incident types outside phishing
- Deployed Splunk SOAR instance or live integration credentials — this is a playbook design and specification deliverable, not an implemented automation

**Objectives**
- Design a phishing triage playbook mapped to NIST SP 800-61 Revision 2 and the SANS Incident Handler's Handbook
- Automate IOC extraction and reputation scoring
- Integrate threat intelligence enrichment (VirusTotal, AbuseIPDB, MISP)
- Define conditional decision logic with risk-based human-in-the-loop checkpoints per NIST SP 800-53 Revision 5, IR-4 and IR-5
- Specify containment, eradication, and recovery actions with rollback methods
- Define documentation, closure, and post-incident feedback loop procedures

---

## Methodology

The playbook was developed against the four-phase incident response lifecycle defined in NIST SP 800-61 Revision 2, Section 2.3, and cross-mapped to the six-phase model in the SANS Institute's Incident Handler's Handbook.

| NIST SP 800-61r2 (four-phase) | SANS IH Handbook (six-phase) |
|---|---|
| Preparation | Preparation |
| Detection & Analysis | Identification |
| Containment, Eradication & Recovery | Containment |
| | Eradication |
| | Recovery |
| Post-Incident Activity | Lessons Learned |

**Decision logic** consumes an aggregate confidence score (0-100) produced during automated enrichment:

| Confidence Score | Action |
|---|---|
| < 25 | Auto-close as false positive |
| 25-59 | Route to SOC Tier 1 analyst review |
| 60-84 | Auto-execute reversible containment; Tier 2 approval required for further action |
| >= 85 | Escalate to IR Lead; full containment set requires analyst approval |

High-impact actions (endpoint isolation, account disablement, org-wide domain blocking, or any action affecting a critical-tier asset) always require explicit analyst approval regardless of score, per NIST SP 800-53 Revision 5, IR-4. Evidence is preserved prior to any destructive containment action, per NIST SP 800-86.

---

## Deliverable

The primary deliverable is a NIST Special Publication-formatted report (LaTeX/PDF) specifying:

- Playbook metadata, trigger conditions, and asset scope
- Dual NIST/SANS lifecycle mapping with divergence analysis
- Automated enrichment logic (reputation scoring, threat intelligence lookups, sandbox detonation, historical correlation)
- Full if/then decision-tree logic for confidence-based branching
- Containment, eradication, and recovery actions with rollback methods
- Communication and escalation matrix with SLA timers per severity tier
- Documentation, closure, and metrics captured (MTTD, MTTR, false-positive rate)
- Post-incident activity and playbook feedback loop
- Design principles: idempotency, fail-safe defaults, auditability, modularity, version control
- Full reference list, cited by specific document and section throughout

Every framework-tied claim in the report cites the specific document and section (e.g., NIST SP 800-61r2 Section 3.2.6, NIST SP 800-53r5 IR-4/IR-5) rather than the framework generically.

---

## Tools and Environment

| Category | Tool / Standard |
|---|---|
| SOAR Platform (target) | Splunk SOAR (Phantom) |
| Threat Intelligence | VirusTotal, AbuseIPDB, MISP |
| Incident Handling Framework | NIST SP 800-61 Revision 2 |
| Security Controls Framework | NIST SP 800-53 Revision 5 |
| Threat Intel Sharing Framework | NIST SP 800-150 |
| Forensic Handling Framework | NIST SP 800-86 |
| Adversary Technique Mapping | MITRE ATT&CK for Enterprise |
| Incident Handling Reference | SANS Incident Handler's Handbook |
| Documentation | LaTeX (TikZ, tcolorbox, longtable) |

---

## Repository Contents

| File | Description |
|---|---|
| `Final_Report.pdf` | Final compiled project report (NIST SP-formatted) |
| `PlayBook- Building a SOAR Playbook for Automated Incident Triage.pdf` | Standalone SOAR playbook specification document |
| `README.md` | This file |

No live platform screenshots or execution logs are included, as this project is a design and specification deliverable rather than a deployed automation.

---

## Lessons Learned

- Dual-framework mapping (NIST four-phase, SANS six-phase) surfaces terminology gaps that a single-framework playbook would miss, particularly around where NIST's combined Containment/Eradication/Recovery phase splits into three distinct SANS phases.
- Fail-safe default design (routing incomplete enrichment to manual review rather than the highest-confidence branch) is a deliberate tradeoff against full automation, prioritizing defensibility over speed.
- Citing frameworks at the specific section level, rather than generically, is significantly more time-consuming to produce but materially strengthens the playbook's audit defensibility.

---

## References

- National Institute of Standards and Technology. *NIST Special Publication 800-61 Revision 2: Computer Security Incident Handling Guide*. U.S. Department of Commerce, 2012.
- National Institute of Standards and Technology. *NIST Special Publication 800-53 Revision 5: Security and Privacy Controls for Information Systems and Organizations*. U.S. Department of Commerce, 2020.
- National Institute of Standards and Technology. *NIST Special Publication 800-150: Guide to Cyber Threat Information Sharing*. U.S. Department of Commerce, 2016.
- National Institute of Standards and Technology. *NIST Special Publication 800-86: Guide to Integrating Forensic Techniques into Incident Response*. U.S. Department of Commerce, 2006.
- MITRE Corporation. *MITRE ATT&CK for Enterprise*. https://attack.mitre.org
- MITRE Corporation. *MITRE D3FEND Matrix*. https://d3fend.mitre.org
- SANS Institute. *Incident Handler's Handbook*. SANS Reading Room.

---

## Author

**Michael Asante Anim** | `0x1aerixis`
BSc Cyber Security -- University of Mines and Technology (UMaT), Tarkwa, Ghana
Index Number: FCM.41.018.057.23 | Course: CY376 -- Network Monitoring, Security and Auditing | Team: Blue Team

[![GitHub](https://img.shields.io/badge/GitHub-anim--michael--asante-black?logo=github)](https://github.com/anim-michael-asante)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://linkedin.com/in/michael-asante-anim)
[![TryHackMe](https://img.shields.io/badge/TryHackMe-0x1aerixis-red?logo=tryhackme)](https://tryhackme.com/p/0x1aerixis)
[![X](https://img.shields.io/badge/X-0x1aerixis-black?logo=x)](https://x.com/0x1aerixis)
[![Discord](https://img.shields.io/badge/Discord-0x1aerixis-5865F2?logo=discord)](https://discord.com/users/0x1aerixis)

---
> **Disclaimer:** This repository documents a SOAR playbook design and specification exercise produced for CY376 coursework. No live Splunk SOAR instance, integration credentials, or production systems are included. This project is intended for educational and portfolio purposes only.