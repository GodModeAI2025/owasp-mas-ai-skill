---
name: owasp-mas
description: "OWASP Mobile Application Security: MASVS v2.1, MASWE, MASTG als KI-Skill. 7 Rollen: Auditor, Entwickler, Architekt, Reporter, Weakness-Analyzer, Updater, Agent-Output. Verwende IMMER bei: Mobile Security, OWASP MASVS, MASWE, MASTG, App-Sicherheit, Pentest mobile App, Sicherheitsaudit, Certificate Pinning, Keychain, Keystore, Secure Coding Android/iOS, Jailbreak Detection, Root Detection, App Hardening, Threat Model, MASVS Compliance, Mobile Top 10, Audit Report, Gap-Analyse, WebView Sicherheit, IPC Sicherheit, API Key Hardcoded, SSL Pinning, Data Protection, CryptoKit, DPoP, Privacy Manifest, IPA-Analyse, APK-Analyse, Binary-Analyse, Skill aktualisieren, OWASP Update, maschinenlesbar, Agent-Output, YAML, JSON, Pipeline."
---

# OWASP Mobile Application Security (MAS)

Vollständiger Skill für mobile Anwendungssicherheit. Vereint sieben Rollen: Auditor, Entwickler, Architekt, Report-Generator, Weakness-Analyzer, Updater (Self-Sync) und Agent-Output (maschinenlesbare Artefakte für AI-Pipelines).

---

## Rollenauswahl

Bestimme anhand der Anfrage die passende Rolle und lies die zugehörige Referenz:

| Anfrage-Typ | Rolle | Referenzdatei(en) |
|---|---|---|
| Audit, Checkliste, Pentest, Compliance-Prüfung | Auditor | `references/masvs-controls.md` + `references/mastg-testing.md` |
| Sichere Implementierung, Code-Review, Secure Coding | Entwickler | `references/secure-coding-android.md` oder `references/secure-coding-ios.md` |
| Architektur-Review, Threat Model, Anforderungen | Architekt | `references/threat-model-template.md` + `references/masvs-controls.md` |
| Bericht erstellen, Executive Summary, Report | Reporter | `references/report-templates.md` |
| Schwachstelle nachschlagen, MASWE-ID, Weakness | Analyzer | `references/maswe-catalog.md` |
| IPA/APK-Analyse, Binary-Analyse | Auditor | `references/mastg-testing.md` |
| Skill aktualisieren, OWASP-Update, neue Version prüfen | Updater | `references/updater-sync.md` |
| Maschinenlesbar, Agent-Output, YAML, JSON, Pipeline, für Agent | Agent-Output | `references/agent-output.md` |

**Lies die Referenzdateien VOR dem Antworten.** Bei gemischten Anfragen mehrere Referenzen laden.

---

## Rolle: Auditor

### Audit-Workflow

**Phase 1 – Scope:**
Kläre: App-Typ (Native/Hybrid/Cross-Platform), Testprofil (L1/L2/R), Plattform (Android/iOS/beide), sensible Daten, regulatorischer Kontext (DSGVO/PCI DSS/KRITIS), Testart (Whitebox/Greybox/Blackbox).

**Phase 2 – Checkliste:**
Generiere MASVS-Checkliste mit allen 24 Kontrollen aus `references/masvs-controls.md`:

```
MASVS Audit-Checkliste – [App] – [Datum]
Profil: [L1/L2/R] | Plattform: [Android/iOS]

Kategorie  | Kontrolle   | Status | Finding | Schweregrad | Kommentar
-----------|-------------|--------|---------|-------------|----------
STORAGE    | STORAGE-1   | ☐      |         |             |
...
```

Status: ✅ Pass | ❌ Fail | ⚠️ Partial | ⏭️ N/A | ☐ Nicht getestet

**Phase 3 – Testdurchführung:**
Für jede Kontrolle aus `references/mastg-testing.md`: konkrete Testschritte, Tools, Pass/Fail-Kriterien.

**Phase 4 – Finding-Dokumentation:**

```markdown
## Finding F-[NR]: [Titel]
| Feld | Wert |
|---|---|
| MASWE-ID | MASWE-[XXXX] |
| MASVS-Kontrolle | MASVS-[CAT]-[N] |
| Schweregrad | Kritisch/Hoch/Mittel/Niedrig/Info |
| Plattform | Android/iOS/Beide |
| CWE | CWE-[XXX] |

**Beschreibung:** [Was gefunden wurde]
**Auswirkung:** [Risiko]
**Empfehlung:** [Konkrete Maßnahme]
```

**Phase 5 – Risikobewertung:**
Kritisch (9.0–10.0) | Hoch (7.0–8.9) | Mittel (4.0–6.9) | Niedrig (0.1–3.9) | Info (0.0)

### IPA/APK Statische Analyse

Bei hochgeladener IPA/APK diese Prüfschritte ausführen:

**IPA (iOS):**
1. `unzip app.ipa -d extracted/`
2. Info.plist parsen: ATS, Permissions, URL-Schemes, Debug-Flags, API-Keys
3. Embedded.mobileprovision: Entitlements, get-task-allow, aps-environment
4. GoogleService-Info.plist, AirshipConfig.plist, Auth0.plist: Exponierte Secrets
5. PrivacyInfo.xcprivacy: Tracking, Collected Data Types, API Reasons
6. Binary: `strings` → Hardcoded Keys, URLs, Environment-Strings, Crypto-Referenzen
7. Binary: `file` → PIE-Flag, Architektur
8. Frameworks: Versionen, Privacy Manifests, bekannte SDKs
9. Mach-O-Header: PIE, flags

**APK (Android):**
1. `apktool d app.apk`
2. AndroidManifest.xml: exported Components, Permissions, allowBackup, debuggable, networkSecurityConfig, minSdk, targetSdk
3. `jadx` → Java-Source: Hardcoded Secrets, Krypto-APIs, WebView-Config
4. res/xml/network_security_config.xml: Cleartext, Pins, Trust-Anchors
5. `strings` auf native Libraries (.so)
6. META-INF: Signatur-Schema

---

## Rolle: Entwickler

### Arbeitsweise
1. Plattform identifizieren (Android/iOS/Cross-Platform)
2. MASVS-Kontrolle zuordnen
3. Konkrete Implementierung mit Code-Beispielen liefern
4. Anti-Patterns zeigen (was NICHT tun)
5. Testbarkeit sicherstellen

Für plattformspezifische Code-Beispiele lies:
- Android: `references/secure-coding-android.md`
- iOS: `references/secure-coding-ios.md`

### Kurzreferenz Secure Coding (plattformübergreifend)

| MASVS | Empfehlung | Anti-Pattern |
|---|---|---|
| STORAGE-1 | Keystore/Keychain, verschlüsselte DBs | Klartext in SharedPrefs/NSUserDefaults |
| STORAGE-2 | Clipboard-Schutz, Backup-Exclusion | Sensible Daten in Logs |
| CRYPTO-1 | AES-256-GCM, SHA-256+, CSPRNG | MD5, SHA-1, DES, ECB, statische IVs |
| CRYPTO-2 | Hardware-Backed Keystore/Secure Enclave | Hartcodierte Schlüssel |
| AUTH-1 | MFA, Step-Up, OAuth2/OIDC, DPoP | Nur lokale Auth |
| AUTH-2 | Token in Keychain/Keystore, TLS | Credentials im Klartext |
| AUTH-3 | Serverseitige Enforcement | Nur Client-Side-Checks |
| NETWORK-1 | TLS 1.2+, kein Cleartext | HTTP, offene Ports |
| NETWORK-2 | Public-Key Pinning + Backup-Pin | Kein Pinning, Cert-Validation aus |
| PLATFORM-1 | FLAG_SECURE, Screenshot-Schutz | Sensible Daten in Notifications |
| PLATFORM-2 | Explizite Intents, Permission-Schutz | Exported Components ohne Schutz |
| PLATFORM-3 | JS minimal, Whitelist, kein File-Access | JS-Bridge ohne Kontrolle |
| CODE-1 | Forced Updates, aktuelles Target-SDK | Veraltete minSdkVersion |
| CODE-2 | Input Validation aller Quellen | Ungeprüfte externe Daten |
| CODE-3 | Prepared Statements, sichere Deser. | SQL Injection, eval() |
| CODE-4 | PIE, Stack Canaries, Dependency-Check | Bekannte CVEs in Libs |
| RESILIENCE-1 | ProGuard/R8, String-Encryption | Kein Obfuskation |
| RESILIENCE-2 | Debug-Code entfernt, kein Test-URLs | DEBUG=true in Release |
| RESILIENCE-3 | JB/Root-Detection, Integrity-Checks | Keine Runtime-Prüfung |
| RESILIENCE-4 | Anti-Debug, ptrace | Keine Debugger-Detection |
| PRIVACY-1 | Minimale Permissions, kein über-Tracking | Alle Permissions pauschal |
| PRIVACY-2 | Privacy Policy, Transparenz | Kein Privacy Manifest |
| PRIVACY-3 | Opt-in/Opt-out, Löschung, Export | Kein Consent-Management |
| PRIVACY-4 | On-Device-ML, Pseudonymisierung | Klartext-IDs an Server |

---

## Rolle: Architekt

### Workflow
1. **System verstehen:** App-Zweck, Datenkategorien, Architekturtyp, Backend, SDKs, Auth, Regulatorik
2. **Architekturdiagramm** mit Ebenen: UI → Business Logic → Data → Crypto → Auth → Cache
3. **Trust Boundaries:** App↔OS, App↔Netzwerk, App↔Backend, App↔Andere Apps, App↔User, App↔3rd Party
4. **STRIDE Threat Modeling** pro Boundary (aus `references/threat-model-template.md`)
5. **MASVS-Anforderungsmatrix:** Element → Kontrolle → Anforderung → Priorität
6. **Attack Surface Analyse:** Lokal, Netzwerk, Plattform, User-seitig
7. **Security Architecture Decision Records** (SADRs)

### STRIDE → MASVS Mapping

| Threat | MASVS-Bezug |
|---|---|
| Spoofing | AUTH |
| Tampering | RESILIENCE, CODE |
| Repudiation | AUTH, STORAGE |
| Information Disclosure | STORAGE, NETWORK, PRIVACY |
| Denial of Service | CODE, NETWORK |
| Elevation of Privilege | AUTH, PLATFORM |

### Security-Architektur-Patterns
- **Defense-in-Depth:** TLS+Pinning → HW-Keystore → MFA+Biometrie → Server-Auth → Obfuskation → Runtime-Checks → Datenminimierung
- **Zero Trust Mobile:** Jede API-Anfrage authentifizieren, Device Attestation, keine implizite Vertrauensstellung, kontinuierliche Validierung

---

## Rolle: Reporter

Lies `references/report-templates.md` für vollständige Templates. Fünf Berichtstypen:

| Typ | Zielgruppe | Umfang |
|---|---|---|
| **Executive Summary** | C-Level, Management | 2–3 Seiten: Ampel, Top-Findings, Handlungsempfehlung |
| **Technischer Prüfbericht** | Security-Team, Devs | Vollständig: Scope, Methodik, Findings, Compliance-Matrix, Maßnahmen |
| **Compliance-Nachweis** | Compliance, Auditoren | Kontroll-Matrix mit Nachweis, Abweichungen, reg. Mapping |
| **Gap-Analyse** | Projektmanagement | Ist vs. Soll, Gap-Tabelle, Roadmap |
| **Maßnahmenplan** | Dev-Team, PM | Priorisiert: Sofort / Kurzfristig / Mittelfristig / Langfristig |

**Gesamtbewertung Ampel:**
- 🟢 Grün: MASVS weitgehend erfüllt, vereinzelte Verbesserungspotenziale
- 🟡 Gelb: Mehrere Schwachstellen, zeitnah beheben, keine kritischen Lücken
- 🔴 Rot: Kritische Sicherheitslücken, Veröffentlichung nicht empfohlen

Für DOCX-Berichte den `docx`-Skill, für Präsentationen den `slide-master`- oder `pptx`-Skill nutzen.

---

## Rolle: Weakness Analyzer

Lies `references/maswe-catalog.md` für den vollständigen Katalog (118+ Schwachstellen).

### Ausgabeformat für einzelne Schwachstelle

```markdown
## MASWE-[XXXX]: [Titel]
| Feld | Wert |
|---|---|
| MASVS-Kategorie | [CATEGORY] |
| MASVS-Kontrolle | MASVS-[CAT]-[N] |
| Schweregrad | Kritisch/Hoch/Mittel/Niedrig |
| Plattform | Android/iOS/Beide |
| CWE | CWE-[XXX] |

**Beschreibung:** [Was und warum gefährlich]
**Erkennung:** [Tools, Techniken]
**Behebung:** [Plattformspezifische Maßnahmen]
**Verwandte Schwachstellen:** [Korrelierte MASWE-IDs]
```

### OWASP Mobile Top 10 (2024) → MASWE Mapping

| Rang | Titel | MASWE-IDs |
|---|---|---|
| M1 | Improper Credential Usage | 0005, 0013, 0036, 0037 |
| M2 | Inadequate Supply Chain Security | 0076 |
| M3 | Insecure Authentication/Authorization | 0028–0046 |
| M4 | Insufficient Input/Output Validation | 0079–0088 |
| M5 | Insecure Communication | 0047–0052 |
| M6 | Inadequate Privacy Controls | 0103–0115 |
| M7 | Insufficient Binary Protections | 0089–0102 |
| M8 | Security Misconfiguration | 0067, 0074, 0075, 0078 |
| M9 | Insecure Data Storage | 0001–0007 |
| M10 | Insufficient Cryptography | 0009–0027 |

---

## Rolle: Updater (Self-Sync)

Prüft die drei OWASP-Quell-Repositories auf neue Versionen und generiert aktualisierte Referenzdateien. Lies `references/updater-sync.md` für den vollständigen Workflow.

### Aktuelle Baseline

```
MASVS: v2.1.0
MASWE: 117 IDs (Stand: März 2026)
MASTG: v1.7.0
Rollen: 7
Referenzdateien: 9
Skill-Version: 1.1.0
```

### Trigger-Phrasen

- "Prüfe ob es OWASP-Updates gibt"
- "Aktualisiere den Skill"
- "Gibt es neue MASWE-Schwachstellen?"
- "MASVS neue Version?"
- "Skill updaten"
- "Sync mit OWASP"

### Quick-Check (ohne Rebuild)

Nur Versionscheck, kein Rebuild:

1. `web_search`: "OWASP MASVS latest release site:github.com"
2. `web_search`: "OWASP MASTG latest release site:github.com"
3. `web_fetch`: https://mas.owasp.org/MASWE/ → MASWE-IDs zählen
4. Delta-Report erstellen und dem Benutzer zeigen

### Vollständiger Update-Workflow

1. **Versionscheck** – GitHub Releases/Commits aller 3 Repos prüfen
2. **Delta-Report** – Dem Benutzer zeigen: Was hat sich geändert?
3. **Bestätigung** – Benutzer bestätigt Rebuild (vollständig oder partiell)
4. **Rohdaten laden** – Neue Inhalte von mas.owasp.org fetchen
5. **Referenzen regenerieren** – Betroffene references/*.md neu schreiben
6. **SKILL.md patchen** – Baseline-Block aktualisieren, Version hochzählen
7. **Validieren** – Vollständigkeitsprüfung (8 Kategorien, Kontrollen-Zählung, MASWE-IDs)
8. **Ausgeben** – Neue .skill-ZIP erstellen und per `present_files` bereitstellen

### Partieller vs. vollständiger Rebuild

| Repo geändert | Betroffene Referenzen | Aufwand |
|---|---|---|
| Nur MASVS | masvs-controls.md | Klein |
| Nur MASWE | maswe-catalog.md | Klein–Mittel |
| Nur MASTG | mastg-testing.md, ggf. secure-coding-*.md | Mittel |
| MASVS + MASWE | masvs-controls.md, maswe-catalog.md | Mittel |
| Alle drei | Vollständiger Rebuild aller 7 Referenzen | Groß |

### Validierung nach Rebuild

```
Mindestwerte:
- MASVS-Kategorien: 8
- MASVS-Kontrollen: 24 (Baseline, steigend)
- MASWE-IDs: 117 (Baseline, steigend)
- Referenzdateien: 9 (inkl. agent-output.md + updater-sync.md)
- SKILL.md Rollen: 7
- Gesamtzeilen: 3000+
```

### Versionierung

- Neuer MASVS-Major (z.B. v3.0) → Skill MAJOR hochzählen
- Neue MASWE-Schwachstellen oder neue MASTG-Tests → Skill MINOR hochzählen
- Korrekturen, Textänderungen → Skill PATCH hochzählen

---

## Rolle: Agent-Output (maschinenlesbar)

Erzeugt strukturierte YAML/JSON-Artefakte, die von nachgelagerten AI-Agents direkt verarbeitet werden. Lies `references/agent-output.md` für alle Schemata.

### Wann aktivieren

- Explizit: "als Agent-Output", "maschinenlesbar", "für Agent", "YAML", "JSON", "für Pipeline"
- Implizit: Wenn erkennbar ist, dass der Output an einen anderen Agent geht
- Dual-Modus: "für Mensch und Agent" → Prosa + YAML-Artefakt

### Verfügbare Output-Schemata

| Schema | Zweck | Downstream-Agent |
|---|---|---|
| `finding` | Einzelnes oder mehrere Findings | Ticket-Agent, Remediation-Agent |
| `compliance` | MASVS-Compliance-Matrix | Compliance-Agent, GRC-System |
| `threat_model` | STRIDE-Analyse | Architecture-Agent, Risk-System |
| `weakness` | MASWE-Schwachstellen-Lookup | Knowledge-Agent, Vulnerability-Mgmt |
| `secure_coding` | Code-Pattern mit Plattform-Code | Code-Gen-Agent, PR-Review-Bot |

### Agent Summary Block

Jeder Agent-Output endet mit einem `agent_summary`-Block:

```yaml
agent_summary:
  source_skill: "owasp-mas"
  skill_version: "1.1.0"
  output_role: "[rolle]"
  app:
    id: "[bundle-id]"
    version: "[version]"
    platform: "[ios|android]"
  assessment:
    profile: "[L1|L2|R]"
    verdict: "[pass|conditional_pass|fail]"
  metrics:
    findings_total: N
    critical: N
    high: N
  blocked_controls: [...]
  next_agent_actions:
    immediate: [...]
    short_term: [...]
  pipeline:
    release_blocked: true|false
    retest_required: true|false
```

### Downstream-Trigger

Findings enthalten `downstream_triggers` für Agent-Orchestrierung:

| Trigger | Empfänger |
|---|---|
| `rotate_secret` | Secret-Management-Agent |
| `create_ticket` | Ticket-Agent (Jira/Linear/GitHub) |
| `ci_gate` | CI/CD-Agent (block/unblock) |
| `code_fix` | Remediation-Agent, PR-Bot |
| `notify` | Slack-/E-Mail-Agent |
| `retest` | Audit-Agent (Selbst-Referenz) |
| `compliance_update` | GRC-System |
| `dependency_scan` | Snyk/Trivy-Agent |
| `privacy_review` | DPO-Workflow |

### Sicherheitsregeln für Agent-Output

- Nie Klartext-Secrets im Output (immer `value_hash` statt `value`)
- Nie Credentials in `evidence.snippet` (immer `[REDACTED]`)
- `agent_summary.pipeline.release_blocked` ist `true` wenn mindestens 1 Critical Finding offen

---

## Verifikation

Vor jeder Antwort prüfen:

1. Alle relevanten MASVS-Kategorien berücksichtigt (8 bei Vollaudit)
2. Jedes Finding hat MASWE-ID, Schweregrad und konkrete Empfehlung
3. Plattform-Unterschiede (Android vs. iOS) korrekt
4. Empfehlungen sind umsetzbar (keine generischen Ratschläge)
5. Code-Beispiele syntaktisch korrekt und plattform-passend
6. Testprofil (L1/L2/R) korrekt angewendet
7. Bei Berichten: Disclaimer und Vertraulichkeitsstufe enthalten
8. Bei Updates: Delta-Report vor Rebuild zeigen, Validierung nach Rebuild bestanden
9. Bei Agent-Output: Schema-Version gesetzt, agent_summary vollständig, keine Klartext-Secrets, downstream_triggers vorhanden

Bei Fehlern automatisch korrigieren, dann präsentieren.
