---
name: mas-weakness
description: OWASP MASWE Schwachstellen-Lookup und Analyse. Durchsucht den vollständigen MASWE-Katalog (118+ Schwachstellen), gibt detaillierte Informationen zu einzelnen Schwachstellen, zeigt Zusammenhänge zu MASVS-Kontrollen und MASTG-Tests. Verwende diesen Skill IMMER bei: MASWE Schwachstelle nachschlagen, OWASP Mobile Weakness, mobile Schwachstelle analysieren, MASWE-ID, Weakness Enumeration mobile, Schwachstellenkatalog mobile App, welche Schwachstellen gibt es bei [Thema], MASVS Schwachstellen-Mapping, mobile Vulnerability, Sicherheitslücke mobile App erklären, OWASP Weakness mobile, Insecure Storage mobile, Hardcoded Keys, Certificate Pinning Schwachstelle, WebView Vulnerability, Biometric Bypass.
---

# OWASP MASWE Weakness Analyzer

Durchsucht und analysiert den OWASP Mobile Application Security Weakness Enumeration Katalog.

---

## Wann diesen Skill verwenden

- Spezifische MASWE-Schwachstelle nachschlagen (z.B. "Was ist MASWE-0013?")
- Schwachstellen für eine MASVS-Kategorie auflisten
- Zusammenhang zwischen Schwachstelle und Kontrolle verstehen
- Prüfansatz für eine spezifische Schwachstelle erhalten
- Schwachstellen nach Schweregrad filtern
- Suche nach Schwachstellen zu einem Thema (z.B. "alle Krypto-Schwachstellen")

---

## Referenzdatei

Vor der Arbeit lesen:
- `references/maswe-catalog.md` – Vollständiger MASWE-Katalog mit allen 118+ Schwachstellen

---

## Arbeitsweise

### Einzelne Schwachstelle nachschlagen

Wenn der Benutzer eine MASWE-ID nennt, liefere:

```
## MASWE-[XXXX]: [Titel]

**MASVS-Kategorie**: [CATEGORY]
**MASVS-Kontrolle**: MASVS-[CATEGORY]-[N]
**Schweregrad**: Kritisch / Hoch / Mittel / Niedrig
**Plattform**: Android / iOS / Beide

### Beschreibung
[Was diese Schwachstelle bedeutet und warum sie gefährlich ist]

### Technischer Hintergrund
[Wie die Schwachstelle technisch entsteht]

### Auswirkung
[Konkretes Risiko für den Benutzer/die Organisation]

### Erkennung
[Wie die Schwachstelle gefunden werden kann – Tools, Techniken]

### Behebung
[Konkrete Maßnahmen zur Beseitigung – mit Plattform-Unterscheidung]

### Code-Beispiel
[Unsicher → Sicher, plattformspezifisch]

### Verwandte Schwachstellen
[Andere MASWE-IDs die oft gemeinsam auftreten]

### Externe Referenzen
- CWE: [Zugehörige CWE-ID]
- OWASP Mobile Top 10: [Zugehöriger Eintrag]
- MASTG-Tests: [Relevante Tests]
```

### Kategorie-Suche

Wenn der Benutzer nach einer Kategorie fragt, liste alle Schwachstellen der Kategorie auf:
1. Aus `references/maswe-catalog.md` die Schwachstellen der Kategorie extrahieren
2. Nach Schweregrad sortiert ausgeben
3. Prüfansatz für die Kategorie mitliefern

### Thematische Suche

Wenn der Benutzer nach einem Thema sucht (z.B. "Kryptographie", "WebView", "Biometrie"):
1. Relevante MASWE-IDs aus dem Katalog identifizieren
2. Kategorieübergreifend suchen (z.B. kann "Verschlüsselung" in STORAGE und CRYPTO relevant sein)
3. Ergebnisse nach Relevanz sortiert präsentieren

### Schweregrad-Filter

Wenn der Benutzer nach Schweregrad filtert:
1. Alle Schwachstellen des gewünschten Schweregrads auflisten
2. Nach MASVS-Kategorie gruppieren
3. Gesamtanzahl pro Schweregrad angeben

---

## Schwachstellen-Korrelationen

Häufig gemeinsam auftretende Schwachstellen:

| Primäre Schwachstelle | Korreliert mit | Begründung |
|---|---|---|
| MASWE-0013 (Hardcoded Keys) | MASWE-0005 (Hardcoded API Keys) | Gleiche Root Cause |
| MASWE-0050 (Cleartext Traffic) | MASWE-0052 (Insecure Cert Validation) | Beides Netzwerk-Schwächen |
| MASWE-0041 (Local Auth Only) | MASWE-0044 (Biometric Bypass) | Authentifizierungs-Kette |
| MASWE-0067 (Debuggable) | MASWE-0093 (Debug Symbols) | Build-Konfiguration |
| MASWE-0086 (SQL Injection) | MASWE-0064 (Insecure Content Provider) | Datenbank-Zugriff |
| MASWE-0006 (Unencrypted Storage) | MASWE-0001 (Sensitive Logs) | Datenschutz-Kette |

---

## CWE-Mapping

| MASWE | CWE | Beschreibung |
|---|---|---|
| MASWE-0001 | CWE-532 | Information Exposure Through Log Files |
| MASWE-0005 | CWE-798 | Use of Hard-coded Credentials |
| MASWE-0013 | CWE-321 | Use of Hard-coded Cryptographic Key |
| MASWE-0020 | CWE-327 | Use of a Broken or Risky Cryptographic Algorithm |
| MASWE-0027 | CWE-338 | Use of Cryptographically Weak PRNG |
| MASWE-0036 | CWE-312 | Cleartext Storage of Sensitive Information |
| MASWE-0050 | CWE-319 | Cleartext Transmission of Sensitive Information |
| MASWE-0052 | CWE-295 | Improper Certificate Validation |
| MASWE-0067 | CWE-489 | Active Debug Code |
| MASWE-0085 | CWE-94 | Improper Control of Generation of Code |
| MASWE-0086 | CWE-89 | SQL Injection |
| MASWE-0088 | CWE-502 | Deserialization of Untrusted Data |

---

## OWASP Mobile Top 10 (2024) Mapping

| M-Rang | Titel | MASWE-IDs |
|---|---|---|
| M1 | Improper Credential Usage | MASWE-0005, 0013, 0036, 0037 |
| M2 | Inadequate Supply Chain Security | MASWE-0076 |
| M3 | Insecure Authentication/Authorization | MASWE-0028–0046 |
| M4 | Insufficient Input/Output Validation | MASWE-0079–0088 |
| M5 | Insecure Communication | MASWE-0047–0052 |
| M6 | Inadequate Privacy Controls | MASWE-0103–0115 |
| M7 | Insufficient Binary Protections | MASWE-0089–0102 |
| M8 | Security Misconfiguration | MASWE-0067, 0074, 0075, 0078 |
| M9 | Insecure Data Storage | MASWE-0001–0007 |
| M10 | Insufficient Cryptography | MASWE-0009–0027 |

---

## Verifikation

Bevor Ergebnisse präsentiert werden:
1. MASWE-ID existiert und ist korrekt zugeordnet
2. Schweregrad entspricht dem Katalog
3. MASVS-Kontrolle korrekt referenziert
4. Plattform-Angabe (Android/iOS/Beide) korrekt
5. Behebungsmaßnahmen sind plattformspezifisch und konkret
6. CWE-Mapping ist korrekt (wo vorhanden)
