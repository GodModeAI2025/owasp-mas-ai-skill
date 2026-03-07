# /owasp-mas:weakness

Sucht und analysiert OWASP MASWE Schwachstellen.

## Verwendung
```
/owasp-mas:weakness [id-oder-suchbegriff]
```

## Parameter
- `id-oder-suchbegriff`: MASWE-ID, MASVS-Kategorie, Thema oder Schweregrad
  - ID: `MASWE-0013`, `0013`, `13`
  - Kategorie: `STORAGE`, `CRYPTO`, `AUTH`, etc.
  - Thema: `krypto`, `webview`, `biometrie`, `pinning`, etc.
  - Schweregrad: `kritisch`, `hoch`, `mittel`, `niedrig`

## Beispiele
```
/owasp-mas:weakness MASWE-0013
/owasp-mas:weakness CRYPTO
/owasp-mas:weakness biometrie
/owasp-mas:weakness kritisch
```

## Verhalten
1. Sucht im MASWE-Katalog
2. Liefert detaillierte Informationen inkl. Erkennung und Behebung
3. Zeigt Korrelationen und verwandte Schwachstellen
4. Nutzt den `mas-weakness` Skill

## Ausgabe
Strukturierte Schwachstelleninformation mit Prüf- und Behebungshinweisen.
