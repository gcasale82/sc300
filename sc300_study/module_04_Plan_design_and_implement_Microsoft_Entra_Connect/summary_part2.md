# Module 04: Plan, design, and implement Microsoft Entra Connect - Summary (part 2)

<details><summary>Source lessons</summary>

- Manage Microsoft Entra Health
- Summary and resources

</details>

```text
HYBRID IDENTITY: MICROSOFT ENTRA CONNECT & CONNECT HEALTH (SC-300)
│
├── 📧 EMAIL NOTIFICATIONS (enabled by DEFAULT ⚠️)
│   ├── Alerts dialog ──> Notification Settings ──> switch ON ──> Save (changes apply ONLY after Save)
│   ├── Recipients ──> checkbox "all Global Administrators" + free-text "Additional Email Recipients"
│   └── Back-end sync errors ──> DAILY DIGEST email (replaces one message per error)
│
├── 🗑️ DELETE SERVERS / SERVICE INSTANCES (⭐ Microsoft Entra ID PREMIUM license required)
│   ├── Delete server ──> stops data collection + removes from monitoring
│   ├── ⚠️ Health Agent is NOT uninstalled by the delete action (uninstall it yourself first)
│   ├── Collected data ──> NOT deleted immediately (removed per Azure data retention policy)
│   ├── Re-monitor same server ──> MUST uninstall + reinstall the Health Agent
│   ├── Path (AD FS / Entra Connect Sync) ──> Server List ──> Server ──> Delete ──> type name to confirm
│   ├── Path (Entra Domain Services) ──> Domain Controllers dashboard ──> select DC ──> Delete Selected
│   └── Delete service instance ──> agents remain on ALL servers; re-onboard = uninstall + reinstall + register
│
├── 🔐 AZURE RBAC ON CONNECT HEALTH (⭐ apply least privilege)
│   ├── Owner ───────> manage access + view all info + change settings (Global Admins by default, cannot change)
│   ├── Contributor ─> view all info + change settings, but NO access management
│   ├── Reader ──────> view all info only
│   ├── Other roles (User Access Administrator, DevTest Labs Users...) ──> NO impact on Connect Health access
│   ├── Scope ──> All service instances (RECOMMENDED) OR single service instance (segregation)
│   ├── ⭐ Access granted if user has permission at directory level OR service-instance level
│   ├── ⚠️ "Invite Users" feature is NOT supported in Connect Health
│   └── Path ──> Configure ──> Users ──> Add ──> select role ──> pick user(s)/group(s)
│
├── 🩺 DUPLICATED ATTRIBUTE SYNC ERRORS (self-service remediation)
│   ├── ⭐ No upgrade or configuration required to enable the diagnosis feature
│   ├── ⚠️ Diagnose/fix requires CONTRIBUTOR Azure RBAC permission (not Reader)
│   ├── Errors ──> AttributeValueMustBeUnique / QuarantinedAttributeValueMustBeUnique
│   ├── Attributes ──> UserPrincipalName, ProxyAddresses, SipProxyAddress, OnPremiseSecurityIdentifier
│   ├── Statuses ──> Not Started / Manual Fix Required / Pending Sync
│   ├── Flow ──> Diagnose ──> Apply Fix ──> Pending Sync ──> error cleared after next sync cycle
│   └── Orphaned object ──> Source Anchor lost (on-prem deletion never synced) ──> no direct remap
│
└── 🧩 MODULE SCOPE (Summary & resources)
    ├── Entra Connect (AADC) ──> plan, design, implement + manage
    ├── Sign-in options ──> PHS (password hash sync), PTA (pass-through), Seamless SSO, federation
    ├── ⚠️ Federation scope EXCLUDES manual AD FS deployments
    └── Cross-cutting skills ──> troubleshoot sync errors + implement/manage Connect Health
```
