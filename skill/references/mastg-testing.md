# OWASP MASTG – Mobile Application Security Testing Guide Referenz

## Inhaltsverzeichnis
- [Testing-Methodik](#testing-methodik)
- [Android Testing](#android-testing)
- [iOS Testing](#ios-testing)
- [Tools](#tools)
- [Techniken](#techniken)
- [Test-Case-Mapping MASVS → MASTG](#test-case-mapping)

---

## Testing-Methodik

### Phasen eines Mobile Security Tests
1. **Vorbereitung**: Scope definieren, Testumgebung aufsetzen, App beschaffen
2. **Informationssammlung**: App-Metadaten, Permissions, Netzwerkendpunkte
3. **Statische Analyse**: Dekompilierung, Code-Review, Konfigurationsanalyse
4. **Dynamische Analyse**: Runtime-Instrumentierung, Traffic-Interception, Fuzzing
5. **Dokumentation**: Findings dokumentieren, Risikobewertung, Empfehlungen
6. **Nachtest**: Behobene Findings verifizieren

### Testing-Profile (MAS Profiles)

MASVS v2 kennt keine Verifikationsstufen mehr. Die früheren Level L1/L2/R leben als **MAS Testing Profiles** weiter; OWASP pflegt sie seit September 2026 als eigenen Bereich unter [mas.owasp.org/Profiles](https://mas.owasp.org/Profiles/). Ein Audit nennt deshalb nie „MASVS Level 2“, sondern ein Profil.

| Profil | Ziel | Angreifermodell |
|---|---|---|
| **MAS-L1** | Essential Security – Basis für alle Apps | Andere Apps auf dem Gerät sind Angreifer |
| **MAS-L2** | Advanced Security – erweitert MAS-L1 für hoch sensible Daten und sensible Funktionen (Banking, Health) | Dem Betriebssystem ist nicht zu trauen (Root/Jailbreak), Dritte mit oder ohne physischen Gerätezugriff |
| **MAS-R** | Resilient Security – Schutz von Geschäftslogik und geistigem Eigentum gegen Reverse Engineering und Tampering | Der Gerätenutzer selbst ist Angreifer (Reverse Engineer, Cheater) |
| **MAS-P** | Baseline Privacy – für alle Apps mit nutzerbezogenen sensiblen Daten (Kategorie MASVS-PRIVACY seit MASVS v2.1) | Kein Angreifermodell; Schutz personenbezogener Daten und verantwortungsvolle Datenverarbeitung |

**Kombinieren statt wählen:** MAS-L1 oder MAS-L2 bildet die Sicherheitsbasis (L2 erweitert L1). MAS-R ergänzt diese Basis und wird nie allein verwendet, MAS-P kommt bei personenbezogenen Daten dazu. Typische Kombinationen:

| Kombination | Wann |
|---|---|
| MAS-L1+P | Keine Business-Assets, nur gering sensible Daten (Name, E-Mail) |
| MAS-L1+P+R | Wie oben, aber Geschäftslogik/Umsatz in der App (z. B. werbefinanziert) |
| MAS-L2+P | Mittel/hoch sensible Daten oder sensible Funktionen |
| MAS-L2+P+R | Zusätzlich Business-Assets, die gegen Manipulation geschützt werden müssen |

Die Standardprofile sind ein Ausgangspunkt für Teams ohne eigenes Threat Model; das höchste Schutzniveau bietet ein eigenes, aus dem Threat Model abgeleitetes Profil. Ziel ist nicht, jede Prüfung eines Profils zu erfüllen: Das Profil wird aus dem Threat Model abgeleitet (`references/threat-model-template.md`) und darf pro Kategorie abweichen, etwa L2 für Storage und L1 für Network, wenn hoch sensible Daten das Gerät nie verlassen. Abweichungen im Bericht begründen.

---

## Android Testing

### Testumgebung
- Physisches Gerät oder Emulator (Genymotion, Android Studio AVD)
- ADB-Zugang konfiguriert
- Root-Zugang oder Magisk für erweiterte Tests
- Burp Suite/mitmproxy als Proxy konfiguriert
- Frida-Server auf Gerät installiert

### Statische Analyse Android
1. **APK entpacken**: `apktool d app.apk`
2. **DEX zu Java**: `jadx app.apk` oder `dex2jar` + `jd-gui`
3. **AndroidManifest.xml analysieren**:
   - Exported Components (Activities, Services, Receivers, Providers)
   - Permissions
   - Backup-Flag (`android:allowBackup`)
   - Debuggable-Flag (`android:debuggable`)
   - Network Security Config
   - `minSdkVersion`, `targetSdkVersion`
4. **String-Analyse**: Hardcoded Secrets, URLs, API-Keys
5. **Native Libraries**: `.so`-Dateien mit IDA/Ghidra analysieren

### Dynamische Analyse Android
1. **Logcat**: `adb logcat | grep -i <package>`
2. **Dateisystem**: `/data/data/<package>/` untersuchen
3. **Frida-Scripts**: Krypto-Funktionen hooken, SSL-Pinning umgehen
4. **Traffic-Interception**: Proxy-Konfiguration, Cert-Installation
5. **Drozer**: IPC-Komponenten testen

### Wichtige Android-Pfade
- `/data/data/<package>/shared_prefs/` – SharedPreferences
- `/data/data/<package>/databases/` – SQLite-Datenbanken
- `/data/data/<package>/files/` – Interne Dateien
- `/data/data/<package>/cache/` – Cache-Dateien
- `/sdcard/Android/data/<package>/` – Externer Speicher

---

## iOS Testing

### Testumgebung
- Physisches Gerät mit Jailbreak (checkra1n, unc0ver, palera1n)
- Xcode + Simulator für grundlegende Tests
- Burp Suite/mitmproxy als Proxy
- Frida/Objection installiert
- SSH-Zugang zum Gerät

### Statische Analyse iOS
1. **IPA entpacken**: `unzip app.ipa`
2. **Binary analysieren**: `otool -L`, `class-dump`, Hopper/IDA/Ghidra
3. **Info.plist analysieren**:
   - URL Schemes
   - App Transport Security (ATS) Konfiguration
   - Permissions (Usage Descriptions)
   - Background Modes
4. **Entitlements prüfen**: `codesign -d --entitlements :- app.app`
5. **String-Analyse**: `strings binary | grep -i "key\|secret\|password"`

### Dynamische Analyse iOS
1. **Keychain-Dump**: `keychain-dumper` oder Objection
2. **Dateisystem**: Application-Container durchsuchen
3. **Frida/Objection**: Runtime-Manipulation, Method-Tracing
4. **SSL-Pinning-Bypass**: `objection explore --startup-command "ios sslpinning disable"`
5. **Snapshot-Analyse**: Background-Screenshots prüfen

### Wichtige iOS-Pfade
- `Documents/` – Persistente Daten
- `Library/Preferences/` – NSUserDefaults/Plist-Dateien
- `Library/Caches/` – Cache-Daten
- `tmp/` – Temporäre Dateien
- Keychain – Sichere Credential-Speicherung

---

## Tools

### Essenzielle Tools
| Tool | Zweck | Plattform |
|---|---|---|
| Frida | Runtime-Instrumentierung | Android/iOS |
| Objection | Mobile Exploration Toolkit | Android/iOS |
| Burp Suite | Traffic-Interception | Beide |
| mitmproxy | Traffic-Interception (Open Source) | Beide |
| jadx | Java-Dekompilierung | Android |
| apktool | APK-Dekompilierung | Android |
| Drozer | IPC-Testing | Android |
| Ghidra | Binary-Analyse | Beide |
| Hopper | Binary-Analyse | iOS |
| class-dump | ObjC Header Extraction | iOS |
| MobSF | Automatisierte Analyse | Beide |
| QARK | Automatisierte Analyse | Android |
| Needle | iOS Security Testing | iOS |
| r2frida | Reverse Engineering | Beide |
| nuclei | Vulnerability Scanning | Beide |

### Automatisierte Scanner
- **MobSF**: Statische und dynamische Analyse, API-Fuzzing
- **QARK**: Quick Android Review Kit – automatisierte Schwachstellensuche
- **AndroBugs**: Android-Schwachstellen-Scanner
- **Oversecured**: Cloud-basierte statische Analyse

---

## Techniken

### SSL Pinning Bypass
```javascript
// Frida-Script: Universal SSL Pinning Bypass (Kurzversion)
Java.perform(function() {
    var TrustManagerImpl = Java.use('com.android.org.conscrypt.TrustManagerImpl');
    TrustManagerImpl.verifyChain.overload(/* params */).implementation = function() {
        // Bypass logic
        return arguments[0];
    };
});
```

### Root/Jailbreak Detection Bypass
- Frida-Scripts zum Hooken von Detection-Methoden
- Magisk Hide / Zygisk Module
- Liberty Lite / Shadow (iOS)

### Krypto-Analyse
- Krypto-API-Calls mit Frida hooken
- Schlüsselmaterial zur Laufzeit extrahieren
- IV/Key/Plaintext-Logging

### Intent-Fuzzing (Android)
```bash
# Exported Activities finden und aufrufen
adb shell am start -n com.example/.ExportedActivity
adb shell am broadcast -a com.example.ACTION
adb shell content query --uri content://com.example.provider/
```

---

## Test-Case-Mapping

### MASVS-STORAGE → Tests
| Kontrolle | Android-Tests | iOS-Tests |
|---|---|---|
| STORAGE-1 | Dateisystem, SharedPrefs, SQLite, Logs | Keychain, Plist, SQLite, Core Data, Logs |
| STORAGE-2 | Clipboard, Backups, Screenshots, Keyboard Cache | Clipboard, Backups, Snapshots, Pasteboard |

### MASVS-CRYPTO → Tests
| Kontrolle | Prüfgegenstand |
|---|---|
| CRYPTO-1 | Algorithmus-Whitelist, IV-Generierung, Padding, CSPRNG |
| CRYPTO-2 | KeyStore/Keychain-Nutzung, Key Derivation, Rotation, Hardcoding |

### MASVS-AUTH → Tests
| Kontrolle | Prüfgegenstand |
|---|---|
| AUTH-1 | MFA-Flow, Step-Up, Re-Auth, OAuth2/OIDC |
| AUTH-2 | Token-Speicherung, Transportverschlüsselung, Token-Validierung |
| AUTH-3 | Server-Side Enforcement, Biometrie-Implementierung |

### MASVS-NETWORK → Tests
| Kontrolle | Prüfgegenstand |
|---|---|
| NETWORK-1 | TLS-Konfiguration, Cleartext, Open Ports |
| NETWORK-2 | Certificate Pinning, Zertifikatsvalidierung |

### MASVS-PLATFORM → Tests
| Kontrolle | Prüfgegenstand |
|---|---|
| PLATFORM-1 | UI-Leaks, Notifications, Screenshots |
| PLATFORM-2 | IPC, Deep Links, Exported Components |
| PLATFORM-3 | WebView-Konfiguration, JS-Bridges, Content Loading |

### MASVS-CODE → Tests
| Kontrolle | Prüfgegenstand |
|---|---|
| CODE-1 | Forced Updates, API-Level |
| CODE-2 | Input Validation aller Quellen |
| CODE-3 | SQL Injection, Deserialization, Dynamic Loading |
| CODE-4 | Compiler-Flags, Dependency-Check |

### MASVS-RESILIENCE → Tests
| Kontrolle | Prüfgegenstand |
|---|---|
| RESILIENCE-1 | ProGuard/R8, String-Encryption, Resource-Shrinking |
| RESILIENCE-2 | Debug-Symbole, Test-Code, Feature-Flags |
| RESILIENCE-3 | Integrity Checks, Root/JB-Detection, Emulator-Detection |
| RESILIENCE-4 | Anti-Debug, ptrace, Debugger-Detection |

### MASVS-PRIVACY → Tests
| Kontrolle | Prüfgegenstand |
|---|---|
| PRIVACY-1 | Permissions, Tracking-SDKs, Datenminimierung |
| PRIVACY-2 | Privacy Policy, Transparenz |
| PRIVACY-3 | Opt-in/Opt-out, Löschung, Export |
| PRIVACY-4 | Identifikatoren, On-Device-Processing |
