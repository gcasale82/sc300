# Module 13: Plan your line of business application registration strategy - Summary

<details><summary>Source lessons</summary>

- Introduction
- Plan your line of business application registration strategy
- Implement application registration
- Register an application
- Configure permission for an application
- Grant tenant-wide admin consent to applications
- Implement application authorization
- Exercise add app roles to an application and receive tokens
- Manage and monitor application by using app governance
- Summary and resources

</details>

```text
APP REGISTRATIONS, PERMISSIONS & GOVERNANCE (SC-300)
│
├── 🎯 WHY APPS INTEGRATE WITH MICROSOFT ENTRA ID
│   ├── User & app authentication / authorization ──> core IAM services from the directory
│   ├── SSO (federation or password) ───────────────> single sign-on to integrated apps
│   ├── App roles (RBAC) ───────────────────────────> directory-defined roles for app authz
│   ├── OAuth authorization services ───────────────> M365 + others authorize API/resource access
│   ├── Provisioning & sync ────────────────────────> user provisioning + attribute mappings
│   ├── Application Proxy ──────────────────────────> publish a private-network app to the internet
│   └── Schema extensions ──────────────────────────> extra attributes on user / SP objects
│
├── 🧩 APP OBJECT vs SERVICE PRINCIPAL ⭐ top exam topic
│   ├── Application object ───> "App registrations" blade; ONE per app, HOME tenant only
│   │   └── holds ──> redirect URIs, secrets, exposed scopes, app roles, SSO/proxy config
│   ├── Service principal ────> "Enterprise applications" blade; instance in EVERY tenant used
│   │   └── holds ──> consent records, role assignments, CA policies, claims transformation
│   ├── ⚠️ Microsoft Graph ──> REQUIRES an app object before creating a service principal
│   └── SPs also created by ──> first user consent, M365 subscription, gallery add, App Proxy
│
├── 📝 REGISTER AN APP (Entra admin center > Identity > Applications > App registrations)
│   ├── Managed by ──> Application Administrator / Cloud Application Administrator
│   ├── ⚠️ Default ──> ALL users can register apps and consent on their own behalf
│   ├── Trust is unidirectional ──> the app trusts the identity platform, never the reverse
│   ├── Audience ──> single-tenant (own directory) | multitenant (any Entra dir ± personal MSAs)
│   ├── Redirect URI ──> where the platform sends security tokens after authentication
│   ├── Platforms ──> Web & SPA = manual URI | iOS/macOS = Bundle ID | Android = pkg + hash
│   ├── Credentials ──> certificate (.cer/.pem/.crt) or client secret ("application password")
│   │   ├── ⭐ Certificates ──> higher assurance, recommended for production
│   │   └── ⚠️ Secret value ──> shown ONLY once, never displayed again after leaving the page
│   └── ⚠️ Web API registration ──> no redirect URI needed (no interactive user sign-in)
│
├── 🔓 SCOPES & PERMISSIONS ("Expose an API" > Add a scope)
│   ├── Full scope string ──> App ID URI + scope name (e.g. api://<app-id>/Employees.Read.All)
│   ├── Scope naming ──> resource.operation.constraint (Employees.Read.All / Employees.Write.All)
│   ├── Who can consent field ──> "Admins and users" (user prompt) | "Admins only" (privileged)
│   ├── Delegated permissions ──> signed-in user present; app acts AS that user
│   ├── Application permissions ──> no user present (daemons / background); ADMIN consent ONLY
│   ├── ⚠️ Effective permissions ──> delegated = intersection with signed-in user's privileges
│   │                              └ application = FULL privileges the permission implies
│   ├── OIDC scopes ──> openid, email, profile, offline_access (offline_access = refresh tokens)
│   ├── ⚠️ NOT supported ──> address & phone OIDC scopes
│   └── Pre-authorized client app (PCA) ──> suppresses consent prompt for trusted clients
│
├── ✅ CONSENT
│   ├── User consent ──> scope parameter on /authorize; recorded, not re-prompted at sign-in
│   ├── ⚠️ Auto-added to first consent ──> offline_access + user.read
│   ├── Tenant-wide admin consent paths:
│   │   ├── App registrations > API permissions > Grant admin consent (apps you registered)
│   │   └── Enterprise applications > Security > Permissions > Grant admin consent
│   ├── ⚠️ New tenant-wide grant ──> REVOKES prior tenant-wide grants (per-user grants kept)
│   ├── Consenter roles ──> Privileged Role Administrator, or custom role w/ grant permission
│   ├── Admin consent URL ──> login.microsoftonline.com/{tenant-id}/adminconsent?client_id={id}
│   ├── ⚠️ tenant parameter ──> use tenant ID or verified domain; never "common"
│   ├── ⚠️ Admin-restricted scopes ──> User.Read.All, Directory.ReadWrite.All, Groups.Read.All
│   └── Application permissions ──> MUST use the admin consent endpoint (not /authorize)
│
├── 🛑 RESTRICT SELF-SERVICE (User settings)
│   ├── "Users can register applications" ──> No (Microsoft Entra ID > User settings)
│   ├── "Users can consent to apps accessing company data on their behalf" ──> No
│   │   └── path ──> Microsoft Entra ID > Enterprise applications > User settings
│   └── ⚠️ Once disabled ──> an ADMIN must consent to every new app users need
│
├── 👥 APP ROLES (application authorization)
│   ├── roles claim ──> emitted in token for direct assignments AND group membership
│   ├── Declare via ──> App roles UI (public preview) OR App manifest editor
│   ├── Create role ──> display name, member types (User/Groups), Value (Survey.Create),
│   │                  description is MANDATORY, enable toggle
│   └── Assign ──> Enterprise applications > Users and groups > Add user/group (or MS Graph)
│
└── 🛡️ APP GOVERNANCE (add-on to Defender for Cloud Apps)
    ├── Target ──> OAuth-enabled apps accessing Microsoft 365 data via Microsoft Graph APIs
    ├── Pillars ──> Insights | Governance | Detection | Remediation
    ├── Enable ──> portal.cloudappsecurity.com > Settings > Threat Protection > App Governance
    ├── Prereqs ──> Office 365 connected + Entra ID apps enabled in Defender for Cloud Apps
    └── Proof of sync ──> OAuth App Reputation / OAuth Phishing Detection / App Governance policies
```
