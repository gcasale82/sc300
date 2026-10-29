# Module 03: Describe guest access and Business to Business accounts - Summary

<details><summary>Source lessons</summary>

- Introduction
- Describe guest access and Business to Business accounts
- Manage external collaboration
- Exercise - configure external collaboration
- Invite external users - individually and in bulk
- Exercise - add guest users to directory
- Exercise - invite guest users bulk
- Demo - manage guest users in Microsoft Entra ID
- Manage external user accounts in Microsoft Entra ID
- Manage external users in Microsoft 365 workloads
- Exercise - explore dynamic groups
- Implement and manage Microsoft Entra Verified ID
- Configure identity providers
- Implement cross-tenant access controls
- Summary and resources

</details>

```text
B2B COLLABORATION & EXTERNAL IDENTITIES (SC-300)
│
├── 👥 B2B FOUNDATIONS
│   ├── B2B collaboration ──> Guest signs in with OWN credentials; user object in YOUR tenant
│   ├── Guest markers ──────> UserType = "Guest" + #EXT# tag inside the user principal name
│   ├── Management ─────────> Guests live in your directory: managed like employees (groups)
│   ├── Invitation ─────────> NEVER expires; guest must redeem it to access resources
│   ├── SSO ────────────────> To all Entra-connected apps (M365, SaaS, on-premises)
│   ├── Security policy ────> Enforced by the HOST/inviting org (e.g. Conditional Access)
│   └── ⚠️ UserType has NO relation to sign-in method, directory role or permissions
│
├── ⚙️ COLLABORATION SETTINGS (Entra admin center > Identity > External Identities)
│   ├── Invite policy ──────> Default = ALL users, including guests, can invite
│   ├── Guest Inviter role ─> Lets non-admins invite when invitations are restricted
│   ├── Guest access tiers ─>
