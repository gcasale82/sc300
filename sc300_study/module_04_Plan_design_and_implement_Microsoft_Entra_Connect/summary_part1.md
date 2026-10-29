# Module 04: Plan, design, and implement Microsoft Entra Connect - Summary (part 1)

<details><summary>Source lessons</summary>

- Introduction
- Plan, design, and implement Microsoft Entra Connect
- Implement manage password hash synchronization (PHS)
- Implement manage pass-through authentication (PTA)
- Explore pass-through authentication and seamless single sign-on (SSO)
- Implement and manage federation
- Trouble-shoot synchronization errors
- Implement Microsoft Entra Connect Health

</details>

```text
HYBRID IDENTITY WITH MICROSOFT ENTRA CONNECT (SC-300)
│
├── 🌉 ENTRA CONNECT — CORE COMPONENTS
│   ├── Role ───────────> Bridges on-prem AD + Microsoft Entra ID (single tool, unified ID)
│   ├── Capabilities ───> Sync objects + PHS + PTA + Federation + Health monitoring
│   ├── Connector Space ─> Staging area, one per connected directory (each forest / Entra)
│   ├── Metaverse (MV) ──> Global staging area ⭐ only ONE MV exists
│   ├── Sync rules ─────> Decide projection/join + attribute flow between staging areas
│   └── Operations ─────> Import / Synchronization / Export (Export = main error source)
│
├── 🔑 AUTHENTICATION METHODS (core exam comparison)
│   ├── PHS ────────────> Hash of the hash synced to Entra ID; least effort/infrastructure
│   │   ├── Runs every 2 minutes (fixed, cannot modify); overwrites the cloud password
│   │   ├── Express settings ──> PHS auto-enabled; initial sync = ALL in-scope users
│   │   ├── Selective PHS ──> available only AFTER the initial full synchronization
│   │   ├── ⚠️ Does NOT instantly enforce on-prem account state (disabled user keeps access)
│   │   ├── FIPS mode ──> MD5 blocked; fix in miiserver.exe.config (enforceFIPSPolicy=false)
│   │   └── ⭐ Enable PHS with ANY primary method (DR, outage survival, leaked-creds report)
│   ├── PTA ────────────> Agents validate passwords directly against on-prem AD
│   │   ├── Enforces account state, password policy + sign-in hours AT sign-in time
│   │   ├── ⭐ Deploy 3 agents (1 on Connect server + 2 extra); NEVER in perimeter network
│   │   ├── PHS backup ──> Failover is MANUAL via Entra Connect (never automatic)
│   │   └── Tenant-level feature ──> affects ALL managed domains
│   │       └── ⚠️ Switching AD FS → PTA: wait 12 hours before shutting down AD FS
│   └── Federation ─────> Entra ID hands off auth to a trusted external system
│       ├── Needed for ─> Smartcards/certs, 3rd-party MFA, sAMAccountName (DOMAIN\user) login
│       └── Topology ───> Load-balanced farm: internal AD FS + perimeter WAP servers
│
├── 🧬 DESIGN CONCEPTS & TOPOLOGIES
│   ├── sourceAnchor (immutableId) ──> Immutable for object lifetime; < 60 characters
│   │   ├── Special chars count as 3; must be unique, not case-sensitive, not name-based
│   │   └── ⭐ objectGuid = recommended (single forest, express settings, legacy DirSync)
│   ├── UPN sign-in ──> Suffix must be a VERIFIED custom domain (else *.onmicrosoft.com used)
│   ├── Non-routable domain (contoso.local) ──> Connect warns; use CUSTOM installation
│   ├── Express settings ──> supports ONLY single forest + single tenant topology
│   ├── Multi-forest ──> ONE sync server must reach ALL forests (may sit in perimeter/DMZ)
│   ├── GALSync ──> FIM 2010 / MIM 2016 ⚠️ Entra Connect CANNOT do on-prem GALSync
│   ├── Staging server ──> Reads everything, writes NOTHING (standby / disaster recovery)
│   └── ⚠️ Sync server : tenant = strict 1:1 (one Connect install per Entra tenant)
│
├── ☁️ ENTRA CONNECT CLOUD SYNC (alternate engine)
│   ├── Cloud provisioning agent ──> Light-weight bridge AD → Entra; config managed in cloud
│   ├── Use cases ──> Disconnected multi-forest (mergers/acquisitions); coexists w/ Connect
│   ├── HA ──> Multiple provisioning agents supported (critical for PHS-dependent orgs)
│   └── Limits ──> Groups up to 50,000 members (use OU scoping filter); sync every 2 minutes
│
├── 🚀 SEAMLESS SSO
│   ├── Auto sign-in ──> Domain-joined corporate devices on the corporate network
│   ├── Combines with ──> PHS or PTA ⚠️ NOT applicable to AD FS federation
│   ├── Mechanism ──> Kerberos ticket for AZUREADSSOACC computer account; Entra decrypts it
│   ├── Native clients ──> WS-Trust MEX endpoint (exclusive to Seamless SSO) → SAML → tokens
│   └── Deployment ──> Group Policy (partial or full rollout); no extra on-prem components
│
├── 🛠️ FEDERATION ADMIN (AD FS + PingFederate via Entra Connect)
│   ├── AD FS farm ──> Windows Server 2012 R2+; ALL authentication stays on-premises
│   ├── PingFederate ──> Version 8.4+; TLS/SSL cert for the federation service name
│   ├── Wizard flow ──> AD FS servers (domain-joined) → WAP (perimeter, extranet-facing)
│   │   └── AD FS service account ──> Group Managed Service Account OR domain user account
│   ├── Tasks ──> Repair trust, add federated domain (single domain per initial run),
│   │   add AD FS server, add WAP server
│   ├── AlternateID ──> Sign in with mail attribute when UPN is non-routable or unusable
│   └── Device writeback ──> CN=RegisteredDevices; enables device-based CA for AD FS apps
│
├── 🚨 SYNC ERROR TROUBLESHOOTING (Export errors → connector "contoso.onmicrosoft.com")
│   ├── Hard match ──> sourceAnchor vs immutableId; Soft match ──> ProxyAddresses / UPN
│   ├── InvalidSoftMatch ──> Soft-match hit an object that ALREADY holds an immutableId
│   ├── AttributeValueMustBeUnique ──> Duplicate ProxyAddresses / UserPrincipalName
│   ├── ObjectTypeMismatch ──> Different object types (User/Group) share a proxyAddress
│   ├── FederatedDomainChangeError ──> UPN suffix changed between two FEDERATED domains
│   ├── IdentityDataValidationFailed ──> Invalid characters / format in userPrincipalName
│   ├── LargeObject ──> Limits: userCertificate & userSMIMECertificate max 15 certs,
│   │   oversized thumbnailPhoto, too many proxyAddresses
│   ├── Admin role conflict ──> No soft-match for users WITH admin roles → remove roles,
│   │   hard-delete quarantined object, resync, restore roles
│   └── Reports ──> Entra Connect Health for sync (Azure portal), refreshed every 30 min
│
└── 🩺 MICROSOFT ENTRA CONNECT HEALTH
    ├── License ──> ⭐ Requires Entra ID Premium P1 (no portal configuration without it)
    ├── Access ──> Global Administrator; agent installed on EACH targeted server
