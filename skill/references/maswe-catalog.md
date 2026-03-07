# OWASP MASWE – Mobile Application Security Weakness Enumeration

## Inhaltsverzeichnis
- [MASVS-STORAGE Weaknesses](#masvs-storage-weaknesses)
- [MASVS-CRYPTO Weaknesses](#masvs-crypto-weaknesses)
- [MASVS-AUTH Weaknesses](#masvs-auth-weaknesses)
- [MASVS-NETWORK Weaknesses](#masvs-network-weaknesses)
- [MASVS-PLATFORM Weaknesses](#masvs-platform-weaknesses)
- [MASVS-CODE Weaknesses](#masvs-code-weaknesses)
- [MASVS-RESILIENCE Weaknesses](#masvs-resilience-weaknesses)
- [MASVS-PRIVACY Weaknesses](#masvs-privacy-weaknesses)

---

## MASVS-STORAGE Weaknesses

| ID | Schwachstelle | Schweregrad |
|---|---|---|
| MASWE-0001 | Insertion of Sensitive Data into Logs | Hoch |
| MASWE-0002 | Sensitive Data Stored With Insufficient Access Restrictions in Internal Locations | Hoch |
| MASWE-0003 | Backup Unencrypted | Mittel |
| MASWE-0004 | Sensitive Data Not Excluded From Backup | Mittel |
| MASWE-0006 | Sensitive Data Stored Unencrypted in Private Storage Locations | Hoch |
| MASWE-0007 | Sensitive Data Stored Unencrypted in Shared Storage Requiring No User Interaction | Hoch |

### Prüfansatz STORAGE
1. Statische Analyse: Logging-Statements mit sensiblen Daten suchen
2. Dateisystem-Analyse: SharedPreferences, Databases, Plist-Dateien prüfen
3. Backup-Analyse: adb backup / iTunes Backup extrahieren und durchsuchen
4. Runtime-Monitoring: Dateizugriffe während der Nutzung überwachen

---

## MASVS-CRYPTO Weaknesses

| ID | Schwachstelle | Schweregrad |
|---|---|---|
| MASWE-0009 | Improper Cryptographic Key Generation | Hoch |
| MASWE-0010 | Improper Cryptographic Key Derivation | Hoch |
| MASWE-0011 | Cryptographic Key Rotation Not Implemented | Mittel |
| MASWE-0012 | Insecure or Wrong Usage of Cryptographic Key | Hoch |
| MASWE-0013 | Hardcoded Cryptographic Keys in Use | Kritisch |
| MASWE-0014 | Cryptographic Keys Not Properly Protected at Rest | Hoch |
| MASWE-0015 | Deprecated Android KeyStore Implementations | Mittel |
| MASWE-0016 | Unsafe Handling of Imported Cryptographic Keys | Mittel |
| MASWE-0017 | Cryptographic Keys Not Properly Protected on Export | Mittel |
| MASWE-0018 | Cryptographic Keys Access Not Restricted | Hoch |
| MASWE-0019 | Risky Cryptography Implementations | Hoch |
| MASWE-0020 | Improper Encryption | Kritisch |
| MASWE-0021 | Improper Hashing | Hoch |
| MASWE-0022 | Predictable Initialization Vectors (IVs) | Hoch |
| MASWE-0023 | Risky Padding | Mittel |
| MASWE-0024 | Improper Use of Message Authentication Code (MAC) | Hoch |
| MASWE-0025 | Improper Generation of Cryptographic Signatures | Hoch |
| MASWE-0026 | Improper Verification of Cryptographic Signature | Kritisch |
| MASWE-0027 | Improper Random Number Generation | Hoch |

### Prüfansatz CRYPTO
1. Statische Code-Analyse: Krypto-API-Aufrufe identifizieren
2. Algorithmus-Check: Verwendete Algorithmen gegen Whitelist prüfen
3. Key-Management-Audit: Schlüsselspeicherung, -generierung, -rotation
4. Runtime: Krypto-Operationen mit Frida/Objection hooking

---

## MASVS-AUTH Weaknesses

| ID | Schwachstelle | Schweregrad |
|---|---|---|
| MASWE-0005 | API Keys Hardcoded in the App Package | Kritisch |
| MASWE-0028 | MFA Implementation Best Practices Not Followed | Hoch |
| MASWE-0029 | Step-Up Authentication Not Implemented After Login | Mittel |
| MASWE-0030 | Re-Authentication Not Triggered On Contextual State Changes | Mittel |
| MASWE-0031 | Insecure use of Android Protected Confirmation | Mittel |
| MASWE-0032 | Platform-provided Authentication APIs Not Used | Mittel |
| MASWE-0033 | Authentication/Authorization Protocol Security Best Practices Not Followed | Hoch |
| MASWE-0034 | Insecure Implementation of Confirm Credentials | Mittel |
| MASWE-0035 | Passwordless Authentication Not Implemented | Niedrig |
| MASWE-0036 | Authentication Material Stored Unencrypted on the Device | Kritisch |
| MASWE-0037 | Authentication Material Sent over Insecure Connections | Kritisch |
| MASWE-0038 | Authentication Tokens Not Validated | Hoch |
| MASWE-0039 | Shared Web Credentials and Website-association Not Implemented | Niedrig |
| MASWE-0040 | Insecure Authentication in WebViews | Hoch |
| MASWE-0041 | Authentication Enforced Only Locally Instead of Server-side | Kritisch |
| MASWE-0042 | Authorization Enforced Only Locally Instead of Server-side | Kritisch |
| MASWE-0043 | App Custom PIN Not Bound to Platform KeyStore | Hoch |
| MASWE-0044 | Biometric Authentication Can Be Bypassed | Hoch |
| MASWE-0045 | Fallback to Non-biometric Credentials Allowed for Sensitive Transactions | Mittel |
| MASWE-0046 | Crypto Keys Not Invalidated on New Biometric Enrollment | Hoch |

### Prüfansatz AUTH
1. Authentifizierungsfluss-Analyse: Alle Auth-Endpunkte und Flows dokumentieren
2. Token-Analyse: JWT-Validierung, Expiry, Refresh-Mechanismen prüfen
3. Biometrie-Test: Bypass-Versuche mit Frida/Objection
4. Server-Side-Verification: Prüfen ob Autorisierung serverseitig erzwungen wird

---

## MASVS-NETWORK Weaknesses

| ID | Schwachstelle | Schweregrad |
|---|---|---|
| MASWE-0047 | Insecure Identity Pinning | Hoch |
| MASWE-0048 | Insecure Machine-to-Machine Communication | Hoch |
| MASWE-0049 | Proven Networking APIs Not Used | Mittel |
| MASWE-0050 | Cleartext Traffic | Kritisch |
| MASWE-0051 | Unprotected Open Ports | Hoch |
| MASWE-0052 | Insecure Certificate Validation | Kritisch |

### Prüfansatz NETWORK
1. Traffic-Analyse: Burp Suite/mitmproxy – gesamten Netzwerkverkehr abfangen
2. TLS-Konfiguration: Cipher Suites, Protokollversionen prüfen
3. Pinning-Test: Certificate Pinning Bypass versuchen
4. Port-Scan: Offene Ports auf dem Gerät identifizieren

---

## MASVS-PLATFORM Weaknesses

| ID | Schwachstelle | Schweregrad |
|---|---|---|
| MASWE-0053 | Sensitive Data Leaked via the User Interface | Hoch |
| MASWE-0054 | Sensitive Data Leaked via Notifications | Mittel |
| MASWE-0055 | Sensitive Data Leaked via Screenshots or Screen Recordings | Mittel |
| MASWE-0056 | Tapjacking Attacks | Hoch |
| MASWE-0057 | StrandHogg Attack / Task Affinity Vulnerability | Hoch |
| MASWE-0058 | Insecure Deep Links | Hoch |
| MASWE-0059 | Use Of Unauthenticated Platform IPC | Hoch |
| MASWE-0060 | Insecure Use of UIActivity | Mittel |
| MASWE-0061 | Insecure Use of App Extensions | Mittel |
| MASWE-0062 | Insecure Services | Hoch |
| MASWE-0063 | Insecure Broadcast Receivers | Hoch |
| MASWE-0064 | Insecure Content Providers | Hoch |
| MASWE-0065 | Sensitive Data Permanently Shared with Other Apps | Hoch |
| MASWE-0066 | Insecure Intents | Hoch |
| MASWE-0067 | Debuggable Flag Not Disabled | Kritisch |
| MASWE-0068 | JavaScript Bridges in WebViews | Hoch |
| MASWE-0069 | WebViews Allows Access to Local Resources | Hoch |
| MASWE-0070 | JavaScript Loaded from Untrusted Sources | Hoch |
| MASWE-0071 | WebViews Loading Content from Untrusted Sources | Hoch |
| MASWE-0072 | Universal XSS | Kritisch |
| MASWE-0073 | Insecure WebResourceResponse Implementations | Mittel |
| MASWE-0074 | Web Content Debugging Enabled | Mittel |
| MASWE-0118 | Sensitive Data Not Removed After Use | Mittel |

### Prüfansatz PLATFORM
1. Manifest-Analyse: AndroidManifest.xml / Info.plist auf exported Components
2. IPC-Fuzzing: Intent/URL-Scheme-Fuzzing
3. WebView-Audit: JavaScript-Bridges, loadUrl-Aufrufe analysieren
4. UI-Schutz: Screenshot-Blocking, Tapjacking-Tests

---

## MASVS-CODE Weaknesses

| ID | Schwachstelle | Schweregrad |
|---|---|---|
| MASWE-0075 | Enforced Updating Not Implemented | Mittel |
| MASWE-0076 | Dependencies with Known Vulnerabilities | Hoch |
| MASWE-0077 | Running on a Recent Platform Version Not Ensured | Mittel |
| MASWE-0078 | Latest Platform Version Not Targeted | Niedrig |
| MASWE-0079 | Unsafe Handling of Data from the Network | Hoch |
| MASWE-0080 | Unsafe Handling of Data from Backups | Mittel |
| MASWE-0081 | Unsafe Handling Of Data From External Interfaces | Hoch |
| MASWE-0082 | Unsafe Handling of Data From Local Storage | Mittel |
| MASWE-0083 | Unsafe Handling of Data From The User Interface | Hoch |
| MASWE-0084 | Unsafe Handling of Data from IPC | Hoch |
| MASWE-0085 | Unsafe Dynamic Code Loading | Kritisch |
| MASWE-0086 | SQL Injection | Kritisch |
| MASWE-0087 | Insecure Parsing and Escaping | Hoch |
| MASWE-0088 | Insecure Object Deserialization | Hoch |
| MASWE-0116 | Compiler Provided Security Features Not Used | Mittel |

### Prüfansatz CODE
1. Dependency-Scan: OWASP Dependency-Check, Snyk, npm audit
2. SAST: Statische Code-Analyse auf Injection-Patterns
3. API-Level-Check: minSdkVersion, targetSdkVersion prüfen
4. Binary-Analyse: Compiler-Flags (PIE, Stack Canaries, ARC) verifizieren

---

## MASVS-RESILIENCE Weaknesses

| ID | Schwachstelle | Schweregrad |
|---|---|---|
| MASWE-0008 | Missing Device Secure Lock Verification Implementation | Mittel |
| MASWE-0089 | Code Obfuscation Not Implemented | Mittel |
| MASWE-0090 | Resource Obfuscation Not Implemented | Niedrig |
| MASWE-0091 | Anti-Deobfuscation Techniques Not Implemented | Niedrig |
| MASWE-0092 | Static Analysis Tools Not Prevented | Mittel |
| MASWE-0093 | Debugging Symbols Not Removed | Mittel |
| MASWE-0094 | Non-Production Resources Not Removed | Mittel |
| MASWE-0095 | Code That Disables Security Controls Not Removed | Hoch |
| MASWE-0096 | App Integrity Not Verified at Runtime | Hoch |
| MASWE-0097 | Device Integrity Not Verified at Runtime | Hoch |
| MASWE-0098 | Jailbreak/Root Detection Not Implemented | Hoch |
| MASWE-0099 | Emulator Detection Not Implemented | Mittel |
| MASWE-0100 | Runtime Manipulation Not Prevented | Hoch |
| MASWE-0101 | Debugger Detection Not Implemented | Mittel |
| MASWE-0102 | Anti-Debugging Techniques Not Implemented | Mittel |

### Prüfansatz RESILIENCE
1. Reverse Engineering: APK/IPA dekompilieren, Obfuskation bewerten
2. Tamper-Test: App repacken und erneut installieren
3. Root/Jailbreak-Test: Auf gerooteten/gejailbreakten Geräten testen
4. Hooking-Test: Frida/Objection einsetzen, Runtime-Manipulation prüfen

---

## MASVS-PRIVACY Weaknesses

| ID | Schwachstelle | Schweregrad |
|---|---|---|
| MASWE-0103 | Excessive Permissions Requested | Mittel |
| MASWE-0104 | Data Minimization Not Applied | Mittel |
| MASWE-0105 | Excessive Tracking | Hoch |
| MASWE-0106 | Privacy Policy Not Accessible | Mittel |
| MASWE-0107 | Purpose of Data Processing Not Communicated | Mittel |
| MASWE-0108 | User Not Informed About Tracking | Hoch |
| MASWE-0109 | No Opt-in/Opt-out for Tracking | Hoch |
| MASWE-0110 | Data Deletion Not Possible | Hoch |
| MASWE-0111 | Data Export Not Possible | Mittel |
| MASWE-0112 | Consent Management Missing | Hoch |
| MASWE-0113 | Non-Privacy-Friendly Identifiers Used | Mittel |
| MASWE-0114 | On-Device Processing Not Used Where Possible | Niedrig |
| MASWE-0115 | Anonymization/Pseudonymization Not Applied | Mittel |

### Prüfansatz PRIVACY
1. Berechtigungsanalyse: Alle angeforderten Berechtigungen gegen App-Funktionen prüfen
2. Traffic-Analyse: Tracking-Domains, SDK-Calls identifizieren
3. Privacy-Policy-Review: Vollständigkeit, Verständlichkeit, Aktualität
4. DSGVO-Konformität: Löschung, Export, Einwilligung testen
