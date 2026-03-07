---
name: mas-developer
description: OWASP MAS Entwickler-Leitfaden für sichere mobile App-Entwicklung. Gibt plattformspezifische Secure-Coding-Empfehlungen nach MASVS v2.1 und MASTG Best Practices. Verwende diesen Skill IMMER bei: sichere App-Entwicklung, Secure Coding mobil, MASVS-konforme Implementierung, Android Security Best Practices, iOS Security Best Practices, sichere Datenspeicherung mobil, Krypto-Implementierung mobil, sichere Authentifizierung mobil, Certificate Pinning implementieren, WebView absichern, Input Validation mobil, sichere IPC Android, Keychain/Keystore verwenden, Biometrie implementieren, App Hardening, Code Review mobile App, Security Code Review, Secure SDLC mobil, DevSecOps mobile.
---

# OWASP MAS Developer Guide

Plattformspezifischer Secure-Coding-Leitfaden für mobile App-Entwicklung basierend auf MASVS v2.1 und MASTG Best Practices.

---

## Wann diesen Skill verwenden

- Sichere Implementierung einer Funktion in einer mobilen App
- Code-Review unter Sicherheitsgesichtspunkten
- Frage zu Secure-Coding-Patterns für Android oder iOS
- Implementierungshilfe für Krypto, Auth, Netzwerk, Storage
- Bewertung von Code-Snippets auf MASVS-Konformität

---

## Referenzdateien

Bei Bedarf lesen:
- `references/secure-coding-android.md` – Android-spezifische Secure-Coding-Patterns
- `references/secure-coding-ios.md` – iOS-spezifische Secure-Coding-Patterns

---

## Arbeitsweise

1. **Plattform identifizieren**: Android (Kotlin/Java), iOS (Swift/ObjC), Cross-Platform
2. **MASVS-Kontrolle zuordnen**: Welche Kontrolle ist relevant?
3. **Konkrete Implementierung liefern**: Code-Beispiele, API-Empfehlungen
4. **Anti-Patterns zeigen**: Was NICHT getan werden sollte
5. **Testbarkeit sicherstellen**: Wie kann die Implementierung getestet werden?

---

## Secure-Coding-Patterns nach MASVS-Kategorie

### STORAGE – Sichere Datenspeicherung

**Android:**
- EncryptedSharedPreferences für sensible Key-Value-Daten
- AndroidX Security Crypto Library für Dateiverschlüsselung
- Room mit SQLCipher für verschlüsselte Datenbanken
- `android:allowBackup="false"` im Manifest
- Kein Logging sensibler Daten (ProGuard-Regel: `-assumenosideeffects class android.util.Log`)

**iOS:**
- Keychain Services für Credentials (kSecAttrAccessibleWhenUnlockedThisDeviceOnly)
- Data Protection API (NSFileProtectionComplete)
- Kein Speichern in NSUserDefaults für sensible Daten
- Core Data mit Encryption
- isExcludedFromBackup für sensible Dateien

**Anti-Patterns:**
- ❌ Sensible Daten in SharedPreferences/NSUserDefaults unverschlüsselt
- ❌ Hardcoded Credentials im Quellcode
- ❌ Logging von Passwörtern, Tokens, PII
- ❌ Sensible Daten auf SD-Karte / externem Speicher

### CRYPTO – Kryptographie

**Empfohlene Algorithmen:**
- Verschlüsselung: AES-256-GCM (authentifiziert)
- Hashing: SHA-256, SHA-384, SHA-512
- Key Derivation: PBKDF2 (min 600.000 Iterationen), Argon2id
- Signatur: ECDSA (P-256), Ed25519, RSA-2048+
- Random: SecureRandom (Android), Security.framework (iOS)

**Android:**
- Android Keystore System für Schlüsselspeicherung
- KeyGenParameterSpec mit setUserAuthenticationRequired(true)
- Javax.crypto mit AES/GCM/NoPadding
- Kein Bouncy Castle wenn Android-APIs ausreichen

**iOS:**
- Secure Enclave für Schlüsselgenerierung
- CryptoKit (ab iOS 13) für moderne Krypto
- CommonCrypto als Fallback
- Keychain für Schlüsselspeicherung

**Anti-Patterns:**
- ❌ MD5, SHA-1, DES, 3DES, RC4
- ❌ ECB-Modus
- ❌ Statische/vorhersagbare IVs
- ❌ Hartcodierte Schlüssel
- ❌ Eigene Krypto-Implementierungen

### AUTH – Authentifizierung

**Biometrie Android:**
```kotlin
// BiometricPrompt mit CryptoObject für echte Sicherheit
val biometricPrompt = BiometricPrompt(activity, executor, callback)
val cipher = getCipher() // aus Keystore initialisiert
val cryptoObject = BiometricPrompt.CryptoObject(cipher)
biometricPrompt.authenticate(promptInfo, cryptoObject)
```

**Biometrie iOS:**
```swift
// LAContext mit biometryCurrentSet für Enrollment-Binding
let context = LAContext()
context.evaluatePolicy(.deviceOwnerAuthenticationWithBiometrics,
    localizedReason: "Authenticate") { success, error in
    // Krypto-Operation mit Keychain-Item das Biometrie erfordert
}
```

**Best Practices:**
- Token-basierte Authentifizierung (OAuth2/OIDC)
- Refresh-Token sicher im Keystore/Keychain speichern
- Serverseitige Session-Validierung erzwingen
- Step-Up-Auth für sensible Operationen
- Token-Expiry und Rotation

**Anti-Patterns:**
- ❌ Nur lokale Authentifizierung ohne Servervalidierung
- ❌ BiometricPrompt ohne CryptoObject (Android)
- ❌ Credentials im Klartext speichern/senden
- ❌ Hardcoded API-Keys

### NETWORK – Netzwerk

**Certificate Pinning Android:**
```xml
<!-- network_security_config.xml -->
<network-security-config>
    <domain-config>
        <domain includeSubdomains="true">api.example.com</domain>
        <pin-set expiration="2025-12-31">
            <pin digest="SHA-256">base64_encoded_pin=</pin>
            <pin digest="SHA-256">backup_pin=</pin>
        </pin-set>
    </domain-config>
</network-security-config>
```

**Certificate Pinning iOS:**
```swift
// TrustKit oder URLSession Delegate
func urlSession(_ session: URLSession,
    didReceive challenge: URLAuthenticationChallenge,
    completionHandler: @escaping (URLSession.AuthChallengeDisposition, URLCredential?) -> Void) {
    // Public Key Pinning Validierung
}
```

**Best Practices:**
- TLS 1.2+ erzwingen
- Certificate/Public Key Pinning mit Backup-Pin
- Cleartext-Traffic blockieren (networkSecurityConfig / ATS)
- Kein Akzeptieren selbstsignierter Zertifikate in Production

### PLATFORM – Plattforminteraktion

**Android IPC Sicherheit:**
- Exported Components nur wenn nötig (`exported="false"` als Default)
- Permission-basierte Zugriffskontrolle für Content Providers
- Intent-Filterung und Validierung
- Pending Intents mit FLAG_IMMUTABLE

**iOS URL Schemes:**
- Universal Links bevorzugen (verifiziert)
- URL-Scheme-Input validieren und sanitisieren
- App Groups minimal konfigurieren

**WebView Sicherheit:**
- JavaScript nur wenn nötig aktivieren
- loadUrl/loadRequest nur mit Whitelist
- JavaScript-Bridge minimal halten
- setAllowFileAccess(false) (Android)
- WebView-Debugging in Production deaktivieren

### CODE – Codequalität

**Input Validation:**
- Alle externen Eingaben validieren (Netzwerk, IPC, UI, Storage)
- Prepared Statements für SQL-Queries
- Keine dynamische Code-Ausführung (eval, DexClassLoader mit externen Quellen)
- Sichere Deserialisierung (keine ObjectInputStream für untrusted Data)

**Dependency Management:**
- Regelmäßige Dependency-Updates
- OWASP Dependency-Check / Snyk in CI/CD integrieren
- Gradle/CocoaPods Lock-Files versionieren
- minSdkVersion und targetSdkVersion aktuell halten

**Compiler-Sicherheit:**
- Android: ProGuard/R8, minification
- iOS: PIE, Stack Canaries, ARC, Position Independent Code

### RESILIENCE – App-Härtung

**Obfuskation:**
- Android: R8/ProGuard mit aggressiven Regeln, DexGuard für Premium
- iOS: Swift-Compiler-Optimierungen, LLVM Obfuscator
- String-Encryption für sensible Strings
- Resource-Obfuskation

**Runtime-Schutz:**
- Root/Jailbreak-Detection (mehrere Checks kombinieren)
- Debugger-Detection (ptrace, isDebuggerAttached)
- Integritätsprüfung (Signatur-Verifikation)
- Emulator-Detection
- Frida/Hooking-Detection (multiple Checks)

### PRIVACY – Datenschutz

**Best Practices:**
- Berechtigungen erst bei Bedarf anfragen (Just-in-Time)
- Datenminimierung: nur erheben was nötig ist
- ATT-Framework nutzen (iOS App Tracking Transparency)
- IDFA/GAID nur mit Einwilligung
- On-Device-ML statt Cloud (Core ML, TensorFlow Lite)
- Privacy Manifest (iOS) korrekt pflegen

---

## Code-Review-Checkliste

Bei Code-Reviews diese Punkte prüfen:
1. Werden sensible Daten korrekt gespeichert (Keystore/Keychain)?
2. Werden bewährte Krypto-APIs mit korrekten Parametern verwendet?
3. Ist die Authentifizierung serverseitig erzwungen?
4. Wird TLS für alle Verbindungen verwendet?
5. Sind IPC-Komponenten korrekt geschützt?
6. Werden alle Eingaben validiert?
7. Gibt es keine hartcodierten Secrets?
8. Sind Debugging-Artefakte entfernt?
9. Werden Berechtigungen minimal angefordert?
10. Ist die Datenschutzerklärung korrekt verlinkt?

---

## Verifikation

Bevor Code-Empfehlungen präsentiert werden:
1. Plattform-spezifische API korrekt (keine Android-API für iOS und umgekehrt)
2. API-Level/iOS-Version-Kompatibilität geprüft
3. Empfohlene Algorithmen auf aktueller Whitelist
4. Code-Beispiele kompilierbar und syntaktisch korrekt
5. Anti-Patterns klar als solche markiert
6. MASVS-Kontrolle explizit referenziert
