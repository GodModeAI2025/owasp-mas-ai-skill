# iOS Secure Coding – Referenz

## Inhaltsverzeichnis
- [Sichere Datenspeicherung](#sichere-datenspeicherung)
- [Kryptographie](#kryptographie)
- [Netzwerksicherheit](#netzwerksicherheit)
- [Plattforminteraktion](#plattforminteraktion)
- [WebView-Sicherheit](#webview-sicherheit)
- [Biometrie](#biometrie)
- [App-Härtung](#app-härtung)
- [Privacy](#privacy)

---

## Sichere Datenspeicherung

### Keychain Services
```swift
// Credential in Keychain speichern
func saveToKeychain(key: String, data: Data) -> Bool {
    let query: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrAccount as String: key,
        kSecValueData as String: data,
        kSecAttrAccessible as String: kSecAttrAccessibleWhenUnlockedThisDeviceOnly
    ]
    SecItemDelete(query as CFDictionary) // Vorherigen Eintrag löschen
    return SecItemAdd(query as CFDictionary, nil) == errSecSuccess
}

// Mit Biometrie-Schutz
func saveWithBiometricProtection(key: String, data: Data) -> Bool {
    let access = SecAccessControlCreateWithFlags(
        nil,
        kSecAttrAccessibleWhenUnlockedThisDeviceOnly,
        [.biometryCurrentSet, .privateKeyUsage],
        nil
    )!
    let query: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrAccount as String: key,
        kSecValueData as String: data,
        kSecAttrAccessControl as String: access
    ]
    return SecItemAdd(query as CFDictionary, nil) == errSecSuccess
}
```

### Data Protection API
```swift
// Datei mit vollständigem Schutz schreiben
let data = sensitiveData
try data.write(to: fileURL, options: .completeFileProtection)

// Attribut prüfen
let attributes = try FileManager.default.attributesOfItem(atPath: fileURL.path)
let protection = attributes[.protectionKey] as? FileProtectionType
assert(protection == .complete)
```

### Backup-Ausschluss
```swift
var url = getDocumentsDirectory().appendingPathComponent("sensitive.dat")
var resourceValues = URLResourceValues()
resourceValues.isExcludedFromBackup = true
try url.setResourceValues(resourceValues)
```

### Core Data mit Verschlüsselung
```swift
// NSPersistentStoreDescription mit Encryption
let description = NSPersistentStoreDescription()
description.setOption(
    FileProtectionType.complete as NSObject,
    forKey: NSPersistentStoreFileProtectionKey
)
```

---

## Kryptographie

### CryptoKit (iOS 13+)
```swift
import CryptoKit

// AES-GCM Verschlüsselung
let key = SymmetricKey(size: .bits256)
let nonce = AES.GCM.Nonce()
let sealedBox = try AES.GCM.seal(plaintext, using: key, nonce: nonce)
let combined = sealedBox.combined!

// Entschlüsselung
let box = try AES.GCM.SealedBox(combined: combined)
let decrypted = try AES.GCM.open(box, using: key)

// HMAC
let hmac = HMAC<SHA256>.authenticationCode(for: data, using: key)

// Hashing
let digest = SHA256.hash(data: data)

// ECDSA Signatur
let privateKey = P256.Signing.PrivateKey()
let signature = try privateKey.signature(for: data)
let isValid = privateKey.publicKey.isValidSignature(signature, for: data)

// Key Agreement (ECDH)
let sharedSecret = try privateKey.sharedSecretFromKeyAgreement(
    with: otherPublicKey
)
let derivedKey = sharedSecret.hkdfDerivedSymmetricKey(
    using: SHA256.self,
    salt: salt,
    sharedInfo: info,
    outputByteCount: 32
)
```

### Secure Enclave
```swift
// Schlüssel in Secure Enclave generieren
let privateKey = try SecureEnclave.P256.Signing.PrivateKey(
    accessControl: SecAccessControlCreateWithFlags(
        nil,
        kSecAttrAccessibleWhenUnlockedThisDeviceOnly,
        [.privateKeyUsage, .biometryCurrentSet],
        nil
    )!
)

// Signieren
let signature = try privateKey.signature(for: dataToSign)
```

### Sichere Zufallszahlen
```swift
var bytes = [UInt8](repeating: 0, count: 32)
let status = SecRandomCopyBytes(kSecRandomDefault, bytes.count, &bytes)
guard status == errSecSuccess else { fatalError("RNG failed") }
```

---

## Netzwerksicherheit

### App Transport Security (ATS)
```xml
<!-- Info.plist – KEIN NSAllowsArbitraryLoads in Production -->
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <false/>
    <key>NSExceptionDomains</key>
    <dict>
        <key>legacy-api.example.com</key>
        <dict>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.2</string>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <true/>
        </dict>
    </dict>
</dict>
```

### Certificate Pinning mit URLSession
```swift
class PinningDelegate: NSObject, URLSessionDelegate {
    let pinnedHashes: Set<String> = ["BASE64_HASH_1", "BASE64_HASH_2"]

    func urlSession(_ session: URLSession,
                    didReceive challenge: URLAuthenticationChallenge,
                    completionHandler: @escaping (URLSession.AuthChallengeDisposition, URLCredential?) -> Void) {

        guard let serverTrust = challenge.protectionSpace.serverTrust,
              let certificate = SecTrustGetCertificateAtIndex(serverTrust, 0) else {
            completionHandler(.cancelAuthenticationChallenge, nil)
            return
        }

        let serverPublicKey = SecCertificateCopyKey(certificate)!
        let serverPublicKeyData = SecKeyCopyExternalRepresentation(serverPublicKey, nil)! as Data
        let hash = SHA256.hash(data: serverPublicKeyData)
        let hashBase64 = Data(hash).base64EncodedString()

        if pinnedHashes.contains(hashBase64) {
            completionHandler(.useCredential, URLCredential(trust: serverTrust))
        } else {
            completionHandler(.cancelAuthenticationChallenge, nil)
        }
    }
}
```

### TrustKit Integration
```swift
// TrustKit Konfiguration
let trustKitConfig: [String: Any] = [
    kTSKSwizzleNetworkDelegates: true,
    kTSKPinnedDomains: [
        "api.example.com": [
            kTSKEnforcePinning: true,
            kTSKPublicKeyHashes: ["hash1=", "hash2="],
            kTSKExpirationDate: "2026-01-01"
        ]
    ]
]
TrustKit.initSharedInstance(withConfiguration: trustKitConfig)
```

---

## Plattforminteraktion

### Universal Links (bevorzugt über Custom URL Schemes)
```json
// apple-app-site-association
{
    "applinks": {
        "apps": [],
        "details": [{
            "appID": "TEAMID.com.example.app",
            "paths": ["/app/*", "/login/*"]
        }]
    }
}
```

### URL Scheme Input Validation
```swift
func application(_ app: UIApplication, open url: URL,
                 options: [UIApplication.OpenURLOptionsKey: Any] = [:]) -> Bool {
    guard let components = URLComponents(url: url, resolvingAgainstBaseURL: true),
          let host = components.host,
          allowedHosts.contains(host) else {
        return false
    }
    // Parameter validieren
    let params = components.queryItems ?? []
    for param in params {
        guard isValidInput(param.value ?? "") else { return false }
    }
    return handleDeepLink(components)
}
```

### App Extensions Sicherheit
```swift
// Shared Container minimal konfigurieren
let sharedDefaults = UserDefaults(suiteName: "group.com.example.shared")
// NUR nicht-sensible Daten im Shared Container
// Sensible Daten bleiben in der Keychain
```

---

## WebView-Sicherheit

### WKWebView Konfiguration
```swift
let config = WKWebViewConfiguration()
let prefs = WKWebpagePreferences()
prefs.allowsContentJavaScript = false  // Nur wenn nötig

config.defaultWebpagePreferences = prefs
config.preferences.javaScriptCanOpenWindowsAutomatically = false

let webView = WKWebView(frame: .zero, configuration: config)

// Content nur von Whitelist laden
webView.navigationDelegate = self

func webView(_ webView: WKWebView,
             decidePolicyFor navigationAction: WKNavigationAction,
             decisionHandler: @escaping (WKNavigationActionPolicy) -> Void) {
    guard let url = navigationAction.request.url,
          let host = url.host,
          allowedDomains.contains(host) else {
        decisionHandler(.cancel)
        return
    }
    decisionHandler(.allow)
}

// JavaScript Bridge minimal
let userScript = WKUserScript(source: "...", injectionTime: .atDocumentEnd, forMainFrameOnly: true)
config.userContentController.addUserScript(userScript)
```

---

## Biometrie

### LAContext mit Keychain-Binding
```swift
// Keychain-Item mit Biometrie-Schutz erstellen
let access = SecAccessControlCreateWithFlags(
    nil,
    kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly,
    .biometryCurrentSet,  // Invalidiert bei neuer Biometrie
    nil
)!

let query: [String: Any] = [
    kSecClass as String: kSecClassGenericPassword,
    kSecAttrAccount as String: "auth_token",
    kSecValueData as String: tokenData,
    kSecAttrAccessControl as String: access,
    kSecUseAuthenticationUI as String: kSecUseAuthenticationUIAllow
]

// Beim Lesen wird automatisch Biometrie angefordert
let readQuery: [String: Any] = [
    kSecClass as String: kSecClassGenericPassword,
    kSecAttrAccount as String: "auth_token",
    kSecReturnData as String: true,
    kSecUseOperationPrompt as String: "Authenticate to access token"
]
var result: AnyObject?
let status = SecItemCopyMatching(readQuery as CFDictionary, &result)
```

---

## App-Härtung

### Jailbreak-Detection
```swift
func isJailbroken() -> Bool {
    #if targetEnvironment(simulator)
    return false
    #else
    let paths = ["/Applications/Cydia.app", "/Library/MobileSubstrate",
                 "/bin/bash", "/usr/sbin/sshd", "/etc/apt",
                 "/usr/bin/ssh", "/private/var/lib/apt/"]
    if paths.contains(where: { FileManager.default.fileExists(atPath: $0) }) {
        return true
    }
    if let result = try? String(contentsOfFile: "/etc/fstab", encoding: .utf8),
       result.count > 0 {
        return true
    }
    let testPath = "/private/jailbreak_test_\(UUID().uuidString)"
    do {
        try "test".write(toFile: testPath, atomically: true, encoding: .utf8)
        try FileManager.default.removeItem(atPath: testPath)
        return true
    } catch { }
    return false
    #endif
}
```

### Anti-Debug
```swift
func disableDebugger() {
    // ptrace-basiert
    let PT_DENY_ATTACH: CInt = 31
    ptrace(PT_DENY_ATTACH, 0, nil, 0)
}

func isDebuggerAttached() -> Bool {
    var info = kinfo_proc()
    var size = MemoryLayout<kinfo_proc>.stride
    var mib: [Int32] = [CTL_KERN, KERN_PROC, KERN_PROC_PID, getpid()]
    sysctl(&mib, UInt32(mib.count), &info, &size, nil, 0)
    return (info.kp_proc.p_flag & P_TRACED) != 0
}
```

---

## Privacy

### App Tracking Transparency
```swift
import AppTrackingTransparency

func requestTrackingPermission() {
    ATTrackingManager.requestTrackingAuthorization { status in
        switch status {
        case .authorized: enableTracking()
        case .denied, .restricted: disableTracking()
        case .notDetermined: break
        @unknown default: disableTracking()
        }
    }
}
```

### Privacy Manifest (PrivacyInfo.xcprivacy)
```xml
<dict>
    <key>NSPrivacyTracking</key>
    <false/>
    <key>NSPrivacyTrackingDomains</key>
    <array/>
    <key>NSPrivacyCollectedDataTypes</key>
    <array>
        <dict>
            <key>NSPrivacyCollectedDataType</key>
            <string>NSPrivacyCollectedDataTypeName</string>
            <key>NSPrivacyCollectedDataTypeLinked</key>
            <false/>
            <key>NSPrivacyCollectedDataTypeTracking</key>
            <false/>
            <key>NSPrivacyCollectedDataTypePurposes</key>
            <array>
                <string>NSPrivacyCollectedDataTypePurposeAppFunctionality</string>
            </array>
        </dict>
    </array>
    <key>NSPrivacyAccessedAPITypes</key>
    <array/>
</dict>
```
