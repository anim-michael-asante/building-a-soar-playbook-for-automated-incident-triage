# building-a-soar-playbook-for-automated-incident-triage

```markdown
# Building a SOAR Playbook for Automated Incident Triage

A NIST SP 800-61r2 and SANS Incident Handler's Handbook-aligned SOAR playbook for automated phishing incident triage, built for Splunk SOAR (Phantom).

---

## Author

Michael Asante Anim
Index Number: FCM.41.018.057.23
Course: CY376 — Network Monitoring, Security and Auditing
Team Designation: Blue Team

---

## Project Overview

Security Operations Centers spend disproportionate analyst time on repetitive phishing triage tasks: reputation lookups, header analysis, sandbox submission. This project designs a complete SOAR playbook that automates the repeatable portion of that workflow in Splunk SOAR (Phantom), while preserving human control over high-impact, potentially destructive containment actions.

## Problem Statement

Manual phishing triage does not scale at enterprise email volume. Common failure modes include:

- Alert fatigue from high phishing report volume
- Inconsistent triage decisions between analysts
- Slow Mean Time to Detect (MTTD) and Mean Time to Respond (MTTR)
- Evidence handling that is not consistently forensically sound

This playbook addresses these by codifying enrichment, decision logic, and containment as a standards-aligned, auditable automation.

## Objectives

- Design a complete phishing triage playbook mapped to NIST SP 800-61 Revision 2 and the SANS Incident Handler's Handbook
- Automate IOC extraction and reputation scoring
- Integrate threat intelligence enrichment (VirusTotal, AbuseIPDB, MISP)
- Define conditional decision logic with risk-based human-in-the-loop checkpoints
- Specify containment, eradication, and recovery actions with rollback methods
- Define documentation, closure, and post-incident feedback loop procedures

## Incident Response Lifecycle Mapping

Playbook phases are mapped against both the NIST SP 800-61r2 four-phase model and the SANS six-phase model:
```

NIST SP 800-61r2 SANS Incident Handler's Handbook
───────────────── ─────────────────────────────────
Preparation → Preparation
Detection & Analysis → Identification
Containment, Eradication → Containment
& Recovery → Eradication
→ Recovery
Post-Incident Activity → Lessons Learned

```

## Playbook Workflow

```

Alert Trigger (gateway / SIEM / user report)
↓
False-Positive Filter
↓
Automated Enrichment
├── Sender reputation
├── SPF / DKIM / DMARC validation
├── IOC extraction and scoring
├── Historical correlation
└── Sandbox detonation
↓
Decision Logic (confidence-scored branching)
↓
Analyst Approval Gate (high-impact actions)
↓
Containment → Eradication → Recovery
↓
Documentation, Closure & Metrics
↓
Post-Incident Review / Playbook Feedback Loop

```

## Decision Logic Summary

| Confidence Score | Action |
|---|---|
| < 25 | Auto-close as false positive |
| 25–59 | Route to SOC Tier 1 analyst review |
| 60–84 | Auto-execute reversible containment; Tier 2 approval for further action |
| ≥ 85 | Escalate to IR Lead; full containment set requires analyst approval |

## Frameworks and Standards Referenced

- NIST SP 800-61 Revision 2 — Computer Security Incident Handling Guide
- NIST SP 800-53 Revision 5 — Security and Privacy Controls for Information Systems and Organizations (IR-4, IR-5)
- NIST SP 800-150 — Guide to Cyber Threat Information Sharing
- NIST SP 800-86 — Guide to Integrating Forensic Techniques into Incident Response
- MITRE ATT&CK for Enterprise (T1566 — Phishing, and sub-techniques)
- MITRE D3FEND Matrix
- SANS Institute — Incident Handler's Handbook

## Design Principles

- **Idempotency** — re-running the playbook against the same case produces no duplicate actions
- **Fail-safe defaults** — incomplete enrichment routes to manual review, never to auto-remediation
- **Auditability** — every automated step logs actor, timestamp, and action
- **Modularity** — enrichment and containment logic implemented as reusable sub-playbooks
- **Version control** — playbook maintained as code, not GUI-only

## Scope and Limitations

This playbook covers phishing delivered via email (MITRE ATT&CK T1566 and sub-techniques) only. Non-email initial access vectors, and incident types outside phishing, are out of scope for this version. This is a playbook design and specification project; it does not include a deployed Splunk SOAR instance or live integration credentials.

## Repository Structure

```

.
├── docs/
│ └── soar-phishing-triage-playbook.pdf # Final compiled report (LaTeX-generated)
└── README.md # Project overview and documentation

```

## Contact

GitHub: [anim-michael-asante](https://github.com/anim-michael-asante)
LinkedIn: [michael-asante-anim](https://linkedin.com/in/michael-asante-anim)
Email: animmichaelasante@gmail.com
```
