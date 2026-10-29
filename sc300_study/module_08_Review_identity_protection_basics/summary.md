# Module 08: Review identity protection basics - Summary

<details><summary>Source lessons</summary>

- Introduction
- Review identity protection basics
- Implement and manage user risk policy
- Exercise enable sign-in risk policy
- Exercise configure Microsoft Entra multifactor authentication registration polic
- Monitor, investigate, and remediate elevated risky users
- Implement security for workload identities
- Explore Microsoft Defender for Identity
- Explore the Identity Risk Management Agent
- Summary and resources

</details>

```text
IDENTITY PROTECTION & RISK MANAGEMENT (SC-300)
│
├── 🏢 FOUNDATIONS & LICENSING
│   ├── Identity Protection ──> 3 core tasks: automate detection/remediation, investigate in
│   │                            portal, export risk data to third-party tools / SIEM
│   ├── License ──────────────> ⚠️ Microsoft Entra ID Premium P2 required (risk policies, MFA
│   │                            registration policy, alerts, full reports — limited/none on Free/P1)
│   ├── Signal scale ─────────> Microsoft analyzes ~6.5 trillion signals/day (Entra ID + MS
│   │                            Accounts + Xbox)
│   └── Risk data feeds ──────> Conditional Access access decisions or SIEM investigation
│
├── 👥 ROLES & PERMISSIONS (exam-favorite table)
│   ├── Security Administrator ──> Full access; ⚠️ can't reset a user password
│   ├── Security Operator ───────> View reports + Overview, dismiss risk, confirm compromise/safe;
│   │                             ⚠️ can't open Risky sign-ins report, no policies/resets/alerts
│   ├── Security Reader ─────────> View-only (reports + Overview); can't give detection feedback
│   ├── Global Reader ───────────> Can access Identity Protection
│   └── Conditional Access Admin > Can create policies using sign-in risk as a condition
│
├── 🚨 RISK DETECTION TYPES
│   ├── Entra ID detections ──> Anonymous IP (Tor/VPN), atypical travel, malicious IP, unfamiliar
│   │                            sign-in properties, leaked credentials, password spray,
│   │                            suspicious browser, verified threat actor IP, anomalous token,
│   │                            token issuer anomaly (SAML), Entra threat intelligence
│   └── MDCA-sourced ─────────> New country, activity from anonymous IP, suspicious inbox forwarding
│
├── ⚙️ RISK POLICIES (all require P2)
│   ├── User risk policy ─────> Probability the ACCOUNT is compromised
│   │   ├── Threshold ────────> ⭐ Microsoft recommendation: HIGH
│   │   └── Control ──────────> ⭐ Recommended: Allow access + Require password change
│   ├── Sign-in risk policy ──> Probability the SIGN-IN was made by someone other than the user
│   │   ├── Threshold ────────> ⭐ Microsoft recommendation: MEDIUM and above
│   │   └── Control ──────────> Require multifactor authentication
│   ├── MFA registration policy > Control "Require Entra ID MFA registration" is locked (fixed)
│   ├── Exclusions ───────────> Emergency access / break-glass accounts; review regularly
│   ├── Trusted networks ─────> Configured trusted locations reduce false positives
│   ├── Self-remediation ─────> ⚠️ Users must be pre-registered for MFA + SSPR (combined
│   │                            registration experience recommended)
│   └── Portal path ──────────> Entra admin center > Identity > Protection > Identity Protection
│
├── 📊 INVESTIGATION REPORTS (3 reports)
│   ├── Risky users ──────────> Risk history + risky sign-in history; actions: reset password,
│   │                            confirm compromise, dismiss risk, block sign-in, open in MDI
│   ├── Risky sign-ins ───────> ⚠️ Retention: last 30 days only; confirm compromised / confirm safe
│   ├── Risk detections ──────> ⚠️ Retention: last 90 days; clickable link to MDCA portal
│   └── Downloads ────────────> CSV or JSON; ⚠️ users & sign-ins = 2,500 rows max,
│                                detections = 5,000 records max
│
├── 🔧 REMEDIATION & UNBLOCKING
│   ├── User risk level ──────> low/medium/high built from ACTIVE detections; "Closed (system)"
│   ├── Options ──────────────> Self-remediation via policy, manual password reset, dismiss risk,
│   │                            close individual detections manually
│   ├── Password reset ───────> Temporary password (must contact user) OR user self-reset
│   │                            (self-reset only available if registered for MFA + SSPR)
│   ├── Dismiss risk ─────────> Closes events but ⚠️ does NOT restore safe state (password intact)
│   ├── Auto-dismiss ─────────> "AI confirmed sign-in safe" when risk resolved (MFA / password change)
│   ├── Unblock (user risk) ──> Reset password / dismiss risk or close detections / exclude user /
│   │                            disable policy
│   └── Unblock (sign-in risk)> Sign in from a familiar location or device / exclude / disable
│
├── 🤖 GRAPH API AUTOMATION
│   ├── APIs ─────────────────> riskDetection, riskyUsers, signIn
│   ├── App registration ─────> Application permissions: IdentityRiskEvent.Read.All +
│   │                            IdentityRiskyUser.Read.All + admin consent + client secret
│   ├── Endpoint ─────────────> GET .../v1.0/identityProtection/riskDetections
│   │                            ?$filter=detectionTimingType eq 'offline'
│   ├── MFA-passed users ─────> riskyUsers?$filter=riskDetail eq
│   │                            'userPassedMFADrivenByRiskBasedPolicy'
│   └── PowerShell ───────────> Microsoft Graph PowerShell SDK Preview module (Azure GitHub samples)
│
├── 🖥️ WORKLOAD IDENTITY PROTECTION (service principals, apps, managed identities)
│   ├── Why riskier ──────────> Can't do MFA, no formal lifecycle, credentials must be stored
│   ├── Requirements ─────────> Entra ID P2 + Security Administrator / Operator / Reader role
│   ├── Detections (offline) ─> Threat intelligence, suspicious sign-ins (learns baseline over
│   │                            2–60 days), suspicious OAuth app credential addition (MDCA),
│   │                            leaked credentials, admin confirmed compromised
│   │                            (riskyServicePrincipals API)
│   └── Conditional Access ───> ⚠️ Only single-tenant service principals registered in YOUR tenant;
│                                third-party SaaS, multi-tenant apps & managed identities excluded
│
└── 🛡️ DEFENDER FOR IDENTITY & RISK MANAGEMENT AGENT
    ├── MDI (ex-Azure ATP) ───> Cloud-based; uses on-prem AD signals for hybrid threat detection
    ├── MDI components ───────> Microsoft Defender portal (security.microsoft.com) + Sensor
    │                            (installed on DCs & AD FS, no port mirroring) + Cloud service
    │                            (Azure: US / Europe / Asia)
    ├── Agent prereqs ────────> ⚠️ Entra ID P2 + available SCUs + Security Administrator to
    │                            activate/act; Security Reader & Global Reader = view only
    ├── Agent triggers ───────> Continuous (every 5 min) / daily / manual run; checking for new
    │                            risky users consumes no SCUs (SCUs used when generating suggestions)
    ├── Agent scope ──────────> Default: 100 most recent risky users, last 90 days (settable 1–100;
    │                            time frames 7 / 14 / 30 / custom up to 90 days)
    └── Agent actions ────────> Dismiss risk or Reset password; LLM chat, agent memory for
                                 custom instructions, verdict Compromised / Not compromised
```
