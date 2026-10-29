# SC-300 - All Summary

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


---

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


---

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


---

# Module 04: Plan, design, and implement Microsoft Entra Connect - Summary

<details><summary>Source lessons</summary>

- Introduction
- Plan, design, and implement Microsoft Entra Connect
- Implement manage password hash synchronization (PHS)
- Implement manage pass-through authentication (PTA)
- Explore pass-through authentication and seamless single sign-on (SSO)
- Implement and manage federation
- Trouble-shoot synchronization errors
- Implement Microsoft Entra Connect Health
- Manage Microsoft Entra Health
- Summary and resources

</details>

```text
HYBRID IDENTITY WITH MICROSOFT ENTRA CONNECT (SC-300)
│
├── 🧭 HYBRID IDENTITY OVERVIEW
│   ├── Goal ──> unified authN/authZ across on-prem AD + cloud (Microsoft Entra ID)
│   └── Entra Connect ──> sync + PHS + PTA + federation + health; included in subscription
│
├── 🔑 AUTHENTICATION METHODS (choose by effort / UX / scenarios / continuity)
│   ├── PHS ──> hash of the hash synced; ⭐ enable with ANY chosen method
│   │   ├── Runs every 2 min (can't change) ──> initial sync = ALL in-scope users
│   │   ├── Selective PHS possible after initial sync; auto-enabled by Express Settings
│   │   ├── FIPS blocks MD5 ──> edit miiserver.exe.config (enforceFIPSPolicy = false)
│   │   └── Backup for PTA failover ──> ⚠️ manual switch via Entra Connect, never auto
│   ├── PTA ──> agents validate passwords directly against on-prem AD
│   │   ├── 3 agents for HA ──> 1 on Connect server + 2 extra
│   │   ├── Agents need DC access + internet ──> ⚠️ NOT in perimeter network
│   │   ├── Enforces on-prem state instantly (disabled / locked / sign-in hours)
│   │   └── Tenant-level feature ──> wait 12h before shutting down old AD FS
│   └── Federation ──> hands auth to AD FS or PingFederate (8.4+)
│       ├── Required for smartcards, 3rd-party MFA, DOMAIN\username sign-in
│       └── ⚠️ Exam scope excludes MANUAL AD FS deployments
│
├── 🪟 SEAMLESS SSO
│   ├── Combines with PHS or PTA ──> ⚠️ NOT applicable to AD FS
│   ├── Browser flow ──> Kerberos ticket for AZUREADSSOACC computer account
│   └── Native clients ──> WS-Trust MEX endpoint (exclusive) → SAML → tokens
│
├── 🏗️ ENTRA CONNECT DESIGN CONCEPTS
│   ├── sourceAnchor (immutableId) ──> immutable for object lifetime
│   │   ├── Single forest ──> objectGUID (also Express default)
│   │   └── Rules ──> <60 chars, no special chars, globally unique, not name-based
│   ├── UPN suffix ──> must be a VERIFIED domain else *.onmicrosoft.com is applied
│   ├── Topologies ──> Express install = single forest + single tenant ONLY
│   │   ├── Staging server ──> reads all, writes nothing (standby / DR)
│   │   └── ⚠️ 1:1 sync server per tenant; filter for mutually exclusive objects
│   ├── Engine ──> Connector Space (per directory) + ONE Metaverse; sync rules + run profiles
│   └── Cloud sync ──> lightweight provisioning agent; disconnected multi-forest (M&A)
│       └── Groups up to 50,000 members ──> use OU scoping filter; sync every 2 min
│
├── 🤝 FEDERATION DEPLOYMENT (Entra Connect wizard)
│   ├── Prereqs ──> local admin on AD FS + WAP servers; WinRM connectivity
│   ├── WAP ──> perimeter network, handles extranet auth requests
│   ├── Service account ──> group Managed Service Account OR domain user account
│   ├── AlternateID ──> sign in with mail attribute when UPN is non-routable
│   └── Device writeback ──> devices synced to AD → device-based Conditional Access
│
├── 🚨 SYNC ERROR TROUBLESHOOTING (export to Entra; Health report refreshes every 30 min)
│   ├── InvalidSoftMatch ──> hard match fails; soft match hits object w/ different immutableId
│   ├── AttributeValueMustBeUnique ──> duplicate ProxyAddresses or UPN values
│   ├── ObjectTypeMismatch ──> user vs group/contact share one proxyAddress
│   ├── FederatedDomainChangeError ──> UPN suffix moved between two FEDERATED domains
│   ├── LargeObject ──> ⚠️ 15-certificate hard limit (userCertificate / userSMIMECertificate)
│   ├── IdentityDataValidationFailed ──> invalid characters / format in UPN
│   ├── Admin Role Conflict ──> no soft-match to cloud user holding an admin role
│   │   └── Fix ──> strip roles → hard-delete quarantined object → sync → restore roles
│   └── Generic fix ──> correct the SOURCE directory, then let Connect sync it
│
├── 🩺 ENTRA CONNECT HEALTH (⭐ requires Entra ID Premium P1)
│   ├── Prereqs ──> global admin, agent per server, PowerShell 4.0+, FIPS disabled
│   ├── Ports ──> 443 (+ legacy 5671; latest agents need only 443)
│   ├── Agents ──> AD FS agent on its own server (⚠️ never on the Sync server)
│   │   └── Sync agent ──> auto-installed with latest Entra Connect
│   ├── RBAC ──> Owner (manage access + settings) / Contributor (settings) / Reader (view)
│   │   └── ⚠️ Diagnose needs Contributor; scope = all instances or single instance
│   ├── Email alerts ON by default ──> sync errors arrive as a daily digest
│   └── Delete server ──> stops monitoring; does NOT uninstall agent or erase data
│
└── 🧯 SELF-SERVICE DIAGNOSE (duplicated attribute remediation)
    ├── Covers ──> UPN / ProxyAddresses / SipProxyAddress / OnPremiseSecurityIdentifier
    ├── Statuses ──> Not Started → Manual Fix Required → Pending Sync
    └── Orphaned object ──> Apply Fix in portal → cleared after next sync cycle
```


---

# Module 04: Plan, design, and implement Microsoft Entra Connect - Summary (part 1)

<details><summary>Source lessons</summary>

- Introduction
- Plan, design, and implement Microsoft Entra Connect
- Implement manage password hash synchronization (PHS)
- Implement manage pass-through authentication (PTA)
- Explore pass-through authentication and seamless single sign-on (SSO)
- Implement and manage federation
- Trouble-shoot synchronization errors
- Implement Microsoft Entra Connect Health

</details>

```text
HYBRID IDENTITY WITH MICROSOFT ENTRA CONNECT (SC-300)
│
├── 🌉 ENTRA CONNECT — CORE COMPONENTS
│   ├── Role ───────────> Bridges on-prem AD + Microsoft Entra ID (single tool, unified ID)
│   ├── Capabilities ───> Sync objects + PHS + PTA + Federation + Health monitoring
│   ├── Connector Space ─> Staging area, one per connected directory (each forest / Entra)
│   ├── Metaverse (MV) ──> Global staging area ⭐ only ONE MV exists
│   ├── Sync rules ─────> Decide projection/join + attribute flow between staging areas
│   └── Operations ─────> Import / Synchronization / Export (Export = main error source)
│
├── 🔑 AUTHENTICATION METHODS (core exam comparison)
│   ├── PHS ────────────> Hash of the hash synced to Entra ID; least effort/infrastructure
│   │   ├── Runs every 2 minutes (fixed, cannot modify); overwrites the cloud password
│   │   ├── Express settings ──> PHS auto-enabled; initial sync = ALL in-scope users
│   │   ├── Selective PHS ──> available only AFTER the initial full synchronization
│   │   ├── ⚠️ Does NOT instantly enforce on-prem account state (disabled user keeps access)
│   │   ├── FIPS mode ──> MD5 blocked; fix in miiserver.exe.config (enforceFIPSPolicy=false)
│   │   └── ⭐ Enable PHS with ANY primary method (DR, outage survival, leaked-creds report)
│   ├── PTA ────────────> Agents validate passwords directly against on-prem AD
│   │   ├── Enforces account state, password policy + sign-in hours AT sign-in time
│   │   ├── ⭐ Deploy 3 agents (1 on Connect server + 2 extra); NEVER in perimeter network
│   │   ├── PHS backup ──> Failover is MANUAL via Entra Connect (never automatic)
│   │   └── Tenant-level feature ──> affects ALL managed domains
│   │       └── ⚠️ Switching AD FS → PTA: wait 12 hours before shutting down AD FS
│   └── Federation ─────> Entra ID hands off auth to a trusted external system
│       ├── Needed for ─> Smartcards/certs, 3rd-party MFA, sAMAccountName (DOMAIN\user) login
│       └── Topology ───> Load-balanced farm: internal AD FS + perimeter WAP servers
│
├── 🧬 DESIGN CONCEPTS & TOPOLOGIES
│   ├── sourceAnchor (immutableId) ──> Immutable for object lifetime; < 60 characters
│   │   ├── Special chars count as 3; must be unique, not case-sensitive, not name-based
│   │   └── ⭐ objectGuid = recommended (single forest, express settings, legacy DirSync)
│   ├── UPN sign-in ──> Suffix must be a VERIFIED custom domain (else *.onmicrosoft.com used)
│   ├── Non-routable domain (contoso.local) ──> Connect warns; use CUSTOM installation
│   ├── Express settings ──> supports ONLY single forest + single tenant topology
│   ├── Multi-forest ──> ONE sync server must reach ALL forests (may sit in perimeter/DMZ)
│   ├── GALSync ──> FIM 2010 / MIM 2016 ⚠️ Entra Connect CANNOT do on-prem GALSync
│   ├── Staging server ──> Reads everything, writes NOTHING (standby / disaster recovery)
│   └── ⚠️ Sync server : tenant = strict 1:1 (one Connect install per Entra tenant)
│
├── ☁️ ENTRA CONNECT CLOUD SYNC (alternate engine)
│   ├── Cloud provisioning agent ──> Light-weight bridge AD → Entra; config managed in cloud
│   ├── Use cases ──> Disconnected multi-forest (mergers/acquisitions); coexists w/ Connect
│   ├── HA ──> Multiple provisioning agents supported (critical for PHS-dependent orgs)
│   └── Limits ──> Groups up to 50,000 members (use OU scoping filter); sync every 2 minutes
│
├── 🚀 SEAMLESS SSO
│   ├── Auto sign-in ──> Domain-joined corporate devices on the corporate network
│   ├── Combines with ──> PHS or PTA ⚠️ NOT applicable to AD FS federation
│   ├── Mechanism ──> Kerberos ticket for AZUREADSSOACC computer account; Entra decrypts it
│   ├── Native clients ──> WS-Trust MEX endpoint (exclusive to Seamless SSO) → SAML → tokens
│   └── Deployment ──> Group Policy (partial or full rollout); no extra on-prem components
│
├── 🛠️ FEDERATION ADMIN (AD FS + PingFederate via Entra Connect)
│   ├── AD FS farm ──> Windows Server 2012 R2+; ALL authentication stays on-premises
│   ├── PingFederate ──> Version 8.4+; TLS/SSL cert for the federation service name
│   ├── Wizard flow ──> AD FS servers (domain-joined) → WAP (perimeter, extranet-facing)
│   │   └── AD FS service account ──> Group Managed Service Account OR domain user account
│   ├── Tasks ──> Repair trust, add federated domain (single domain per initial run),
│   │   add AD FS server, add WAP server
│   ├── AlternateID ──> Sign in with mail attribute when UPN is non-routable or unusable
│   └── Device writeback ──> CN=RegisteredDevices; enables device-based CA for AD FS apps
│
├── 🚨 SYNC ERROR TROUBLESHOOTING (Export errors → connector "contoso.onmicrosoft.com")
│   ├── Hard match ──> sourceAnchor vs immutableId; Soft match ──> ProxyAddresses / UPN
│   ├── InvalidSoftMatch ──> Soft-match hit an object that ALREADY holds an immutableId
│   ├── AttributeValueMustBeUnique ──> Duplicate ProxyAddresses / UserPrincipalName
│   ├── ObjectTypeMismatch ──> Different object types (User/Group) share a proxyAddress
│   ├── FederatedDomainChangeError ──> UPN suffix changed between two FEDERATED domains
│   ├── IdentityDataValidationFailed ──> Invalid characters / format in userPrincipalName
│   ├── LargeObject ──> Limits: userCertificate & userSMIMECertificate max 15 certs,
│   │   oversized thumbnailPhoto, too many proxyAddresses
│   ├── Admin role conflict ──> No soft-match for users WITH admin roles → remove roles,
│   │   hard-delete quarantined object, resync, restore roles
│   └── Reports ──> Entra Connect Health for sync (Azure portal), refreshed every 30 min
│
└── 🩺 MICROSOFT ENTRA CONNECT HEALTH
    ├── License ──> ⭐ Requires Entra ID Premium P1 (no portal configuration without it)
    ├── Access ──> Global Administrator; agent installed on EACH targeted server


---

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


---

# Module 05: What is Microsoft Entra multifactor authentication - Summary

<details><summary>Source lessons</summary>

- Introduction
- What is Microsoft Entra multifactor authentication
- Plan your multifactor authentication deployment
- Exercise - Enable Microsoft Entra multifactor authentication
- Configure multifactor authentication methods
- Summary

</details>

```text
MICROSOFT ENTRA MULTIFACTOR AUTHENTICATION (SC-300)
│
├── 🧠 MFA CORE CONCEPTS
│   ├── Definition ────────────> Full authentication requires 2+ verification elements
│   ├── Factor categories ─────> Something you KNOW / POSSESS (app, token) / ARE (biometric


---

# Module 06: Administer FIDO2 and passwordless authentication methods - Summary

<details><summary>Source lessons</summary>

- Introduction
- Administer FIDO2 and passwordless authentication methods
- Explore Authenticator app and OATH tokens
- Implement an authentication solution based on Windows Hello for Business
- Exercise configure and deploy self-service password reset
- Deploy and manage password protection
- Configure smart lockout thresholds
- Exercise - Manage Microsoft Entra smart lockout values
- Implement Kerberos and certificate-based authentication in Microsoft Entra ID
- Configure Microsoft Entra user authentication for virtual machines
- Summary and resources

</details>

```text
USER AUTHENTICATION IN MICROSOFT ENTRA ID (SC-300)
│
├── 🔐 METHOD STRENGTH & FACTOR TYPE (exam table)
│   ├── HIGH security ──> Windows Hello for Business / Authenticator app / FIDO2 key
│   ├── MEDIUM ──> OATH hardware (preview) & software tokens / SMS / Voice
│   ├── LOW ──> Password ⭐ can NEVER be disabled in Entra ID — only supplement with MFA
│   ├── Primary factor ──> WHfB, FIDO2, password; Authenticator app & SMS (preview)
│   ├── Secondary only ──> OATH tokens + Voice call (usable for MFA and SSPR)


---

# Module 07: Plan security defaults - Summary

<details><summary>Source lessons</summary>

- Introduction
- Plan security defaults
- Exercise - Work with security defaults
- Plan Conditional Access policies
- Implement Conditional Access policy controls and assignments
- Exercise - Implement Conditional Access policies roles and assignments
- Test and troubleshoot Conditional Access policies
- Implement application controls
- Implement session management and continuous access evaluation
- Exercise - Configure authentication session controls
- Microsoft Entra Conditional Access Optimization agent
- Summary and resources

</details>

```text
CONDITIONAL ACCESS & SECURITY DEFAULTS (SC-300)
│
├── 🏢 SECURITY DEFAULTS (works on Free tier, no extra cost)
│   ├── Auto-enabled ──────> tenants created on/after Oct 22, 2019 (all new tenants)
│   ├── Portal path ───────> Entra admin center > Overview > Properties > Manage security defaults
│   ├── Required role ─────> Conditional Access Administrator or Security Administrator
│   ├── Enforces ──────────> MFA registration for ALL users (no grace period, number
│   │                         matching), MFA every sign-in for 16 admin roles,
│   │                         protect privileged activity (Azure portal)
│   └── ⚠️ Legacy auth ────> blocked (IMAP/SMTP/POP3, Exchange ActiveSync basic) —
│                             legacy protocols bypass MFA entirely
│
├── 🧩 CA POLICY ANATOMY (if-then: assignments -> controls)
│   ├── Assignments ───────> Users/groups • Cloud apps or actions • Conditions
│   ├── Access controls ───> Grant (MFA, compliant device, hybrid joined) or Block access
│   ├── Session controls ──> App enforced permissions, CA App Control, sign-in frequency
│   ├── Licenses ──────────> Free / Office 365 = NO CA • P1 (M365 E3+) = standard CA
│   │                         • P2 = + risk-based policies (Identity Protection)
│   ├── ⚠️ Token default ──> no assignment matched + no policy = access token IS issued
│   └── ⭐ Block access ───> overrides ALL other assignments; exclude break-glass
│                             accounts or you lock out the whole tenant
│
├── 📍 CONDITIONS & RISK (risk-based policies require P2)
│   ├── Named locations ───> Protection > CA > Named locations (IP ranges / countries,
│   │                         optional "include unknown areas")
│   ├── ⭐ Best practice ──> block a named location of never-expected countries,
│   │                         but EXEMPT administrators from that policy
│   ├── Sign-in risk ──────> probability the request isn't by the identity owner (P2)
│   ├── User risk ─────────> leaked credentials -> force secure password change (P2)
│   ├── User actions ──────> "Register security info": block unless trusted location
│   │                         or compliant / hybrid Entra joined device state
│   └── ⭐ Always exclude ─> emergency/break-glass accounts + service accounts (MFA
│                             can't run programmatically); CA never blocks service
│                             principals — replace scripts with managed identities
│
├── 📱 DEVICE & APP CONTROLS (Entra ID P1/P2 + Intune)
│   ├── Compliant device ──> Intune reports compliance (PIN, encryption, OS version,
│   │                         no jailbreak) -> CA grants/blocks
│   │                         ⚠️ does NOT block Intune enrollment
│   ├── Approved client app ─> requires Entra registration first (iOS/Android),
│   │                         works for modern auth + Exchange ActiveSync clients
│   ├── CA App Control ────> Defender for Cloud Apps (reverse proxy): block download/
│   │                         cut/copy/print, label on download, block unlabeled
│   │                         uploads, monitor risky sessions in real time
│   └── App protection (MAM) ─> app-level rules (PIN, block data transfer), works
│                             WITHOUT MDM (MAM-WE for BYOD); target by managed state
│
├── ⏱️ SESSION MANAGEMENT & CAE
│   ├── Sign-in frequency ─> default = rolling 90-day window; OAuth2/OIDC apps + SAML
│   │                         (if not dropping own cookies); device unlock/sign-in
│   │                         satisfies the requirement
│   ├── Browser session ───> "Stay signed in?" prompt on personal/unmanaged devices
│   ├── ⭐ Don't over-prompt ─> users trained to type credentials blindly fall for
│                             phishing; policy violations still revoke sessions
│   └── CAE ───────────────> near real-time enforcement (disable, password change,
│                             location change) via 401 + claim challenge -> reauth
│
├── 🧪 TESTING & TROUBLESHOOTING
│   ├── Report-only ───────> Microsoft recommends ALL new policies start here
│   ├── What If tool ──────> simulate sign-in; session controls appear in results
│   ├── Sign-in logs ──────> Identity > Monitoring > Sign-ins > Conditional Access tab
│   ├── Roles ─────────────> Security Administrator or Global Reader
│   ├── Error page ────────> "More Details" -> correlation ID / request ID for support
│   └── ⚠️ Forbidden ──────> All users + all cloud apps: Block, require hybrid joined,
│                             or require app protection policy = org-wide lockout
│
├── 🤖 CA OPTIMIZATION AGENT
│   ├── Requirements ──────> Microsoft Entra ID P1 minimum + Security Compute Units (SCU)
│   ├── Activation role ───> Security Administrator or higher (first activation);
│   │                         CA Admins can be granted Security Copilot access
│   ├── Features ──────────> require MFA for uncovered users, device controls (needs
│   │                         Intune), block legacy auth, block device code flow,
│   │                         consolidate overlapping policies, one-click remediation
│   └── Entra Agent ID ────> AI agents = first-class principals; target in CA like
│                             workload identities, exclude trusted/emergency agents
│
└── 📜 TERMS OF USE (TOU)
    ├── Created in ────────> Identity Governance > Terms of use (PDF upload required;
    │                         set expiry, require open before accept)
    └── Enforced via ──────> Conditional Access grant control; consent can expire and
                              users must re-attest
```


---

# Module 08: Review identity protection basics - Summary

<details><summary>Source lessons</summary>

- Introduction
- Review identity protection basics
- Implement and manage user risk policy
- Exercise enable sign-in risk policy
- Exercise configure Microsoft Entra multifactor authentication registration polic
- Monitor, investigate, and remediate elevated risky users
- Implement security for workload identities
- Explore Microsoft Defender for Identity
- Explore the Identity Risk Management Agent
- Summary and resources

</details>

```text
IDENTITY PROTECTION & RISK MANAGEMENT (SC-300)
│
├── 🏢 FOUNDATIONS & LICENSING
│   ├── Identity Protection ──> 3 core tasks: automate detection/remediation, investigate in
│   │                            portal, export risk data to third-party tools / SIEM
│   ├── License ──────────────> ⚠️ Microsoft Entra ID Premium P2 required (risk policies, MFA
│   │                            registration policy, alerts, full reports — limited/none on Free/P1)
│   ├── Signal scale ─────────> Microsoft analyzes ~6.5 trillion signals/day (Entra ID + MS
│   │                            Accounts + Xbox)
│   └── Risk data feeds ──────> Conditional Access access decisions or SIEM investigation
│
├── 👥 ROLES & PERMISSIONS (exam-favorite table)
│   ├── Security Administrator ──> Full access; ⚠️ can't reset a user password
│   ├── Security Operator ───────> View reports + Overview, dismiss risk, confirm compromise/safe;
│   │                             ⚠️ can't open Risky sign-ins report, no policies/resets/alerts
│   ├── Security Reader ─────────> View-only (reports + Overview); can't give detection feedback
│   ├── Global Reader ───────────> Can access Identity Protection
│   └── Conditional Access Admin > Can create policies using sign-in risk as a condition
│
├── 🚨 RISK DETECTION TYPES
│   ├── Entra ID detections ──> Anonymous IP (Tor/VPN), atypical travel, malicious IP, unfamiliar
│   │                            sign-in properties, leaked credentials, password spray,
│   │                            suspicious browser, verified threat actor IP, anomalous token,
│   │                            token issuer anomaly (SAML), Entra threat intelligence
│   └── MDCA-sourced ─────────> New country, activity from anonymous IP, suspicious inbox forwarding
│
├── ⚙️ RISK POLICIES (all require P2)
│   ├── User risk policy ─────> Probability the ACCOUNT is compromised
│   │   ├── Threshold ────────> ⭐ Microsoft recommendation: HIGH
│   │   └── Control ──────────> ⭐ Recommended: Allow access + Require password change
│   ├── Sign-in risk policy ──> Probability the SIGN-IN was made by someone other than the user
│   │   ├── Threshold ────────> ⭐ Microsoft recommendation: MEDIUM and above
│   │   └── Control ──────────> Require multifactor authentication
│   ├── MFA registration policy > Control "Require Entra ID MFA registration" is locked (fixed)
│   ├── Exclusions ───────────> Emergency access / break-glass accounts; review regularly
│   ├── Trusted networks ─────> Configured trusted locations reduce false positives
│   ├── Self-remediation ─────> ⚠️ Users must be pre-registered for MFA + SSPR (combined
│   │                            registration experience recommended)
│   └── Portal path ──────────> Entra admin center > Identity > Protection > Identity Protection
│
├── 📊 INVESTIGATION REPORTS (3 reports)
│   ├── Risky users ──────────> Risk history + risky sign-in history; actions: reset password,
│   │                            confirm compromise, dismiss risk, block sign-in, open in MDI
│   ├── Risky sign-ins ───────> ⚠️ Retention: last 30 days only; confirm compromised / confirm safe
│   ├── Risk detections ──────> ⚠️ Retention: last 90 days; clickable link to MDCA portal
│   └── Downloads ────────────> CSV or JSON; ⚠️ users & sign-ins = 2,500 rows max,
│                                detections = 5,000 records max
│
├── 🔧 REMEDIATION & UNBLOCKING
│   ├── User risk level ──────> low/medium/high built from ACTIVE detections; "Closed (system)"
│   ├── Options ──────────────> Self-remediation via policy, manual password reset, dismiss risk,
│   │                            close individual detections manually
│   ├── Password reset ───────> Temporary password (must contact user) OR user self-reset
│   │                            (self-reset only available if registered for MFA + SSPR)
│   ├── Dismiss risk ─────────> Closes events but ⚠️ does NOT restore safe state (password intact)
│   ├── Auto-dismiss ─────────> "AI confirmed sign-in safe" when risk resolved (MFA / password change)
│   ├── Unblock (user risk) ──> Reset password / dismiss risk or close detections / exclude user /
│   │                            disable policy
│   └── Unblock (sign-in risk)> Sign in from a familiar location or device / exclude / disable
│
├── 🤖 GRAPH API AUTOMATION
│   ├── APIs ─────────────────> riskDetection, riskyUsers, signIn
│   ├── App registration ─────> Application permissions: IdentityRiskEvent.Read.All +
│   │                            IdentityRiskyUser.Read.All + admin consent + client secret
│   ├── Endpoint ─────────────> GET .../v1.0/identityProtection/riskDetections
│   │                            ?$filter=detectionTimingType eq 'offline'
│   ├── MFA-passed users ─────> riskyUsers?$filter=riskDetail eq
│   │                            'userPassedMFADrivenByRiskBasedPolicy'
│   └── PowerShell ───────────> Microsoft Graph PowerShell SDK Preview module (Azure GitHub samples)
│
├── 🖥️ WORKLOAD IDENTITY PROTECTION (service principals, apps, managed identities)
│   ├── Why riskier ──────────> Can't do MFA, no formal lifecycle, credentials must be stored
│   ├── Requirements ─────────> Entra ID P2 + Security Administrator / Operator / Reader role
│   ├── Detections (offline) ─> Threat intelligence, suspicious sign-ins (learns baseline over
│   │                            2–60 days), suspicious OAuth app credential addition (MDCA),
│   │                            leaked credentials, admin confirmed compromised
│   │                            (riskyServicePrincipals API)
│   └── Conditional Access ───> ⚠️ Only single-tenant service principals registered in YOUR tenant;
│                                third-party SaaS, multi-tenant apps & managed identities excluded
│
└── 🛡️ DEFENDER FOR IDENTITY & RISK MANAGEMENT AGENT
    ├── MDI (ex-Azure ATP) ───> Cloud-based; uses on-prem AD signals for hybrid threat detection
    ├── MDI components ───────> Microsoft Defender portal (security.microsoft.com) + Sensor
    │                            (installed on DCs & AD FS, no port mirroring) + Cloud service
    │                            (Azure: US / Europe / Asia)
    ├── Agent prereqs ────────> ⚠️ Entra ID P2 + available SCUs + Security Administrator to
    │                            activate/act; Security Reader & Global Reader = view only
    ├── Agent triggers ───────> Continuous (every 5 min) / daily / manual run; checking for new
    │                            risky users consumes no SCUs (SCUs used when generating suggestions)
    ├── Agent scope ──────────> Default: 100 most recent risky users, last 90 days (settable 1–100;
    │                            time frames 7 / 14 / 30 / custom up to 90 days)
    └── Agent actions ────────> Dismiss risk or Reset password; LLM chat, agent memory for
                                 custom instructions, verdict Compromised / Not compromised
```


---

# Module 09: Assign Azure roles - Summary

<details><summary>Source lessons</summary>

- Introduction
- Assign Azure roles
- Configure custom Azure roles
- Create and configure managed identities
- Access Azure resources with managed identities
- Analyze Azure role permissions
- Configure Azure Key Vault RBAC policies
- Retrieve objects from Azure Key Vault
- Summary and resources

</details>

```text
AZURE RBAC, MANAGED IDENTITIES & KEY VAULT (SC-300)
│
├── 🔑 AZURE ROLES (Azure RBAC ──> authorization system for Azure resources)
│   ├── Who gets access? ──> User / Group / Service Principal / Managed Identity
│   ├── Built-in roles ─────> Owner (full + grant access) / Contributor (no grant access)
│   │                        Reader (view only) / User Access Administrator (assign access)
│   ├── Scope levels ───────> Management group > Subscription > Resource group > Resource
│   │                        ⭐ Lower levels INHERIT permissions from higher levels
│   ├── Limits ─────────────> 4,000 role assignments per subscription
│   │                        500 role assignments per management group
│   ├── Portal path ────────> Access control (IAM) blade (user, group, RG, or subscription)
│   ├── PowerShell ─────────> New-AzRoleAssignment -ObjectId -RoleDefinitionName -Scope
│   └── CLI ────────────────> az role assignment create --assignee --role --resource-group
│
├── 🛠️ CUSTOM ROLES (when built-ins don't fit ──> least privilege)
│   ├── Assignable scopes ──> Management group (preview only) / Subscription / Resource group
│   ├── Limits ─────────────> Up to 5,000 custom roles per directory
│   │                        ⭐ Shared across subscriptions, stored in Microsoft Entra ID
│   ├── Portal path ────────> Entra admin center > Identity > Roles and administration
│   │                        > + New custom role
│   └── JSON template ──────> roleName / description / assignableScopes / permissions
│                            (actions + notActions + dataActions + notDataActions,
│                             * = wildcard at any level, e.g. Microsoft.Billing/*/read)
│
├── 🤖 MANAGED IDENTITIES (eliminate credential management ──> Entra tokens, no secrets)
│   ├── System-assigned ────> Tied to the service instance lifecycle
│   │                        ⚠️ Deleted automatically when the resource is deleted
│   │                        Only that resource can use it to request tokens
│   ├── User-assigned ──────> Standalone Azure resource, managed separately
│   │                        ⭐ Can be assigned to ONE OR MORE service instances
│   ├── Cost ───────────────> No extra cost; works with any Entra-auth resource
│   ├── Portal path ────────> App Service > Identity > System assigned / User assigned
│   ├── CLI ────────────────> az webapp identity assign --identities <identity-name>
│   ├── PowerShell ─────────> Update-AzFunctionApp -IdentityType UserAssigned
│   │                        (AZ.ManagedServiceIdentity module)
│   └── Grant access ───────> Target resource > Access control (IAM) > Add role assignment
│                            (pick Owner / Contributor / Reader per least privilege)
│
├── 👤 USER PERMISSIONS (default rights ──> member vs guest)
│   ├── Member users ───────> Enumerate users / invite guests / create Security & M365 groups
│   │                        / register apps / read app properties
│   ├── Guest users ────────> ⚠️ Slightly FEWER default permissions than members
│   ├── Restrict ───────────> Entra ID > User settings (block app registration, portal
│   │                        access, LinkedIn, external collaboration settings)
│   ├── Add permissions ────> Roles and administrators (assign roles to users/groups)
│   └── Audit a role ───────> Roles and administrators > select role > (...) > description
│                            page (Attribute definition reader)
│
├── 🗝️ AZURE KEY VAULT ACCESS (two authorization models ──> pick per security posture)
│   ├── Access policies ────> More granular but harder to manage
│   │                        ⚠️ Limit: 1,024 access policy entries per vault
│   │                        ⭐ Assign policies to GROUPS, not individual users
│   ├── Azure RBAC ─────────> One place to manage permissions across ALL vaults
│   │                        Scopes: management group / subscription / RG / resource
│   │                        Requires: enable RBAC on vault + assign via Identity & Access (IAM)
│   ├── RBAC built-ins ─────> Key Vault Administrator (all data-plane ops, no RBAC mgmt)
│   │                        Certificates Officer / Crypto Officer / Secrets Officer
│   │                        Crypto User (crypto ops) / Crypto Service Encryption User
│   │                        Reader (metadata only) / Secrets User (read contents)
│   └── Best practice ──────> ⭐ One vault per application per environment (Dev / Pre-Prod / Prod)
│
├── 📥 KEY VAULT SECRETS (add & retrieve)
│   ├── Add secret ─────────> Key Vault > Secrets > Generate/Import (Manual / Name / Value)
│   ├── Portal retrieve ────> Open secret > "Show secret value" button
│   ├── CLI ────────────────> az keyvault secret show --name --vault-name --query "value"
│   ├── PowerShell ─────────> Get-AzKeyVaultSecret -VaultName -Name -AsPlainText
│   └── Applications ───────> SDKs available for .NET / Node.js / Python
│
└── 🛡️ GOLDEN RULES (exam mindset)
    ├── Least privilege ────> Narrowest role at narrowest scope (limits blast radius)
    ├── Managed identities ─> Assign to the APP, configure inside the hosting service
    └── Permissions flow ───> Assigned at user/group level, always reach the user in the end
```


---

# Module 10: Explore Global Secure Access - Summary

<details><summary>Source lessons</summary>

- Introduction
- Explore Global Secure Access
- Deploy and configure Microsoft Entra Internet Access
- Deploy and configure Microsoft Entra Private Access
- Explore how to use the Dashboard to drive Global Secure Access
- Create remote networks for use with Global Secure Access
- Use Conditional Access with Global Secure Access
- Explore logs and monitoring options with Global Secure Access
- Summary and resources

</details>

```text
GLOBAL SECURE ACCESS — SSE (SC-300)
│
├── 🌐 FOUNDATION ──> Security Service Edge = Entra Internet Access + Entra Private Access
│   ├── Zero Trust basis ──> least privilege, explicit verification, assume breach
│   ├── Delivery ──> Microsoft WAN (global regions + edge locations) | portal: entra.microsoft.com
│   ├── Licensing ⭐ ──> Entra ID P1 or P2 + Internet Access and/or Private Access license
│   └── Plan via ──> Zero Trust Guidance Center (recommended before rollout)
│
├── 🛡️ INTERNET ACCESS ──> Secure Web Gateway (SWG) for M365, SaaS & public internet
│   ├── Key features ──> token-replay prevention, universal tenant restrictions, enriched logs
│   ├── Web filtering ──> allow/block sites by content category + domain name
│   ├── Tenant restrictions v2 ──> auth plane: Entra ID blocks unsanctioned external accounts
│   │   └── Data plane ──> Graph + SPO anonymous blocked; ⚠️ 3rd-party apps (Slack) NOT in scope
│   ├── Setup path ──> Identity > External Identities > Cross-tenant access > Organizational
│   │   settings ──> Add organization (domain or tenant ID) ──> edit inherited settings
│   └── ⚠️ Enable tagging to enforce ──> needs BOTH Global Secure Access AND Security Admin
│
├── 🔒 PRIVATE ACCESS ──> VPN-less access to private resources (any IP, FQDN, port, protocol)
│   ├── Connector ⭐ ──> Windows Server 2016+ | .NET 4.7.2+ | TLS 1.2 | OUTBOUND 80 + 443 only
│   ├── URLs for connector ──> *.msappproxy.net + *.servicebus.windows.net (443/HTTPS)
│   ├── High availability ──> install on >1 server; verify services Running (connector + Updater)
│   ├── Quick Access app ──> segments: IPv4 / FQDN+wildcards / CIDR / IP-to-IP range + ports
│   │   ├── ⚠️ NetBIOS NOT supported ──> use contoso.local/app1, never contoso/app1
│   │   └── Ports ──> 22 SSH | 80 HTTP | 443 HTTPS | 445 SMB | 3389 RDP (comma / hyphen ranges)
│   └── Deploy (4 steps) ──> connector+group > Quick Access > Private profile > GSA client
│
├── 📲 CLIENT & TRAFFIC FORWARDING
│   ├── Client download ──> Global Secure Access > Connect > Client download (GSA Admin)
│   ├── Platforms ──> Windows (manual or Intune) | Android (Intune or Defender for Endpoint)
│   ├── Profiles path ──> Global Secure Access > Connect > Traffic forwarding (toggle profile)
│   ├── Microsoft profile creates ──> routing policies (Exchange, SPO+OneDrive, Entra+MSGraph)
│   └── ⚠️ CA on Microsoft traffic ──> enforced ONLY when GSA client installed on device
│
├── 🌍 REMOTE NETWORKS ──> branch offices via IPsec tunnel from CPE router to nearest edge
│   ├── Steps 1-4 (portal or Graph) ──> Basics (name+region) > Connectivity (device, IP,
│   │   redundancy, bandwidth) > Traffic forwarding > Review configuration
│   └── ⚠️ Step 5 = on-prem router ──> enter MS settings in CPE console or NO tunnel (IKE
│       is bidirectional — do not skip)
│
├── 🛂 CONDITIONAL ACCESS + GSA ──> first enable signaling (one time, GSA Admin)
│   ├── Signaling path ──> GSA > Global settings > Session management > Adaptive access
│   │   └── Auto-enables ──> CAE signaling for Office 365 (preview)
│   ├── Compliant network check ──> auto Named location "All Compliant Network locations"
│   │   ├── Enforcement ──> auth plane (Entra ID) + data plane via CAE (preview)
│   │   └── ⚠️ CAE data plane ──> only Exchange Online + SharePoint Online today
│   ├── Policy recipe ──> Include All users > Target specific apps > Location Any >
│   │   Exclude compliant network location > Grant: Block access
│   ├── Source IP restoration ──> keeps original user IP; only Microsoft traffic (EXO, SPO,
│   │   Teams, Graph); enabled automatically with signaling
│   │   └── ⚠️ With CAE strict location ──> users blocked even in trusted IP range
│   ├── CA on Private/Quick Access apps ──> GSA > Applications > Enterprise applications >
│   │   app > Conditional Access (app auto-set as target resource)
│   ├── ⚠️ App picker trap ──> generic "Office 365" cloud app NOT supported — pick specific apps
│   └── ⭐ Always exclude ──> break-glass accounts + service accounts (Entra Connect sync)
│
├── 📊 DASHBOARD & MONITORING ──> GSA > Dashboard (Global Secure Access Administrator)
│   ├── Snapshot widget ──> Users (UPN) / Devices (device ID) / Workloads (FQDN+IP) in 24h
│   ├── Alerts ──> unhealthy remote network, more external tenants, token-device mismatch,
│   │   web content blocked
│   ├── Other widgets ──> usage profiling (transactions/users/devices/bytes), top destinations,
│   │   cross-tenant access, web category filtering, device status (active 24h / inactive 7d)
│   └── Cross-tenant widget ──> sign-ins, distinct + unseen tenants (24h vs prior 7 days)
│
├── 📜 LOGS ──> GSA > Audit logs (pre-filtered) | Identity > Monitoring & health > Audit logs
│   ├── Traffic logs hierarchy ⭐ ──> Session > Connection (5-tuple: IPs, ports, FQDN) >
│   │   Transaction (unique request + response) | view: GSA > Monitor > Traffic logs
│   ├── Enriched O365 logs ──> join OfficeActivity + NetworkAccessTraffic via Unique Token ID
│   │   (Sentinel recommended workbook) | ⚠️ only SharePoint Online available for enrichment
│   ├── Diagnostic settings (Security Admin) ──> NetworkAccessTrafficLogs to Log Analytics /
│   │   storage account / event hub / partner (SIEM)
│   └── Retention ⚠️ ──> traffic + remote network health: 30 days | Office logs: 24 hours |
│       audit logs: varies by Microsoft Entra ID license
│
└── 🔑 ROLES CHEAT SHEET
    ├── Global Secure Access Administrator ──> profiles, client, remote nets, signaling, dashboard
    ├── Security Administrator ──> tenant restrictions setup + diagnostic settings
    ├── Conditional Access Administrator ──> compliant network + private app CA policies
    └── Global Administrator ──> install/register private network connector (App Proxy tenant)
```


---

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


---

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


---

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


---

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


---

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


---

# Module 16: Plan for access reviews - Summary

<details><summary>Source lessons</summary>

- Introduction
- Plan for access reviews
- Create access reviews for groups and apps
- Create and configure access reviews programmatically
- Monitor access review findings
- Automate access review management tasks
- Configure recurring access reviews
- Explore the Access Review Agent in Microsoft Entra
- Summary and resources

</details>

```text
ACCESS REVIEWS · MICROSOFT ENTRA ID GOVERNANCE (SC-300)
│
├── 🎯 FOUNDATIONS & LICENSING
│   ├── Purpose ────────────> Periodic recertification of group, app & role access;
│   │                        denied users removed automatically, no manual chase
│   ├── Full licensing ─────> Microsoft Entra ID Governance or Microsoft Entra Suite
│   │                        ⚠️ Entra ID P2 alone unlocks only SOME capabilities
│   ├── P2 per user ────────> Required for each reviewer, self-reviewer, group or
│   │                        app owner performing a review
│   │                        ⭐ Global Admin / User Admin (setup or apply) need NO license
│   └── Governance goals ───> Right people, right access, right resources; auditable
│
├── 🏢 PLANNING & ADMIN ROLES
│   ├── Stakeholders ───────> IT, Security, Development, Business units, Governance
│   ├── Pilot ──────────────> Small scope, noncritical resources, auto-apply OFF,
│   │                        document removals, valid emails, watch audit logs
│   ├── Reviewable targets ─> Groups, SSO apps, Access Packages, Entra + Azure roles
│   │                        (PIM), custom data resources (preview)
│   ├── Create grp/app ─────> Global Admin, User Admin, Identity Governance Admin,
│   │                        Privileged Role Admin (Entra assignable groups only)
│   ├── Create Entra roles ─> Global Admin, Privileged Role Administrator
│   ├── Create Azure roles ─> Global Admin, User Access Administrator, Resource Owner
│   ├── Read results ───────> Global Reader, User Admin, IG Admin, Security Reader,
│   │                        Privileged Role Admin, group owner
│   ├── Reviewer choice ────> ⚠️ Fixed at creation — cannot change once review starts
│   ├── Personas ───────────> Resource owners, selected delegates, self-attest users
│   ├── Group owners ───────> ⭐ Best reviewers; Teams creator auto-becomes owner;
│   │                        synced groups can't have Entra owners → pick people
│   ├── On-prem synced ─────> ⚠️ Review possible but membership NOT changed in cloud
│   │                        (source of authority on-prem; results via CSV/Graph)
│   ├── CA exclusions ──────> Review membership of CA exception groups (e.g. sales)
│   ├── External users ─────> Dynamic groups by attributes; internal sponsor reviews
│   └── Communications ─────> Custom reviewer email; reminders mid-way or 1 day prior
│
├── ⚙️ CREATE A REVIEW (PORTAL)
│   ├── Portal path ────────> Entra admin center > ID Governance > Access reviews >
│   │                        New access review
│   ├── Required role ──────> Identity Governance Administrator or Global Administrator
│   ├── What to review ─────> Teams + Groups (finite set OR all M365 groups w/ guests)
│   │                        OR Applications ⚠️ apps have no owner-reviewer option
│   ├── Scope ──────────────> Guest users only OR Everyone
│   │                        ⚠️ "All M365 groups w/ guests" forces Guest users only
│   ├── Inactive users ─────> Group reviews can target inactivity up to 730 days
│   ├── Reviewer types ─────> Group owner(s), selected users/groups, self-review,
│   │                        managers of users
│   └── Fallback reviewer ──> Covers users with no manager / groups with no owner
│
├── 🤖 AUTOMATION & COMPLETION SETTINGS
│   ├── Recurrence ─────────> Weekly / Monthly / Quarterly / Semi-annually / Annually
│   │                        ⚠️ Monthly review: max duration 27 days (no overlap)
│   ├── Auto-apply results ─> Enable = denied users removed automatically at the end
│   ├── If no response ─────> No change / Remove access / Approve access /
│   │                        Take recommendations (applied to non-responders)
│   ├── Denied guests ──────> Remove from resource only OR block sign-in 30 days
│   │                        then delete (admin can re-enable within the 30 days)
│   ├── Guest action rule ──> ⚠️ Not configurable on Everyone-scope reviews (default:
│   │                        remove membership); also fixed for all-M365-guest reviews
│   ├── Guest reviews ──────> Per-partner reviews for compliance; guests get access
│   │                        via groups, Teams, apps/packages, or privileged roles
│   ├── Decision helpers ───> Show recommendations: last sign-in + peer outlier
│   └── Advanced ───────────> Justification required, email notifications, reminders,
│                            additional content for reviewer email
│
├── 📊 PERFORM & MONITOR (REVIEWER SIDE)
│   ├── Portal ─────────────> https://myaccess.microsoft.com > Access reviews
│   ├── Decisions ──────────> Approve / Deny / "Don't know" (keeps access, audited)
│   ├── Start email ────────> Sent by default at review start; unaccepted guest
│   │                        invites receive NO email
│   ├── Multi-reviewer ─────> ⚠️ The LAST submitted decision is the one recorded
│   ├── Recommendations ────> Deny if no sign-in for 30 days OR peer outlier in the
│   │                        reporting structure; accept one-by-one or in bulk
│   ├── Removal timing ─────> Denied users removed at period end or when admin stops
│   │                        the review (if Auto-apply is enabled)
│   ├── Status flow ────────> NotStarted > Initializing > Starting > InProgress >
│   │                        Completing > Auto-Reviewing > Auto-Reviewed > Applying >
│   │                        Applied
│   ├── Failed status ──────> ⚠️ Tenant deletion, license change, internal changes
│   └── Audit logs ─────────> Entra audit logs, category "Policy" (create/update/end/
│                            delete review, approve/deny/reset/apply decision);
│                            export to Log Analytics or Event Hubs for dashboards
│
├── 💻 PROGRAMMATIC (MICROSOFT GRAPH)
│   ├── Create permission ──> AccessReview.ReadWrite.All (delegated or application)
│   ├── Delegated caller ───> Must hold the Identity Governance Administrator role
│   ├── Read permission ────> AccessReview.Read.All (application/service principal)
│   ├── PowerShell cmdlet ──> New-MgIdentityGovernanceAccessReviewDefinition
│   └── Tip ────────────────> Prototype queries in Graph Explorer before scripting
│
├── 🔄 RECURRING REVIEWS (LIFECYCLE)
│   ├── Series ends ────────> Never, a specific end date, or after N occurrences
│   ├── Update scope ───────> ⭐ Apply edits to "Current" instance OR whole "Series"
│   └── Use case ───────────> Reviewer left the org → replace them across the Series
│
└── 🧠 ACCESS REVIEW AGENT (PREVIEW)
    ├── What it does ────────> Natural-language reviews inside Microsoft Teams with
    │                        insights + approve/deny recommendation & justification
    ├── Licensing ───────────> ID Governance / Suite + Security Copilot (min 1 SCU)
    ├── Admin roles ─────────> ALL of: Identity Governance Administrator + Lifecycle
    │                        Workflows Administrator + Security Copilot Contributor
    ├── Reviewer needs ──────> Teams + an assigned active review + Security Copilot
    │                        Contributor role
    ├── Scoring signals ─────> Inactivity, user-to-group affiliation, accountEnabled,
    │                        employeeLeaveDateTime, mover workflow in past 30 days,
    │                        prior review decisions, access-package request history
    ├── Enable path ─────────> Entra admin center > Agents > Access Review Agent >
    │                        Start agent; per review: Settings > Advanced > tick
    │                        "Access Review Agent (Preview)"
    └── Limitation ──────────> ⚠️ Once started, the agent CANNOT be stopped or paused
```


---

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


---

# Module 18: Analyze and investigate sign-in logs to troubleshoot access issues - Summary

<details><summary>Source lessons</summary>

- Introduction
- Analyze and investigate sign-in logs to troubleshoot access issues
- Review and monitor Microsoft Entra audit logs
- Exercise connect data from Microsoft Entra ID to Microsoft Sentinel
- Export logs to third-party security information and event management system
- Analyze Microsoft Entra workbooks and reporting
- Monitor security posture with Identity Secure Score
- Summary and resources

</details>

```text
MONITORING & SECURITY REPORTING IN MICROSOFT ENTRA ID (SC-300)
│
├── 📚 REPORTING ARCHITECTURE (log components)
│   ├── Activity ──────> Sign-ins | Audit logs | Provisioning logs (Workday / ServiceNow)
│   ├── Security ──────> Risky sign-ins + Users flagged for risk (Identity Protection)
│   └── Reader roles ──> Security Admin | Security Reader | Global Reader | Report Reader
│
├── 🔐 SIGN-IN LOGS ──> Entra ID > Monitoring > Sign-ins
│   ├── License ───────> ALL editions (also accessible via Microsoft Graph API)
│   ├── ⚠️ Latency ─────> Up to 2 HOURS before sign-in records appear in the portal
│   ├── ⚠️ Scope ───────> INTERACTIVE sign-ins only (non-interactive are NOT displayed)
│   ├── Default view ──> Date | User | App | Status | Risk detection | MFA requirement
│   ├── Filters ───────> User/UPN, App, Status (Success/Failure/Interrupted), IP, Location,
│   │                    Client app, OS, Correlation ID, Conditional Access status
│   ├── CA troubleshooting ─> "Conditional Access" tab = policy list + result per sign-in
│   ├── Columns rule ──> No multi-value fields as column (auth details, CA, network location)
│   ├── Download ──────> CSV or JSON, max 250,000 records (limited by retention policies)
│   └── Shortcuts ─────> Identity Protection (30-day graph) | Enterprise apps (top 3 apps)
│
├── 🧾 AUDIT LOGS ──> Entra ID > Monitoring > Audit logs
│   ├── Default view ──> Date/time | Service | Category+Activity (what) | Status | Target | Actor
│   ├── Filters ───────> Service, Category (UserManagement, GroupManagement...), Activity, Status
│   ├── Date range ────> 7 days | 24 hours | Custom (start + end time)
│   ├── ⚠️ Case-sensitive ─> Target and "Initiated by" name / UPN searches
│   ├── Graph API ──> graph.windows.net/<tenant>/activities/auditActivityTypesV2?api-version=beta
│   ├── Download ──────> Up to 250,000 records, CSV or JSON
│   └── Shortcuts ─────> Users (UserManagement preselected) | Groups (GroupManagement) | Ent. apps
│
├── 🛡️ MICROSOFT SENTINEL ──> Scalable cloud-native SIEM + SOAR
│   ├── ⚠️ Sign-in logs ─> Ingestion into Sentinel requires Microsoft Entra ID P1 or P2
│   ├── Other log types ─> ANY license is enough (Free / O365 / P1 / P2)
│   ├── Required roles ──> Sentinel Contributor (workspace) + Security Administrator (tenant)
│   │                     + read/write rights on Entra diagnostic settings
│   ├── Connect ─────────> Sentinel > Data connectors > Microsoft Entra ID > tick Sign-in +
│   │                     Audit logs checkboxes > Apply changes
│   └── Workspace ───────> New Log Analytics workspace, globally unique name, Pay-as-you-go
│
├── 🔌 EXPORT TO THIRD-PARTY SIEM ──> Azure Monitor + Event Hubs (⭐ recommended approach)
│   ├── Flow ───────────> Diagnostic settings route logs to Event Hubs; SIEM connectors consume
│   ├── Splunk ─────────> Azure Monitor Add-On for Splunk
│   ├── IBM QRadar ─────> Microsoft Azure DSM + Microsoft Azure Event Hubs Protocol
│   ├── ArcSight ───────> ArcSight Azure Event Hubs smart connector
│   └── AzLog (legacy) ─> Old Azure Log Integration tool; migrate to Azure Monitor integrations
│
├── 📈 USAGE & INSIGHTS REPORT (app-centric workbook view)
│   ├── ⚠️ License ─────> Microsoft Entra ID P1 or P2 REQUIRED
│   ├── Roles ──────────> Security Administrator | Security Reader | Report Reader
│   ├── Path ───────────> Entra ID > Enterprise applications > Activity > Usage and insights
│   └── Answers ────────> Most used apps? Most failed sign-ins? Top sign-in errors per app?
│
├── 🔢 IDENTITY SECURE SCORE
│   ├── License ────────> Available in ALL editions of Microsoft Entra ID
│   ├── Path ───────────> Azure portal > Entra ID > Security > Identity Secure Score
│   ├── Dashboard ──────> Score | comparison vs same-industry / similar-size tenants | trend
│   └── Scoring ────────> Binary (100%/0%) or partial % (MFA for 5/100 users = 0.53% of 10.71%)
│
└── 🎯 SCOPE & KEY RULES (Introduction + Summary)
    ├── Objectives ─────> Investigate sign-ins | Audit logs | Diagnostic logs to Log Analytics /
    │                     Sentinel | Export to SIEM | Workbooks | Secure score | Notifications
    ├── ⭐ Self sign-ins ─> Any NON-admin user can access their OWN sign-ins
    ├── ⚠️ M365 logs ────> Full Microsoft 365 activity view ONLY in M365 admin center
    │                     (also via Office 365 Management APIs)
    └── KQL note ────────> Review activity via Log Analytics / Sentinel WITHOUT KQL in this module
```
