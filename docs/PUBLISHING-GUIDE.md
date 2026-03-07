# GitHub Repository – Publishing Guide

## Empfohlener Repo-Name

```
owasp-mas-ai-skill
```

Alternativen:
- `claude-owasp-mas` (Claude im Namen → besser auffindbar)
- `mobile-security-ai-skill` (generischer, weniger OWASP-gebunden)

**Empfehlung: `owasp-mas-ai-skill`** – klar, beschreibend, SEO-tauglich.

---

## Repo-Struktur

```
owasp-mas-ai-skill/
├── README.md                          ← Hauptdokumentation (fertig)
├── LICENSE                            ← CC BY-SA 4.0 (fertig)
├── CHANGELOG.md                       ← Versionshistorie
│
├── skill/                             ← Der Claude-Skill (einzeln nutzbar)
│   ├── SKILL.md
│   └── references/
│       ├── masvs-controls.md
│       ├── maswe-catalog.md
│       ├── mastg-testing.md
│       ├── secure-coding-android.md
│       ├── secure-coding-ios.md
│       ├── threat-model-template.md
│       └── report-templates.md
│
├── cowork-plugin/                     ← Das Cowork-Plugin (5 Skills + 4 Commands)
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── skills/
│   │   ├── mas-auditor/
│   │   │   ├── SKILL.md
│   │   │   └── references/
│   │   ├── mas-developer/
│   │   │   ├── SKILL.md
│   │   │   └── references/
│   │   ├── mas-architect/
│   │   │   ├── SKILL.md
│   │   │   └── references/
│   │   ├── mas-report/
│   │   │   └── SKILL.md
│   │   └── mas-weakness/
│   │       ├── SKILL.md
│   │       └── references/
│   ├── commands/
│   │   ├── mas-audit.md
│   │   ├── mas-check.md
│   │   ├── mas-report.md
│   │   └── mas-weakness.md
│   ├── README.md
│   └── CHANGELOG.md
│
├── releases/                          ← Fertige Downloads
│   └── owasp-mas.skill               ← ZIP zum direkten Upload in Claude
│
└── docs/                              ← Zusätzliche Dokumentation
    ├── USAGE.md                       ← Anwendungsbeispiele
    ├── CONTRIBUTING.md                ← Beitragsrichtlinien
    └── OWASP-ATTRIBUTION.md          ← Detaillierte Quellenangabe
```

---

## Lizenz-Situation im Detail

### Die Ausgangslage

| Repo | Lizenz | Bedeutung |
|---|---|---|
| OWASP/masvs | CC BY-SA 4.0 | Derivate erlaubt, Attribution + ShareAlike Pflicht |
| OWASP/maswe | CC BY-SA 4.0 | Derivate erlaubt, Attribution + ShareAlike Pflicht |
| OWASP/mastg | CC BY-SA 4.0 | Derivate erlaubt, Attribution + ShareAlike Pflicht |

### Was CC BY-SA 4.0 für dich bedeutet

**Erlaubt:**
- Inhalte transformieren und in neues Format bringen ✅
- Inhalte konsolidieren und anreichern ✅
- Kommerzielle Nutzung (auch Verkauf) ✅
- Eigene Code-Beispiele hinzufügen ✅
- Eigene Templates und Workflows hinzufügen ✅

**Pflicht:**
- Attribution: OWASP als Quelle nennen (in LICENSE + README) ⚠️
- ShareAlike: Dein Derivat muss CC BY-SA 4.0 sein ⚠️
- Änderungen kennzeichnen ⚠️

**Nicht erlaubt:**
- Behaupten, OWASP endorst dein Projekt ❌
- OWASP-Trademark als eigene Marke nutzen ❌
- Restriktivere Lizenz wählen ❌

### Was du NICHT kopiert hast (wichtig!)

Die Skill-Inhalte sind **keine 1:1-Kopien** der OWASP-Repos. Folgende Eigenleistungen unterscheiden das Derivat vom Original:

1. **Transformation ins Skill-Format** – OWASP liefert Markdown-Dokumente und YAML-Files. Der Skill hat eine eigene Architektur (SKILL.md + Progressive Disclosure + Rollen-Routing).

2. **Konsolidierung** – OWASP verteilt das Wissen auf drei getrennte Repos mit unterschiedlicher Struktur. Der Skill integriert sie in einen durchgängigen Workflow.

3. **Eigenständige Code-Beispiele** – Die Secure-Coding-Referenzen (Kotlin/Swift) sind komplett eigenständig geschrieben, nicht aus OWASP-Repos kopiert.

4. **Eigenständige Templates** – Berichts-Templates, Threat-Model-Template, Finding-Schemata, regulatorische Mappings, CWE-Zuordnungen.

5. **KI-Optimierung** – Verifikationslogik, Rollen-basierter Zugriff, Hook-Beschreibungen für Skill-Triggering.

---

## GitHub Repo Settings

### Description
```
OWASP Mobile Application Security (MASVS + MASWE + MASTG) as an AI Skill for Claude – for auditors, developers, architects & compliance teams.
```

### Topics (Tags)
```
owasp, masvs, maswe, mastg, mobile-security, claude, ai-skill, 
security-audit, penetration-testing, ios-security, android-security, 
secure-coding, threat-modeling, compliance
```

### About → Website
```
https://mas.owasp.org
```

---

## Release-Strategie

### v1.0.0 (Initial Release)
- `.skill`-Datei als Release-Asset anhängen
- Cowork-Plugin als ZIP anhängen
- Release Notes:

```markdown
## v1.0.0 – Initial Release

Basierend auf:
- OWASP MASVS v2.1
- OWASP MASWE (Stand März 2026)
- OWASP MASTG (Stand März 2026)

### Enthält
- **Claude Skill** (`owasp-mas.skill`) – 1 SKILL.md + 7 Referenzdateien, alle 5 Rollen
- **Cowork Plugin** – 5 Skills, 4 Slash-Commands

### Abdeckung
- 24/24 MASVS-Kontrollen
- 117 MASWE-Schwachstellen
- Android + iOS Secure Coding (Kotlin/Swift)
- 5 Berichtsformate
- STRIDE Threat Model Template
- CWE + Mobile Top 10 (2024) + Regulatorisches Mapping
```

### Versionierung
- MAJOR: Bei neuer MASVS-Version (z.B. MASVS v3.0)
- MINOR: Bei neuen MASWE-Schwachstellen, neuen Rollen, neuen Referenzen
- PATCH: Korrekturen, Formulierungsverbesserungen

---

## OWASP-Attribution – Best Practices

### Im README (erledigt)
- Tabelle mit allen drei Quell-Repos + Lizenz
- Abschnitt "Beziehung zu den OWASP-Quellen"
- Expliziter Disclaimer: "not affiliated with OWASP"

### In der LICENSE (erledigt)
- Attribution Notice mit allen drei Repos
- Modifications-Abschnitt

### Optional: Separates OWASP-ATTRIBUTION.md

```markdown
# OWASP Attribution

This project builds upon the work of the OWASP Mobile Application Security
(MAS) project. We gratefully acknowledge the contributions of the OWASP
community and specifically:

## OWASP MASVS
- Repository: https://github.com/OWASP/masvs
- License: CC BY-SA 4.0
- Project Leaders: Carlos Holguera, Sven Schleier
- Used content: Control definitions, control categories, control descriptions

## OWASP MASWE
- Repository: https://github.com/OWASP/maswe
- License: CC BY-SA 4.0
- Project Leaders: Carlos Holguera, Sven Schleier
- Used content: Weakness IDs, weakness descriptions, weakness categorization

## OWASP MASTG
- Repository: https://github.com/OWASP/mastg
- License: CC BY-SA 4.0
- Project Leaders: Carlos Holguera, Sven Schleier
- Used content: Testing methodologies, tool references, platform-specific
  testing approaches

## Disclaimer
OWASP® is a registered trademark of the OWASP Foundation, Inc.
This project is an independent derivative work and is not affiliated with,
endorsed by, or sponsored by the OWASP Foundation.
```

---

## Verkaufsargument / Positionierung

### Gegenüber OWASP-Original

Du verkaufst nicht OWASP-Inhalte. Du verkaufst die **Integration und KI-Aufbereitung**:

| OWASP-Original | Dein Skill |
|---|---|
| 3 getrennte Repos, tausende Dateien | 1 Skill, 8 Dateien |
| Manuell lesen und anwenden | KI generiert Checklisten, Reports, Code |
| Kein Rollen-Konzept | 5 spezialisierte Rollen |
| Kein Report-Generator | 5 Berichtsformate auf Knopfdruck |
| Keine Code-Beispiele | Plattformspezifisch Kotlin + Swift |
| Keine regulatorischen Mappings | DSGVO, PCI DSS, BSI, ISO 27001 |
| Keine Korrelationsanalyse | Schwachstellen-Korrelationen, CWE-Mapping |

### Gegenüber kommerziellen Tools (MobSF, NowSecure, etc.)

| Kommerzielles Tool | Dein Skill |
|---|---|
| Lizenzkosten 5.000–50.000€/Jahr | Kostenlos (CC BY-SA 4.0) |
| Blackbox, proprietäre Logik | Vollständig transparent und erweiterbar |
| Fixe Reports | 5 konfigurierbare Berichtsformate |
| Kein Architektur-Review | STRIDE Threat Modeling integriert |
| Kein Secure-Coding-Guide | Plattformspezifische Code-Patterns |

---

## Checkliste vor Veröffentlichung

- [ ] README.md finalisiert
- [ ] LICENSE mit Attribution Notice
- [ ] CHANGELOG.md mit v1.0.0
- [ ] skill/ Ordner mit SKILL.md + 7 Referenzen
- [ ] cowork-plugin/ Ordner mit vollständigem Plugin
- [ ] owasp-mas.skill als Release-Asset
- [ ] GitHub Topics gesetzt
- [ ] Repo Description gesetzt
- [ ] Release v1.0.0 erstellt mit Notes
- [ ] LinkedIn-Post zum Launch vorbereitet
