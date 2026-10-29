# Module 15: Define access packages - Summary

<details><summary>Source lessons</summary>

- Introduction
- Define access packages
- Exercise create and manage a resource catalog with Microsoft Entra entitlement m
- Configure entitlement management
- Exercise add terms of use acceptance report
- Exercise manage the lifecycle of external users with Microsoft Entra identity go
- Configure and manage connected organizations
- Review per-user entitlements
- Summary and resources

</details>

```text
MICROSOFT ENTRA ENTITLEMENT MANAGEMENT (SC-300)
│
├── 📚 CORE TERMINOLOGY (learn these cold)
│   ├── Access package ────> Bundle of resources + policies; users REQUEST it
│   ├── Catalog ───────────> Container of resources + packages; creator auto-becomes owner
│   ├── Policy ────────────> Who can request / who approves / how long access lasts
│   ├── Assignment ────────> Grants ALL resource roles of the package; usually time-limited
│   ├── Connected org ─────> External Entra directory or domain allowed to request access
│   └── Resource roles ────> Group = member/owner · SharePoint = 3+ roles · Apps = custom
│
├── 📦 WHAT ACCESS PACKAGES GOVERN
│   ├── Direct ────────────> Entra security groups · M365 Groups/Teams · Enterprise apps
│   │                         (SaaS + federated SSO/provisioning) · SharePoint Online sites
│   ├── Indirect via group ─> M365 licenses (group-based licensing) · Azure role assignments
│   └── Indirect via group ─> Entra roles (use groups assignable to roles)
│
├── 🏢 PORTAL, LICENSES & ADMIN ROLES
│   ├── Portal path ───────> Entra admin center > ID Governance > Entitlement management
│   │                         (Catalogs · Access packages · Settings · Connected orgs · ToU)
│   ├── License ───────────> Entra ID P2 / ID Governance; exercises list P1, P2, EMS E3/E5
│   ├── Catalog tasks ─────> Security Administrator OR Conditional Access Administrator
│   ├── ToU tasks ──────────> Global administrator · External lifecycle: User administrator
│   ├── Catalog knobs ─────> Enabled = No stages it; "Enabled for external users" opens it
│   │                         to selected external directories
│   └── Graph API ─────────> EntitlementManagement.ReadWrite.All (manage) / .Read.All (query)
│
├── 📜 TERMS OF USE (ToU)
│   ├── Document ──────────> PDF format; 24pt font recommended for mobile users
│   ├── Options ───────────> Require expand · consent on EVERY device · expire consents
│   │                         (start date + frequency, OR duration in days)
│   ├── ⚠️ Every-device consent ──> users must register devices with Entra ID first
│   ├── Enforcement ───────> Conditional Access GRANT control (custom policy or create later)
│   ├── Reports ───────────> Accepted / Declined counts; per-user "View History"
│   ├── User view ──────────> myaccount.microsoft.com > Settings & Privacy > Privacy tab
│   └── ⚠️ Document not editable in place ──> upload new version + "Require reaccept" toggle;
│                                             changing other settings = create NEW terms
│
├── 🔄 EXTERNAL USER LIFECYCLE (Entitlement management > Settings > Edit)
│   ├── Default ───────────> Last assignment lost ──> blocked from sign-in; guest removed
│   │                         from the directory after 30 days
│   ├── Block sign-in ─────> Yes ──> user can no longer re-request access (set No if needed)
│   ├── Remove external ───> Yes + number of days; 0 = remove as soon as assignment is lost
│   └── ⚠️ Only guests INVITED via entitlement management are removed; pre-existing guests stay
│
├── 🤝 CONNECTED ORGANIZATIONS
│   ├── 3 ways to define ──> another Entra directory (any cloud) · direct-federated directory
│   │                         · non-Entra directory whose emails share one domain
│   ├── ⚠️ Limit ───────────> ONE directory or domain per connected organization
│   ├── Sponsors ──────────> Optional internal/external users = point of contact
│   ├── Required role ─────> Identity Governance administrator OR User administrator
│   └── State ─────────────> Auto-set to "Configured" when created
│
├── 🔍 REVIEW PER-USER ENTITLEMENTS
│   ├── Portal ────────────> Access packages > open package > Assignments; filter "Delivering"
│   │                         (provisioning errors) or "Expired"; Download = CSV export
│   ├── Roles to view ─────> IG admin · User admin · Catalog owner · AP manager · AP assigner
│   ├── Direct assign ─────> Possible only if the package's policy allows it
│   ├── Remove ────────────> Checkbox next to user > Remove button
│   └── PowerShell ────────> Connect-MgGraph -Scopes "EntitlementManagement.Read.All"
│                             Select-MgProfile beta · Get-MgEntitlementManagementAccessPackage*
│
└── ⭐ GOLDEN RULES
    ├── Delete a catalog ──> ONLY when it contains no access packages ⚠️
    ├── Packages complement ──> they do NOT replace group-based licensing / dynamic groups
    └── Best fit ──────────> Time-limited tasks · manager approvals · dept self-service · B2B
```
