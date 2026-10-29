# Module 11: Discover apps by using Microsoft Defender for Cloud Apps and Active Directory Fe - Summary

<details><summary>Source lessons</summary>

- Introduction
- Discover apps by using Microsoft Defender for Cloud Apps and Active Directory Fe
- Configure connectors to apps
- Exercise implement access management for apps
- Design and implement app management roles
- Exercise create a custom role to manage app registration
- Configure preintegrated gallery SaaS apps
- Implement and manage policies for OAuth apps
- Summary and resources

</details>

```text
APP DISCOVERY & ACCESS MANAGEMENT (SC-300)
│
├── 🎯 MODULE SCOPE (Objectives)
│   ├── Discover apps ─────> MDCA Cloud Discovery + AD FS application activity report
│   ├── Access management ─> user/group assignments, consent & registration restrictions
│   ├── App mgmt roles ────> owners, built-in admin roles, custom roles
│   └── Gallery SaaS apps ─> properties, app connectors, OAuth app policies
│
├── 🔍 APP DISCOVERY (Microsoft Defender for Cloud Apps)
│   ├── CASB ──────────────> security policy enforcement point between users & cloud providers
│   ├── MDCA ──────────────> Microsoft's CASB: log collection, API connectors, reverse proxy
│   ├── Cloud Discovery ───> analyzes traffic logs to expose Shadow IT in your org
│   │   ├── Snapshot report ──> manually upload firewall/proxy log files once
│   │   └── Continuous report ─> MDCA log collectors forward logs periodically
│   ├── ⭐ Dashboard first ────> usage overview, top users/IPs, HQ map, risk scores, alerts
│   ├── Filters ───────────> tags, domains, categories, compliance (HIPAA, ISO 27001, SOC 2,
│   │                       PCI-DSS), risk score, security/legal/usage factors
│   ├── Cloud app catalog ─> 16,000+ apps scored on 80+ risk factors ──> sanction/unsanction
│   └── Conditional Access App Control ──> real-time visibility & control in cloud apps
│
├── 🛡️ AD FS APPLICATION ACTIVITY REPORT (Migration to Microsoft Entra ID)
│   ├── AD FS ─────────────> on-prem standards-based identity service (federation SSO)
│   ├── Portal path ───────> Entra ID > Enterprise applications > Activity > Usage & insights
│   ├── Report scope ──────> AD FS apps with active user sign-ins in the last 30 days
│   ├── Access roles ──────> Global Admin / Global Reader, Report Reader, Security Reader,
│   │                       Application Admin, Cloud Application Admin
│   ├── Migration status ──> "Ready to migrate" / "Needs review" / "Additional steps required"
│   ├── Modern protocols ──> SAML & OIDC first ──> gallery connector or app registration
│   ├── Legacy protocols ──> integrate via Application Proxy / Microsoft Entra Domain Services
│   └── ⭐ Migrate ALL app auth to Entra ID ──> one control plane for identity & access
│
├── 🔌 APP CONNECTORS (MDCA)
│   ├── Mechanism ─────────> use provider APIs for visibility + governance; all traffic HTTPS
│   ├── ⚠️ API throttling ──> per-service limits/quotas ──> some policies run for hours or days
│   ├── Multi-instance ────> connect several instances (e.g. 2x Salesforce) in one console
│   │   └── ⚠️ only for API-connected apps ──> NOT Cloud Discovered or Proxy connected apps
│   ├── Scans ─────────────> users, groups, activities, files; periodic every 12 hours +
│   │                       real-time scan on every detected change
│   └── Governance powers ─> suspend users, revoke passwords, remove app tokens, quarantine
│                           files (including files in trash)
│
├── 🧩 APP MANAGEMENT ROLES & DELEGATION
│   ├── Restrict defaults ─> User settings: "Users can register applications" = No;
│   │                       "Users can consent to apps accessing company data" = No
│   ├── Application Developer role ──> restores register + consent on own behalf when disabled
│   ├── First owner ───────> creator of an app registration is auto-added as first owner
│   ├── Owners ────────────> app registration: users + service principals; enterprise app: users
│   │   └── ⚠️ groups can NEVER be owners of either object type
│   ├── Application Administrator ────────> all app config incl. App Proxy; consent except Graph
│   ├── Cloud Application Administrator ──> same permissions but NO application proxy
│   │   └── ⚠️ neither manages Conditional Access; both can add credentials (privilege
│   │           elevation via app impersonation)
│   ├── Custom roles ──────> two separate steps: create role definition, then role assignment
│   │   ├── Scopes ────────> directory-wide (all apps) or single app registration (object)
│   │   ├── ⚠️ work only in current App registrations screen (not legacy); no Entra portal
│   │   │       access if "Restrict access to Microsoft Entra ID administration portal" = Yes
│   │   └── ⚠️ assignments visible only in "All applications" tab, not "Owned applications"
│   ├── ⭐ Built-in roles ──> recommended way to delegate broad app configuration rights
│   └── Exercise ──────────> Roles and administrators > New custom role > Basics > Permissions
│                           (e.g. "credentials" manage) > Create
│
├── 🎨 GALLERY SAAS APPS (Preintegrated)
│   ├── Add app ───────────> Enterprise applications > + New application > browse gallery
│   ├── Properties ────────> "Enabled for users to sign in?", "User assignment required?",
│   │                       "Visible to users?" (My Apps + Microsoft 365 app launcher)
│   ├── Custom logo ───────> 215 x 215 pixels, .png format
│   ├── Notes field ───────> free documentation for app management
│   └── Exercise needs ────> Microsoft Entra ID Premium P2 trial + Global Administrator account
│
└── 🔐 OAUTH APP POLICIES (MDCA)
    ├── Portal path ───────> https://security.microsoft.com > Cloud apps > OAuth apps
    ├── Policy triggers ───> permission level, number of authorizing users, group membership
    ├── Community use ─────> common / uncommon / rare ──> triage high-severity permissions
    └── ⚠️ Mark app as banned ──> disables the corresponding Enterprise Application
```
