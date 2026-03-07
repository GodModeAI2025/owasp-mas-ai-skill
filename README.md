# OWASP MAS AI Skill

> Den gesamten OWASP Mobile Application Security Standard als KI-Skill für Claude – nutzbar von Auditoren, Entwicklern, Architekten und Prüfern. Mit maschinenlesbarem Agent-Output für AI-Pipelines und Self-Sync mit den OWASP-Quellen.

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![OWASP MASVS](https://img.shields.io/badge/OWASP-MASVS%20v2.1-blue)](https://mas.owasp.org/MASVS/)
[![OWASP MASWE](https://img.shields.io/badge/OWASP-MASWE-blue)](https://mas.owasp.org/MASWE/)
[![OWASP MASTG](https://img.shields.io/badge/OWASP-MASTG-blue)](https://mas.owasp.org/MASTG/)

---

## Was ist das?

Ein Claude-Skill, der die drei OWASP Mobile Application Security Repositories in ein einziges, KI-nutzbares Wissenspaket überführt:

| OWASP-Quelle | Fokus | Was im Skill daraus wird |
|---|---|---|
| [OWASP MASVS](https://github.com/OWASP/masvs) | **WAS** geprüft wird – 24 Kontrollen in 8 Kategorien | Audit-Checklisten, Anforderungsmatrizen, Compliance-Nachweise |
| [OWASP MASWE](https://github.com/OWASP/maswe) | **WONACH** gesucht wird – 117+ Schwachstellen | Schwachstellen-Lookup, CWE-Mapping, Korrelationsanalysen |
| [OWASP MASTG](https://github.com/OWASP/mastg) | **WIE** getestet wird – Techniken, Tools, Testverfahren | Testanleitungen, Tool-Referenzen, plattformspezifische Prüfschritte |

## Installation

### .skill-Datei erstellen

Das Repo enthält den `skill/`-Ordner mit allen Dateien. Daraus wird eine `.skill`-Datei (ZIP-Archiv), die direkt in Claude hochgeladen werden kann:

```bash
# Repo klonen
git clone https://github.com/GodModeAI2025/owasp-mas-ai-skill.git
cd owasp-mas-ai-skill

# .skill-Datei erstellen (ZIP des skill/-Ordners)
cd skill
zip -r ../owasp-mas.skill .
cd ..
```

Die resultierende `owasp-mas.skill`-Datei in den Claude-Chat ziehen – fertig.

### Manuell als Custom Skill

Alternativ den `skill/`-Ordner direkt kopieren:

```bash
# Für Claude Desktop / Cowork
cp -r skill/ /pfad/zu/skills/user/owasp-mas/
```

## Inhalt

```
skill/
├── SKILL.md                              (Orchestrator – 7 Rollen)
└── references/
    ├── masvs-controls.md                 (24 MASVS v2.1 Kontrollen, vollständig)
    ├── maswe-catalog.md                  (117 MASWE-Schwachstellen + Prüfansätze)
    ├── mastg-testing.md                  (Testing Guide: Android + iOS + 20+ Tools)
    ├── secure-coding-android.md          (Kotlin/Java Secure-Coding-Patterns)
    ├── secure-coding-ios.md              (Swift Secure-Coding-Patterns)
    ├── threat-model-template.md          (STRIDE-Template mit MASVS-Mapping)
    ├── report-templates.md               (5 Berichtsformate + CWE-Mapping)
    ├── agent-output.md                   (Maschinenlesbare Schemata für AI-Pipelines)
    └── updater-sync.md                   (Self-Sync mit OWASP GitHub Repos)
```

## Die 7 Rollen

| Rolle | Trigger | Output |
|---|---|---|
| **Auditor** | Audit, Checkliste, Pentest, IPA/APK-Analyse | MASVS-Checkliste, Findings, Risikobewertung |
| **Entwickler** | Secure Coding, Code-Review, Implementierung | Plattformspezifischer Code, Anti-Patterns |
| **Architekt** | Threat Model, Architecture Review, Anforderungen | STRIDE-Analyse, Anforderungsmatrix, SADRs |
| **Reporter** | Bericht, Executive Summary, Compliance | 5 Berichtsformate (DOCX/MD/PPTX) |
| **Weakness Analyzer** | MASWE-ID, Schwachstelle, Vulnerability | Schwachstellendetails, CWE, Behebung |
| **Updater** | Skill aktualisieren, OWASP-Update prüfen | Delta-Report, aktualisierte Referenzen, neuer .skill-ZIP |
| **Agent-Output** | maschinenlesbar, YAML, JSON, für Agent, Pipeline | Strukturierte Artefakte für AI-Agent-Consumption |

## Agent-Output für AI-Pipelines

Der Skill erzeugt auf Anfrage maschinenlesbare YAML/JSON-Artefakte, die nachgelagerte AI-Agents direkt verarbeiten:

| Schema | Downstream-Agent |
|---|---|
| `finding` – Findings mit Evidenz und Remediation | Ticket-Agent, Remediation-Agent, CI/CD-Gate |
| `compliance` – MASVS-Matrix mit Verdikt | GRC-System, Compliance-Agent |
| `threat_model` – STRIDE-Analyse mit Risk-Ratings | Architecture-Agent, Risk-Management |
| `weakness` – MASWE-Lookup mit Detection + Fix | Knowledge-Agent, Vulnerability-Management |
| `secure_coding` – Code-Pattern mit Plattform-Code | Code-Gen-Agent, PR-Review-Bot |

Jeder Output endet mit einem `agent_summary`-Block: Metriken, blocked_controls, Release-Gate-Entscheidung, priorisierte next_agent_actions. 10 Downstream-Triggers für Agent-Orchestrierung. 3 Modi: Nur Agent-Output | Nur Mensch | Dual (Prosa + YAML-Artefakt).

## Self-Sync mit OWASP-Quellen

Der Skill prüft auf Anfrage die drei OWASP-Repositories auf neue Versionen und aktualisiert sich selbst:

```
"Prüfe ob es OWASP-Updates gibt"
→ Quick-Check: Versions-Delta ohne Rebuild

"Aktualisiere den Skill"
→ Full Rebuild: Referenzen regenerieren, validieren, neuen .skill-ZIP ausgeben
```

Unterstützt partiellen Rebuild und kaskadierende Updates (MASWE-Änderung → auch CWE-Mapping und Agent-Output-Schemata).

## Abdeckung

### MASVS v2.1 – alle 8 Kategorien, alle 24 Kontrollen

| Kategorie | Kontrollen | Fokus |
|---|---|---|
| MASVS-STORAGE | 2 | Sichere Datenspeicherung (Data-at-Rest) |
| MASVS-CRYPTO | 2 | Kryptographische Verfahren und Schlüsselverwaltung |
| MASVS-AUTH | 3 | Authentifizierung und Autorisierung |
| MASVS-NETWORK | 2 | Netzwerkkommunikation (Data-in-Transit) |
| MASVS-PLATFORM | 3 | Plattforminteraktion und IPC |
| MASVS-CODE | 4 | Codequalität und sichere Build-Praktiken |
| MASVS-RESILIENCE | 4 | Reverse Engineering und Tampering |
| MASVS-PRIVACY | 4 | Datenschutz und Privacy |

### Weitere Mappings
- 117 MASWE-Schwachstellen mit Schweregrad und CWE
- MASWE → OWASP Mobile Top 10 (2024)
- MASVS → DSGVO / PCI DSS / BSI-Grundschutz / ISO 27001
- 20+ Tool-Referenzen (Frida, Objection, Burp Suite, MobSF, Ghidra, ...)

## Beispiele

```
„Analysiere diese IPA auf MASVS-Konformität."
„Was ist MASWE-0013?"
„Wie implementiere ich Certificate Pinning in Swift?"
„Erstelle ein Threat Model für unsere Banking-App."
„Erstelle einen technischen Prüfbericht aus diesen Findings."
„Gib die Findings als Agent-Output aus."
„Prüfe ob es OWASP-Updates gibt."
```

## Beziehung zu den OWASP-Quellen

Dieses Projekt ist ein **Derivat** der folgenden OWASP-Projekte:

| Projekt | Repository | Lizenz |
|---|---|---|
| OWASP MASVS | [github.com/OWASP/masvs](https://github.com/OWASP/masvs) | CC BY-SA 4.0 |
| OWASP MASWE | [github.com/OWASP/maswe](https://github.com/OWASP/maswe) | CC BY-SA 4.0 |
| OWASP MASTG | [github.com/OWASP/mastg](https://github.com/OWASP/mastg) | CC BY-SA 4.0 |

Die OWASP-Inhalte wurden **nicht 1:1 kopiert**, sondern für den Einsatz als KI-Skill transformiert: strukturiert ins Claude-Skill-Format, konsolidiert aus drei Repos, angereichert mit Code-Beispielen (Kotlin/Swift), erweitert um Berichts-Templates und regulatorische Mappings, agent-fähig gemacht mit YAML-Schemata und Downstream-Triggers, Self-Sync eingebaut.

Dieses Projekt ersetzt nicht die Originale. Es macht sie in einem KI-gestützten Workflow nutzbar.

## Lizenz

Lizenziert unter [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

- ✅ Teilen, Bearbeiten, Kommerziell nutzen
- ⚠️ Namensnennung (OWASP als Quelle)
- ⚠️ Weitergabe unter gleichen Bedingungen

```
This work is derived from the OWASP Mobile Application Security (MAS) project:
- OWASP MASVS (https://github.com/OWASP/masvs) – CC BY-SA 4.0
- OWASP MASWE (https://github.com/OWASP/maswe) – CC BY-SA 4.0
- OWASP MASTG (https://github.com/OWASP/mastg) – CC BY-SA 4.0

OWASP is a trademark of the OWASP Foundation.
This project is not affiliated with, endorsed by, or sponsored by the OWASP Foundation.
```

## Autor

**Mark Zimmermann** – KI-Stratege in der Energiewende. 20+ Jahre Enterprise.

- LinkedIn: [Mark Zimmermann](https://www.linkedin.com/in/markzimmermann/)
- Podcast: [Think Different. Think AI.](https://thinkdifferenthinkai.com)

## Beitragen

Issues und Pull Requests sind willkommen. Insbesondere:
- Aktualisierungen bei neuen MASVS/MASWE/MASTG-Versionen
- Zusätzliche Secure-Coding-Patterns (Flutter, React Native, .NET MAUI)
- Weitere Agent-Output-Schemata und Downstream-Triggers
- Weitere regulatorische Mappings (HIPAA, SOC 2, NIS2)
