# OWASP MASVS v2.1 – Vollständige Kontrollreferenz

## Inhaltsverzeichnis
- [MASVS-STORAGE](#masvs-storage)
- [MASVS-CRYPTO](#masvs-crypto)
- [MASVS-AUTH](#masvs-auth)
- [MASVS-NETWORK](#masvs-network)
- [MASVS-PLATFORM](#masvs-platform)
- [MASVS-CODE](#masvs-code)
- [MASVS-RESILIENCE](#masvs-resilience)
- [MASVS-PRIVACY](#masvs-privacy)

---

## MASVS-STORAGE

Sichere Speicherung sensibler Daten auf dem Gerät (Data-at-Rest).

### MASVS-STORAGE-1
**Die App speichert sensible Daten sicher.**
- Sensible Daten werden identifiziert und klassifiziert
- Daten werden in plattformspezifischen sicheren Speichern abgelegt (Keychain/Keystore)
- Keine sensiblen Daten in Logs, Backups oder Caches
- Zugriffsbeschränkungen auf interne Speicherorte

### MASVS-STORAGE-2
**Die App verhindert Datenlecks durch das System.**
- Clipboard-Schutz für sensible Felder
- Kein Leaking über Screenshots, Tastatur-Caches, Hintergrund-Snapshots
- Sensible Daten aus Backups ausgeschlossen
- Third-Party-Keyboards für sensible Eingaben deaktiviert

---

## MASVS-CRYPTO

Kryptographische Funktionalität zum Schutz sensibler Daten.

### MASVS-CRYPTO-1
**Die App verwendet bewährte kryptographische Verfahren korrekt.**
- Keine veralteten/unsicheren Algorithmen (MD5, SHA-1, DES, RC4)
- Korrekte Parametrisierung (IV, Padding, Modus)
- Sichere Zufallszahlengenerierung (CSPRNG)
- Korrekte Signaturerstellung und -prüfung
- Kein Eigenimplementierung von Krypto-Primitiven

### MASVS-CRYPTO-2
**Die App verwaltet kryptographische Schlüssel sicher.**
- Schlüssel werden im Hardware-Backed Keystore/Secure Enclave gespeichert
- Schlüsselgenerierung folgt Best Practices (Ableitungsfunktionen)
- Schlüsselrotation implementiert
- Keine hartcodierten Schlüssel
- Zugriffsbeschränkungen auf Schlüssel konfiguriert
- Schlüssel bei Export geschützt

---

## MASVS-AUTH

Authentifizierung und Autorisierung der mobilen App.

### MASVS-AUTH-1
**Die App verwendet sichere Authentifizierung.**
- MFA-Best-Practices umgesetzt
- Step-Up-Authentifizierung nach Login
- Re-Authentifizierung bei Kontextänderungen
- Plattform-APIs für Authentifizierung verwendet
- Protokoll-Sicherheit (OAuth2/OIDC Best Practices)
- Passwortlose Authentifizierung wo möglich

### MASVS-AUTH-2
**Die App schützt Authentifizierungsmaterial.**
- Credentials nicht unverschlüsselt gespeichert
- Tokens nicht über unsichere Verbindungen gesendet
- Token-Validierung implementiert
- Shared Web Credentials / App-Website-Association
- Keine unsichere Authentifizierung in WebViews

### MASVS-AUTH-3
**Die App erzwingt serverseitige Authentifizierung und Autorisierung.**
- Authentifizierung nicht nur lokal erzwungen
- Autorisierung serverseitig geprüft
- Custom-PIN an Platform KeyStore gebunden
- Biometrie-Bypass verhindert
- Fallback auf Non-Biometric bei sensiblen Transaktionen kontrolliert
- Krypto-Schlüssel bei neuer Biometrie-Enrollment invalidiert

---

## MASVS-NETWORK

Sichere Netzwerkkommunikation (Data-in-Transit).

### MASVS-NETWORK-1
**Die App sichert den Netzwerkverkehr.**
- TLS für alle Verbindungen
- Keine Cleartext-Kommunikation
- Bewährte Networking-APIs verwendet
- Keine offenen Ports
- Sichere Machine-to-Machine-Kommunikation

### MASVS-NETWORK-2
**Die App verifiziert die Identität des Kommunikationspartners.**
- Certificate Pinning korrekt implementiert
- Zertifikatsvalidierung nicht deaktiviert
- Keine Akzeptanz selbstsignierter Zertifikate in Production

---

## MASVS-PLATFORM

Sichere Interaktion mit der Plattform und anderen Apps.

### MASVS-PLATFORM-1
**Die App schützt sensible Daten auf der Benutzeroberfläche.**
- Kein Leaking über UI (Autocomplete, Text-Vorschläge)
- Sensible Daten nicht in Benachrichtigungen
- Screenshot-/Screen-Recording-Schutz
- Tapjacking-Schutz
- StrandHogg/Task-Affinity-Schutz

### MASVS-PLATFORM-2
**Die App nutzt sichere IPC-Mechanismen.**
- Deep Links sicher implementiert
- IPC authentifiziert
- UIActivity sicher verwendet (iOS)
- App Extensions sicher (iOS)
- Services/BroadcastReceivers/ContentProviders sicher (Android)
- Intents sicher konfiguriert (Android)
- Keine permanente Datenfreigabe an andere Apps
- Debug-Flag deaktiviert

### MASVS-PLATFORM-3
**Die App nutzt WebViews sicher.**
- JavaScript-Bridges kontrolliert
- Kein Zugriff auf lokale Ressourcen über WebViews
- JavaScript nur aus vertrauenswürdigen Quellen
- Content nur aus vertrauenswürdigen Quellen geladen
- Universal XSS verhindert
- WebResourceResponse sicher implementiert
- Web Content Debugging deaktiviert
- Sensible Daten nach Verwendung entfernt

---

## MASVS-CODE

Codequalität und sichere Build-Praktiken.

### MASVS-CODE-1
**Die App erzwingt App-Updates und Plattform-Aktualität.**
- Enforced Updating implementiert
- Sicherstellung aktueller Plattform-Version
- Neueste Plattform-Version als Target

### MASVS-CODE-2
**Die App validiert und sanitisiert alle Eingabedaten.**
- Sichere Verarbeitung von Netzwerkdaten
- Sichere Verarbeitung von Backup-Daten
- Sichere Verarbeitung externer Interfaces
- Sichere Verarbeitung lokaler Speicherdaten
- Sichere Verarbeitung von UI-Eingaben
- Sichere Verarbeitung von IPC-Daten

### MASVS-CODE-3
**Die App schützt gegen Injection-Angriffe.**
- Keine SQL-Injection
- Sicheres Parsing und Escaping
- Sichere Objekt-Deserialisierung
- Kein unsicheres dynamisches Code-Loading

### MASVS-CODE-4
**Die App nutzt Compiler-Sicherheitsfeatures und hat keine bekannten Schwachstellen.**
- Compiler-Sicherheitsfeatures aktiviert (PIE, Stack Canaries, ARC)
- Keine Dependencies mit bekannten Vulnerabilities

---

## MASVS-RESILIENCE

Resilienz gegen Reverse Engineering und Tampering.

### MASVS-RESILIENCE-1
**Die App nutzt Code-Obfuskation.**
- Code-Obfuskation implementiert
- Ressourcen-Obfuskation
- Anti-Deobfuskation-Techniken
- Statische Analysetools verhindert

### MASVS-RESILIENCE-2
**Die App enthält keine Debug-Artefakte.**
- Debugging-Symbole entfernt
- Nicht-Produktions-Ressourcen entfernt
- Code zum Deaktivieren von Sicherheitskontrollen entfernt

### MASVS-RESILIENCE-3
**Die App implementiert Anti-Tampering.**
- Integritätsprüfung der App
- Integritätsprüfung des Geräts
- Jailbreak/Root-Detection
- Emulator-Detection
- Laufzeitmanipulation verhindert (Hooking-Frameworks)

### MASVS-RESILIENCE-4
**Die App implementiert Anti-Reverse-Engineering.**
- Debugger-Detection und -Blockierung
- Anti-Debugging-Techniken

---

## MASVS-PRIVACY

Datenschutzkontrollen zum Schutz der Benutzer-Privatsphäre.

### MASVS-PRIVACY-1
**Die App minimiert den Zugriff auf sensible Daten und Ressourcen.**
- Nur notwendige Berechtigungen angefordert
- Datenminimierung umgesetzt
- Kein übermäßiges Tracking

### MASVS-PRIVACY-2
**Die App informiert transparent über Datenverarbeitung.**
- Datenschutzerklärung zugänglich und verständlich
- Zweck der Datenverarbeitung kommuniziert
- Nutzer über Tracking informiert

### MASVS-PRIVACY-3
**Die App gibt dem Nutzer Kontrolle über seine Daten.**
- Opt-in/Opt-out für Tracking
- Datenlöschung möglich
- Datenexport möglich
- Einwilligungsmanagement

### MASVS-PRIVACY-4
**Die App nutzt Privacy-Enhancing Technologies.**
- Datenschutzfreundliche Identifikatoren
- On-Device-Processing wo möglich
- Anonymisierung/Pseudonymisierung
