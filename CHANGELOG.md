# Changelog

## [1.1.0] – 2026-03-07

### Hinzugefügt
- **Rolle: Agent-Output** – Maschinenlesbare YAML/JSON-Artefakte für AI-Agent-Pipelines
  - 5 Output-Schemata: finding, compliance, threat_model, weakness, secure_coding
  - Agent Summary Block mit Metriken, Pipeline-Steuerung, next_agent_actions
  - 10 Downstream-Triggers (rotate_secret, create_ticket, ci_gate, code_fix, notify, retest, compliance_update, threat_model_update, dependency_scan, privacy_review)
  - 3 Modi: Nur Agent, Nur Mensch, Dual
  - Sicherheitsregeln: Nie Klartext-Secrets, immer value_hash
- **references/agent-output.md** – Vollständige Schema-Dokumentation (471 Zeilen)

### Geändert
- SKILL.md: 6 → 7 Rollen, Routing-Tabelle erweitert, Verifikation um Agent-Output-Checks
- references/updater-sync.md: Rebuild-Plan auf 11 Schritte erweitert (inkl. agent-output.md + Selbstaktualisierung), Kaskaden-Logik für partielle Rebuilds, Validierungsscript auf 9 Prüfungen erweitert, Mindestwerte aktualisiert (9 Refs, 7 Rollen, 3000+ Zeilen)
- Baseline: Skill-Version 1.0.0 → 1.1.0, Referenzdateien 7 → 9

## [1.0.0] – 2026-03-07

### Hinzugefügt

**Claude Skill (skill/)**
- SKILL.md Orchestrator mit 6 Rollen (Auditor, Entwickler, Architekt, Reporter, Weakness Analyzer, Updater)
- references/masvs-controls.md – Alle 24 MASVS v2.1 Kontrollen
- references/maswe-catalog.md – 117 MASWE-Schwachstellen mit Schweregrad
- references/mastg-testing.md – Testing Guide (Android + iOS + 20+ Tools)
- references/secure-coding-android.md – Kotlin/Java Secure-Coding-Patterns
- references/secure-coding-ios.md – Swift Secure-Coding-Patterns
- references/threat-model-template.md – STRIDE-basiertes Threat-Model-Template
- references/report-templates.md – 5 Berichtsformate + CWE + regulatorische Mappings
- references/updater-sync.md – Self-Sync mit OWASP GitHub Repos

**Cowork Plugin (cowork-plugin/)**
- 5 Skills: mas-auditor, mas-developer, mas-architect, mas-report, mas-weakness
- 4 Commands: /owasp-mas:audit, /owasp-mas:check, /owasp-mas:report, /owasp-mas:weakness

### Quellen
- OWASP MASVS v2.1 (https://github.com/OWASP/masvs) – CC BY-SA 4.0
- OWASP MASWE (https://github.com/OWASP/maswe) – CC BY-SA 4.0
- OWASP MASTG (https://github.com/OWASP/mastg) – CC BY-SA 4.0
