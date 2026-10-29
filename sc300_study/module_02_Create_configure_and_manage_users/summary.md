# Module 02: Create, configure, and manage users - Summary

<details><summary>Source lessons</summary>

- Introduction
- Create, configure, and manage users
- Exercise - assign licenses to users
- Exercise - restore or remove deleted users
- Create, configure, and manage groups
- Exercise - add groups in Microsoft Entra ID
- Configure and manage device registration
- Manage licenses
- Exercise - change group license assignments
- Exercise - change user license assignments
- Create custom security attributes
- Explore automatic user creation
- Summary and resources

</details>

```text
MANAGE USERS, GROUPS, LICENSES & DEVICES (SC-300)
│
├── 👤 USERS & LIFECYCLE
│   ├── Portal ────────────────> Identity > Users > All users (one directory at a time;
│   │                             switch via "Directory + Subscription" / toolbar button)
│   ├── Cloud identities ──────> Source: Microsoft Entra ID or External Entra directory
│   ├── Dir-synchronized ──────> Source: Windows Server AD; Cloud Sync recommended,
│   │                             Connect Sync for device sync or groups > 50,000 members
│   ├── Guest users ───────────> Source: Invited user (vendors / contractors)
│   └── Deleted users ─────────> Suspended 30 days ⚠️ then auto-permanent (no restore)
│       └── Restore roles ─────> Global admin / Partner Tier-1 / Tier-2 / User admin
│
├── 👥 GROUPS
│   ├── Security groups ───────> Resource access; members: users, devices, service
│   │                             principals; requires an administrator
│   ├── Microsoft 365 groups ──> Collaboration (mailbox, calendar, SharePoint); admins
│   │                             AND users can create
│   ├── Membership types ──────> Assigned | Dynamic User | Dynamic Device
│   ├── ⚠️ Dynamic Device ─────> Security groups ONLY (M365 groups = dynamic users only)
│   └── ⭐ Dynamic membership ──> Needs Entra ID P1 (or Intune for Education for device
│                                 rules); all rules reevaluated on attribute change
│
├── 📱 DEVICE REGISTRATION
│   ├── Entra registered ──────> BYOD / mobile; local credential sign-in; user OR org
│   │                             owned; MDM / MAM via Intune
│   ├── Entra joined ──────────> Cloud-first; org account sign-in; OOBE / Autopilot /
│   │                             bulk enrollment; ⚠️ Windows Home editions NOT supported
│   ├── Hybrid Entra joined ───> On-prem AD + Entra; Group Policy / Config Manager;
│   │                             Windows Server 2016/2019/2022; password or WHfB
│   └── ⚠️ Device writeback ───> No longer supported ──> replaced by Cloud Kerberos Trust
│
├── 🪪 GROUP-BASED LICENSING
│   ├── ⭐ License needed ─────> Entra ID P1+ or Office 365 E3+ (paid or trial)
│   ├── Only assignment UI ────> M365 admin center > Billing > Licenses (also PowerShell
│   │                             and Microsoft Graph for management)
│   ├── Entra admin center ────> Groups > Licenses blade = read-only view of assignments
│   ├── Supported groups ──────> Any security group: synced, cloud-only, or dynamic
│   ├── Capacity rule ─────────> License per unique member: 1,000 members = 1,000 licenses
│   ├── Dedup rule ────────────> Same license direct + group = consumed only once
│   └── Usage location ────────> Set BEFORE assigning; unset users inherit the directory
│                                 location; changes apply within minutes
│
├── 🚨 LICENSING ERRORS (group errors recorded on user object)
│   ├── Direct (Set-MgUserLicense) ──> Fails immediately; group failures surface in portal
│   ├── CountViolation ────────> Not enough licenses in the tenant
│   ├── MutuallyExclusiveViolation ──> Conflicting service plans (E1 + E3: SharePoint /
│   │                             Exchange Plan 1 vs Plan 2)
│   ├── DependencyViolation ───> Another product depends on this service plan
│   ├── ProhibitedInUsageLocationViolation ──> Usage location missing or not allowed
│   ├── Duplicate proxyAddresses ──> "Proxy address is already being used" (Exchange)
│   ├── LicenseAssignmentAttributeConcurrencyException ──> Multi-group race; self-resolves
│   └── ⭐ Conflicts ──────────> Entra never auto-resolves — admin decides; fix, then
│                                 Reprocess button (on the group or the user)
│
├── 🛑 DELETION & ADD-ONS (exam traps)
│   ├── Delete licensed group ──> You must remove ALL group licenses FIRST
│   ├── Dependent licenses ────> On group deletion: inherited ──> direct (e.g., Audio
│                                 Conferencing depends on Skype for Business)
│   └── Add-on products ───────> Prerequisite must live in the SAME group (Workplace
│                                 Analytics needs Exchange Online Plan 1 or 2)
│
├── 🔄 MIGRATION: direct ──> group licenses
│   ├── 1. Keep automation ────> Leave existing PowerShell running as is
│   ├── 2. Build group ────────> Mirror the current license state via the group
│   ├── 3. Verify ─────────────> User shows license as direct AND inherited (1 consumed);
│   │                             check group processing state + Audit Logs
│   └── 4. Remove direct ──────> Gradually, on a subset first ⚠️ leftover direct licenses
│                                 persist when the user leaves the group
│
└── 🧩 ATTRIBUTES & AUTOMATIC PROVISIONING
    ├── Custom security attributes ──> Business key-value pairs (Boolean / integer /
    │                             string); tenant-wide; users & service principals
    │   └── ⚠️ NOT supported in: Entra Domain Services, SAML claims, JWT claims
    ├── SCIM 2.0 flow ──────────> HCM system ──> Entra Provisioning Service ──> target
    │                             system (SCIM endpoint): auto provision / deprovision
    └── API-driven inbound provisioning ──> GA March 2024; Workday, SAP SuccessFactors
                                  or custom HR system pushes to the provisioning API
```
