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

Der Skill kombiniert diese drei Quellen zu einem durchgängigen Audit-Workflow, der von der Anforderungsdefinition über die Schwachstellenerkennung bis zum fertigen Prüfbericht reicht.

## Für wen?

| Rolle | Nutzen |
|---|---|
| **Security-Auditoren** | MASVS-Checklisten generieren, IPA/APK-Binaries analysieren, Findings nach MASWE klassifizieren, Berichte erstellen |
| **Mobile-Entwickler** | Plattformspezifische Secure-Coding-Patterns (Swift/Kotlin) mit Code-Beispielen |
| **Security-Architekten** | STRIDE-Threat-Models mit MASVS-Mapping, Sicherheitsanforderungen ableiten |
| **Prüfer & Compliance** | Compliance-Nachweise gegen MASVS, regulatorisches Mapping (DSGVO/PCI DSS/BSI/ISO 27001) |
| **AI-Agent-Pipelines** | Maschinenlesbare YAML/JSON-Artefakte mit Downstream-Triggers für Ticket-, Remediation- und CI/CD-Agents |

## Installation

### Claude.ai (Web/App)
Die `.skill`-Datei aus dem [Release](../../releases/latest) herunterladen und in den Claude-Chat ziehen.

### Claude Cowork (als Plugin)
Das Plugin-Verzeichnis `cowork-plugin/` in den Plugin-Ordner kopieren.

### Manuell (als Custom Skill)
Den Ordner `skill/` nach `/mnt/skills/user/owasp-mas/` kopieren.

## Inhalt

```
skill/
├── SKILL.md                              (Orchestrator – 7 Rollen, 394 Zeilen)
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

**5 Output-Schemata:**

| Schema | Downstream-Agent |
|---|---|
| `finding` – Findings mit Evidenz und Remediation | Ticket-Agent, Remediation-Agent, CI/CD-Gate |
| `compliance` – MASVS-Matrix mit Verdikt | GRC-System, Compliance-Agent |
| `threat_model` – STRIDE-Analyse mit Risk-Ratings | Architecture-Agent, Risk-Management |
| `weakness` – MASWE-Lookup mit Detection + Fix | Knowledge-Agent, Vulnerability-Management |
| `secure_coding` – Code-Pattern mit Plattform-Code | Code-Gen-Agent, PR-Review-Bot |

**Agent Summary Block** an jedem Output: Metriken, blocked_controls, Release-Gate-Entscheidung, priorisierte next_agent_actions.

**10 Downstream-Triggers:** `rotate_secret`, `create_ticket`, `ci_gate`, `code_fix`, `notify`, `retest`, `compliance_update`, `threat_model_update`, `dependency_scan`, `privacy_review`.

**3 Modi:** Nur Agent-Output | Nur Mensch | Dual (Prosa + YAML-Artefakt).

## Self-Sync mit OWASP-Quellen

Der Skill prüft auf Anfrage die drei OWASP-Repositories auf neue Versionen und aktualisiert sich selbst:

```
"Prüfe ob es OWASP-Updates gibt"
→ Quick-Check: Versions-Delta ohne Rebuild

"Aktualisiere den Skill"
→ Full Rebuild: Referenzen regenerieren, validieren, neuen .skill-ZIP ausgeben
```

Unterstützt partiellen Rebuild (nur betroffene Referenzen) und kaskadierende Updates (MASWE-Änderung → auch CWE-Mapping und Agent-Output-Schemata).

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

## Beispiel-Anwendungen

**IPA/APK-Analyse:**
> „Analysiere diese IPA auf MASVS-Konformität."

**Schwachstellen-Lookup:**
> „Was ist MASWE-0013?"

**Secure-Coding-Hilfe:**
> „Wie implementiere ich Certificate Pinning in Swift?"

**Threat Model:**
> „Erstelle ein Threat Model für unsere Banking-App."

**Audit-Report:**
> „Erstelle einen technischen Prüfbericht aus diesen Findings."

**Agent-Pipeline:**
> „Gib die Findings als Agent-Output aus."

**Skill aktualisieren:**
> „Prüfe ob es OWASP-Updates gibt."

## Beziehung zu den OWASP-Quellen

Dieses Projekt ist ein **Derivat** der folgenden OWASP-Projekte:

| Projekt | Repository | Lizenz |
|---|---|---|
| OWASP MASVS | [github.com/OWASP/masvs](https://github.com/OWASP/masvs) | CC BY-SA 4.0 |
| OWASP MASWE | [github.com/OWASP/maswe](https://github.com/OWASP/maswe) | CC BY-SA 4.0 |
| OWASP MASTG | [github.com/OWASP/mastg](https://github.com/OWASP/mastg) | CC BY-SA 4.0 |

### Was wurde verändert?

Die OWASP-Inhalte wurden **nicht 1:1 kopiert**, sondern für den Einsatz als KI-Skill transformiert:

- **Strukturiert** in das Claude-Skill-Format (SKILL.md + references/)
- **Konsolidiert** aus drei getrennten Repositories in ein integriertes Wissenspaket
- **Angereichert** mit plattformspezifischen Code-Beispielen (Kotlin/Swift)
- **Erweitert** um Berichts-Templates, CWE-Mappings, regulatorische Zuordnungen
- **Agent-fähig** gemacht mit maschinenlesbaren YAML-Schemata und Downstream-Triggers
- **Self-Sync** eingebaut für automatische Aktualisierung gegen die Quell-Repositories
- **Optimiert** für KI-Verarbeitung (progressive Disclosure, Rollen-basierter Zugriff, Verifikationslogik)

Dieses Projekt ersetzt nicht die Originale. Es macht sie in einem KI-gestützten Workflow nutzbar.

## Lizenz

Lizenziert unter [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

- ✅ Teilen, Bearbeiten, Kommerziell nutzen
- ⚠️ Namensnennung (OWASP als Quelle)
- ⚠️ Weitergabe unter gleichen Bedingungen

## Attribution

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
- Übersetzungen
