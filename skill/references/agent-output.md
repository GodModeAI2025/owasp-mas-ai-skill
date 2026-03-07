# OWASP MAS – Agent Output Formate

Maschinenlesbare Ausgabeformate für AI-Agent-Consumption. Jedes Format ist so gestaltet, dass ein nachgelagerter Agent es ohne Parsing-Logik direkt als Input verwenden kann.

## Inhaltsverzeichnis
- [Ausgabemodus](#ausgabemodus)
- [Finding-Schema (YAML)](#finding-schema)
- [Compliance-Matrix-Schema](#compliance-matrix-schema)
- [Threat-Model-Schema](#threat-model-schema)
- [Weakness-Lookup-Schema](#weakness-lookup-schema)
- [Secure-Coding-Schema](#secure-coding-schema)
- [Agent Summary Block](#agent-summary-block)
- [Downstream-Trigger-Katalog](#downstream-trigger-katalog)
- [Kombi-Ausgabe](#kombi-ausgabe)

---

## Ausgabemodus

Der Agent-Output wird aktiviert durch:
- Explizite Anfrage: "als Agent-Output", "maschinenlesbar", "für Agent", "YAML", "JSON"
- Pipeline-Kontext: Wenn erkennbar ist, dass der Output an einen anderen Agent weitergereicht wird
- Dual-Modus: "für Mensch und Agent" → Menschenlesbare Antwort + Agent-Artefakt als Code-Block

**Format-Wahl:**
- YAML als Default (besser lesbar, weniger Escaping-Probleme)
- JSON auf Anfrage oder wenn der Benutzer es spezifiziert
- Beide Formate sind schema-identisch

---

## Finding-Schema

Jedes Finding als strukturiertes Artefakt. Ein Agent kann daraus direkt Tickets, Fixes oder Gate-Entscheidungen ableiten.

```yaml
# owasp-mas-finding.yaml
schema_version: "1.0"
finding:
  id: "F-01"
  title: "API Subscription Key hardcoded in Info.plist"
  
  # Klassifikation
  maswe_id: "MASWE-0005"
  masvs_control: "MASVS-AUTH-2"
  masvs_category: "AUTH"
  cwe: "CWE-798"
  owasp_mobile_top10: "M1"
  
  # Bewertung
  severity: "critical"           # critical | high | medium | low | info
  cvss_estimate: 9.1
  confidence: "confirmed"        # confirmed | likely | possible
  
  # Kontext
  platform: "ios"                # ios | android | both
  app_id: "com.enbw.ih.eap.dev"
  app_version: "3.0.0"
  build: "283"
  
  # Evidenz
  evidence:
    type: "static_analysis"      # static_analysis | dynamic_analysis | config_review
    file: "Info.plist"
    key: "CSB_OCP_APIM_SUBSCRIPTION_KEY"
    value_hash: "sha256:a1b2c3d4..."  # Nie den Klarwert, immer Hash
    location: "app_bundle/Info.plist"
    snippet: "CSB_OCP_APIM_SUBSCRIPTION_KEY = [REDACTED]"
  
  # Remediation
  remediation:
    action: "move_to_runtime_config"
    description: "Subscription Key aus Info.plist entfernen. Zur Laufzeit vom Backend beziehen."
    priority: "immediate"        # immediate | short_term | mid_term | long_term
    effort: "S"                  # S | M | L | XL
    breaking_change: false
    requires_backend_change: true
  
  # Downstream-Aktionen für andere Agents
  downstream_triggers:
    - action: "rotate_secret"
      target: "azure_apim"
      key_id: "CSB_OCP_APIM_SUBSCRIPTION_KEY"
      urgency: "immediate"
    - action: "create_ticket"
      system: "jira"
      type: "security_bug"
      priority: "critical"
      labels: ["security", "hardcoded-credential", "owasp-mas"]
    - action: "ci_gate"
      gate: "block_release"
      condition: "until_resolved"
    - action: "notify"
      channel: "security_team"
      template: "credential_exposure"
```

### Finding-Liste (mehrere Findings)

```yaml
schema_version: "1.0"
assessment:
  app_id: "com.enbw.ih.eap.dev"
  app_version: "3.0.0"
  profile: "MAS-L2"
  date: "2026-03-07"
  type: "static_analysis"
  
findings:
  - id: "F-01"
    maswe_id: "MASWE-0005"
    masvs_control: "MASVS-AUTH-2"
    severity: "critical"
    title: "API Subscription Key hardcoded in Info.plist"
    evidence:
      file: "Info.plist"
      key: "CSB_OCP_APIM_SUBSCRIPTION_KEY"
    remediation:
      action: "move_to_runtime_config"
      priority: "immediate"
      effort: "S"
    downstream_triggers:
      - action: "rotate_secret"
      - action: "create_ticket"
      - action: "ci_gate"
        gate: "block_release"

  - id: "F-02"
    maswe_id: "MASWE-0005"
    masvs_control: "MASVS-AUTH-2"
    severity: "critical"
    title: "Airship Push Credentials (Dev+Prod) in Bundle"
    # ... (gleiches Schema)
```

---

## Compliance-Matrix-Schema

Für Compliance-Agents, die MASVS-Erfüllung tracken.

```yaml
schema_version: "1.0"
compliance:
  standard: "OWASP-MASVS"
  version: "2.1"
  profile: "MAS-L2"
  app_id: "com.enbw.ih.eap.dev"
  date: "2026-03-07"
  
  summary:
    total_controls: 24
    pass: 7
    fail: 6
    partial: 8
    not_applicable: 3
    compliance_rate: 0.29    # pass / (total - n/a)
    verdict: "non_compliant"  # compliant | partially_compliant | non_compliant
  
  controls:
    - id: "MASVS-STORAGE-1"
      category: "STORAGE"
      status: "fail"           # pass | fail | partial | not_applicable
      findings: ["F-07"]
      comment: "NSFileProtectionNone referenziert"
      
    - id: "MASVS-STORAGE-2"
      category: "STORAGE"
      status: "partial"
      findings: []
      comment: "Backup-Exclusion vorhanden, Screenshot-Schutz unklar"
      
    - id: "MASVS-CRYPTO-1"
      category: "CRYPTO"
      status: "pass"
      findings: []
      comment: "CryptoKit/SecureEnclave, DPoP implementiert"
    
    # ... (alle 24 Kontrollen)

  regulatory_mapping:
    - control: "MASVS-STORAGE-1"
      gdpr: ["Art. 32"]
      pci_dss: ["3.4", "6.5"]
      bsi: ["APP.1"]
      iso27001: ["A.14.1"]
    # ...
```

---

## Threat-Model-Schema

Für Architecture-Review-Agents und Risk-Management-Systeme.

```yaml
schema_version: "1.0"
threat_model:
  app_id: "com.enbw.ih.eap.dev"
  date: "2026-03-07"
  methodology: "STRIDE"
  
  trust_boundaries:
    - id: "TB-01"
      name: "App ↔ OS"
      components: ["keystore", "permissions", "file_system"]
      
    - id: "TB-02"
      name: "App ↔ Backend"
      components: ["api_gateway", "auth_server", "push_service"]
  
  threats:
    - id: "T-01"
      boundary: "TB-02"
      stride_category: "spoofing"     # spoofing|tampering|repudiation|information_disclosure|dos|elevation
      description: "MitM mit gefälschtem Zertifikat"
      masvs_control: "MASVS-NETWORK-2"
      likelihood: "high"
      impact: "high"
      risk: "critical"
      mitigation:
        status: "missing"              # implemented | partial | missing
        recommendation: "Certificate Pinning implementieren"
        effort: "M"
      related_findings: ["F-06"]

  attack_surface:
    local:
      - vector: "filesystem_access"
        risk: "high"
        requires: "root_jailbreak"
      - vector: "backup_extraction"
        risk: "medium"
        requires: "physical_access"
    network:
      - vector: "mitm"
        risk: "high"
        requires: "network_position"
    platform:
      - vector: "ipc_interception"
        risk: "low"
        requires: "malicious_app"
```

---

## Weakness-Lookup-Schema

Für Knowledge-Agents und Vulnerability-Management-Systeme.

```yaml
schema_version: "1.0"
weakness:
  id: "MASWE-0005"
  title: "API Keys Hardcoded in the App Package"
  
  classification:
    masvs_category: "AUTH"
    masvs_control: "MASVS-AUTH-2"
    severity: "critical"
    cwe: "CWE-798"
    owasp_mobile_top10: "M1"
    
  platforms: ["ios", "android"]
  
  detection:
    static:
      - method: "string_analysis"
        tool: "strings"
        pattern: "grep -E '[A-Za-z0-9]{20,}'"
      - method: "plist_analysis"
        tool: "plistlib"
        target: "Info.plist, GoogleService-Info.plist"
    dynamic:
      - method: "traffic_interception"
        tool: "burp_suite"
        indicator: "API key in request headers"
  
  remediation:
    ios:
      approach: "runtime_config"
      description: "Key vom Server bei App-Start beziehen. In Keychain cachen."
      api: "URLSession + Keychain Services"
    android:
      approach: "runtime_config"
      description: "Key vom Server bei App-Start beziehen. In EncryptedSharedPreferences cachen."
      api: "Retrofit + EncryptedSharedPreferences"
  
  correlates_with:
    - "MASWE-0013"  # Hardcoded Cryptographic Keys
    - "MASWE-0036"  # Auth Material Stored Unencrypted
```

---

## Secure-Coding-Schema

Für Code-Generation-Agents und PR-Review-Bots.

```yaml
schema_version: "1.0"
secure_coding:
  masvs_control: "MASVS-NETWORK-2"
  topic: "certificate_pinning"
  
  implementations:
    - platform: "ios"
      language: "swift"
      approach: "trustkit"
      code: |
        let config: [String: Any] = [
            kTSKSwizzleNetworkDelegates: true,
            kTSKPinnedDomains: [
                "api.example.com": [
                    kTSKEnforcePinning: true,
                    kTSKPublicKeyHashes: ["hash1=", "backup_hash="],
                    kTSKExpirationDate: "2027-01-01"
                ]
            ]
        ]
        TrustKit.initSharedInstance(withConfiguration: config)
      dependencies: ["TrustKit"]
      min_os: "14.0"
      
    - platform: "android"
      language: "kotlin"
      approach: "network_security_config"
      code: |
        <!-- network_security_config.xml -->
        <network-security-config>
            <domain-config>
                <domain includeSubdomains="true">api.example.com</domain>
                <pin-set expiration="2027-01-01">
                    <pin digest="SHA-256">hash1=</pin>
                    <pin digest="SHA-256">backup_hash=</pin>
                </pin-set>
            </domain-config>
        </network-security-config>
      dependencies: []
      min_sdk: 24
      
  anti_patterns:
    - description: "Kein Pinning implementiert"
      maswe_id: "MASWE-0047"
      severity: "high"
    - description: "Selbstsignierte Zertifikate akzeptiert"
      maswe_id: "MASWE-0052"
      severity: "critical"
```

---

## Agent Summary Block

Wird an JEDE Agent-Ausgabe angehängt. Gibt dem nächsten Agent in der Pipeline den Gesamtkontext.

```yaml
agent_summary:
  # Identifikation
  source_skill: "owasp-mas"
  skill_version: "1.1.0"
  output_role: "auditor"         # auditor|developer|architect|reporter|analyzer
  timestamp: "2026-03-07T14:30:00Z"
  
  # App-Kontext
  app:
    id: "com.enbw.ih.eap.dev"
    name: "EnBW+"
    version: "3.0.0"
    build: "283"
    platform: "ios"
  
  # Assessment-Kontext
  assessment:
    profile: "MAS-L2"
    type: "static_analysis"
    verdict: "fail"              # pass | conditional_pass | fail
    
  # Metriken
  metrics:
    findings_total: 11
    critical: 2
    high: 4
    medium: 4
    low: 1
    controls_pass: 7
    controls_fail: 6
    controls_partial: 8
    compliance_rate: 0.29
    
  # Blockierte Kontrollen (für CI/CD-Gates)
  blocked_controls:
    - "MASVS-AUTH-2"
    - "MASVS-NETWORK-2"
    - "MASVS-RESILIENCE-2"
    - "MASVS-RESILIENCE-3"
    - "MASVS-CRYPTO-2"
    - "MASVS-STORAGE-1"
    
  # Empfohlene nächste Aktionen für Downstream-Agents
  next_agent_actions:
    immediate:
      - action: "rotate_exposed_secrets"
        targets: ["azure_apim_key", "airship_dev_secret", "airship_prod_secret"]
      - action: "remove_debug_config"
        targets: ["DEBUG_MENU_ENABLED"]
    short_term:
      - action: "implement_certificate_pinning"
        endpoints: ["enbwtest.azure-api.net", "qas-login.enbw.com"]
      - action: "enable_firebase_app_check"
      - action: "strip_test_urls_from_release"
    mid_term:
      - action: "implement_jailbreak_detection"
      - action: "upgrade_file_protection"
        target: "NSFileProtectionComplete"
      - action: "consent_gate_medallia_tracking"
  
  # Pipeline-Steuerung
  pipeline:
    release_blocked: true
    block_reason: "2 critical findings unresolved"
    retest_required: true
    retest_scope: ["AUTH-2", "NETWORK-2", "RESILIENCE-2", "RESILIENCE-3"]
```

---

## Downstream-Trigger-Katalog

Standardisierte Aktionen, die andere Agents auswerten:

| Trigger | Beschreibung | Typische Empfänger |
|---|---|---|
| `rotate_secret` | Exponiertes Secret sofort rotieren | Secret-Management-Agent, DevOps |
| `create_ticket` | Jira/Linear/GitHub Issue erstellen | Ticket-Agent, Projektmanagement |
| `ci_gate` | CI/CD-Pipeline blockieren/freigeben | CI/CD-Agent, GitHub Actions |
| `notify` | Team benachrichtigen | Slack-Agent, E-Mail-Agent |
| `code_fix` | Automatischen Fix vorschlagen/erstellen | Remediation-Agent, PR-Bot |
| `retest` | Nachtest nach Behebung anstoßen | Audit-Agent (Selbst-Referenz) |
| `compliance_update` | Compliance-Status aktualisieren | Compliance-Agent, GRC-System |
| `threat_model_update` | Threat Model anpassen | Architecture-Agent |
| `dependency_scan` | Abhängigkeiten auf CVEs prüfen | Dependency-Agent, Snyk/Trivy |
| `privacy_review` | DSGVO-Impact-Assessment anstoßen | Privacy-Agent, DPO-Workflow |

---

## Kombi-Ausgabe

Wenn der Benutzer "für Mensch und Agent" anfordert, wird die Ausgabe in zwei Blöcken geliefert:

1. **Menschenlesbare Antwort** – Prosa, Tabellen, Empfehlungen (wie bei den anderen Rollen)
2. **Agent-Artefakt** – YAML-Code-Block mit dem vollständigen maschinenlesbaren Output

```markdown
[Menschenlesbare Analyse...]

## Agent-Artefakt

\```yaml
schema_version: "1.0"
assessment:
  app_id: "..."
findings:
  - id: "F-01"
    ...
agent_summary:
  ...
\```
```

Der Agent-Artefakt-Block ist immer vollständig und selbstbeschreibend. Ein nachgelagerter Agent braucht keinen Kontext aus der menschenlesbaren Antwort.
