---
name: mas-auditor
description: OWASP Mobile Application Security Auditor. Führt vollständige Sicherheitsaudits mobiler Apps nach MASVS v2.1, MASWE und MASTG durch. Erstellt strukturierte Audit-Checklisten, bewertet Findings, gibt Empfehlungen. Verwende diesen Skill IMMER bei: Mobile Security Audit, OWASP MASVS Prüfung, App-Sicherheitstest, Penetrationstest mobile App, MASWE Schwachstellenanalyse, MASTG Testdurchführung, Mobile App Pentest, App Security Assessment, Sicherheitsbewertung mobile Anwendung, MASVS Checklist, MAS Compliance Check, Mobile App Audit planen, Audit-Scope definieren, Testplan erstellen, Findings bewerten, Risikobewertung mobile App, Gap-Analyse MASVS.
---

# OWASP MAS Auditor

Vollständiger Sicherheitsaudit-Skill für mobile Anwendungen basierend auf dem OWASP Mobile Application Security Framework (MASVS v2.1, MASWE, MASTG).

---

## Wann diesen Skill verwenden

- Sicherheitsaudit einer mobilen App planen oder durchführen
- MASVS-Compliance prüfen
- Audit-Checkliste nach MASVS erstellen
- Findings aus Mobile-Security-Tests bewerten und priorisieren
- Gap-Analyse gegen MASVS durchführen
- Testplan für Mobile-Security-Assessment erstellen

---

## Referenzdateien

Vor der Arbeit die relevante Referenz lesen:
- `references/masvs-controls.md` – Alle MASVS v2.1 Kontrollen mit Detailbeschreibung
- `references/mastg-testing.md` – MASTG Testing Guide, Tools, Techniken, Test-Case-Mapping

---

## Audit-Workflow

### Phase 1: Scope & Profil

Kläre mit dem Benutzer:
1. **App-Typ**: Native (Android/iOS), Hybrid, Cross-Platform (Flutter/React Native)
2. **Testprofil**: MAS-L1 (Standard), MAS-L2 (Defense-in-Depth), MAS-R (Resilience)
3. **Plattform(en)**: Android, iOS oder beide
4. **Sensitive Daten**: Welche Datenkategorien verarbeitet die App?
5. **Regulatorischer Kontext**: DSGVO, PCI DSS, HIPAA, KRITIS, branchenspezifisch
6. **Testart**: Whitebox (Source-Code-Zugang), Greybox (APK/IPA + Doku), Blackbox

Erstelle daraus einen strukturierten Audit-Scope.

### Phase 2: Checkliste generieren

Basierend auf Profil und Plattform eine MASVS-Checkliste generieren:

```
MASVS Audit-Checkliste – [App-Name] – [Datum]
Profil: [L1/L2/R]
Plattform: [Android/iOS/Beide]

Kategorie | Kontrolle | Status | Finding | Schweregrad | Kommentar
---------|-----------|--------|---------|-------------|----------
STORAGE  | STORAGE-1 | ☐      |         |             |
STORAGE  | STORAGE-2 | ☐      |         |             |
CRYPTO   | CRYPTO-1  | ☐      |         |             |
...
```

Status-Werte: ✅ Bestanden | ❌ Nicht bestanden | ⚠️ Teilweise | ⏭️ Nicht anwendbar | ☐ Nicht getestet

### Phase 3: Testdurchführung-Guidance

Für jede Kontrolle:
1. Lies die Kontrolle aus `references/masvs-controls.md`
2. Identifiziere zugehörige MASWE-Weaknesses
3. Beschreibe konkrete Testschritte aus `references/mastg-testing.md`
4. Nenne die benötigten Tools
5. Beschreibe erwartete Ergebnisse (Pass/Fail-Kriterien)

### Phase 4: Finding-Bewertung

Jedes Finding nach diesem Schema dokumentieren:

```
## Finding F-[NR]: [Titel]

**MASWE-ID**: MASWE-[XXXX]
**MASVS-Kontrolle**: MASVS-[CATEGORY]-[N]
**Plattform**: Android / iOS / Beide
**Schweregrad**: Kritisch / Hoch / Mittel / Niedrig / Info

### Beschreibung
[Was wurde gefunden]

### Auswirkung
[Welches Risiko entsteht]

### Reproduktion
[Schritte zur Reproduktion]

### Empfehlung
[Konkrete Maßnahmen zur Behebung]

### Referenzen
- MASWE: [Link/ID]
- MASTG-Test: [Relevanter Test]
- CWE: [Zugehörige CWE-ID falls anwendbar]
```

### Phase 5: Risikobewertung

Schweregrad-Matrix (CVSS-angelehnt):

| Schweregrad | Beschreibung | CVSS-Bereich |
|---|---|---|
| Kritisch | Direkter Zugriff auf sensible Daten, Remote Code Execution | 9.0–10.0 |
| Hoch | Umgehung von Sicherheitskontrollen, Datenleck mit hohem Impact | 7.0–8.9 |
| Mittel | Eingeschränktes Datenleck, lokaler Angriff erforderlich | 4.0–6.9 |
| Niedrig | Informationsleck mit geringem Impact, Härtungsmangel | 0.1–3.9 |
| Info | Best-Practice-Empfehlung, kein direktes Sicherheitsrisiko | 0.0 |

---

## Audit-Ausgabeformate

Je nach Bedarf erstelle:
1. **Checkliste** (Markdown/DOCX): Vollständige MASVS-Checkliste mit Status
2. **Executive Summary**: Management-taugliche Zusammenfassung
3. **Technischer Bericht**: Detaillierte Findings mit Reproduktionsschritten
4. **Risikoübersicht**: Priorisierte Finding-Liste nach Schweregrad
5. **Maßnahmenplan**: Empfehlungen mit Priorität und Aufwandsschätzung

Für die Berichterstellung den Skill `mas-report` verwenden.

---

## Verifikation

Bevor das Ergebnis dem Benutzer präsentiert wird:

1. Alle 8 MASVS-Kategorien in der Checkliste enthalten
2. Alle Kontrollen der gewählten Kategorie(n) abgedeckt
3. Jedes Finding hat eine MASWE-ID-Zuordnung
4. Schweregrad ist für jedes Finding gesetzt
5. Empfehlungen sind konkret und umsetzbar (keine generischen Ratschläge)
6. Plattform-spezifische Unterschiede (Android vs. iOS) berücksichtigt
7. Testprofil (L1/L2/R) korrekt angewendet

Bei Fehlern: Automatisch korrigieren und erneut prüfen.
