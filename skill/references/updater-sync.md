# OWASP MAS Skill – Update & Sync Referenz

## Inhaltsverzeichnis
- [Update-Workflow](#update-workflow)
- [Versionscheck](#versionscheck)
- [MASVS Sync](#masvs-sync)
- [MASWE Sync](#maswe-sync)
- [MASTG Sync](#mastg-sync)
- [Rebuild-Logik](#rebuild-logik)
- [Validierung](#validierung)
- [Ausgabe](#ausgabe)

---

## Update-Workflow

### Übersicht

```
1. Versionscheck    → GitHub API: Releases/Tags aller 3 Repos prüfen
2. Delta ermitteln  → Welche Repos haben neue Versionen?
3. Rohdaten fetchen → Neue Inhalte von GitHub/Website laden
4. Referenzen neu   → Betroffene references/*.md neu generieren
   generieren
5. SKILL.md patchen → Versionsnummern aktualisieren
6. Validieren       → Vollständigkeitsprüfung
7. Ausgeben         → Neuen .skill-ZIP erstellen
```

### Aktuelle Baseline

Bei jedem Update diese Baseline mit den Ergebnissen vergleichen:

```
MASVS: v2.1.0 (Stand: März 2026)
MASWE: Kein formelles Release-Tagging (commit-basiert, Stand: März 2026)
MASTG: v1.7.0 / v2.0.0-beta (Stand: März 2026)

Kontrollen:  24 (8 Kategorien)
Schwachstellen: 117 MASWE-IDs
Rollen: 7 (Auditor, Entwickler, Architekt, Reporter, Analyzer, Updater, Agent-Output)
Referenzdateien: 9
Gesamtzeilen: 3099
Skill-Version: 1.1.0
```

---

## Versionscheck

### Schritt 1: GitHub Releases und Tags abfragen

Für jedes Repo die neueste Version ermitteln. Nutze `web_search` und `web_fetch`:

**MASVS:**
```
web_search: "OWASP MASVS latest release site:github.com"
web_fetch:  https://github.com/OWASP/masvs/releases/latest
```

**MASWE:**
```
web_search: "OWASP MASWE latest changes site:github.com"
web_fetch:  https://github.com/OWASP/maswe/commits/main
```
MASWE hat keine formellen Releases. Prüfe den letzten Commit auf main.

**MASTG:**
```
web_search: "OWASP MASTG latest release site:github.com"
web_fetch:  https://github.com/OWASP/mastg/releases/latest
```

### Schritt 2: Delta-Report erstellen

Vergleiche mit der Baseline und erstelle einen Delta-Report:

```markdown
## OWASP MAS Update Check – [Datum]

| Repo | Baseline | Aktuell | Delta |
|---|---|---|---|
| MASVS | v2.1.0 | [Version] | [Neu/Unverändert] |
| MASWE | März 2026 | [Letzter Commit] | [X neue Schwachstellen] |
| MASTG | v1.7.0 | [Version] | [Neu/Unverändert] |

### Änderungen
- [Auflistung der Änderungen pro Repo]
```

Dem Benutzer den Delta-Report zeigen und bestätigen lassen, bevor der Rebuild startet.

---

## MASVS Sync

### Neue Kontrollen und Kategorien laden

**Quelle:** https://mas.owasp.org/MASVS/

Für jede Kategorie die Kontrollen abrufen:

```
web_fetch: https://mas.owasp.org/MASVS/05-MASVS-STORAGE/
web_fetch: https://mas.owasp.org/MASVS/06-MASVS-CRYPTO/
web_fetch: https://mas.owasp.org/MASVS/07-MASVS-AUTH/
web_fetch: https://mas.owasp.org/MASVS/08-MASVS-NETWORK/
web_fetch: https://mas.owasp.org/MASVS/09-MASVS-PLATFORM/
web_fetch: https://mas.owasp.org/MASVS/10-MASVS-CODE/
web_fetch: https://mas.owasp.org/MASVS/11-MASVS-RESILIENCE/
web_fetch: https://mas.owasp.org/MASVS/12-MASVS-PRIVACY/
```

Für einzelne Kontrollen:
```
web_fetch: https://mas.owasp.org/MASVS/controls/MASVS-STORAGE-1/
web_fetch: https://mas.owasp.org/MASVS/controls/MASVS-STORAGE-2/
... (alle 24)
```

### Rebuild: references/masvs-controls.md

Aus den geladenen Daten die Referenzdatei neu generieren. Struktur beibehalten:

```markdown
# OWASP MASVS v[NEUE VERSION] – Vollständige Kontrollreferenz

## MASVS-STORAGE
### MASVS-STORAGE-1
**[Kontrolltitel aus Quelle]**
- [Anforderungspunkte aus Quelle]
...
```

**Prüfpunkte nach Rebuild:**
- Alle Kategorien vorhanden (STORAGE, CRYPTO, AUTH, NETWORK, PLATFORM, CODE, RESILIENCE, PRIVACY)
- Alle Kontrollen vorhanden (Anzahl mit Baseline vergleichen – neue zählen)
- Keine leeren Sektionen
- Kontrolltitel und -beschreibungen vollständig

---

## MASWE Sync

### Neue Schwachstellen laden

**Quelle:** https://mas.owasp.org/MASWE/

Kategorieübersichten abrufen:
```
web_fetch: https://mas.owasp.org/MASWE/MASVS-STORAGE/
web_fetch: https://mas.owasp.org/MASWE/MASVS-CRYPTO/
web_fetch: https://mas.owasp.org/MASWE/MASVS-AUTH/
web_fetch: https://mas.owasp.org/MASWE/MASVS-NETWORK/
web_fetch: https://mas.owasp.org/MASWE/MASVS-PLATFORM/
web_fetch: https://mas.owasp.org/MASWE/MASVS-CODE/
web_fetch: https://mas.owasp.org/MASWE/MASVS-RESILIENCE/
web_fetch: https://mas.owasp.org/MASWE/MASVS-PRIVACY/
```

Daraus die MASWE-ID-Liste extrahieren. Neue IDs identifizieren (nicht in Baseline).

Für neue Schwachstellen Details laden:
```
web_fetch: https://mas.owasp.org/MASWE/MASVS-[CAT]/MASWE-[XXXX]/
```

### Rebuild: references/maswe-catalog.md

Bestehenden Katalog um neue Schwachstellen ergänzen. Für jede neue ID:

```markdown
| MASWE-[XXXX] | [Titel] | [Schweregrad] |
```

Schweregrad nach diesen Regeln zuweisen (wenn nicht von OWASP angegeben):
- Credential-Exposition, RCE-Enabler → Kritisch
- Auth-Bypass, Datenleck → Hoch
- Lokaler Angriff nötig, eingeschränktes Leck → Mittel
- Info-Leak, Härtungsmangel → Niedrig

**Prüfpunkte nach Rebuild:**
- Alle MASWE-IDs aus der Quelle vorhanden
- Keine Duplikate
- Jede Kategorie hat mindestens 1 Schwachstelle
- Neue IDs sind im Delta-Report gelistet

---

## MASTG Sync

### Neue Tests und Tools laden

**Quelle:** https://mas.owasp.org/MASTG/

Prüfe auf:
1. Neue Test-IDs (MASTG-TEST-XXXX)
2. Neue Techniken
3. Neue Tools
4. Geänderte Best Practices

```
web_fetch: https://mas.owasp.org/MASTG/tests/
web_fetch: https://mas.owasp.org/MASTG/techniques/
web_fetch: https://mas.owasp.org/MASTG/tools/
web_fetch: https://mas.owasp.org/MASTG/best-practices/
```

### Rebuild: references/mastg-testing.md

Betroffene Sektionen aktualisieren:
- Tool-Tabelle: Neue Tools ergänzen, veraltete markieren
- Test-Case-Mapping: Neue MASVS→Test-Zuordnungen
- Techniken: Neue Techniken ergänzen

### Rebuild: references/secure-coding-android.md + secure-coding-ios.md

Nur anpassen wenn:
- Neue API-Empfehlungen (z.B. neue Android Keystore APIs, neue iOS CryptoKit-Features)
- Veraltete APIs (Deprecations)
- Neue Plattform-Versionen mit Security-relevanten Änderungen

---

## Rebuild-Logik

### Vollständiger Rebuild

Wenn der Benutzer einen vollständigen Rebuild anfordert, alle Referenzen neu generieren:

```bash
# Arbeitsverzeichnis
mkdir -p /home/claude/owasp-mas-update/skill/references

# 1. MASVS Controls neu generieren
# → references/masvs-controls.md

# 2. MASWE Catalog neu generieren
# → references/maswe-catalog.md

# 3. MASTG Testing neu generieren
# → references/mastg-testing.md

# 4. Secure Coding prüfen und ggf. aktualisieren
# → references/secure-coding-android.md
# → references/secure-coding-ios.md

# 5. Threat Model Template prüfen
# → references/threat-model-template.md (selten Änderungen)

# 6. Report Templates prüfen
# → references/report-templates.md (CWE-Mapping bei neuen MASWE-IDs aktualisieren)

# 7. Agent Output Schemata prüfen
# → references/agent-output.md (Schema-Version prüfen, neue downstream_triggers
#    bei neuen MASWE-Kategorien, agent_summary-Felder bei neuen Kontrollen)

# 8. Updater-Sync selbst aktualisieren
# → references/updater-sync.md (Baseline-Werte, Mindestwerte, URLs bei Repo-Umzug)

# 9. SKILL.md patchen
# → Baseline-Block aktualisieren (Versionen, MASWE-ID-Zählung)
# → Rollenauswahl-Tabelle prüfen (neue Referenzen erreichbar?)
# → Description prüfen (neue Trigger-Begriffe bei neuen Kategorien?)
# → Version hochzählen

# 10. Validierung
# → Alle 8+ Kategorien? Alle Kontrollen? Alle MASWE-IDs?
# → Alle 9 Referenzdateien vorhanden und nicht-leer?
# → Agent-Output-Schemata konsistent mit Finding-Schema?

# 11. ZIP erstellen
# → owasp-mas.skill
```

### Partieller Rebuild

Wenn nur ein Repo aktualisiert wurde:

| Repo geändert | Betroffene Referenzen | Kaskade |
|---|---|---|
| Nur MASVS | masvs-controls.md | + agent-output.md (compliance-Schema prüfen) |
| Nur MASWE | maswe-catalog.md | + report-templates.md (CWE-Mapping), + agent-output.md (weakness-Schema) |
| Nur MASTG | mastg-testing.md, ggf. secure-coding-*.md | + agent-output.md (secure_coding-Schema) |
| MASVS + MASWE | masvs-controls.md, maswe-catalog.md | + report-templates.md, + agent-output.md |
| Alle drei | Vollständiger Rebuild aller 9 Referenzen | Immer auch updater-sync.md Baseline anpassen |

---

## Validierung

### Nach jedem Rebuild diese Prüfungen ausführen:

```bash
# 1. Alle MASVS-Kategorien vorhanden
for cat in STORAGE CRYPTO AUTH NETWORK PLATFORM CODE RESILIENCE PRIVACY; do
    grep -c "MASVS-$cat" references/masvs-controls.md
done

# 2. Kontrollen-Zählung (mindestens 24)
grep -c "^### MASVS-" references/masvs-controls.md

# 3. MASWE-IDs zählen (mindestens 117)
grep -oP "MASWE-\d+" references/maswe-catalog.md | sort -u | wc -l

# 4. Alle 9 Referenzdateien vorhanden und nicht-leer
expected="agent-output mastg-testing masvs-controls maswe-catalog report-templates secure-coding-android secure-coding-ios threat-model-template updater-sync"
for name in $expected; do
    f="references/${name}.md"
    if [ -f "$f" ]; then
        lines=$(wc -l < "$f")
        echo "✅ $f: $lines lines"
    else
        echo "❌ $f: MISSING"
    fi
done

# 5. SKILL.md Rollenreferenzen intakt (7 Rollen)
grep -c "## Rolle:" SKILL.md

# 6. SKILL.md Routing-Tabelle referenziert alle Referenzdateien
for ref in agent-output mastg-testing masvs-controls maswe-catalog report-templates secure-coding-android secure-coding-ios threat-model-template updater-sync; do
    if grep -q "references/${ref}.md" SKILL.md; then
        echo "✅ $ref in Routing-Tabelle"
    else
        echo "⚠️ $ref nicht direkt in Routing-Tabelle (OK wenn nur via andere Rolle)"
    fi
done

# 7. Agent-Output-Schemata konsistent
grep -c "schema_version" references/agent-output.md

# 8. Keine Klartext-Secrets in agent-output.md
if grep -qiE "(password|secret|key):\s*\"[A-Za-z0-9]" references/agent-output.md; then
    echo "❌ Mögliche Klartext-Secrets in agent-output.md"
else
    echo "✅ Keine Klartext-Secrets"
fi

# 9. Description unter 1024 Zeichen
head -4 SKILL.md | python3 -c "
import sys
for l in sys.stdin:
    if 'description:' in l:
        d = l.split('description:')[1].strip().strip('\"')
        print(f'Description: {len(d)} chars {\"✅\" if len(d) <= 1024 else \"❌\"}')"
```

### Erwartete Mindestwerte

| Prüfpunkt | Minimum |
|---|---|
| MASVS-Kategorien | 8 |
| MASVS-Kontrollen | 24 |
| MASWE-IDs | 117 |
| Referenzdateien | 9 (inkl. agent-output.md + updater-sync.md) |
| SKILL.md Rollen | 7 |
| SKILL.md Routing-Einträge | 8+ |
| Description-Länge | ≤ 1024 Zeichen |
| Agent-Output schema_version | ≥ 1 |
| Gesamtzeilen alle .md | 3000+ |

---

## Ausgabe

### Update-Bericht dem Benutzer zeigen

```markdown
## OWASP MAS Skill Update – [Datum]

### Versionsstände
| Repo | Alt | Neu |
|---|---|---|
| MASVS | v[alt] | v[neu] |
| MASWE | [alt] IDs | [neu] IDs |
| MASTG | v[alt] | v[neu] |

### Änderungen
- [X] neue MASWE-Schwachstellen hinzugefügt: [IDs]
- [X] neue MASVS-Kontrollen: [IDs]
- [X] neue MASTG-Tests: [IDs]
- Secure-Coding-Referenzen: [aktualisiert/unverändert]
- Agent-Output-Schemata: [aktualisiert/unverändert]
- Report-Templates (CWE-Mapping): [aktualisiert/unverändert]

### Validierung
- MASVS-Kategorien: [8/8] ✅
- MASVS-Kontrollen: [X/X] ✅
- MASWE-IDs: [X] ✅
- Referenzdateien: [9/9] ✅
- SKILL.md Rollen: [7/7] ✅
- Description: [X] chars ≤ 1024 ✅
- Agent-Output schema_version: [X] ✅
- Keine Klartext-Secrets: ✅

### Skill-Version
[Alt] → [Neu]
```

### Neue .skill-Datei erstellen

```bash
cd /home/claude/owasp-mas-update/skill
zip -r /mnt/user-data/outputs/owasp-mas-v[VERSION].skill .
```

Datei dem Benutzer über `present_files` zur Verfügung stellen.

### CHANGELOG aktualisieren

Neuen Eintrag am Anfang der CHANGELOG.md:

```markdown
## [X.Y.0] – [Datum]

### Aktualisiert
- MASVS: v[alt] → v[neu]
- MASWE: [X] neue Schwachstellen ([IDs])
- MASTG: v[alt] → v[neu]

### Geänderte Referenzen
- [Liste der neu generierten Dateien]

### Quellen
- OWASP MASVS ([commit/tag])
- OWASP MASWE ([commit/tag])
- OWASP MASTG ([commit/tag])
```
