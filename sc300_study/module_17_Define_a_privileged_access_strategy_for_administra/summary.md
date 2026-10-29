# Module 17: Define a privileged access strategy for administrative users - Summary

<details><summary>Source lessons</summary>

- Introduction
- Define a privileged access strategy for administrative users
- Configure Privileged Identity Management for Azure resources
- Exercise configure Privileged Identity Management for Microsoft Entra roles
- Exercise assign Microsoft Entra roles in Privileged Identity Management
- Exercise assign Azure resource roles in Privileged Identity Management
- Plan and configure Privileged Access Groups
- Analyze Privileged Identity Management audit history and reports
- Create and manage emergency access accounts
- Summary and resources

</details>

```text
PRIVILEGED IDENTITY MANAGEMENT (PIM) - SC-300
│
├── 🧭 OVERVIEW & LICENSING
│   ├── PIM ──────────────> Entra ID service to manage, control & monitor privileged access
│   ├── Scope ────────────> Entra ID roles + Azure resources (M365 / Intune via Entra roles)
│   ├── ⭐ License ────────> Premium P2 is required for PIM
│   └── Core model ───────> Just-in-time + approval-based activation, time-bound (start/end dates)
│
├── 🔓 LEAST-PRIVILEGE STRATEGY
│   ├── Replace Global Admin ──> granular built-in / custom roles (Microsoft runs on ~10 GAs)
│   ├── PIM Discovery & insights (preview) ──> list privileged roles, reduce exposure
│   ├── Access reviews ──> reviewer = Members (self); enable "Require reason on approval"
│   ├── ⚠️ Reviews rely on email ──> populate secondary email on privileged accounts
│   ├── Protect FIRST ──> Global Administrator + Security Administrator (most harm if compromised)
│   ├── Guest users holding roles ──> manage ALL with PIM (high attack risk)
│   ├── Azure: non-critical subscriptions ──> still protect Owner + User Access Administrator
│   └── Stakeholders ──> identity architect, service owner, security owner, helpdesk, role users
│
├── 🎫 ROLE ASSIGNMENTS (Eligible vs Active)
│   ├── Eligible ──> must activate (MFA / justification / approval) before using the role
│   ├── Active ────> privileges always assigned — no action needed to use the role
│   ├── ⭐ Golden rule ──> zero permanent assignments except 2 break-glass accounts (permanent GA)
│   ├── Assign path ──> PIM > Microsoft Entra roles > Roles > + Add assignments
│   ├── Activate path ──> PIM > My roles > Activate (reason + MFA, once per session)
│   ├── Restricted scope ──> some roles limitable to admin unit / service principal / app
│   └── ⚠️ Group-assigned roles ──> the USER's membership activates, never the group itself
│
├── ⚙️ ROLE SETTINGS (draft before rollout)
│   ├── Controls ──> MFA, notification, incident ticket, require approval, activation duration
│   ├── Approver default ──> all Privileged Role Administrators (eligible + permanent)
│   ├── ⚠️ Multiple approvers ──> ONE approve/deny completes it — can't require 2+
│   ├── ⚠️ Self-approval blocked ──> eligible user who is also approver can't approve self
│   ├── Notification to ──> Global Admin, Privileged Role Admin, Security Administrator
│   └── Azure expiration options ──> 15 d / 1 mo / 3 mo / 6 mo / 1 yr / permanent (active+eligible)
│
├── ☁️ PIM FOR AZURE RESOURCES
│   ├── Onboard ──> PIM > Azure resources > Discover resources > Manage resource
│   ├── Who onboards ──> only subscription / management group OWNERS (GA may elevate access)
│   ├── No limit ──> number of resources managed; parent onboarded covers child resources
│   ├── Roles managed ──> Owner, User Access Admin, Contributor, Security Admin/Manager + custom
│   └── Service accounts ──> treat like users; supported as time-bound assignments
│
├── 👥 PRIVILEGED ACCESS GROUPS
│   ├── What ──> role-assignable group managed by PIM (member/owner eligibility + activation)
│   ├── Enable ──> group's Activity section; one JIT request ──> multiple Entra roles at once
│   ├── Membership managers ──> Privileged Role Admin, Global Admin, group owners only
│   └── Separate groups ──> distinct policies (e.g., stricter approval flow for B2B partners)
│
├── 🚨 EMERGENCY ACCESS ACCOUNTS (break-glass)
│   ├── Create ──> 2+ cloud-only accounts on .onmicrosoft.com (NOT federated / synced)
│   ├── Assignment ──> permanent Global Administrator role
│   ├── ⚠️ Exclude ≥1 account from Conditional Access + phone-based MFA policies
│   ├── Credentials ──> not tied to an individual; never expire / outside automated cleanup
│   ├── Monitor ──> Azure Log Analytics on sign-ins ──> email/SMS alerts to admins
│   └── Validate ──> every 90 days + on IT staff change / subscription change
│
└── 📊 AUDIT HISTORY & REPORTS
    ├── Resource audit ──> all group activity: PIM > Groups > Activity > Resource audit
    ├── My audit ──> personal role activity for the group (same path)
    ├── Filter ──> predefined dates or custom range
    └── ⚠️ Azure delegated resource management ──> service-provider assignments NOT shown
```
