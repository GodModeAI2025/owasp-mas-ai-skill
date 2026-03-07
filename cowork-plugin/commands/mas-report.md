# /owasp-mas:report

Generiert einen OWASP MAS Sicherheitsbericht.

## Verwendung
```
/owasp-mas:report [typ] [format]
```

## Parameter
- `typ`: `executive`, `technical`, `compliance`, `gap`, `action` (Standard: `technical`)
- `format`: `md`, `docx`, `pptx` (Standard: `md`)

## Beispiele
```
/owasp-mas:report executive pptx
/owasp-mas:report technical docx
/owasp-mas:report compliance md
/owasp-mas:report gap md
/owasp-mas:report action docx
```

## Verhalten
1. Fragt nach vorhandenen Findings oder startet Erfassung
2. Generiert Bericht im gewählten Format
3. Bei `docx` wird der `docx`-Skill verwendet
4. Bei `pptx` wird der `slide-master`- oder `pptx`-Skill verwendet
5. Nutzt den `mas-report` Skill

## Ausgabe
Professioneller Sicherheitsbericht als Datei.
