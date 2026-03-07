---
name: mas-architect
description: OWASP MAS Security Architecture Review für mobile Anwendungen. Bewertet Sicherheitsarchitekturen, erstellt Threat Models, definiert Sicherheitsanforderungen und prüft Designentscheidungen gegen MASVS v2.1. Verwende diesen Skill IMMER bei: Mobile Security Architecture, Sicherheitsarchitektur mobile App, Threat Modeling mobile App, Security Requirements mobile, Design Review Sicherheit, Architektur-Review App, MASVS-Anforderungen definieren, Sicherheitskonzept mobile App, Risikoanalyse Architektur, Data Flow Diagramm mobil, Trust Boundaries mobile App, Attack Surface Analyse mobil, Security by Design mobile, Zero Trust mobile, Defense-in-Depth mobile App.
---

# OWASP MAS Security Architect

Sicherheitsarchitektur-Review und Threat Modeling für mobile Anwendungen nach MASVS v2.1.

---

## Wann diesen Skill verwenden

- Sicherheitsarchitektur einer mobilen App bewerten
- Threat Model für eine mobile App erstellen
- Sicherheitsanforderungen aus MASVS ableiten
- Designentscheidungen unter Sicherheitsaspekten bewerten
- Security-Architecture-Review vor Entwicklungsbeginn
- Attack-Surface-Analyse einer mobilen Anwendung

---

## Referenzdateien

Bei Bedarf lesen:
- `references/threat-model-template.md` – STRIDE-basiertes Threat-Model-Template
- Skill `mas-auditor/references/masvs-controls.md` – MASVS-Kontrollen
- Skill `mas-weakness/references/maswe-catalog.md` – MASWE Schwachstellen

---

## Architektur-Review-Workflow

### Phase 1: System verstehen

Kläre mit dem Benutzer:
1. **App-Zweck**: Was macht die App? Welche Geschäftsprozesse?
2. **Datenkategorien**: PII, Finanzdaten, Gesundheitsdaten, Credentials, Business-Daten
3. **Architekturtyp**: Native, Hybrid, Cross-Platform, PWA
4. **Backend-Anbindung**: REST API, GraphQL, WebSocket, gRPC
5. **Drittanbieter-SDKs**: Analytics, Crash-Reporting, Payment, Push
6. **Authentifizierung**: OAuth2, SAML, Custom, Biometrie
7. **Regulatorischer Rahmen**: DSGVO, PCI DSS, HIPAA, KRITIS, branchenspezifisch
8. **Schutzbedarf**: Welches Testprofil (L1/L2/R)?

### Phase 2: Architekturdiagramm erstellen

Erstelle ein Architekturdiagramm mit diesen Ebenen:

```
┌─────────────────────────────────────────┐
│              MOBILE CLIENT               │
│  ┌───────────┐  ┌──────────┐  ┌───────┐ │
│  │ UI Layer  │  │ Business │  │ Data  │ │
│  │           │  │  Logic   │  │ Layer │ │
│  └───────────┘  └──────────┘  └───────┘ │
│  ┌───────────┐  ┌──────────┐  ┌───────┐ │
│  │ WebViews  │  │ Crypto   │  │ Local │ │
│  │           │  │ Module   │  │ Store │ │
│  └───────────┘  └──────────┘  └───────┘ │
│  ┌───────────┐  ┌──────────┐  ┌───────┐ │
│  │   IPC     │  │   Auth   │  │ Cache │ │
│  │ Channels  │  │ Module   │  │       │ │
│  └───────────┘  └──────────┘  └───────┘ │
└─────────────────┬───────────────────────┘
                  │ TLS 1.2+
                  │ Certificate Pinning
┌─────────────────┴───────────────────────┐
│            BACKEND SERVICES              │
│  ┌──────┐  ┌──────┐  ┌───────┐  ┌─────┐│
│  │ API  │  │ Auth │  │ Push  │  │ CDN ││
│  │Server│  │Server│  │Service│  │     ││
│  └──────┘  └──────┘  └───────┘  └─────┘│
└─────────────────────────────────────────┘
┌─────────────────────────────────────────┐
│         THIRD-PARTY SERVICES             │
│  Analytics │ Payment │ Maps │ Social    │
└─────────────────────────────────────────┘
```

### Phase 3: Trust Boundaries identifizieren

Definiere Trust Boundaries:
1. **App ↔ OS**: Plattform-APIs, Keystore/Keychain, Permissions
2. **App ↔ Netzwerk**: TLS-Verbindungen, API-Calls
3. **App ↔ Backend**: Authentifizierung, Autorisierung, Input-Validation
4. **App ↔ Andere Apps**: IPC, Deep Links, Shared Storage
5. **App ↔ Benutzer**: UI, Biometrie, Eingabevalidierung
6. **App ↔ Drittanbieter**: SDK-Integrationen, WebViews

### Phase 4: STRIDE Threat Modeling

Für jede Trust Boundary STRIDE anwenden:

| Threat | Beschreibung | MASVS-Bezug |
|---|---|---|
| **S**poofing | Identitätsvortäuschung | MASVS-AUTH |
| **T**ampering | Datenmanipulation | MASVS-RESILIENCE, MASVS-CODE |
| **R**epudiation | Abstreitbarkeit | MASVS-AUTH, MASVS-STORAGE |
| **I**nformation Disclosure | Informationsleck | MASVS-STORAGE, MASVS-NETWORK, MASVS-PRIVACY |
| **D**enial of Service | Verfügbarkeitsstörung | MASVS-CODE, MASVS-NETWORK |
| **E**levation of Privilege | Rechteeskalation | MASVS-AUTH, MASVS-PLATFORM |

### Phase 5: MASVS-Anforderungsmatrix

Erstelle eine Matrix: Architekturelement → MASVS-Kontrolle → Anforderung

```
Element             | MASVS-Kontrolle  | Anforderung                    | Priorität
--------------------|-------------------|--------------------------------|----------
Local Database      | STORAGE-1         | SQLCipher-Verschlüsselung     | Hoch
API Communication   | NETWORK-1,2       | TLS 1.2+ mit Pinning          | Kritisch
Login Module        | AUTH-1,2,3        | OAuth2 + Biometrie + MFA      | Kritisch
WebView Integration | PLATFORM-3        | JS deaktiviert, Whitelist      | Hoch
User Tracking       | PRIVACY-1,2,3,4   | ATT, Opt-in, Minimierung      | Hoch
App Binary          | RESILIENCE-1,2,3  | Obfuskation + Integrity Check  | Mittel
```

### Phase 6: Attack Surface Analyse

Bewerte die Angriffsoberfläche:

**Lokale Angriffsvektoren:**
- Dateisystem-Zugriff (Root/Jailbreak)
- Backup-Extraktion
- Clipboard/Screenshot
- Memory Dump
- Binary-Reverse-Engineering

**Netzwerk-Angriffsvektoren:**
- Man-in-the-Middle
- API-Manipulation
- Session Hijacking
- Replay-Angriffe

**Plattform-Angriffsvektoren:**
- IPC-Interception
- Deep-Link-Hijacking
- Overlay-Angriffe
- WebView-Exploitation

**Benutzer-seitige Angriffsvektoren:**
- Social Engineering
- Phishing
- Credential Theft

### Phase 7: Security Architecture Decision Records

Dokumentiere jede Sicherheitsentscheidung:

```
## SADR-[NR]: [Titel]

**Status**: Akzeptiert / In Diskussion / Abgelehnt
**Kontext**: [Warum diese Entscheidung nötig ist]
**MASVS-Bezug**: MASVS-[CATEGORY]-[N]
**Entscheidung**: [Was entschieden wurde]
**Begründung**: [Warum diese Option gewählt wurde]
**Alternativen**: [Verworfene Optionen und Gründe]
**Risiko**: [Verbleibendes Restrisiko]
**Verantwortlich**: [Rolle/Person]
```

---

## Sicherheitsarchitektur-Patterns

### Defense-in-Depth für Mobile
1. **Transport**: TLS 1.2+ mit Certificate Pinning
2. **Speicherung**: Hardware-Backed Keystore/Secure Enclave
3. **Authentifizierung**: MFA + Biometrie + Token-Rotation
4. **Autorisierung**: Serverseitig + Client-Caching
5. **Code**: Obfuskation + Integrity Checks
6. **Runtime**: Root/JB-Detection + Anti-Tampering
7. **Privacy**: Datenminimierung + On-Device-Processing

### Zero Trust für Mobile
- Jede API-Anfrage authentifizieren und autorisieren
- Gerätezustand bei jeder Anfrage mitbewerten (Device Attestation)
- Keine implizite Vertrauensstellung
- Kontinuierliche Validierung statt einmaliger Prüfung
- Minimale Daten auf dem Gerät

---

## Verifikation

Bevor Architektur-Empfehlungen präsentiert werden:
1. Alle 8 MASVS-Kategorien in der Analyse berücksichtigt
2. Trust Boundaries vollständig identifiziert
3. STRIDE auf alle Boundaries angewendet
4. Empfehlungen sind architektonisch (nicht nur Code-Level)
5. Regulatorischer Kontext berücksichtigt
6. Restrisiken explizit dokumentiert
