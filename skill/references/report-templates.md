# OWASP MAS Report Templates

## Inhaltsverzeichnis
- [Executive Summary](#1-executive-summary)
- [Technischer Prüfbericht](#2-technischer-prüfbericht)
- [Compliance-Nachweis](#3-compliance-nachweis)
- [Gap-Analyse-Bericht](#4-gap-analyse-bericht)
- [Maßnahmenplan](#5-maßnahmenplan)
- [Textbausteine](#textbausteine)

---

## 1. Executive Summary

Für: C-Level, Management, Entscheidungsträger. Maximal 3 Seiten.

### Struktur

```markdown
# Security Assessment – [App-Name] v[Version]

## Management Summary
| Eigenschaft | Wert |
|---|---|
| App | [Name] v[Version] (Build [N]) |
| Plattform | [iOS/Android/Beide] |
| Zeitraum | [Datum von–bis] |
| Profil | MAS-[L1/L2/R] |
| Gesamtbewertung | 🔴/🟡/🟢 [ROT/GELB/GRÜN] |

[2–3 Sätze Gesamteinschätzung]

## Finding-Verteilung
| Kritisch | Hoch | Mittel | Niedrig |
|:---:|:---:|:---:|:---:|
| **X** | **X** | **X** | **X** |

## Kritische Findings (Top 3–5)
1. [Finding-Titel] – [1 Satz Impact]
2. ...

## Handlungsempfehlung
[3–5 Sätze: Was sofort tun, was kurzfristig, strategisch]
```

---

## 2. Technischer Prüfbericht

Für: Security-Team, Entwickler, Architekten. Vollständig.

### Struktur

```markdown
# OWASP MAS – Technical Security Assessment
## [App-Name] v[Version]

| Eigenschaft | Wert |
|---|---|
| App-Name | [Name] |
| Bundle-ID / Package | [ID] |
| Version | [Version] (Build [N]) |
| Plattform | [iOS/Android] |
| Testprofil | MAS-[L1/L2/R] |
| Testart | [Whitebox/Greybox/Blackbox] |
| Prüfzeitraum | [Datum] |
| Vertraulichkeit | VERTRAULICH |

---

## 1. Executive Summary
[Gesamtbewertung, Ampel, Statistik, Positiv-Befunde]

## 2. Scope und Methodik
- Prüfumfang: [Was geprüft wurde]
- Ausschlüsse: [Was nicht geprüft wurde]
- Methodik: OWASP MASVS v2.1, MASTG
- Testumgebung: [Geräte, Tools, Versionen]

## 3. Finding-Übersicht
| ID | Finding | MASWE | MASVS | Schweregrad |
|---|---|---|---|---|
| F-01 | [Titel] | MASWE-[XXXX] | [Kontrolle] | [Schwere] |

## 4. Detaillierte Findings
[Pro Finding: ID, Titel, MASWE, MASVS, Schweregrad, Beschreibung, 
Auswirkung, Reproduktion, Empfehlung, CWE]

## 5. MASVS Compliance Matrix
| Kategorie | Kontrolle | Status | Kommentar |
|---|---|---|---|
[Alle 24 Kontrollen]

## 6. App-Details
[Bundle-Info, Frameworks, SDKs, Entitlements, Konfigurationen]

## 7. Positiv-Befunde
[Was gut gemacht wurde – wichtig für ausgewogenen Bericht]

## 8. Priorisierter Maßnahmenplan
- Sofort (0–2 Wo): [Kritische Findings]
- Kurzfristig (2–8 Wo): [Hohe Findings]
- Mittelfristig (2–6 Mo): [Mittlere Findings]
- Langfristig (6+ Mo): [Strategische Verbesserungen]

## 9. Disclaimer
[Standardtext – siehe Textbausteine]
```

---

## 3. Compliance-Nachweis

Für: Compliance-Abteilung, externe Auditoren, Regulatoren.

### Struktur

```markdown
# MASVS v2.1 Compliance-Nachweis
## [App-Name] v[Version]

## 1. Compliance-Statement
Geprüfter Standard: OWASP MASVS v2.1, Profil MAS-[L1/L2/R]
Erfüllungsgrad: [X]% ([Y] von [Z] Kontrollen bestanden)

## 2. Kontroll-Matrix
| Kategorie | Kontrolle | Status | Nachweis | MASTG-Test |
|---|---|---|---|---|
| STORAGE | STORAGE-1 | ✅/❌/⚠️ | [Beschreibung] | [Test-ID] |

## 3. Abweichungen
| Kontrolle | Abweichung | Kompensatorische Maßnahme | Restrisiko |
|---|---|---|---|

## 4. Regulatorisches Mapping
| MASVS | DSGVO | PCI DSS | BSI | ISO 27001 |
|---|---|---|---|---|
| STORAGE | Art. 32 | 3.4, 6.5 | APP.1 | A.14.1 |
| CRYPTO | Art. 32 | 3.5, 3.6 | CON.1 | A.10.1 |
| AUTH | Art. 32 | 8.1–8.8 | ORP.4 | A.9.2 |
| NETWORK | Art. 32 | 4.1 | NET.1 | A.13.1 |
| PLATFORM | Art. 32 | 6.5 | APP.1 | A.14.2 |
| CODE | Art. 32 | 6.3, 6.5 | APP.1 | A.14.2 |
| RESILIENCE | Art. 32 | 11.5 | APP.1 | A.12.6 |
| PRIVACY | Art. 5,6,7,25 | – | APP.1 | A.18.1 |
```

---

## 4. Gap-Analyse-Bericht

Für: Projektmanagement, Security-Team.

### Struktur

```markdown
# MASVS Gap-Analyse
## [App-Name] – Ist vs. Soll

## 1. Ist-Zustand
| Kategorie | Erfüllungsgrad | Status |
|---|---|---|
| STORAGE | [X]% | 🟡 |

## 2. Soll-Zustand
Ziel-Profil: MAS-[L1/L2/R]
Ziel-Erfüllung: 100% aller Pflicht-Kontrollen

## 3. Gap-Tabelle
| Kontrolle | Ist | Soll | Gap | Aufwand | Priorität |
|---|---|---|---|---|---|
| STORAGE-1 | Partial | Pass | Verschlüsselung fehlt | M | Hoch |

## 4. Roadmap
### Phase 1 (0–3 Monate): [Kritische Gaps]
### Phase 2 (3–6 Monate): [Wichtige Gaps]
### Phase 3 (6–12 Monate): [Strategische Gaps]
```

---

## 5. Maßnahmenplan

Für: Entwicklungsteam, Projektmanagement.

### Struktur

```markdown
# Sicherheits-Maßnahmenplan
## [App-Name] v[Version]

## Sofortmaßnahmen (0–2 Wochen)
| Finding | Maßnahme | Verantwortlich | Aufwand |
|---|---|---|---|
| F-XX | [Konkrete Maßnahme] | [Rolle] | [S/M/L] |

## Kurzfristig (2–8 Wochen)
[Gleiche Tabelle]

## Mittelfristig (2–6 Monate)
[Gleiche Tabelle]

## Langfristig (6+ Monate)
[Gleiche Tabelle]

## Empfohlene Folgeaktivitäten
- Dynamischer Pentest mit Runtime-Instrumentierung
- Code-Review der identifizierten Bereiche
- Retest nach Behebung der Findings
```

---

## Textbausteine

### Gesamtbewertung

**Grün:** Die App erfüllt die MASVS-Anforderungen des gewählten Profils weitgehend. Vereinzelte Verbesserungspotenziale bestehen, stellen jedoch kein signifikantes Risiko dar.

**Gelb:** Die App weist mehrere Schwachstellen auf, die zeitnah behoben werden sollten. Kritische Sicherheitslücken wurden nicht identifiziert, jedoch bestehen Risiken bei hohen und mittleren Findings.

**Rot:** Die App weist kritische Sicherheitslücken auf, die ein unmittelbares Risiko darstellen. Eine Veröffentlichung oder Weiternutzung in der aktuellen Form wird nicht empfohlen.

### Disclaimer

> Dieser Bericht stellt den Sicherheitszustand der geprüften Anwendung zum Zeitpunkt der Prüfung dar. Er ist keine Garantie für die Abwesenheit von Sicherheitslücken. Die Prüfung basiert auf dem OWASP MASVS v2.1 und dem OWASP MASTG. Empfehlungen sollten im Kontext der individuellen Risikoakzeptanz und des regulatorischen Rahmens bewertet werden.

### Finding-Schweregrad-Skala

| Schweregrad | Beschreibung | CVSS |
|---|---|---|
| Kritisch | Direkter Zugriff auf sensible Daten, RCE, Credential-Exposition | 9.0–10.0 |
| Hoch | Umgehung von Sicherheitskontrollen, Datenleck mit hohem Impact | 7.0–8.9 |
| Mittel | Eingeschränktes Datenleck, lokaler Angriff erforderlich | 4.0–6.9 |
| Niedrig | Informationsleck mit geringem Impact, Härtungsmangel | 0.1–3.9 |
| Info | Best-Practice-Empfehlung, kein direktes Sicherheitsrisiko | 0.0 |

### CWE-Mapping der häufigsten Mobile-Findings

| Finding-Typ | CWE |
|---|---|
| Hartcodierte Credentials | CWE-798 |
| Hartcodierte Krypto-Keys | CWE-321 |
| Klartext-Speicherung | CWE-312 |
| Klartext-Übertragung | CWE-319 |
| Unsichere Cert-Validierung | CWE-295 |
| SQL Injection | CWE-89 |
| Debug-Code in Production | CWE-489 |
| Schwache Krypto | CWE-327 |
| Schwacher PRNG | CWE-338 |
| Log-Injection | CWE-532 |
| Unsichere Deserialisierung | CWE-502 |
| Code Injection | CWE-94 |
