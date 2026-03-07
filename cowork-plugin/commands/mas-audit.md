# /owasp-mas:audit

Startet einen OWASP MAS Sicherheitsaudit.

## Verwendung
```
/owasp-mas:audit [plattform] [profil]
```

## Parameter
- `plattform`: `android`, `ios` oder `both` (Standard: `both`)
- `profil`: `L1`, `L2` oder `R` (Standard: `L1`)

## Beispiele
```
/owasp-mas:audit android L2
/owasp-mas:audit ios R
/owasp-mas:audit both L1
```

## Verhalten
1. Fragt nach App-Name und Version falls nicht bekannt
2. Generiert vollständige MASVS-Checkliste für das gewählte Profil
3. Führt durch den Audit-Workflow (Scope → Checkliste → Test-Guidance → Finding-Bewertung)
4. Nutzt den `mas-auditor` Skill

## Ausgabe
Vollständige MASVS-Checkliste als Markdown oder DOCX (auf Wunsch).
