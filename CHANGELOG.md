# Changelog

## [Unreleased]

### Geändert
- **MAS Testing Profiles:** Die Level-Schreibweise `L1/L2/R` ist durch die Profile MAS-L1, MAS-L2, MAS-R und MAS-P ersetzt (SKILL.md, mastg-testing.md, report-templates.md, threat-model-template.md, agent-output.md). Neu in `mastg-testing.md`: MAS-P, Angreifermodell je Profil, typische Kombinationen (z. B. MAS-L2+P+R) und der Hinweis, das Profil aus dem Threat Model abzuleiten.
- **Nicht getestete Kontrollen:** Das Compliance-Schema kennt den Status `not_tested` und den Zähler `not_tested`, `agent_summary.metrics` den Zähler `controls_not_tested`. `compliant` ist nur zulässig, wenn keine Kontrolle `fail`, `partial` oder `not_tested` ist. Die Auditor-Checkliste stellt klar, dass nicht Geprüftes nie als Pass erscheint.
- **Updater:** Profile werden aus `mas.owasp.org/Profiles` (Repo `OWASP/mas-website`) synchronisiert, nachdem OWASP sie im September 2026 aus dem MASTG ausgelagert hat. Zusätzlich geprüft werden Spezialprofile und die Profilzuordnung der Tests.

### Hinzugefügt
- **Spezialprofil MAS-EUDIW:** `mastg-testing.md` beschreibt das im September 2026 veröffentlichte Profil für Wallet Instances der EU Digital Identity Wallet (Assets WIA und PID, Mapping auf das Risikoregister nach (EU) 2024/2981, Widerstand gegen Angreifer mit hohem Angriffspotenzial nach (EU) 2015/1502). SKILL.md, `agent-output.md`, `report-templates.md` und `threat-model-template.md` lassen es als Profilwert zu.
- **Profilzuordnung der Tests:** Die aktuellen MASTG-Tests erben ihre Profile von der zugehörigen MASWE und deklarieren sie nicht mehr selbst; nur die als `deprecated` markierten V1-Tests tragen das alte `profiles`-Feld noch (`mastg-testing.md`, `updater-sync.md`).

### Behoben
- **Doppelte Startseite:** `INDEX.HTML` war eine byteidentische Kopie von `index.html`. Auf case-insensitiven Dateisystemen (macOS, Windows) zeigten beide Pfade auf dieselbe Datei, jede Änderung erschien doppelt im `git status`. `INDEX.HTML` entfernt, `index.html` bleibt (GitHub Pages liefert nur diese als Verzeichnisindex).
- **Fehlalarm im Secrets-Check:** Das Evidenz-Feld `key` hieß im Finding-Schema wie ein Wertfeld, enthielt aber nur den Namen des Schlüssels. Die Prüfung 8 des Updaters („Keine Klartext-Secrets“) schlug deshalb auf `agent-output.md` an. Feld in `key_name` umbenannt (analog zum bestehenden `key_id`), Prüfung unverändert streng. Die Namenskonvention steht jetzt in den Sicherheitsregeln von SKILL.md und als Kommentar an der Prüfung.
- **Kombi-Ausgabe ist gültiges YAML:** Der Beispielblock nutzte `...` als Platzhalter. YAML liest das nicht als Platzhalter, sondern als Skalar; der Parser brach mit einem ScannerError ab. Platzhalter sind jetzt `# ...`-Kommentare wie in den übrigen Schemata. Der verschachtelte Code-Block steht in einem vierfachen Fence statt in einem mit Backslash maskierten – so finden Werkzeuge den YAML-Block überhaupt.
- **`compliance_rate` im Beispiel:** `0.29` entsprach `pass / total` (7/24). Die dokumentierte Formel ist `pass / (total - n/a)` = 7/21 = `0.33`. In beiden Beispielen korrigiert und die Rechnung als Kommentar ergänzt.
- **Updater-Validierung:** Prüfung 3 nutzte `grep -oP`, das BSD-grep (macOS) nicht kennt – auf `grep -oE` umgestellt. Neue Prüfung 10 parst alle YAML-Blöcke in `agent-output.md`.

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
