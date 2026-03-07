# /owasp-mas:check

Schnelle MASVS-Compliance-Prüfung für eine spezifische Kategorie oder Kontrolle.

## Verwendung
```
/owasp-mas:check [kategorie-oder-kontrolle]
```

## Parameter
- `kategorie-oder-kontrolle`: MASVS-Kategorie oder einzelne Kontrolle
  - Kategorien: `STORAGE`, `CRYPTO`, `AUTH`, `NETWORK`, `PLATFORM`, `CODE`, `RESILIENCE`, `PRIVACY`
  - Kontrollen: `STORAGE-1`, `CRYPTO-2`, `AUTH-3`, etc.

## Beispiele
```
/owasp-mas:check CRYPTO
/owasp-mas:check AUTH-2
/owasp-mas:check NETWORK
```

## Verhalten
1. Zeigt alle Kontrollen der Kategorie (oder die einzelne Kontrolle)
2. Listet zugehörige MASWE-Schwachstellen
3. Gibt konkrete Prüfschritte und erwartete Ergebnisse
4. Nutzt `mas-auditor` und `mas-weakness` Skills

## Ausgabe
Kompakte Prüfanleitung mit Checkliste für die gewählte Kategorie/Kontrolle.
