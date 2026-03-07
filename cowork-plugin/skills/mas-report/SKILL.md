---
name: mas-report
description: Erstellt professionelle OWASP MAS Sicherheitsberichte als DOCX, Markdown oder Präsentation. Unterstützt Executive Summary, technische Berichte, Compliance-Nachweise, Gap-Analysen und Maßnahmenpläne. Verwende diesen Skill IMMER bei: Sicherheitsbericht mobile App, Audit Report erstellen, MASVS Compliance Report, Pentest-Bericht mobile, Executive Summary Security, Gap-Analyse Bericht, Maßnahmenplan Sicherheit, Prüfbericht erstellen, Audit-Dokumentation, OWASP Report, Security Assessment Report, Compliance-Nachweis MASVS, Testbericht mobile Sicherheit, Findings-Report, Risikobericht App-Sicherheit.
---

# OWASP MAS Report Generator

Erstellt professionelle Sicherheitsberichte für mobile Anwendungen basierend auf OWASP MAS.

---

## Wann diesen Skill verwenden

- Sicherheitsbericht nach einem Mobile-Security-Audit
- Executive Summary für Management
- Compliance-Nachweis gegen MASVS
- Technischer Detailbericht mit Findings
- Gap-Analyse-Bericht
- Maßnahmenplan nach Sicherheitstest

---

## Berichtstypen

### 1. Executive Summary
Für: C-Level, Management, Entscheidungsträger

Struktur:
1. **Management Summary** (1 Seite)
   - Geprüfte App, Version, Zeitraum
   - Gesamtbewertung (Ampel: Rot/Gelb/Grün)
   - Kritische Findings (Top 3–5)
   - Handlungsempfehlung
2. **Findings-Übersicht** (1 Seite)
   - Verteilung nach Schweregrad (Tabelle + ggf. Grafik)
   - Verteilung nach MASVS-Kategorie
3. **Risikoeinschätzung** (1 Seite)
   - Gesamtrisiko
   - Top-Risiken mit Business-Impact
4. **Nächste Schritte**
   - Priorisierte Maßnahmen
   - Timeline-Empfehlung

### 2. Technischer Prüfbericht
Für: Security-Team, Entwickler, Architekten

Struktur:
1. **Deckblatt**
   - App-Name, Version, Paketname
   - Plattform (Android/iOS)
   - Testprofil (L1/L2/R)
   - Prüfzeitraum
   - Prüfer
   - Vertraulichkeitsstufe
2. **Scope & Methodik**
   - Prüfumfang
   - Ausschlüsse
   - Testmethodik (MASTG-basiert)
   - Testumgebung (Geräte, Tools, Versionen)
3. **Zusammenfassung**
   - Gesamtbewertung
   - Statistik: X Findings (Y Kritisch, Z Hoch, ...)
   - Positiv-Befunde (was gut gemacht wurde)
4. **Detaillierte Findings**
   - Pro Finding das vollständige Template (siehe mas-auditor)
   - Sortiert nach Schweregrad (Kritisch → Info)
   - Screenshots/Evidenz wo möglich referenziert
5. **MASVS Compliance Matrix**
   - Vollständige Checkliste mit Status pro Kontrolle
   - Prozentuale Erfüllung pro Kategorie
6. **Maßnahmenplan**
   - Priorisierte Empfehlungen
   - Aufwandsschätzung (S/M/L)
   - Verantwortliche Rolle
7. **Anhänge**
   - Tool-Konfigurationen
   - Testdaten
   - Glossar

### 3. Compliance-Nachweis
Für: Compliance-Abteilung, Auditoren, Regulatoren

Struktur:
1. **Compliance-Statement**
   - Geprüfter Standard (MASVS v2.1 + Profil)
   - Erfüllungsgrad
2. **Kontroll-Matrix**
   - Jede MASVS-Kontrolle mit Status und Nachweis
   - Verweise auf technische Tests (MASTG-Test-IDs)
3. **Abweichungen**
   - Nicht erfüllte Kontrollen mit Begründung
   - Kompensatorische Maßnahmen
   - Akzeptiertes Restrisiko
4. **Regulatorisches Mapping**
   - MASVS → DSGVO Art. 32 (Sicherheit der Verarbeitung)
   - MASVS → PCI DSS (Mobile Payment)
   - MASVS → BSI-Grundschutz
   - MASVS → ISO 27001 A.14 (System-Entwicklung)

### 4. Gap-Analyse-Bericht
Für: Projektmanagement, Security-Team

Struktur:
1. **Ist-Zustand**: Aktuelle MASVS-Erfüllung pro Kategorie
2. **Soll-Zustand**: Ziel-Profil und Anforderungen
3. **Gap-Tabelle**:
   | Kontrolle | Ist | Soll | Gap | Aufwand | Priorität |
   |---|---|---|---|---|---|
   | STORAGE-1 | Teilweise | Vollständig | Verschlüsselung fehlt | M | Hoch |
4. **Roadmap**: Phasenweise Schließung der Gaps

### 5. Maßnahmenplan
Für: Entwicklungsteam, Projektmanagement

Struktur:
1. **Sofortmaßnahmen** (0–2 Wochen): Kritische Findings
2. **Kurzfristig** (2–8 Wochen): Hohe Findings
3. **Mittelfristig** (2–6 Monate): Mittlere Findings, Architektur-Verbesserungen
4. **Langfristig** (6+ Monate): Strategische Verbesserungen, Tooling

---

## Berichtsformat

### DOCX-Erstellung
Für professionelle Berichte den `docx`-Skill nutzen:
- Corporate-Formatting (Deckblatt, Kopfzeilen, Seitenzahlen)
- Inhaltsverzeichnis
- Tabellenformatierung für Findings und Checklisten
- Fußnoten für Referenzen

### Markdown-Erstellung
Für schnelle Berichte direkt in Markdown:
- Klare Überschriften-Hierarchie
- Tabellen für Findings und Checklisten
- Code-Blöcke für technische Details

### Präsentation
Für Management-Präsentationen den `pptx`-Skill oder `slide-master`-Skill nutzen:
- Executive Summary auf 5–10 Folien
- Ampel-Darstellung
- Finding-Highlights mit Impact

---

## Finding-Schweregrad-Farbschema

| Schweregrad | Farbe | Hex |
|---|---|---|
| Kritisch | Dunkelrot | #CC0000 |
| Hoch | Rot | #FF3333 |
| Mittel | Orange | #FF9900 |
| Niedrig | Gelb | #FFCC00 |
| Info | Blau | #3399FF |

---

## Textbausteine

### Gesamtbewertung
- **Grün**: Die App erfüllt die MASVS-Anforderungen des gewählten Profils weitgehend. Vereinzelte Verbesserungspotenziale bestehen, stellen jedoch kein signifikantes Risiko dar.
- **Gelb**: Die App weist mehrere Schwachstellen auf, die zeitnah behoben werden sollten. Kritische Sicherheitslücken wurden nicht identifiziert, jedoch bestehen Risiken bei hohen und mittleren Findings.
- **Rot**: Die App weist kritische Sicherheitslücken auf, die ein unmittelbares Risiko darstellen. Eine Veröffentlichung oder Weiternutzung in der aktuellen Form wird nicht empfohlen.

### Disclaimer
> Dieser Bericht stellt den Sicherheitszustand der geprüften Anwendung zum Zeitpunkt der Prüfung dar. Er ist keine Garantie für die Abwesenheit von Sicherheitslücken. Die Prüfung basiert auf dem OWASP MASVS v2.1 und dem OWASP MASTG. Empfehlungen sollten im Kontext der individuellen Risikoakzeptanz und des regulatorischen Rahmens bewertet werden.

---

## Verifikation

Bevor ein Bericht dem Benutzer präsentiert wird:
1. Berichtstyp passt zum angegebenen Zielpublikum
2. Alle Findings enthalten Schweregrad, MASWE-ID und Empfehlung
3. MASVS-Compliance-Matrix ist vollständig (alle 8 Kategorien)
4. Keine Widersprüche zwischen Findings und Compliance-Status
5. Executive Summary ist Management-tauglich (kein Fachjargon ohne Erklärung)
6. Disclaimer ist enthalten
7. Vertraulichkeitsstufe ist angegeben
