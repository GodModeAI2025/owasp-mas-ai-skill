# Changelog

## [1.0.0] – 2026-03-07

### Hinzugefügt
- **mas-auditor**: Vollständiger MASVS-Audit-Skill mit Checklisten-Generator, Finding-Templates, Risikobewertung
- **mas-developer**: Secure-Coding-Leitfaden für Android (Kotlin/Java) und iOS (Swift) mit Code-Beispielen
- **mas-architect**: Security Architecture Review mit STRIDE Threat Model, Trust Boundaries, Anforderungsmatrix
- **mas-report**: 5 Berichtstypen (Executive, Technical, Compliance, Gap, Action) mit Templates
- **mas-weakness**: Vollständiger MASWE-Katalog (118+ Schwachstellen) mit CWE- und Top-10-Mapping
- **Referenzen**: masvs-controls.md, maswe-catalog.md, mastg-testing.md, secure-coding-android.md, secure-coding-ios.md, threat-model-template.md
- **Commands**: /owasp-mas:audit, /owasp-mas:check, /owasp-mas:report, /owasp-mas:weakness

### Design-Entscheidungen
- 5 Skills statt einem Monolith-Skill für bessere Triggering-Genauigkeit nach Rolle
- Referenzdateien pro Skill statt zentral, um Context-Window-Verbrauch zu optimieren
- MASWE-Katalog mit Schweregrad-Bewertung und Prüfansatz für direkte Nutzbarkeit
- Plattform-getrennte Secure-Coding-Referenzen (Android/iOS) für Code-Review-Tauglichkeit
- STRIDE-basiertes Threat Modeling mit MASVS-Mapping für durchgängige Nachvollziehbarkeit
