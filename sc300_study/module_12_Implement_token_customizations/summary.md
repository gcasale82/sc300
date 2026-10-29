# Module 12: Implement token customizations - Summary

<details><summary>Source lessons</summary>

- Introduction
- Implement token customizations
- Implement and configure consent settings
- Integrate on-premises apps with Microsoft Entra application proxy
- Integrate custom SaaS apps for single sign-on
- Implement application-based user provisioning
- Monitor and audit access to Microsoft Entra integrated enterprise applications
- Create and manage application collections
- Summary and resources

</details>

```text
ENTERPRISE APP ACCESS & INTEGRATION (SC-300)
│
├── 🎯 MODULE SCOPE (Intro & Summary)
│   ├── Objectives ──> Token customizations, consent, App Proxy, SaaS SSO, provisioning
│   ├── Also covers ──> App collections + monitoring/auditing Entra ID enterprise apps
│   ├── Prerequisites ──> Manage users/admins in Entra ID + Conditional Access experience
│   └── Resources ──> ClaimsXRay tool, session-management docs, My Apps portal docs
│
├── 🔑 TOKEN CUSTOMIZATIONS
│   ├── Token lifetime policy ──> Policy object setting access/ID + refresh token lifetime
│   ├── Policy scope ──> All org apps OR a multitenant app OR one specific service principal
│   ├── Default policy ──> Applies org-wide unless overridden by a higher-priority policy
│   ├── CA session controls ──> Restrict auth sessions: unmanaged devices, sensitive data,
│   │                            high-impact users, critical business apps
│   ├── Optional claims ──> Add/change claims in tokens; v1.0, v2.0 and SAML supported
│   └── ⚠️ v2.0 tokens ──> Smaller by design; dropped claims must be requested per app
│
├── ✅ CONSENT SETTINGS
│   ├── Default ──> All users consent to permissions that don't require admin consent
│   ├── Options ──> Disable | Verified publishers + low-impact only | All apps | Custom policy
│   ├── Low-impact path ──> Requires classifying permissions first (prerequisite step)
│   ├── Disable consent ──> Blocks new consents; previously consented apps still usable
│   ├── Step-up consent ──> Risky request → admin consent; ON by default, acts only when
│   │                        end-user consent is enabled
│   ├── AADSTS90094 ──> "Only an admin can grant" error when request workflow is disabled
│   ├── Audit trail ──> Category: ApplicationManagement | Activity: Consent to application
│   │                    | Status Reason: Risky application detected
│   └── ⭐ Best practice ──> Allow user consent ONLY for apps from a verified publisher
│
├── 🏢 APP PROXY (ON-PREM APPS)
│   ├── Architecture ──> Cloud Application Proxy service + Connector on on-prem server
│   ├── Purpose ──> Remote access + SSO to on-prem web apps; replaces VPN / reverse proxy
│   ├── ⚠️ Not for internal users ──> Corporate-network users cause performance issues
│   ├── Supported apps ──> IWA, form-based, header-based, web APIs, RD Gateway, MSAL clients
│   ├── Flow ──> Sign-in → token → Proxy service (reads UPN/SPN) → Connector → app → back
│   └── ⭐ SSO ──> Connector performs any extra authentication on the user's behalf
│
├── 🔗 SAAS SSO & PROTOCOLS
│   ├── Entra gallery ──> Pre-configured apps; any other app can be set up manually
│   ├── OAuth 2.0 ──> AUTHORIZATION | OIDC ──> AUTHENTICATION (built on top of OAuth 2.0)
│   ├── SAML ──> Authentication; common with AD FS federated to Entra ID (enterprise apps)
│   ├── OIDC ──> Pure-cloud apps: mobile apps, websites, web APIs
│   └── ⚠️ Trap ──> OAuth = authorization, NOT authentication (classic distractor)
│
├── 👥 USER PROVISIONING & SCIM
│   ├── App provisioning ──> Auto create/maintain/remove user identities in SaaS apps
│   ├── Capabilities ──> Deprovisioning, data sync, group provisioning, audits, alerts,
│   │                    Log Analytics integration, attribute-mapping customization
│   ├── Brown field ──> Matches existing identities already present in the target system
│   ├── Gallery marker ──> "Provisioning" icon = automatic connector supported
│   ├── Manual mode ──> No connector: create users by hand or upload a spreadsheet
│   ├── SCIM 2.0 ──> Standard schema + /Users and /Groups REST endpoints (POST/PUT/DELETE)
│   └── ⭐ SCIM + SAML/OIDC ──> End-to-end, standards-based access management
│
├── 📊 MONITOR & AUDIT ACCESS
│   ├── Usage & insights ──> Identity > Applications > Enterprise apps > Activity section
│   ├── Report answers ──> Top used apps, most failed sign-ins, top sign-in errors per app
│   ├── Features ──> Sort success/failure, "load more", date range, per-day drill-down
│   ├── Audit logs path ──> Entra ID > Monitoring > Audit logs (compliance records)
│   ├── Audit roles ──> Security Admin, Security Reader, Report Reader, Global Reader, Admin
│   ├── Log fields ──> Date/time, service, category + activity, status, target, initiator
│   └── App audit view ──> Enterprise apps > Activity > Audit logs (type pre-filtered)
│
└── 🗂️ APP COLLECTIONS (MY APPS)
    ├── License ⭐ ──> Microsoft Entra ID P1 or P2 required for collections
    ├── Purpose ──> Group related apps on separate tabs; filter of apps user can access
    ├── Admin path ──> Identity > Applications > Enterprise apps > App launchers > New
    ├── Collection parts ──> Name + description, ordered apps, owners, Review + Create
    ├── ⚠️ Owners ──> Assigned via Azure portal → manage the collection ONLY there
    ├── My Apps portal ──> myapps.microsoft.com (no Azure or M365 subscription needed)
    └── User abilities ──> Discover apps, request self-service apps, personal collections,
                           remove apps from the default Apps collection
```
