# Android Secure Coding – Referenz

## Inhaltsverzeichnis
- [Sichere Datenspeicherung](#sichere-datenspeicherung)
- [Kryptographie](#kryptographie)
- [Netzwerksicherheit](#netzwerksicherheit)
- [Komponentensicherheit](#komponentensicherheit)
- [WebView-Sicherheit](#webview-sicherheit)
- [Biometrie](#biometrie)
- [App-Härtung](#app-härtung)

---

## Sichere Datenspeicherung

### EncryptedSharedPreferences
```kotlin
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()

val encryptedPrefs = EncryptedSharedPreferences.create(
    context,
    "secure_prefs",
    masterKey,
    EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
    EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
)
encryptedPrefs.edit().putString("token", authToken).apply()
```

### EncryptedFile
```kotlin
val encryptedFile = EncryptedFile.Builder(
    context,
    File(context.filesDir, "secret.dat"),
    masterKey,
    EncryptedFile.FileEncryptionScheme.AES256_GCM_HKDF_4KB
).build()

encryptedFile.openFileOutput().use { it.write(data) }
```

### SQLCipher mit Room
```kotlin
val passphrase = SQLiteDatabase.getBytes("passphrase".toCharArray())
val factory = SupportFactory(passphrase)
val db = Room.databaseBuilder(context, AppDatabase::class.java, "app.db")
    .openHelperFactory(factory)
    .build()
```

### Backup-Ausschluss
```xml
<!-- AndroidManifest.xml -->
<application android:allowBackup="false"
    android:fullBackupContent="@xml/backup_rules">

<!-- backup_rules.xml -->
<full-backup-content>
    <exclude domain="sharedpref" path="secure_prefs.xml"/>
    <exclude domain="database" path="app.db"/>
</full-backup-content>
```

### Logging entfernen (ProGuard)
```proguard
-assumenosideeffects class android.util.Log {
    public static int v(...);
    public static int d(...);
    public static int i(...);
    public static int w(...);
    public static int e(...);
}
```

---

## Kryptographie

### AES-256-GCM mit Android Keystore
```kotlin
val keyGenerator = KeyGenerator.getInstance(
    KeyProperties.KEY_ALGORITHM_AES, "AndroidKeyStore"
)
keyGenerator.init(
    KeyGenParameterSpec.Builder("my_key",
        KeyProperties.PURPOSE_ENCRYPT or KeyProperties.PURPOSE_DECRYPT)
        .setBlockModes(KeyProperties.BLOCK_MODE_GCM)
        .setEncryptionPaddings(KeyProperties.ENCRYPTION_PADDING_NONE)
        .setKeySize(256)
        .setUserAuthenticationRequired(true)
        .setUserAuthenticationValidityDurationSeconds(30)
        .build()
)
val secretKey = keyGenerator.generateKey()

// Verschlüsselung
val cipher = Cipher.getInstance("AES/GCM/NoPadding")
cipher.init(Cipher.ENCRYPT_MODE, secretKey)
val iv = cipher.iv
val encrypted = cipher.doFinal(plaintext)

// Entschlüsselung
cipher.init(Cipher.DECRYPT_MODE, secretKey, GCMParameterSpec(128, iv))
val decrypted = cipher.doFinal(encrypted)
```

### PBKDF2 Key Derivation
```kotlin
val factory = SecretKeyFactory.getInstance("PBKDF2WithHmacSHA256")
val spec = PBEKeySpec(password, salt, 600000, 256)
val key = factory.generateSecret(spec)
```

### Sichere Zufallszahlen
```kotlin
val random = SecureRandom()
val bytes = ByteArray(32)
random.nextBytes(bytes)
```

---

## Netzwerksicherheit

### Network Security Config
```xml
<!-- network_security_config.xml -->
<network-security-config>
    <base-config cleartextTrafficPermitted="false">
        <trust-anchors>
            <certificates src="system"/>
        </trust-anchors>
    </base-config>
    <domain-config>
        <domain includeSubdomains="true">api.example.com</domain>
        <pin-set expiration="2026-01-01">
            <pin digest="SHA-256">HASH1=</pin>
            <pin digest="SHA-256">BACKUP_HASH=</pin>
        </pin-set>
    </domain-config>
</network-security-config>
```

### OkHttp Certificate Pinner
```kotlin
val certificatePinner = CertificatePinner.Builder()
    .add("api.example.com", "sha256/HASH1=")
    .add("api.example.com", "sha256/BACKUP_HASH=")
    .build()

val client = OkHttpClient.Builder()
    .certificatePinner(certificatePinner)
    .build()
```

---

## Komponentensicherheit

### Exported Components sichern
```xml
<!-- Nicht exportiert (Default ab targetSdk 31) -->
<activity android:name=".InternalActivity"
    android:exported="false"/>

<!-- Exportiert mit Permission-Schutz -->
<provider android:name=".SecureProvider"
    android:exported="true"
    android:permission="com.example.PROVIDER_ACCESS"
    android:readPermission="com.example.READ"
    android:writePermission="com.example.WRITE"/>

<!-- BroadcastReceiver mit Permission -->
<receiver android:name=".SecureReceiver"
    android:exported="true"
    android:permission="com.example.RECEIVER_ACCESS"/>
```

### Intent-Sicherheit
```kotlin
// Explizite Intents verwenden
val intent = Intent(this, TargetActivity::class.java)

// PendingIntent immutable
val pendingIntent = PendingIntent.getActivity(
    context, 0, intent, PendingIntent.FLAG_IMMUTABLE
)

// Intent-Daten validieren
val receivedData = intent.getStringExtra("data") ?: return
if (!isValidInput(receivedData)) return
```

### Content Provider Security
```kotlin
// Parameterized Queries
val cursor = contentResolver.query(
    uri,
    projection,
    "column = ?",  // Selection mit Placeholder
    arrayOf(userInput),  // SelectionArgs
    null
)
```

---

## WebView-Sicherheit

```kotlin
val webView = WebView(context).apply {
    settings.javaScriptEnabled = false  // Nur wenn nötig aktivieren
    settings.allowFileAccess = false
    settings.allowContentAccess = false
    settings.allowFileAccessFromFileURLs = false
    settings.allowUniversalAccessFromFileURLs = false

    // Nur vertrauenswürdige URLs laden
    webViewClient = object : WebViewClient() {
        override fun shouldOverrideUrlLoading(view: WebView, request: WebResourceRequest): Boolean {
            val allowedHosts = setOf("api.example.com", "www.example.com")
            return request.url.host !in allowedHosts
        }
    }
}

// JavaScript Interface minimal halten
@JavascriptInterface
fun getToken(): String {
    // Nur notwendige Daten exponieren
    return limitedToken
}

// WebView Debugging in Release deaktivieren
if (!BuildConfig.DEBUG) {
    WebView.setWebContentsDebuggingEnabled(false)
}
```

---

## Biometrie

```kotlin
// Starke Biometrie mit CryptoObject
val biometricPrompt = BiometricPrompt(activity, executor,
    object : BiometricPrompt.AuthenticationCallback() {
        override fun onAuthenticationSucceeded(result: BiometricPrompt.AuthenticationResult) {
            val cipher = result.cryptoObject?.cipher ?: return
            // Krypto-Operation mit authentifiziertem Cipher
            val decrypted = cipher.doFinal(encryptedData)
        }
    }
)

val promptInfo = BiometricPrompt.PromptInfo.Builder()
    .setTitle("Authentifizierung")
    .setAllowedAuthenticators(BiometricManager.Authenticators.BIOMETRIC_STRONG)
    .setNegativeButtonText("Abbrechen")
    .build()

// Cipher aus Keystore initialisieren
val keyStore = KeyStore.getInstance("AndroidKeyStore")
keyStore.load(null)
val key = keyStore.getKey("biometric_key", null) as SecretKey
val cipher = Cipher.getInstance("AES/GCM/NoPadding")
cipher.init(Cipher.ENCRYPT_MODE, key)

biometricPrompt.authenticate(promptInfo, BiometricPrompt.CryptoObject(cipher))
```

---

## App-Härtung

### ProGuard/R8 Konfiguration
```proguard
-optimizationpasses 5
-overloadaggressively
-repackageclasses ''
-allowaccessmodification

# String-Verschlüsselung (DexGuard)
# -encryptstrings class com.example.security.**

# Debugging-Checks
-assumenosideeffects class android.util.Log { *; }
```

### Root-Detection (Multi-Check)
```kotlin
fun isRooted(): Boolean {
    return checkSuBinary() || checkSuCommand() || checkBuildTags() ||
           checkMagisk() || checkBusybox() || checkRWSystem() ||
           checkDangerousApps() || checkSELinux()
}

private fun checkSuBinary(): Boolean {
    val paths = arrayOf("/system/bin/su", "/system/xbin/su", "/sbin/su",
        "/data/local/xbin/su", "/data/local/bin/su")
    return paths.any { File(it).exists() }
}

private fun checkBuildTags(): Boolean {
    return Build.TAGS?.contains("test-keys") == true
}
```

### Debugger-Detection
```kotlin
fun isDebuggerAttached(): Boolean {
    return Debug.isDebuggerConnected() ||
           Debug.waitingForDebugger() ||
           (context.applicationInfo.flags and ApplicationInfo.FLAG_DEBUGGABLE) != 0
}
```

### Integrity Check
```kotlin
fun verifyAppSignature(context: Context): Boolean {
    val packageInfo = context.packageManager.getPackageInfo(
        context.packageName, PackageManager.GET_SIGNING_CERTIFICATES
    )
    val signatures = packageInfo.signingInfo?.apkContentsSigners ?: return false
    val expectedHash = "EXPECTED_SHA256_HASH"
    return signatures.any {
        val digest = MessageDigest.getInstance("SHA-256")
        val hash = Base64.encodeToString(digest.digest(it.toByteArray()), Base64.NO_WRAP)
        hash == expectedHash
    }
}
```
