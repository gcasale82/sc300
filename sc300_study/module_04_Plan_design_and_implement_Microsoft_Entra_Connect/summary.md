# Module 04: Plan, design, and implement Microsoft Entra Connect - Summary

<details><summary>Source lessons</summary>

- Introduction
- Plan, design, and implement Microsoft Entra Connect
- Implement manage password hash synchronization (PHS)
- Implement manage pass-through authentication (PTA)
- Explore pass-through authentication and seamless single sign-on (SSO)
- Implement and manage federation
- Trouble-shoot synchronization errors
- Implement Microsoft Entra Connect Health
- Manage Microsoft Entra Health
- Summary and resources

</details>

```text
HYBRID IDENTITY WITH MICROSOFT ENTRA CONNECT (SC-300)
│
├── 🧭 HYBRID IDENTITY OVERVIEW
│   ├── Goal ──> unified authN/authZ across on-prem AD + cloud (Microsoft Entra ID)
│   └── Entra Connect ──> sync + PHS + PTA + federation + health; included in subscription
│
├── 🔑 AUTHENTICATION METHODS (choose by effort / UX / scenarios / continuity)
│   ├── PHS ──> hash of the hash synced; ⭐ enable with ANY chosen method
│   │   ├── Runs every 2 min (can't change) ──> initial sync = ALL in-scope users
│   │   ├── Selective PHS possible after initial sync; auto-enabled by Express Settings
│   │   ├── FIPS blocks MD5 ──> edit miiserver.exe.config (enforceFIPSPolicy = false)
│   │   └── Backup for PTA failover ──> ⚠️ manual switch via Entra Connect, never auto
│   ├── PTA ──> agents validate passwords directly against on-prem AD
│   │   ├── 3 agents for HA ──> 1 on Connect server + 2 extra
│   │   ├── Agents need DC access + internet ──> ⚠️ NOT in perimeter network
│   │   ├── Enforces on-prem state instantly (disabled / locked / sign-in hours)
│   │   └── Tenant-level feature ──> wait 12h before shutting down old AD FS
│   └── Federation ──> hands auth to AD FS or PingFederate (8.4+)
│       ├── Required for smartcards, 3rd-party MFA, DOMAIN\username sign-in
│       └── ⚠️ Exam scope excludes MANUAL AD FS deployments
│
├── 🪟 SEAMLESS SSO
│   ├── Combines with PHS or PTA ──> ⚠️ NOT applicable to AD FS
│   ├── Browser flow ──> Kerberos ticket for AZUREADSSOACC computer account
│   └── Native clients ──> WS-Trust MEX endpoint (exclusive) → SAML → tokens
│
├── 🏗️ ENTRA CONNECT DESIGN CONCEPTS
│   ├── sourceAnchor (immutableId) ──> immutable for object lifetime
│   │   ├── Single forest ──> objectGUID (also Express default)
│   │   └── Rules ──> <60 chars, no special chars, globally unique, not name-based
│   ├── UPN suffix ──> must be a VERIFIED domain else *.onmicrosoft.com is applied
│   ├── Topologies ──> Express install = single forest + single tenant ONLY
│   │   ├── Staging server ──> reads all, writes nothing (standby / DR)
│   │   └── ⚠️ 1:1 sync server per tenant; filter for mutually exclusive objects
│   ├── Engine ──> Connector Space (per directory) + ONE Metaverse; sync rules + run profiles
│   └── Cloud sync ──> lightweight provisioning agent; disconnected multi-forest (M&A)
│       └── Groups up to 50,000 members ──> use OU scoping filter; sync every 2 min
│
├── 🤝 FEDERATION DEPLOYMENT (Entra Connect wizard)
│   ├── Prereqs ──> local admin on AD FS + WAP servers; WinRM connectivity
│   ├── WAP ──> perimeter network, handles extranet auth requests
│   ├── Service account ──> group Managed Service Account OR domain user account
│   ├── AlternateID ──> sign in with mail attribute when UPN is non-routable
│   └── Device writeback ──> devices synced to AD → device-based Conditional Access
│
├── 🚨 SYNC ERROR TROUBLESHOOTING (export to Entra; Health report refreshes every 30 min)
│   ├── InvalidSoftMatch ──> hard match fails; soft match hits object w/ different immutableId
│   ├── AttributeValueMustBeUnique ──> duplicate ProxyAddresses or UPN values
│   ├── ObjectTypeMismatch ──> user vs group/contact share one proxyAddress
│   ├── FederatedDomainChangeError ──> UPN suffix moved between two FEDERATED domains
│   ├── LargeObject ──> ⚠️ 15-certificate hard limit (userCertificate / userSMIMECertificate)
│   ├── IdentityDataValidationFailed ──> invalid characters / format in UPN
│   ├── Admin Role Conflict ──> no soft-match to cloud user holding an admin role
│   │   └── Fix ──> strip roles → hard-delete quarantined object → sync → restore roles
│   └── Generic fix ──> correct the SOURCE directory, then let Connect sync it
│
├── 🩺 ENTRA CONNECT HEALTH (⭐ requires Entra ID Premium P1)
│   ├── Prereqs ──> global admin, agent per server, PowerShell 4.0+, FIPS disabled
│   ├── Ports ──> 443 (+ legacy 5671; latest agents need only 443)
│   ├── Agents ──> AD FS agent on its own server (⚠️ never on the Sync server)
│   │   └── Sync agent ──> auto-installed with latest Entra Connect
│   ├── RBAC ──> Owner (manage access + settings) / Contributor (settings) / Reader (view)
│   │   └── ⚠️ Diagnose needs Contributor; scope = all instances or single instance
│   ├── Email alerts ON by default ──> sync errors arrive as a daily digest
│   └── Delete server ──> stops monitoring; does NOT uninstall agent or erase data
│
└── 🧯 SELF-SERVICE DIAGNOSE (duplicated attribute remediation)
    ├── Covers ──> UPN / ProxyAddresses / SipProxyAddress / OnPremiseSecurityIdentifier
    ├── Statuses ──> Not Started → Manual Fix Required → Pending Sync
    └── Orphaned object ──> Apply Fix in portal → cleared after next sync cycle
```
