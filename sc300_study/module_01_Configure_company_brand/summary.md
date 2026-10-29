# Module 01: Configure company brand - Summary

<details><summary>Source lessons</summary>

- Introduction
- Configure company brand
- Configure and manage Microsoft Entra roles
- Exercise manage users roles
- Configure delegation by using administrative units
- Analyze Microsoft Entra role permissions
- Configure and manage custom domains
- Configure tenant-wide setting
- Exercise - setting tenant-wide properties
- Summary and resources

</details>

```text
CONFIGURE & MANAGE A MICROSOFT ENTRA TENANT (SC-300)
│
├── 🎨 COMPANY BRANDING (sign-in pages)
│   ├── License ──────────> Entra ID P1, P2 or Office 365 (else menu option is hidden)
│   ├── Portal path ──────> Entra ID > Manage > Company branding
│   ├── Background image ─> .png/.jpg, max 1920x1080 px AND max 300,000 bytes
│   ├── Banner logo ──────> Shown after username entry + on My Apps portal
│   ├── Username hint ────> Unicode, no links/code, max 64 chars ⚠️ avoid it for guests
│   └── Sign-in page text > Unicode, max 1,024 chars (helpdesk phone, legal note)
│
├── 👑 KEY ENTRA ROLES
│   ├── Global Administrator ───> All admin features; tenant creator = first GA
│   ├── User Administrator ─────> Users & groups, support tickets, service health, resets
│   ├── Billing Administrator ──> Purchases, subscriptions, support tickets, health
│   ├── ⭐ Least privilege ─────> Microsoft does NOT recommend using Global Administrator
│   └── Add / remove roles ─────> User > Assigned roles > + Add assignment / Remove
│
├── 🔀 AZURE ROLES vs ENTRA ROLES (classic exam trap)
│   ├── Azure roles ────> Manage Azure resources; scope: mgmt group / sub / RG / resource
│   ├── Entra roles ────> Manage Entra resources; scope: tenant OR administrative unit
│   ├── Custom roles ───> Both support them; Entra custom: directory OR app-reg scope only
│   ├── GA elevation ───> "Access management for Azure resources" = User Access Administrator
│   └── ⚠️ GA spans Entra + M365 (Exchange, SharePoint) but NOT Azure resources by default
│
├── 🧰 ASSIGNING ROLES (multiple methods, no built-in restrictions)
│   ├── Roles and administrators > select role > + Add assignments (user or group)
│   ├── User / Group blade > Assigned roles > + Add assignments
│   ├── Broad scope (subscription / RG / mgmt group) ──> Access control (IAM)
│   ├── PowerShell or Microsoft Graph API
│   └── PIM (requires Entra ID P2) ──> just-in-time elevation; ONE role at a time ⚠️
│
├── 🏢 ADMINISTRATIVE UNITS (delegation via least privilege)
│   ├── Container of ──> ONLY users, groups and devices
│   ├── Effect ────────> Role permissions restricted to AU members, not whole tenant
│   ├── Roles usable ──> Authentication / Helpdesk / License / Password / User administrator
│   └── Lifecycle ─────> Initial adoption ──> Pruning ──> Stabilization
│
├── 🔓 DEFAULT USER PERMISSIONS (member vs guest)
│   ├── Member user ───> Enumerate users, invite guests, create groups, register apps
│   ├── Guest user ────> Fewer rights; CANNOT enumerate all users/groups ⚠️
│   ├── User settings ─> ONLY place to change defaults (apps, portal access, LinkedIn, guests)
│   ├── Role description > Shows "Role permissions" + "Guest & service principal basic read"
│   └── Directory Readers > Grants non-admins portal read (guests & SPNs lack it by default)
│
├── 🌐 CUSTOM DOMAINS (manageable ONLY by Global Administrator)
│   ├── Limits ─────────> 900 managed domain names; 450 if ALL domains are federated
│   ├── Primary domain ─> Default suffix for new users; must be verified + non-federated
│   ├── Subdomains ─────> Auto-verified once root domain (contoso.com) is verified
│   ├── Delete blocked ─> If any user/group address or app ID URI still uses the domain
│   └── ForceDelete ────> Renames UPN / email / proxyAddress / identifierUri to onmicrosoft.com
│       ├── Requires ──> Under 1000 references; Exchange refs fixed in Exchange Admin Center
│       └── Fails if ──> Over 1000 objects OR a multitenant app is included
│
├── 🧩 TENANT-WIDE SETTINGS
│   ├── Tenant Properties > Name, Tenant ID, technical contact, privacy contact, statement URL
│   ├── ⚠️ Country/region > Set at tenant creation; can NEVER be changed afterwards
│   ├── Guest options ───> Guest access level, who can invite, guest self-service
│   └── Security defaults > FREE for all: MFA registration, admin MFA, legacy auth blocked
│
└── 🛡️ APP DELEGATION & SECURITY PLAN
    ├── Application Administrator ──────> All apps incl. App Proxy; NO Conditional Access
    ├── Cloud Application Administrator > Same but NO Application Proxy ⚠️ (no on-prem rights)
    ├── Enterprise Application Owner ───> SSO + assignments + owners for OWNED apps only
    ├── Application Registration Owner ─> Manifest + owners for OWNED registrations
    ├── Registration default ───────────> All users can register; restrict + App Developer role
    └── ⭐ Protect privileged access ────> Emergency access accounts + Security defaults (MFA)
```
