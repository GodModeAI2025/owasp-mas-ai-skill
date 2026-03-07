# OWASP MAS Plugin v1.0.0

Vollständiges Cowork-Plugin für mobile Anwendungssicherheit basierend auf dem OWASP Mobile Application Security Framework.

## Enthaltene Skills

| Skill | Zielgruppe | Beschreibung |
|---|---|---|
| **mas-auditor** | Auditoren, Prüfer | Vollständige MASVS-Audits mit Checklisten, Finding-Bewertung, Testguidance |
| **mas-developer** | Entwickler | Plattformspezifische Secure-Coding-Patterns (Android/iOS) nach MASVS |
| **mas-architect** | Architekten | Security Architecture Review, STRIDE Threat Modeling, Anforderungsableitung |
| **mas-report** | Alle Rollen | Berichterstellung: Executive Summary, technischer Bericht, Compliance, Gap-Analyse |
| **mas-weakness** | Alle Rollen | MASWE-Katalog-Lookup mit 118+ Schwachstellen, CWE/Top-10-Mapping |

## Slash-Commands

| Command | Beschreibung |
|---|---|
| `/owasp-mas:audit` | Startet vollständigen MASVS-Audit |
| `/owasp-mas:check` | Schnelle Prüfung einer Kategorie/Kontrolle |
| `/owasp-mas:report` | Generiert Sicherheitsbericht |
| `/owasp-mas:weakness` | Sucht MASWE-Schwachstellen |

## Abdeckung

### MASVS v2.1 Kontrollen (vollständig)
- MASVS-STORAGE (2 Kontrollen)
- MASVS-CRYPTO (2 Kontrollen)
- MASVS-AUTH (3 Kontrollen)
- MASVS-NETWORK (2 Kontrollen)
- MASVS-PLATFORM (3 Kontrollen)
- MASVS-CODE (4 Kontrollen)
- MASVS-RESILIENCE (4 Kontrollen)
- MASVS-PRIVACY (4 Kontrollen)

### MASWE Schwachstellen (118+ IDs)
Vollständiger Katalog aller MASWE-Schwachstellen mit Schweregrad, Prüfansatz und Behebung.

### MASTG Testing Guide
- Android Testing (statisch + dynamisch)
- iOS Testing (statisch + dynamisch)
- Tool-Referenz (20+ Tools)
- Technik-Referenz (Hooking, Pinning Bypass, Root/JB Detection)

### Zusätzliche Mappings
- MASWE → CWE
- MASWE → OWASP Mobile Top 10 (2024)
- MASVS → DSGVO / PCI DSS / BSI-Grundschutz / ISO 27001

## Quellen
- https://github.com/OWASP/masvs
- https://github.com/OWASP/maswe
- https://github.com/OWASP/mastg
- https://mas.owasp.org
