# Module 14: Plan for app registration - Summary

<details><summary>Source lessons</summary>

- Introduction
- Plan for app registration
- Explore application objects and service principals
- Create app registrations
- Configure app authentication
- Configure API permissions
- Create app roles
- Summary

</details>

```text
APP REGISTRATIONS IN MICROSOFT ENTRA ID (SC-300)
│
├── 🏢 REGISTRATION BASICS (Portal & Prerequisites)
│   ├── Path ─────────> Entra admin center > Identity > Applications > App registrations > New
│   ├── Role ─────────> At least "Application Developer" to register an app
│   ├── Output ───────> Record Application (client) ID + Directory (tenant) ID from Overview
│   ├── Admin center ─> Auto-creates application object + service principal in home tenant
│   ├── MS Graph ⚠️ ──> Creating the service principal is a SEPARATE step (not automatic)
│   └── Visibility ───> New apps hidden by default; enable via Enterprise apps > Properties
│
├── 🧩 APP OBJECT vs SERVICE PRINCIPAL (top exam topic)
│   ├── Application object ─> Global "blueprint"/template in home tenant; 1-to-1 with the app
│   ├── Service principal ──> Local "instance" per tenant using the app; 1-to-many
│   ├── Multitenant flow ───> SP at registration (home) + at FIRST user sign-in in other tenants
│   ├── Single-tenant ──────> Only ONE service principal (home tenant)
│   ├── SP types ───────────> Application / Managed identity / Legacy (migrate when possible)
│   ├── SP stores ──────────> Role assignments, consent grants, local policies (incl. CA)
│   ├── Find SPs ───────────> App registration overview > "Managed application in local directory"
│   └── Deletion trap ⚠️ ───> Deleting app object deletes home SP; restoring does NOT restore SP
│
├── 🌐 SUPPORTED ACCOUNT TYPES (audience)
│   ├── Single-tenant ───────> "Accounts in this directory only" (your users + guests)
│   ├── Multitenant ─────────> "Accounts in any Microsoft Entra directory" (any work/school)
│   └── Multi + personal ────> Adds personal Microsoft accounts (Skype, Xbox, Outlook.com)
│
├── 🔑 PLATFORM AUTHENTICATION (Authentication blade > Add a platform)
│   ├── Web ─────────> Redirect URI entered manually (+ front-channel sign-out, token settings)
│   ├── SPA ─────────> Redirect URI; auto-uses auth code flow + PKCE + CORS (NOT implicit grant)
│   ├── iOS / macOS ─> Bundle ID; redirect URI auto-generated
│   ├── Android ─────> Package name + signature hash; redirect URI auto-generated
│   └── Mobile/desktop ─> Suggested URIs: ...nativeclient (embedded) / http://localhost (system)
│
├── 🔒 REDIRECT URI RULES (frequent exam traps)
│   ├── Scheme ────────> Must start with https (localhost exception during development)
│   ├── Case ──────────> Case-sensitive; must match the app's running URL path
│   ├── Trailing slash ─> No path segment = "/" appended; path present = no slash appended
│   └── Forbidden chars ─> !  $  '  (  )  ,  ;
│
├── 🔐 CREDENTIALS & TOKENS
│   ├── Confidential client ─> Can hold credentials (web app with trusted back-end server)
│   ├── Best practice ⭐ ─────> Certificates over secrets; managed identities preferred (Azure)
│   └── Tokens ─────> New apps get v2 access tokens by default (since August 2024)
│
├── 📋 API PERMISSIONS & CONSENT
│   ├── OAuth 2.0 ─────> External app accesses web-hosted resources on behalf of a user
│   ├── Resource ──────> Identified by Application ID URI; features split into scopes
│   ├── Delegated perms ─> Act as signed-in user (only data the user could access)
│   ├── Graph basics ───> email, offline_access, openid, profile (OpenID Connect scopes)
│   ├── Admin consent ──> Admin approves once for ALL users (no per-user prompts)
│   └── Golden rule ⭐ ──> Least privilege: request only the permissions the app needs
│
└── 👥 APP ROLES (fine-grained authorization)
    ├── What ──────> Custom claim emitted in tokens; app uses it for authorization
    ├── Members ───> Allowed member types = Users/Groups; assigned like Enterprise app roles
    ├── Token ─────> "roles" claim, e.g. ["Approver","Reviewer"]
    ├── Baseline ⭐ ─> Always define a base user/reader role (avoids everyone getting admin)
    └── Benefit ───> Avoids group overage claims; no Microsoft Entra ID P1 license required
```
