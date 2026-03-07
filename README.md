# OWASP MAS AI Skill

> The complete OWASP Mobile Application Security standard as a Claude AI Skill – built for auditors, developers, architects, and compliance teams. Includes machine-readable agent output for AI pipelines and self-sync with the OWASP source repositories.

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![OWASP MASVS](https://img.shields.io/badge/OWASP-MASVS%20v2.1-blue)](https://mas.owasp.org/MASVS/)
[![OWASP MASWE](https://img.shields.io/badge/OWASP-MASWE-blue)](https://mas.owasp.org/MASWE/)
[![OWASP MASTG](https://img.shields.io/badge/OWASP-MASTG-blue)](https://mas.owasp.org/MASTG/)

---

## What is this?

A Claude Skill that consolidates three OWASP Mobile Application Security repositories into a single, AI-ready knowledge package:

| OWASP Source | Focus | What the Skill produces |
|---|---|---|
| [OWASP MASVS](https://github.com/OWASP/masvs) | **WHAT** to verify – 24 controls across 8 categories | Audit checklists, requirements matrices, compliance evidence |
| [OWASP MASWE](https://github.com/OWASP/maswe) | **WHAT** to look for – 117+ weaknesses | Weakness lookups, CWE mappings, correlation analysis |
| [OWASP MASTG](https://github.com/OWASP/mastg) | **HOW** to test – techniques, tools, test procedures | Test guidance, tool references, platform-specific test steps |

The Skill combines these three sources into an end-to-end audit workflow, from requirements definition through weakness detection to finished assessment reports.

## Installation

### Build a .skill file

The repo contains the `skill/` directory with all files. To create a `.skill` file (ZIP archive) that can be uploaded directly to Claude:

```bash
# Clone the repo
git clone https://github.com/GodModeAI2025/owasp-mas-ai-skill.git
cd owasp-mas-ai-skill

# Build the .skill file (ZIP of the skill/ directory)
cd skill
zip -r ../owasp-mas.skill .
cd ..
```

Drag the resulting `owasp-mas.skill` file into any Claude chat – done.

### Manual installation as Custom Skill

Alternatively, copy the `skill/` directory directly:

```bash
cp -r skill/ /path/to/skills/user/owasp-mas/
```

## Contents

```
skill/
├── SKILL.md                              (Orchestrator – 7 roles)
└── references/
    ├── masvs-controls.md                 (All 24 MASVS v2.1 controls)
    ├── maswe-catalog.md                  (117 MASWE weaknesses + test approaches)
    ├── mastg-testing.md                  (Testing guide: Android + iOS + 20+ tools)
    ├── secure-coding-android.md          (Kotlin/Java secure coding patterns)
    ├── secure-coding-ios.md              (Swift secure coding patterns)
    ├── threat-model-template.md          (STRIDE template with MASVS mapping)
    ├── report-templates.md               (5 report formats + CWE mapping)
    ├── agent-output.md                   (Machine-readable schemas for AI pipelines)
    └── updater-sync.md                   (Self-sync with OWASP GitHub repos)
```

## The 7 Roles

| Role | Triggers | Output |
|---|---|---|
| **Auditor** | audit, checklist, pentest, IPA/APK analysis | MASVS checklist, findings, risk assessment |
| **Developer** | secure coding, code review, implementation | Platform-specific code, anti-patterns |
| **Architect** | threat model, architecture review, requirements | STRIDE analysis, requirements matrix, SADRs |
| **Reporter** | report, executive summary, compliance | 5 report formats (DOCX/MD/PPTX) |
| **Weakness Analyzer** | MASWE ID, weakness, vulnerability | Weakness details, CWE, remediation |
| **Updater** | update skill, check OWASP updates | Delta report, regenerated references, new .skill ZIP |
| **Agent Output** | machine-readable, YAML, JSON, for agent, pipeline | Structured artifacts for AI agent consumption |

## Agent Output for AI Pipelines

On request, the Skill produces machine-readable YAML/JSON artifacts that downstream AI agents can process directly:

| Schema | Downstream Agent |
|---|---|
| `finding` – findings with evidence and remediation | Ticket agent, remediation agent, CI/CD gate |
| `compliance` – MASVS matrix with verdict | GRC system, compliance agent |
| `threat_model` – STRIDE analysis with risk ratings | Architecture agent, risk management |
| `weakness` – MASWE lookup with detection + fix | Knowledge agent, vulnerability management |
| `secure_coding` – code pattern with platform code | Code generation agent, PR review bot |

Every output ends with an `agent_summary` block: metrics, blocked_controls, release gate decision, prioritized next_agent_actions. 10 downstream triggers for agent orchestration. 3 modes: Agent only | Human only | Dual (prose + YAML artifact).

## Self-Sync with OWASP Sources

The Skill checks the three OWASP source repositories for new versions on demand and updates itself:

```
"Check for OWASP updates"
→ Quick check: version delta without rebuild

"Update the skill"
→ Full rebuild: regenerate references, validate, output new .skill ZIP
```

Supports partial rebuild and cascading updates (MASWE change → also updates CWE mapping and agent output schemas).

## Coverage

### MASVS v2.1 – all 8 categories, all 24 controls

| Category | Controls | Focus |
|---|---|---|
| MASVS-STORAGE | 2 | Secure data storage (data at rest) |
| MASVS-CRYPTO | 2 | Cryptographic operations and key management |
| MASVS-AUTH | 3 | Authentication and authorization |
| MASVS-NETWORK | 2 | Network communication (data in transit) |
| MASVS-PLATFORM | 3 | Platform interaction and IPC |
| MASVS-CODE | 4 | Code quality and secure build practices |
| MASVS-RESILIENCE | 4 | Reverse engineering and tampering |
| MASVS-PRIVACY | 4 | Privacy and data protection |

### Additional Mappings
- 117 MASWE weaknesses with severity and CWE
- MASWE → OWASP Mobile Top 10 (2024)
- MASVS → GDPR / PCI DSS / BSI / ISO 27001
- 20+ tool references (Frida, Objection, Burp Suite, MobSF, Ghidra, ...)

## Examples

```
"Analyze this IPA for MASVS compliance."
"What is MASWE-0013?"
"How do I implement certificate pinning in Swift?"
"Create a threat model for our banking app."
"Generate a technical assessment report from these findings."
"Output the findings as agent output."
"Check for OWASP updates."
```

## Relationship to OWASP Sources

This project is a **derivative** of the following OWASP projects:

| Project | Repository | License |
|---|---|---|
| OWASP MASVS | [github.com/OWASP/masvs](https://github.com/OWASP/masvs) | CC BY-SA 4.0 |
| OWASP MASWE | [github.com/OWASP/maswe](https://github.com/OWASP/maswe) | CC BY-SA 4.0 |
| OWASP MASTG | [github.com/OWASP/mastg](https://github.com/OWASP/mastg) | CC BY-SA 4.0 |

The OWASP content was **not copied verbatim** but transformed for use as a Claude AI Skill: restructured into the Claude Skill format, consolidated from three separate repos, enriched with platform-specific code examples (Kotlin/Swift), extended with report templates and regulatory mappings, made agent-ready with YAML schemas and downstream triggers, and equipped with self-sync capabilities.

This project does not replace the originals. It makes them usable in an AI-driven workflow.

## License

Licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

- ✅ Share, adapt, commercial use
- ⚠️ Attribution required (OWASP as source)
- ⚠️ ShareAlike (derivatives must use same license)

```
This work is derived from the OWASP Mobile Application Security (MAS) project:
- OWASP MASVS (https://github.com/OWASP/masvs) – CC BY-SA 4.0
- OWASP MASWE (https://github.com/OWASP/maswe) – CC BY-SA 4.0
- OWASP MASTG (https://github.com/OWASP/mastg) – CC BY-SA 4.0

OWASP is a trademark of the OWASP Foundation.
This project is not affiliated with, endorsed by, or sponsored by the OWASP Foundation.
```

## Author

**Mark Zimmermann** – AI strategist in the energy transition. 20+ years enterprise.

- LinkedIn: [Mark Zimmermann](https://www.linkedin.com/in/markzimmermann/)
- Podcast: [Think Different. Think AI.](https://thinkdifferenthinkai.com)

## Contributing

Issues and pull requests are welcome. In particular:
- Updates for new MASVS/MASWE/MASTG versions
- Additional secure coding patterns (Flutter, React Native, .NET MAUI)
- New agent output schemas and downstream triggers
- Additional regulatory mappings (HIPAA, SOC 2, NIS2)
- Translations
