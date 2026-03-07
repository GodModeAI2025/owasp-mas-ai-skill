# Threat Model Template – Mobile Application (STRIDE + MASVS)

## 1. Dokumentinformationen

| Feld | Wert |
|---|---|
| App-Name | [Name] |
| Version | [Version] |
| Plattform | Android / iOS / Beide |
| Datum | [Datum] |
| Ersteller | [Name] |
| Status | Draft / Review / Final |
| MASVS-Profil | L1 / L2 / R |

---

## 2. System-Kontext

### 2.1 Geschäftszweck
[Beschreibung der App und ihres Zwecks]

### 2.2 Datenkategorien

| Kategorie | Beispiele | Schutzklasse | DSGVO-relevant |
|---|---|---|---|
| Credentials | Passwort, Token, API-Key | Kritisch | Ja |
| PII | Name, Email, Adresse | Hoch | Ja |
| Finanzdaten | IBAN, Transaktionen | Kritisch | Ja |
| Gesundheitsdaten | Diagnosen, Medikation | Kritisch | Ja (Art. 9) |
| Gerätedaten | Device-ID, OS-Version | Mittel | Möglich |
| Nutzungsdaten | Analytics, Logs | Niedrig–Mittel | Möglich |

### 2.3 Akteure

| Akteur | Beschreibung | Vertrauensstufe |
|---|---|---|
| Endbenutzer | Normaler App-Nutzer | Teilvertrauenswürdig |
| Admin | Backend-Administrator | Vertrauenswürdig |
| Angreifer (lokal) | Physischer Zugriff auf Gerät | Nicht vertrauenswürdig |
| Angreifer (remote) | Netzwerk-Angreifer | Nicht vertrauenswürdig |
| Angreifer (Malware) | Schadsoftware auf Gerät | Nicht vertrauenswürdig |
| Drittanbieter-SDK | Integrierte Bibliotheken | Eingeschränkt vertrauenswürdig |

---

## 3. Architekturübersicht

### 3.1 Komponentendiagramm
[Hier Architekturdiagramm einfügen – siehe mas-architect Skill]

### 3.2 Datenflüsse

| ID | Von | Nach | Daten | Kanal | Verschlüsselung |
|---|---|---|---|---|---|
| DF-01 | App | API Server | Credentials | HTTPS | TLS 1.2+ |
| DF-02 | App | Local DB | User-Daten | Lokal | AES-256-GCM |
| DF-03 | API Server | App | Session Token | HTTPS | TLS 1.2+ |
| DF-04 | App | Analytics | Events | HTTPS | TLS 1.2+ |
| DF-05 | App | Push Service | Registration | HTTPS | TLS 1.2+ |

### 3.3 Trust Boundaries

| ID | Boundary | Beschreibung |
|---|---|---|
| TB-01 | App ↔ OS | Plattform-APIs, Keystore, Permissions |
| TB-02 | App ↔ Backend | Netzwerk-Kommunikation |
| TB-03 | App ↔ Andere Apps | IPC, Deep Links, Intents |
| TB-04 | App ↔ User | UI, Eingaben, Biometrie |
| TB-05 | App ↔ 3rd Party | SDK-Calls, WebViews |

---

## 4. STRIDE-Analyse

### 4.1 Pro Trust Boundary

#### TB-01: App ↔ OS

| Threat | Szenario | MASVS | Maßnahme | Risiko |
|---|---|---|---|---|
| S – Spoofing | Malware gibt sich als OS-Service aus | AUTH-3 | Platform-API-Nutzung | Mittel |
| T – Tampering | Root/JB modifiziert App-Daten | RESILIENCE-3 | Integrity Checks | Hoch |
| R – Repudiation | Keine Audit-Logs für kritische Aktionen | STORAGE-1 | Secure Logging | Niedrig |
| I – Info Disclosure | Dateisystem-Zugriff bei Root/JB | STORAGE-1 | Verschlüsselung | Hoch |
| D – DoS | OS-Level Ressourcen-Erschöpfung | CODE-1 | Graceful Degradation | Niedrig |
| E – Elevation | Keystore-Bypass bei Root | CRYPTO-2 | Hardware-Backed + Detection | Hoch |

#### TB-02: App ↔ Backend

| Threat | Szenario | MASVS | Maßnahme | Risiko |
|---|---|---|---|---|
| S – Spoofing | MitM mit gefälschtem Zertifikat | NETWORK-2 | Certificate Pinning | Hoch |
| T – Tampering | API-Request-Manipulation | CODE-2 | Server-Side Validation | Hoch |
| R – Repudiation | Transaktions-Abstreitbarkeit | AUTH-1 | Signed Requests | Mittel |
| I – Info Disclosure | Traffic Sniffing | NETWORK-1 | TLS 1.2+ erzwungen | Hoch |
| D – DoS | API-Rate-Limiting fehlt | CODE-1 | Server-Side Throttling | Mittel |
| E – Elevation | Token-Manipulation | AUTH-3 | Server-Side Auth | Kritisch |

#### TB-03: App ↔ Andere Apps

| Threat | Szenario | MASVS | Maßnahme | Risiko |
|---|---|---|---|---|
| S – Spoofing | Deep-Link-Hijacking | PLATFORM-2 | App Links / Universal Links | Hoch |
| T – Tampering | IPC-Daten-Manipulation | PLATFORM-2 | Input Validation | Hoch |
| I – Info Disclosure | Exported Provider legt Daten frei | PLATFORM-2 | Permission-Schutz | Hoch |
| E – Elevation | Intent-Injection | PLATFORM-2 | Explizite Intents | Mittel |

#### TB-04: App ↔ User

| Threat | Szenario | MASVS | Maßnahme | Risiko |
|---|---|---|---|---|
| S – Spoofing | Overlay/Tapjacking | PLATFORM-1 | filterTouchesWhenObscured | Hoch |
| I – Info Disclosure | Screenshot sensibler Daten | PLATFORM-1 | FLAG_SECURE | Mittel |
| I – Info Disclosure | Clipboard-Leak | STORAGE-2 | Clipboard-Schutz | Mittel |

#### TB-05: App ↔ 3rd Party

| Threat | Szenario | MASVS | Maßnahme | Risiko |
|---|---|---|---|---|
| I – Info Disclosure | SDK sammelt zu viele Daten | PRIVACY-1 | SDK-Audit, Minimierung | Hoch |
| T – Tampering | Kompromittiertes SDK | CODE-4 | Dependency-Scanning | Hoch |
| E – Elevation | WebView-JS-Bridge-Exploit | PLATFORM-3 | Minimale Bridge | Hoch |

---

## 5. Risikobewertung

### 5.1 Risikomatrix

| Auswirkung ↓ / Wahrscheinlichkeit → | Niedrig | Mittel | Hoch |
|---|---|---|---|
| **Kritisch** | Hoch | Kritisch | Kritisch |
| **Hoch** | Mittel | Hoch | Kritisch |
| **Mittel** | Niedrig | Mittel | Hoch |
| **Niedrig** | Info | Niedrig | Mittel |

### 5.2 Top-Risiken

| Rang | Risiko | Schweregrad | MASVS | Maßnahme |
|---|---|---|---|---|
| 1 | [Höchstes Risiko] | Kritisch | [Kontrolle] | [Maßnahme] |
| 2 | ... | ... | ... | ... |
| 3 | ... | ... | ... | ... |

---

## 6. Maßnahmenplan

| ID | Maßnahme | MASVS | Priorität | Aufwand | Verantwortlich |
|---|---|---|---|---|---|
| M-01 | [Maßnahme] | [Kontrolle] | Kritisch | [Tage] | [Rolle] |
| M-02 | ... | ... | ... | ... | ... |

---

## 7. Restrisiken

| ID | Risiko | Begründung für Akzeptanz | Genehmigt durch |
|---|---|---|---|
| RR-01 | [Beschreibung] | [Warum akzeptabel] | [Rolle/Name] |

---

## 8. Änderungshistorie

| Version | Datum | Autor | Änderung |
|---|---|---|---|
| 0.1 | [Datum] | [Name] | Erstversion |
