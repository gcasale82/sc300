# SC-300 Decision Trees - Part 10

# SC-300 Decision Trees & Logic Rules — Part 10

---

### Conditional Access: Inclusion vs. Exclusion Precedence
1. **Exam Scenario:** Multiple CA policies target overlapping users/groups. Users are members of both *included* and *excluded* groups, or hold roles listed as exclusions. You must determine who is prompted for MFA or blocked.
2. **Key Decision Point:** The **exclusion override rule** — exclusions ALWAYS win over inclusions, regardless of which policy contains them.
3. **Decision Path:**
   - IF user is in the CA policy's **included** group AND NOT in any exclusion -> POLICY APPLIES (enforce MFA/block).
   - IF user is in the included group BUT is an **explicit exclusion** (user, group, or role such as Global Administrator) -> REJECT applying the policy (Reason: exclusions always override inclusions in Conditional Access).
   - IF user is NOT in any included group -> REJECT applying the policy (Reason: no inclusion match means no policy evaluation).
   - IF two policies overlap (e.g., CA1 and CA2) -> EVALUATE each independently; a user may be covered by one and exempted from the other.
4. **SC-300 Golden Rule:** *"Include to apply, exclude to escape — the exclusion ALWAYS wins."*

---

### Conditional Access: Blocking Legacy/Basic Authentication
1. **Exam Scenario:** Organization requires blocking legacy protocols (IMAP, POP3, Basic auth clients) connecting to Exchange Online, forcing Modern authentication.
2. **Key Decision Point:** The **Client apps condition** in Conditional Access.
3. **Decision Path:**
   - IF requirement is to block Basic auth/legacy clients -> SELECT a CA policy targeting **"Legacy authentication clients"** under Client apps + **Block** access control (Reason: this is the purpose-built mechanism).
   - IF option is a **Defender for Cloud Apps OAuth app policy** -> REJECT (Reason: governs third-party OAuth app permissions, not authentication protocols).
   - IF option is an **Intune app protection policy** -> REJECT (Reason: controls in-app data actions like copy/paste, not authentication protocols).
   - IF option is an **Intune compliance policy** -> REJECT (Reason: evaluates device health; it feeds CA but doesn't block auth methods directly).
4. **SC-300 Golden Rule:** *"Legacy auth dies in the Client apps condition — Block access, never with App Protection or Compliance policies."*

---

### Conditional Access for Workload Identities & On-Premises Service Authentication
1. **Exam Scenario:** Restrict an app registration (service principal) so it can only authenticate from the corporate network; an on-premises server runs non-interactive scheduled tasks authenticating to Microsoft Graph.
2. **Key Decision Point:** Identity type (workload vs. user) and infrastructure location (Azure vs. on-premises).
3. **Decision Path:**
   - IF restricting an **app registration/service principal** by network -> SELECT a CA policy targeting **workload identities** with a **location condition** (corporate IP ranges).
   - IF on-premises server (non-Azure) needs non-interactive auth -> SELECT a **certificate** (client credentials flow, secure local certificate store).
   - IF option is a **managed identity** for an on-premises server -> REJECT (Reason: managed identities are natively for Azure resources only).
   - IF comparing certificate vs. client secret -> SELECT certificate (Reason: avoids plain-text credential storage).
4. **SC-300 Golden Rule:** *"Azure resources get managed identities; on-premises gets certificates. Workload identities get their own CA policies."*

---

### Azure RBAC: Scope Inheritance & Data Plane vs. Control Plane
1. **Exam Scenario:** Users hold Reader/Contributor roles at different scopes (subscription, resource group, resource). Determine who can read data, create resources, or manage access.
2. **Key Decision Point:** **Role scope inheritance** + whether the role includes **data actions** or **role assignment write** permissions.
3. **Decision Path:**
   - IF user has **Reader** at subscription but only "Reader and Data Access" at a DIFFERENT resource group -> REJECT data access to the other RG (Reason: Reader alone excludes data plane actions; scope is limited to where assigned).
   - IF user inherits Reader at RG2 but Contributor is scoped to RG1 -> REJECT resource creation in RG2 (Reason: Contributor permissions don't cross scopes).
   - IF user has **User Access Administrator** at the resource scope -> SELECT can manage role assignments (Reason: includes `Microsoft.Authorization/roleAssignments/write`).
   - IF role name contains **"and Data Access"** -> remembers it adds data-plane reads, but ONLY at its assigned scope.
4. **SC-300 Golden Rule:** *"Permissions flow DOWN the scope tree, never sideways — and plain Reader never touches data."*

---

### Key Vault RBAC Roles: Metadata vs. Secret Values
1. **Exam Scenario:** Determine which users can read secret *contents* vs. only *metadata*, based on Key Vault role and scope.
2. **Key Decision Point:** The specific Key Vault data-plane role + assignment scope.
3. **Decision Path:**
   - IF role is **Key Vault Administrator** (any scope) -> CAN read secret contents (full data plane; inherits down from subscription/RG scope).
   - IF role is **Key Vault Reader** -> REJECT reading secret values (Reason: metadata only; explicitly denies reading sensitive contents).
   - IF role is **Key Vault Secrets User** at the specific vault scope -> CAN read that vault's secrets ONLY; no assignment on other vaults = no access.
4. **SC-300 Golden Rule:** *"Administrator reads everything, Reader reads labels, Secrets User reads values — and only within scope."*

---

### Managed Identities: Choosing the Right Type
1. **Exam Scenario:** VMs need to access Azure services (SQL Managed Instance) without stored credentials; an Automation account's permissions must vanish when the account is deleted; minimize admin effort.
2. **Key Decision Point:** Lifecycle coupling — should the identity die with the resource?
3. **Decision Path:**
   - IF identity must be **shared across multiple resources** (e.g., two VMs) -> SELECT **user-assigned managed identity** (Reason: one identity, assignable to many resources, minimal admin effort).
   - IF permissions must be **removed automatically when the resource is deleted** -> SELECT **system-assigned managed identity** (Reason: lifecycle tied to the resource; auto-cleanup of role assignments).
   - IF option is a **client secret or certificate** -> REJECT (Reason: manual creation, rotation, and cleanup — fails least effort/least privilege).
   - IF option is a **user-assigned identity** but the requirement is auto-cleanup on deletion -> REJECT (Reason: standalone resource with independent lifecycle that persists).
   - IF option is an **Entra user account** for a service -> REJECT (Reason: requires stored credentials = security risk).
4. **SC-300 Golden Rule:** *"System-assigned lives and dies with the resource; user-assigned is shared and outlives it. Never store secrets you can avoid."*

---

### Managed Identities & Group Membership Constraints
1. **Exam Scenario:** Add a managed identity (service principal) as a member of one of four groups: M365 group, Security (assigned/cloud), Dynamic, or AD-synced.
2. **Key Decision Point:** Group type, membership type, and source of authority.
3. **Decision Path:**
   - IF group is **Security + Assigned + Cloud** -> SELECT can add managed identity (service principals can join security groups).
   - IF group is **Microsoft 365 type** -> REJECT (Reason: service principals cannot be members of M365 groups).
   - IF group is **Dynamic** -> REJECT (Reason: membership controlled by rule, not manual).
   - IF group is **synced from Windows Server AD** -> REJECT (Reason: cloud view is read-only; manage on-premises).
   - IF task is adding a **cloud USER manually** -> allow M365 (All Company), Security-assigned-cloud groups; still reject Dynamic and synced groups.
4. **SC-300 Golden Rule:** *"Service principals only fit Security groups: assigned, cloud-sourced, manually managed."*

---

### Administrative Units: Scoped Role Administration
1. **Exam Scenario:** Password Administrators are assigned at different scopes (AU1, AU2, or tenant-wide); users are placed in AUs via dynamic membership rules; one AU has "Restricted management" enabled/disabled.
2. **Key Decision Point:** Role **scope** (AU vs. tenant) + **restricted management** setting.
3. **Decision Path:**
   - IF admin scoped to AU1 AND user is dynamically a member of AU1 -> CAN administer (Reason: dynamic membership still counts for scoped admin authority).
   - IF admin scoped to AU2 BUT user is in AU1 -> REJECT (Reason: out of scope — no inherited rights across AUs).
   - IF admin has **tenant-wide (Global) scope** AND the target AU has **restricted management = Disabled** -> CAN administer (Reason: tenant admins retain permissions over the AU).
   - IF AU had **restricted management = Enabled** -> tenant-level admins WITHOUT explicit AU assignment would be blocked.
4. **SC-300 Golden Rule:** *"Scope is king in AUs: no AU assignment, no admin rights — and restricted management locks out tenant admins."*

---

### Access Reviews: "Upon Completion" Settings & Non-Responsive Reviewers
1. **Exam Scenario:** Guest users must be automatically removed if the group owner (reviewer) fails to respond by the review end date.
2. **Key Decision Point:** Which access review configuration section governs automated post-review actions.
3. **Decision Path:**
   - IF requirement is automatic action when reviewers don't respond -> SELECT **"Upon completion settings" → "If reviewers don't respond: Remove access."**
   - IF option is the **Reviewers** section -> REJECT (Reason: selects WHO reviews, not outcomes of inaction).
   - IF option is **Advanced settings** -> REJECT (Reason: recommendations, justification prompts, notifications — not non-response logic).
   - IF option is **General** -> REJECT (Reason: name, description, dates only).
4. **SC-300 Golden Rule:** *"No response? The answer lives in 'Upon completion' — set 'Remove access' and walk away."*

---

### Access Reviews for Microsoft Entra Roles (PIM) — Required Role
1. **Exam Scenario:** User must create access reviews for Microsoft Entra **roles** (not groups/apps) in PIM, using least privilege.
2. **Key Decision Point:** Which directory role covers *role-based* (PIM) access reviews vs. group-based reviews.
3. **Decision Path:**
   - IF creating reviews for **Entra roles in PIM** -> SELECT **Privileged Role Administrator** (least privilege; Global Administrator also works but is over-privileged).
   - IF option is **User Administrator** -> REJECT (Reason: group reviews only; cannot review privileged roles).
   - IF option is **Identity Governance Administrator** -> REJECT (Reason: governs group/app reviews and entitlement management, not Entra role reviews).
   - IF option is **User Access Administrator** -> REJECT (Reason: Azure RBAC resource access, not directory role governance).
4. **SC-300 Golden Rule:** *"Reviewing privileged ROLES = Privileged Role Administrator. Reviewing groups = Identity Governance or User Administrator."*

---

### PIM Just-in-Time Access: First Step Order
1. **Exam Scenario:** Two users need JIT activation of the User Administrator role; one activation requires approval by a third user.
2. **Key Decision Point:** Sequence of PIM configuration — eligibility BEFORE activation settings.
3. **Decision Path:**
   - IF setting up JIT with PIM -> FIRST create the **role assignment (eligible membership)** for the users.
   - THEN configure activation settings (approval, approvers, MFA, justification).
   - IF option is **Conditional Access policies** -> REJECT as first step (Reason: enforces conditions during activation but grants no eligibility).
   - IF option is **security groups** -> REJECT as the primary step (Reason: group-based assignment is possible, but the core prerequisite is the eligible role assignment).
   - IF option is **administrative units** -> REJECT (Reason: scope a role's reach, don't grant eligibility).
4. **SC-300 Golden Rule:** *"PIM order: Eligibility FIRST, activation policy SECOND. No eligible role, nothing to activate."*

---

### Protected Actions & Phishing-Resistant MFA: Step Order
1. **Exam Scenario:** Require phishing-resistant MFA to execute a protected administrative action.
2. **Key Decision Point:** Which Entra feature must exist BEFORE the CA policy references it.
3. **Decision Path:**
   - IF enforcing method-specific MFA (e.g., phishing-resistant) for protected actions -> FIRST configure the **Authentication Strength** (built-in "Phishing-resistant MFA" or custom), THEN create the CA policy referencing it.
   - IF option is "Create a Conditional Access policy" as the FIRST step -> REJECT (Reason: it's step two — the strength must exist to be selected).
   - IF option is **access package** -> REJECT (Reason: Entitlement Management governs resource access, not admin action authentication).
   - IF option is **authentication context** -> REJECT (Reason: step-up auth inside apps, not tenant-level protected actions).
4. **SC-300 Golden Rule:** *"Define the strength before you enforce it — Authentication Strength first, CA policy second."*

---

### Entitlement & Consent Governance: Admin Consent Workflow
1. **Exam Scenario:** Disable user consent to applications; enable a workflow so users can request admin approval; determine who can approve pending consent requests.
2. **Key Decision Point:** Consent configuration path + who holds review authority.
3. **Decision Path:**
   - IF disabling user consent -> navigate **Identity → Applications → Enterprise applications → Consent and permissions → User consent settings.**
   - IF enabling the admin consent request workflow -> same blade under **Admin consent settings**; users see a "request approval" option at sign-in.
   - IF deciding WHO can approve -> IF a user is **explicitly listed as a reviewer** in the admin consent settings -> CAN approve regardless of directory roles (even a plain user).
   - IF answer options omit any explicitly configured reviewer -> REJECT (Reason: reviewer list defines authority, not directory roles).
4. **SC-300 Golden Rule:** *"The reviewer list is the authority list — roles don't matter, the configured reviewers do."*

---

### Third-Party App Governance: Defender for Cloud Apps Policy Types
1. **Exam Scenario:** Monitor permissions and risk of third-party OAuth apps connected to Microsoft 365; create session controls for sanctioned apps.
2. **Key Decision Point:** The correct Defender for Cloud Apps policy type per task.
3. **Decision Path:**
   - IF monitoring third-party OAuth app permissions -> SELECT an **OAuth app policy** (alerts on permission level, community trust, user count).
   - IF enforcing real-time session monitoring/filtering -> SELECT a **Session policy** (requires a related CA policy + the **Cloud App Security Administrator** role to create it).
   - IF option is an "app permission policy" -> REJECT (Reason: that's a Microsoft Teams policy for client app installation).
   - IF option is an "access policy" -> REJECT for OAuth governance (Reason: controls user sessions in real time, not app permissions).
   - IF option is an "endpoint protection policy" -> REJECT (Reason: Intune/Defender for Endpoint device settings).
4. **SC-300 Golden Rule:** *"OAuth apps = OAuth app policy; live sessions = Session policy (needs Cloud App Security Administrator + CA); device health = endpoint policy."*

---

### Defender for Cloud Apps: Required Role for Policy Creation
1. **Exam Scenario:** User has Security Administrator (read-only in Defender for Cloud Apps) and must CREATE session policies.
2. **Key Decision Point:** Service-specific admin role vs. related Entra roles.
3. **Decision Path:**
   - IF user must create/modify Defender for Cloud Apps policies -> SELECT **Cloud App Security Administrator** (least privilege, full service permissions).
   - IF current role is **Security Administrator** -> note it grants READ-ONLY access to Defender for Cloud Apps (view alerts/policies only).
   - IF option is **Cloud Application Administrator** -> REJECT (Reason: manages app registrations/enterprise apps in Entra, not MDCA policies).
   - IF option is "create a CA policy" as the first step -> REJECT (Reason: CA enforces session control later; permissions to build the session policy come first).
4. **SC-300 Golden Rule:** *"Security Administrator sees MDCA; Cloud App Security Administrator shapes MDCA."*

---

### Permissions Management (CIM): Onboarding Roles
1. **Exam Scenario:** Onboard an Azure subscription into Microsoft Entra Permissions Management with least privilege.
2. **Key Decision Point:** The role pair required (Entra role + Azure RBAC role) and their scopes.
3. **Decision Path:**
   - IF onboarding/managing Permissions Management -> assign **Permissions Management Administrator** at the **tenant** level (Entra side).
   - IF authorizing the subscription data collection -> assign **User Access Administrator** at the **subscription** level (Azure RBAC side).
   - IF option is **Global Administrator** -> REJECT (Reason: works, but violates least privilege).
   - IF option is **Security Administrator** or **Application Administrator** -> REJECT (Reason: neither can administer the Permissions Management service).
4. **SC-300 Golden Rule:** *"Permissions Management = tenant-level PMA role + subscription-level User Access Administrator. Never Global Admin."*

---

### Permissions Management UI: Insights vs. Analytics Tabs
1. **Exam Scenario:** Inventory all Global Administrators; view an individual user's Permission Creep Index (PCI).
2. **Key Decision Point:** Which PM console tab serves tenant-wide role oversight vs. per-identity deep analysis.
3. **Decision Path:**
   - IF inventorying holders of a privileged role (e.g., Global Admins) -> USE the **Insights tab** → "Review global administrators."
   - IF examining a specific user's PCI score / detailed analytics -> USE the **Analytics tab** (search the identity, view granular PCI).
   - IF the Dashboard shows a heat map -> note it is aggregate, not per-user detail.
4. **SC-300 Golden Rule:** *"Roles overview = Insights; individual PCI dive = Analytics."*

---

### Entra Roles for Global Secure Access Management
1. **Exam Scenario:** Determine which users can configure Microsoft Entra Internet Access (Global Secure Access).
2. **Key Decision Point:** Management vs. read-only directory roles.
3. **Decision Path:**
   - IF user is **Global Administrator** -> CAN manage (universal rights).
   - IF user is **Security Administrator** -> CAN manage (explicit GSA permissions).
   - IF user is **Global Reader** -> REJECT (Reason: strictly read-only; no configuration).
4. **SC-300 Golden Rule:** *"Global Secure Access: Global Admin and Security Admin manage; Global Reader only gazes."*

---

### Global Secure Access: Client Support Matrix
1. **Exam Scenario:** Determine which devices/user groups support the GSA client (Entra joined vs. registered; AVD multi-session; OS platforms).
2. **Key Decision Point:** Platform support + device registration state + session type.
3. **Decision Path:**
   - IF device is **Windows 10/11** (joined OR registered/BYOD) -> SUPPORTED (client v2.26.108+ extends to registered devices).
   - IF device is **macOS, iOS, Android** -> SUPPORTED for Private Access.
   - IF environment is **AVD multi-session / Windows 11 Enterprise multi-session** -> REJECT as unsupported.
   - IF AVD **single-session** -> supported.
   - IF user being an "approver" or contractor with registered device -> registration state alone doesn't disqualify.
4. **SC-300 Golden Rule:** *"GSA client: any join state, four platforms — but multi-session Windows is a hard NO."*

---

### Global Secure Access: On-Premises Linux / Client-less Connectivity
1. **Exam Scenario:** Route internet traffic from on-premises Linux devices (no client available) through GSA.
2. **Key Decision Point:** Endpoint client vs. network-edge connection.
3. **Decision Path:**
   - IF endpoints can't run the GSA client (Linux) -> SELECT **create a remote network** (IPsec tunnel from edge router/firewall to GSA; network-level routing).
   - IF option is **private network connector** -> REJECT (Reason: Entra Private Access component for publishing on-prem apps inbound, not forwarding internet traffic).
   - IF option is **Adaptive Access settings** -> REJECT (Reason: risk-based CA on traffic already flowing through GSA).
   - IF option is **Azure Connected Machine agent** -> REJECT (Reason: Azure Arc management agent, not a network client).
4. **SC-300 Golden Rule:** *"No client? Build a remote network. Connectors publish apps; they don't forward internet."*

---

### Conditional Access + GSA: Compliant Network Location
1. **Exam Scenario:** Allow resource access ONLY from devices running the GSA client.
2. **Key Decision Point:** Prerequisite step that exposes the GSA network signal to CA.
3. **Decision Path:**
   - IF requiring GSA-connected access -> FIRST **enable Global Secure Access signaling** at tenant level; then use the built-in **"All compliant network locations"** in a CA location condition.
   - IF option is "create a named location" -> REJECT (Reason: signaling auto-creates the built-in compliant location; no manual named location).
   - IF option is "enable tenant restrictions tagging" -> REJECT (Reason: controls access to EXTERNAL tenants, not your own resources).
   - IF option is "create a remote network" -> REJECT (Reason: site-to-site branch connectivity, not per-device signaling).
4. **SC-300 Golden Rule:** *"Flip on GSA signaling → 'All compliant network locations' appears → target it in CA."*

---

### CA Policy for GSA Security Profiles
1. **Exam Scenario:** Apply a GSA security profile (web traffic filtering) via Conditional Access.
2. **Key Decision Point:** The two CA components that must reference GSA.
3. **Decision Path:**
   - IF applying a GSA security profile -> configure BOTH:
     - **Target resources** = "All internet resources with Global Secure Access" (traffic, not apps).
     - **Session control** = "Use Global Secure Access security profile" → select the profile.
   - IF either component is missing -> policy cannot enforce the profile.
4. **SC-300 Golden Rule:** *"GSA profile in CA = Target internet resources + Session control with the security profile."*

---

### Entra ID Log Analytics Destination
1. **Exam Scenario:** Stream Entra diagnostic (sign-in/audit) logs for KQL analysis, workbooks, and alerts.
2. **Key Decision Point:** Diagnostic settings destination matched to the task verb ("analyze").
3. **Decision Path:**
   - IF requirement is to ANALYZE with KQL/queries/alerts -> SELECT **Log Analytics workspace**.
   - IF option is **Storage account** -> REJECT (Reason: archival/audit retention, not interactive analysis).
   - IF option is **Event Hub** -> REJECT (Reason: streams to external SIEMs/pipelines, not native querying).
   - IF option is **Azure SQL database** -> REJECT (Reason: not a supported diagnostic settings destination at all).
4. **SC-300 Golden Rule:** *"Analyze = Log Analytics. Archive = Storage. Stream to SIEM = Event Hub. SQL = never."*

---

### Entra ID Login to Azure Windows VMs
1. **Exam Scenario:** Users RDP to Azure Windows VMs using Entra credentials; validate role assignments and client device requirements.
2. **Key Decision Point:** Three gates — correct RBAC login role, client device join state, client OS platform.
3. **Decision Path:**
   - IF user's role is NOT exactly **Virtual Machine User Login** or **Virtual Machine Administrator Login** -> REJECT (Reason: fabricated/incorrect role names don't authorize login).
   - IF source client device is neither **Entra joined nor Entra registered** to the same tenant -> REJECT (Reason: native Entra RDP authentication fails).
   - IF client device is not **Windows 10/11** (e.g., Android) -> REJECT (Reason: unsupported platform for Windows VM Entra RDP credential exchange).
   - ALL three gates must pass → only then = Yes.
4. **SC-300 Golden Rule:** *"VM Entra login triple-check: correct Login RBAC role + joined/registered client + Windows 10/11."*

---

### Terms of Use: Per-Device Consent & Expiration Logic
1. **Exam Scenario:** Terms of Use policy requires consent on every device; consent has an expiration start date; determine when users are prompted.
2. **Key Decision Point:** The "consent on every device" setting + expiration date math.
3. **Decision Path:**
   - IF "Require consent on every device" = On AND user hasn't consented on that device -> user WILL be prompted (enforced via CA, no device registration required in the tenant).
   - IF user consented BEFORE the "expire starting on" date AND the consent is still active -> NOT prompted / cannot re-accept yet (Reason: consent remains valid until expiration).
   - IF consent date is past expiration -> re-prompt occurs.
4. **SC-300 Golden Rule:** *"Every device = every prompt — until the expiration date arrives, then consent resets."*

---

### Hybrid Identity: Bulk User Creation Source of Authority
1. **Exam Scenario:** Create 300 users from a spreadsheet in a hybrid environment where users need access to BOTH on-premises and cloud resources.
2. **Key Decision Point:** Source of authority (AD DS first) + bulk import tooling.
3. **Decision Path:**
   - IF hybrid sync with Entra Connect AND users need on-prem + cloud access -> create users in **on-premises AD DS** using `New-ADUser`.
   - IF bulk-creating from a spreadsheet -> save as **CSV** and use **`Import-Csv`** to pipe into `New-ADUser`.
   - IF option uses **`New-MgUser`** -> REJECT (Reason: creates cloud-only users; breaks hybrid model, no on-prem access, sync conflicts).
   - IF option uses **`Set-ADUser`/`Set-MgUser`** -> REJECT (Reason: these MODIFY existing users, they don't create).
   - IF option uses **`Get-Content`** for CSV -> REJECT (Reason: raw text lines, no structured objects; `Import-Csv` is correct).
4. **SC-300 Golden Rule:** *"Hybrid + bulk = on-prem AD DS + Import-Csv → New-ADUser. New-MgUser makes cloud-only orphans."*

---

### Consent Settings Navigation Paths
1. **Exam Scenario:** Disable user app consent; configure the admin consent request workflow.
2. **Key Decision Point:** Correct admin center blade path.
3. **Decision Path:**
   - IF disabling user consent → **Identity → Applications → Enterprise applications → Consent and permissions → User consent settings.**
   - IF configuring admin consent requests → same blade → **Admin consent settings.**
4. **SC-300 Golden Rule:** *"All consent governance lives under Enterprise applications → Consent and permissions."*

---

### Self-Service App Access: Who Can Request?
1. **Exam Scenario:** Self-service access enabled for an app with Group1 as the assigned target group; determine which users can submit requests.
2. **Key Decision Point:** Is the user ALREADY effectively assigned (via group membership)?
3. **Decision Path:**
   - IF user is already a member of the assignment group -> REJECT request eligibility (Reason: already assigned users cannot request access).
   - IF user is NOT assigned and NOT in the group -> CAN request via My Apps.
   - IF user is a designated APPROVER but not assigned -> CAN still request (approver role doesn't block requestor eligibility).
4. **SC-300 Golden Rule:** *"Already in = can't ask. Approver ≠ excluded from asking."*

---

### Deleted App Registration: Soft-Delete Retention
1. **Exam Scenario:** App registration deleted on a given date; determine the last restorable date.
2. **Key Decision Point:** The 30-day soft-delete window.
3. **Decision Path:**
   - IF app registration deleted -> restorable for **30 days** from deletion date; permanent after.
   - IF option falls within 30 days of deletion -> plausible answer.
   - IF option is ~14 days or 88/180 days after deletion -> REJECT (Reason: outside or well short of the 30-day window).
4. **SC-300 Golden Rule:** *"Deleted app registration = 30-day soft-delete lifeline. Count the days."*

---

### Bulk License Removal: Tool Selection
1. **Exam Scenario:** Remove Microsoft 365 E3 license from 2,500 users with least administrative effort.
2. **Key Decision Point:** License-centric vs. user-centric management views.
3. **Decision Path:**
   - IF bulk-removing a SPECIFIC license product at scale -> SELECT the **Licenses blade in the Microsoft Entra admin center** (license-centric: pick product → view licensed users → bulk remove).
   - IF option is the **Microsoft 365 admin center Licensing node** -> REJECT (Reason: user-centric view; cumbersome for 2,500 users).
   - IF option is **Identity Governance blade** -> REJECT (Reason: access reviews/entitlement/PIM — not direct license operations).
   - IF option is a nonsensical cmdlet (e.g., `Set-WindowsProductKey`) -> REJECT (Reason: OS activation, unrelated).
4. **SC-300 Golden Rule:** *"Bulk license work = license-centric Entra Licenses blade, not user-centric clicking."*

---

### Risk Policies: User Risk vs. Sign-in Risk + MFA Registration State
1. **Exam Scenario:** User risk policy targets Group1 (requires password change on user risk); Sign-in risk policy targets Group2 (requires MFA at "Low and above"); some users are unregistered for MFA.
2. **Key Decision Point:** Which RISK TYPE the policy evaluates + whether the user can satisfy the remediation control.
3. **Decision Path:**
   - IF user is in the **user risk** policy group, has user risk, AND is MFA-registered -> password change enforced = Yes (can complete SSPR flow).
   - IF user has HIGH **user risk** but is only covered by a **sign-in risk** policy -> REJECT trigger (Reason: user risk signals ≠ sign-in risk conditions; different telemetry).
   - IF user has medium **sign-in risk**, is covered by the sign-in policy, but is **NOT MFA-registered** -> sign-in BLOCKED (Reason: cannot satisfy MFA challenge without pre-registration; legacy risk policy blocks instead of prompting).
4. **SC-300 Golden Rule:** *"Match the risk to the policy (user ≠ sign-in), then check MFA registration — unregistered + MFA required = blocked."*# SC-300 Decision Trees - Part 1

# SC-300 Decision Trees & Logic Rules — Part 1 Analysis

---

### 1. Detecting Leaked Credentials for Hybrid Identities (Q1, Q13, Q23)
1. **Exam Scenario:** A hybrid tenant synchronized from on-premises AD must detect compromised passwords found on the dark web and automatically remediate affected accounts.
2. **Key Decision Point:** Which synchronization/authentication method allows the cloud to evaluate the password hash, and which risk type (User vs. Sign-in) applies.
3. **Decision Path:**
   - IF the requirement is **detecting leaked credentials** → SELECT **Password Hash Synchronization (PHS)** (Reason: Identity Protection compares synced hashes against the known-compromised credentials database; only PHS gives the cloud visibility into the hash).
   - IF the scenario uses **federation (ADFS/PingFederate)** → REJECT it (Reason: Azure AD never sees the password; leaked credential detection is impossible).
   - IF the requirement is **preventing weak/banned passwords** → SELECT **Azure AD Password Protection** (Reason: proactive prevention, not detection of existing compromise).
   - IF the requirement is **defining which MFA methods users can register** → SELECT **Authentication method policy** (Reason: unrelated to credential compromise detection).
   - IF the risk is a **compromised identity (leaked credentials)** → SELECT **User risk policy** (Reason: remediation = mandatory secure password reset, requires SSPR registration for self-remediation).
   - IF the risk is an **anomalous sign-in attempt (anonymous IP/Tor, unfamiliar sign-in properties)** → SELECT **Sign-in risk policy** (Reason: remediation = MFA challenge for that specific session).
4. **SC-300 Golden Rule:** *"PHS = Detection, Federation = Blind. Leaked credentials = User Risk (reset password). Suspicious sign-in = Sign-in Risk (MFA challenge)."*
   - **License note:** Identity Protection risk policies require **Microsoft Entra ID P2**.

---

### 2. Conditional Access — Location Conditions & Named Locations (Q2)
1. **Exam Scenario:** Require or bypass MFA for users connecting from a specific physical office (e.g., Boston).
2. **Key Decision Point:** Public vs. private IP visibility, and modern (Named Locations) vs. legacy (Trusted IPs) configuration.
3. **Decision Path:**
   - IF controlling MFA by physical location → SELECT **Named Locations with public IP ranges** inside a **Conditional Access location condition** (Reason: Azure AD only sees the source public IP).
   - IF the option says **private IP ranges** → REJECT (Reason: Azure AD is a cloud service and cannot see internal RFC1918 addresses like 10.0.0.0/8).
   - IF the option says **"Trusted IPs"** → REJECT for modern scenarios (Reason: legacy MFA portal setting; Named Locations + CA is the recommended best practice).
4. **SC-300 Golden Rule:** *"Cloud sees PUBLIC IPs only. Named Locations + Conditional Access = modern answer; Trusted IPs = legacy distractor."*
   - **License note:** Conditional Access requires **Microsoft Entra ID P1**.

---

### 3. Dynamic Groups & Group-Based Licensing (Q3, Q5, Q12, Q19, Q33)
1. **Exam Scenario:** Automatically assign licenses to all users of a company/department (e.g., `user.companyName -eq "A. Datum"`) with zero administrative overhead, including future users.
2. **Key Decision Point:** Group type capable of both dynamic membership and license assignment.
3. **Decision Path:**
   - IF licenses must be assigned **automatically based on user attributes** → SELECT **Dynamic User security group + group-based licensing** (Reason: rule evaluates attributes; members inherit licenses; new users are added automatically).
   - IF the option is a **distribution group** → REJECT (Reason: email only; cannot hold licenses).
   - IF the option is an **Administrative Unit** → REJECT (Reason: AUs scope *admin permissions*, they cannot be assigned licenses).
   - IF the option is an **OU** → REJECT (Reason: on-premises AD DS construct for GPO/delegation; not a cloud licensing object).
   - IF the rule must include **all internal users but exclude guests** → USE the rule: `(user.objectId -ne $null) -and (user.userType -eq "Member")` (Reason: `userType -eq "Member"` filters out "Guest" accounts).
   - IF the option mentions **nested groups** for licensing → REJECT (Reason: nested groups are **not supported** for group-based licensing).
4. **SC-300 Golden Rule:** *"Licenses flow to SECURITY groups only; Dynamic User = attribute-driven automation. Members stay, Guests go (`userType -eq 'Member'`)."*

---

### 4. Azure AD Password Protection Deployment (Q4)
1. **Exam Scenario:** Deploy custom banned password list enforcement across an on-premises AD domain.
2. **Key Decision Point:** Where the proxy service is installed and where the banned list is configured.
3. **Decision Path:**
   - IF installing the **Proxy service** → SELECT a **domain-joined member server** with outbound internet connectivity (Reason: proxy communicates with Entra ID; it must NOT sit on a DC).
   - IF the option installs the proxy **on a Domain Controller** → REJECT (Reason: unsupported; DC agent and proxy are separate components).
   - IF configuring the **custom banned password list** → SELECT the **Entra ID admin portal (cloud)** (Reason: centrally managed; the proxy downloads and distributes it to DC agents).
4. **SC-300 Golden Rule:** *"Proxy on a member server, DC Agent on the DCs, banned list lives in the cloud."*

---

### 5. Directory Extensions for Custom Attributes (Q5)
1. **Exam Scenario:** Synchronize a custom on-premises attribute (e.g., `LWLicenses`) to the cloud so it can drive dynamic group membership.
2. **Key Decision Point:** Which Azure AD Connect feature exposes custom attributes.
3. **Decision Path:**
   - IF a custom on-prem attribute must be usable in the cloud (e.g., dynamic group rules) → SELECT **Directory Extensions** in Azure AD Connect (Reason: extends the Entra ID schema with custom attributes).
   - IF the option is "Change user sign-in" or scheduler cmdlets → REJECT (Reason: those alter authentication method or sync timing, not schema).
4. **SC-300 Golden Rule:** *"Custom attribute to the cloud = Directory Extensions; then drive licensing with a Dynamic User group."*

---

### 6. Delegating Role Management — Entra Roles vs. Azure RBAC (Q6)
1. **Exam Scenario:** Delegate the ability to manage directory roles vs. the ability to manage Azure resource role assignments.
2. **Key Decision Point:** Directory-plane role vs. Azure resource-plane role.
3. **Decision Path:**
   - IF managing **Entra ID directory roles** (Global Admin, Helpdesk Admin, PIM) → SELECT **Privileged Role Administrator** (Reason: assigns directory roles and manages PIM).
   - IF managing **Azure RBAC roles** (Owner, Contributor, Reader on subscriptions/RGs) → SELECT **User Access Administrator** (Reason: explicitly grants ability to manage user access to Azure resources).
4. **SC-300 Golden Rule:** *"Directory roles → Privileged Role Administrator. Azure resource roles → User Access Administrator."*

---

### 7. Publishing On-Premises Apps & Session Controls (Q7, Q16)
1. **Exam Scenario:** Provide secure remote access to internal web apps without VPN, and enforce session-level controls/DLP inside cloud apps like SharePoint Online.
2. **Key Decision Point:** Where the application lives (on-premises vs. cloud SaaS).
3. **Decision Path:**
   - IF the app is **on-premises** and needs Entra pre-auth/SSO/Conditional Access → SELECT **Azure AD Application Proxy** (Reason: reverse-proxy pattern; no VPN, no inbound firewall holes).
   - IF the option is an **app registration alone** → REJECT as the complete solution (Reason: prerequisite identity, but it does not publish the app).
   - IF the option is an **Endpoint Manager policy / app configuration policy** → REJECT (Reason: those manage devices and client apps, not server-side publishing).
   - IF the app is **SharePoint Online** and needs session controls, DLP, or real-time monitoring → SELECT **Microsoft Defender for Cloud Apps + Conditional Access App Control** (Reason: session-level in-app policy enforcement).
4. **SC-300 Golden Rule:** *"On-prem app → Application Proxy. Cloud app session control → Defender for Cloud Apps."*

---

### 8. Restricting App Registration & Delegating with Least Privilege (Q9)
1. **Exam Scenario:** Block all non-admin users from registering applications tenant-wide, but allow specific users to do so.
2. **Key Decision Point:** Tenant-wide switch vs. granular role assignment.
3. **Decision Path:**
   - IF blocking all users from registering apps → SET **"Users can register applications" = No** in User settings.
   - IF specific users must still register apps → ASSIGN the **Application Developer** role (Reason: grants `microsoft.directory/applications/create`, overriding the tenant restriction for those users).
4. **SC-300 Golden Rule:** *"Tenant switch OFF for everyone, Application Developer role ON for the few."*

---

### 9. Entitlement Management — Catalogs, Access Packages & Connected Organizations (Q10, Q18, Q31)
1. **Exam Scenario:** Delegate management of application access to business owners; enable external partners (fabrikam.com) to request access; add resources to existing packages.
2. **Key Decision Point:** The foundational container for delegation, and how external domains are onboarded.
3. **Decision Path:**
   - IF delegating access management to non-administrators → CREATE a **Catalog** and assign **catalog owners** (Reason: the catalog is the container of resources + access packages; owners manage packages within it — this is the *first step*).
   - IF the option is **User consent settings / Admin consent requests** → REJECT (Reason: those govern app permission consent, not access assignment delegation).
   - IF the option is **Programs** → REJECT (Reason: programs only group access reviews; not a delegation mechanism).
   - IF **external users from a specific domain** must request access packages → CREATE a **Connected Organization** (Reason: defines the trusted external domain allowed to request packages).
   - IF bundling groups/apps/SharePoint sites for request-and-approval workflows → CREATE an **Access Package** (Reason: bundles resources with policies for lifecycle, approval, expiration).
   - IF adding a new app to an existing package → NAVIGATE to the package → **Resource roles → Add resource roles** (Reason: additive change; existing resources untouched).
4. **SC-300 Golden Rule:** *"Catalog → Owner → Access Package → Connected Organization for partners. Delegation starts at the catalog."*
   - **License note:** Entitlement Management requires **Microsoft Entra ID P2 / Entra ID Governance**.

---

### 10. Administrative Units — Scoped Administration (Q11)
1. **Exam Scenario:** Helpdesk administrators must manage licenses **only** for users in their own branch office.
2. **Key Decision Point:** Scoping mechanism for role assignment.
3. **Decision Path:**
   - IF a role must apply to a **subset of users** (per office/region/department) → CREATE an **Administrative Unit** per office and assign the role **scoped to the AU** (Reason: AU-restricted role assignments limit admin reach to members of that AU).
   - IF the option is a dynamic group or OU → REJECT (Reason: groups don't scope admin roles; OUs are on-premises).
4. **SC-300 Golden Rule:** *"AU = role scope limiter. Same role, smaller blast radius."*

---

### 11. PIM Role Configuration — JIT & Time-Bound Access (Q14, Q28)
1. **Exam Scenario:** Implement planned governance changes for a privileged role (e.g., User Administrator) using PIM.
2. **Key Decision Point:** Assignment state (Eligible vs. Active) and expiration settings vs. activation-time controls.
3. **Decision Path:**
   - IF implementing **just-in-time access** → SET all assignments to **Eligible** (Reason: users must activate; no standing privileges).
   - IF the option says **set assignments to Active** → REJECT (Reason: permanent standing access; contradicts PIM's purpose).
   - IF enforcing **time-bound eligibility** → MODIFY **"Expire eligible assignments after"** (Reason: forces periodic re-validation, prevents privilege accumulation).
   - IF the option is **Require justification / Require ticket information on activation** → These are valid *activation controls*, but REJECT them when the question asks about the **fundamental nature of the assignment** (Reason: they govern the activation event, not the assignment state).
   - IF the option is **create an access review** → REJECT for *changing role settings* (Reason: access reviews validate existing assignees; they don't alter role behavior).
   - IF the option is **create an administrative unit** → REJECT (Reason: scopes the role, doesn't change its governance settings).
4. **SC-300 Golden Rule:** *"PIM = Eligible + Expiring. Justification/tickets are activation garnish, not the assignment itself."*
   - **License note:** PIM requires **Microsoft Entra ID P2**.

---

### 12. Azure AD Connect — Sync Scope & Deployment Models (Q15, Q20)
1. **Exam Scenario:** Control which users sync from on-prem AD; onboard a newly acquired forest (litware.com) into an existing tenant.
2. **Key Decision Point:** Filtering scope vs. scheduler cmdlets; classic Connect vs. Cloud Sync.
3. **Decision Path:**
   - IF selecting **which users/objects sync** → USE **"Customize synchronization options"** in the Azure AD Connect wizard (Reason: enables domain/OU/attribute filtering).
   - IF the option is `Set-ADSyncScheduler` → REJECT (Reason: changes the sync *schedule*, not the scope).
   - IF the option is `Start-ADSyncSyncCycle` → REJECT (Reason: triggers a manual sync run; changes nothing about scope).
   - IF the option is **"Change user sign-in"** → REJECT (Reason: alters the authentication method — PHS/PTA/federation — not the sync scope).
   - IF a **new forest from a merger/acquisition** must sync to an existing tenant → SELECT **Azure AD Connect Cloud Sync** (Reason: lightweight agent; parallel sync from multiple disconnected forests without touching the existing Connect server).
   - IF the option is **staging mode** → REJECT (Reason: staging mode reads but never writes to Entra ID — it's a backup/test configuration).
4. **SC-300 Golden Rule:** *"Scope = Customize sync options. M&A forest = Cloud Sync. Staging mode = read-only, never active."*

---

### 13. Routing Entra ID Logs for Auditing (Q17)
1. **Exam Scenario:** Stream Azure AD audit and sign-in logs to a Log Analytics workspace for KQL analysis and retention.
2. **Key Decision Point:** The platform log-routing mechanism.
3. **Decision Path:**
   - IF exporting Entra ID logs to Log Analytics/Event Hub/Storage → CONFIGURE **Diagnostic settings** (select AuditLogs + SignInLogs, choose the workspace).
   - IF the option is Company branding / External Identities / App registrations → REJECT (Reason: branding = sign-in UI; External Identities = B2B/B2C; App registrations = app integration — none route logs).
4. **SC-300 Golden Rule:** *"Logs to Log Analytics = Diagnostic settings. Always."*

---

### 14. SSPR — Admin Two-Gate Policy (Q22)
1. **Exam Scenario:** A tenant allows four SSPR methods (Email, Phone, Security questions, Authenticator), but an administrator attempts self-service password reset.
2. **Key Decision Point:** The non-configurable admin policy overriding tenant settings.
3. **Decision Path:**
   - IF the user is a **built-in administrator role member** → APPLY the **two-gate policy** (Reason: hardcoded; requires **2** authentication data pieces, overriding tenant configuration).
   - IF the option includes **Security questions** for admins → REJECT (Reason: explicitly prohibited for administrators).
   - IF the tenant requires 1 gate for users → Remember: admins always need **2 gates** regardless of tenant setting.
4. **SC-300 Golden Rule:** *"Admins: 2 gates, no security questions — ever. Email, Phone, Authenticator only."*

---

### 15. Device Settings — Maximum Devices Per User (Q24)
1. **Exam Scenario:** Sales users hit the device-join limit; the limit must be raised to 10.
2. **Key Decision Point:** Where the tenant-wide device limit lives.
3. **Decision Path:**
   - IF changing the **maximum number of devices per user** → GO TO **Device settings** in Entra ID (Reason: tenant-wide setting).
   - IF the option is User settings → REJECT (Reason: app registrations and external collaboration, not devices).
   - IF the option is Access reviews or Security defaults → REJECT (Reason: access attestation and baseline MFA, respectively — no device limits).
4. **SC-300 Golden Rule:** *"Device join limit lives in Device settings, not User settings."*

---

### 16. Role-Assignable Groups (Q25)
1. **Exam Scenario:** A group (ITGroup1) does not appear when assigning an Entra ID role (e.g., Device Administrators) to it.
2. **Key Decision Point:** The immutable `isAssignableToRole` property.
3. **Decision Path:**
   - IF a group can't be selected for role assignment → CHECK whether it was created with **"Azure AD roles can be assigned to the group" = Yes**; if not → **DELETE and RECREATE** the group with the setting enabled (Reason: `isAssignableToRole` is **immutable** after creation).
   - IF the option is "change membership type to Dynamic User/Dynamic Device" → REJECT (Reason: membership type is independent of role-assignability).
   - IF the option is "add an owner" → REJECT (Reason: owners manage membership, not group capabilities).
4. **SC-300 Golden Rule:** *"Role-assignable is set at birth and never changes. Wrong group? Delete and recreate."*

---

### 17. Access Reviews — Who Can Create & How to Configure (Q26, Q30)
1. **Exam Scenario:** Determine which users can create/manage an access review for an access package; configure a recurring review of guest access to an app.
2. **Key Decision Point:** Required directory roles; review scope, reviewers, and automation.
3. **Decision Path:**
   - IF creating/managing **entitlement management access reviews** → The user must hold **Global Administrator, User Administrator, or Identity Governance Administrator** (Reason: these are the only three roles permitted; check the case-study role table carefully — include *every* qualifying user).
   - IF reviewing **guest access to an application** → SCOPE the review to the app with **"Guest users only"**.
   - IF reviewers must be the user's manager with a fallback → SET **Reviewer = Manager**, **Fallback = specific user** (e.g., Megan Bowen).
   - IF access must be **auto-revoked when reviewers don't respond** → ENABLE **"Auto apply results to resource"** and set no-response action to **"Remove access"** (Reason: non-response within the duration window = automatic revocation).
4. **SC-300 Golden Rule:** *"Access review creators: Global Admin, User Admin, or Identity Governance Admin — no one else. No response = Remove access."*

---

### 18. Guest Invitation Troubleshooting (Q27)
1. **Exam Scenario:** Guest user invitations are failing or blocked; find the correct configuration area.
2. **Key Decision Point:** Invitation flow vs. session/token controls.
3. **Decision Path:**
   - IF the problem is **inviting guests** (who can invite, self-service sign-up, domain allow/deny lists) → CONFIGURE **External collaboration settings** (Reason: central control of the entire invitation pipeline).
   - IF the option is **Continuous Access Evaluation** → REJECT (Reason: CAE revokes active tokens in near real-time; nothing to do with invitations).
   - IF the option is **Access reviews** → REJECT (Reason: reviews existing access, not initial invitations).
   - IF the option is **Conditional Access** → REJECT (Reason: CA enforces controls at sign-in, *after* the guest is already invited).
4. **SC-300 Golden Rule:** *"Invitation problems → External collaboration settings. CAE/CA/Reviews all act AFTER the guest exists."*

---

### 19. Per-User MFA (When Conditional Access Is Forbidden) (Q29)
1. **Exam Scenario:** Require MFA for a group (sg-Finance), exclude one user (Debra Berger), and the question **explicitly forbids** a Conditional Access policy.
2. **Key Decision Point:** Legacy per-user MFA portal states.
3. **Decision Path:**
   - IF CA is forbidden and MFA must be enforced per user/group → USE the **per-user MFA portal**; set members to **Enforced** (Reason: forces MFA registration at next sign-in).
   - IF a specific user must be excluded → SET that user's MFA state to **Disabled** (Reason: explicitly opts them out of MFA prompts).
   - IF a Conditional Access option is offered despite the prohibition → REJECT (Reason: violates the stated constraint).
4. **SC-300 Golden Rule:** *"No CA allowed? Per-user MFA: Enforced for the group, Disabled for the exclusion."*

---

### 20. User Consent Settings & Permission Classification (Q21, Q32)
1. **Exam Scenario:** Govern how applications gain access to organizational data; allow users to consent only to low-impact permissions (e.g., `User.Read`) from verified publishers.
2. **Key Decision Point:** Consent policy mode + permission classification.
3. **Decision Path:**
   - IF controlling **which app permissions users can consent to** → MODIFY **User consent settings** (Reason: tenant-wide policy for app data access consent).
   - IF users may consent only to **specific low-impact permissions** → SELECT **"Allow user consent for apps from verified publishers, for selected permissions"** AND **classify** the delegated permission (e.g., `User.Read`) as **low impact** (Reason: classification builds the pre-approved list; anything else requires admin approval).
   - IF the option is authentication methods / access packages / application proxy → REJECT (Reason: identity proofing, access lifecycle, and app publishing respectively — none govern app data permissions).
4. **SC-300 Golden Rule:** *"Granular consent = Verified publishers + Selected permissions + Low-impact classification."*

---

### 21. Conditional Access — Grant Controls & Legacy Auth Blocking (Q34, Q36)
1. **Exam Scenario:** (a) Require a compliant device **OR** app protection policy for executives; (b) block all legacy authentication tenant-wide.
2. **Key Decision Point:** AND vs. OR grant logic; client app conditions.
3. **Decision Path:**
   - IF the requirement says **"require one of"** two controls → SELECT **both** grant controls and choose **"Require one of the selected controls"** (Reason: enforces OR logic; default is AND).
   - IF blocking **legacy authentication** → CREATE a CA policy: **All users + All cloud apps**, Conditions → **Client apps → Legacy authentication clients**, Grant → **Block access** (Reason: the only modern, scalable method to kill legacy protocols).
   - IF the option is smart lockout or password protection for legacy auth blocking → REJECT (Reason: those protect against brute force/weak passwords, not protocol restriction).
4. **SC-300 Golden Rule:** *"One of = tick both + 'Require one of'. Block legacy auth = Client apps condition + Block access."*

---

### 22. Smart Lockout Configuration (Q35)
1. **Exam Scenario:** Protect accounts from brute-force attacks: lock after 10 failed attempts for 5 minutes.
2. **Key Decision Point:** Threshold and duration values (watch the unit!).
3. **Decision Path:**
   - IF the requirement is "X failed attempts" → SET **Lockout threshold = X** (e.g., 10).
   - IF the requirement is "Y minutes lockout" → SET **Lockout duration = Y × 60 seconds** (e.g., 5 minutes = **300 seconds** — the setting is in seconds).
4. **SC-300 Golden Rule:** *"Smart lockout: threshold in attempts, duration in SECONDS. 5 minutes = 300."*

---

### 23. Microsoft Sentinel — Multi-Stage Attack Detection (Q8)
1. **Exam Scenario:** Detect multi-staged attacks correlating events across sources over time.
2. **Key Decision Point:** Detection logic vs. visualization vs. response automation.
3. **Decision Path:**
   - IF configuring **detection logic** for multi-stage attacks → CREATE/CUSTOMIZE **Analytics rules** using **KQL** (Reason: rules define the correlation patterns that generate incidents).
   - IF the option is a **workbook** → REJECT (Reason: visualization/reporting after detection).
   - IF the option is **data connectors** → REJECT (Reason: prerequisite for ingestion, not detection logic).
   - IF the option is a **playbook** → REJECT (Reason: Logic Apps automation for *response* after detection).
4. **SC-300 Golden Rule:** *"Detect = Analytics rules (KQL). Visualize = Workbook. Respond = Playbook."*

---

## 📋 Master Cheat Sheet — Part 1

| Trigger Phrase in Question | Correct Answer |
|---|---|
| "Detect leaked credentials" (hybrid) | PHS + User Risk policy (P2) |
| "Suspicious sign-in / anonymous IP" | Sign-in risk policy → MFA (P2) |
| "MFA by office location" | Named Locations (public IPs) + CA (P1) |
| "Auto-assign licenses by attribute" | Dynamic User security group |
| "Exclude guests from a group" | `user.userType -eq "Member"` |
| "Delegate app access management" | Entitlement Management Catalog (P2) |
| "External partner requests packages" | Connected Organization |
| "Scope admin role to a branch office" | Administrative Unit |
| "JIT / time-bound privileged access" | PIM: Eligible + Expiration (P2) |
| "Sync only certain users" | Customize synchronization options |
| "New forest from acquisition" | Azure AD Connect Cloud Sync |
| "Group missing from role assignment" | Recreate with `isAssignableToRole = true` |
| "Guest invitations failing" | External collaboration settings |
| "CA forbidden; MFA per user" | Per-user MFA: Enforced / Disabled |
| "Consent only to User.Read" | Verified publishers + selected permissions + low-impact classification |
| "Block legacy authentication" | CA: Client apps → Legacy auth clients → Block |
| "Require compliant device OR MAM" | Both controls + "Require one of" |
| "Lockout after 10 tries, 5 min" | Threshold 10, Duration 300 seconds |# SC-300 Decision Trees - Part 2

# SC-300 Decision Trees & Logic Rules — Part 2 (Questions 37–72)

---

## SECTION 1: PRIVILEGED IDENTITY MANAGEMENT (PIM)

### 1.1 Role-Assignable Groups with PIM Eligible Assignments
1. **Exam Scenario:** A group must be able to hold an administrative role, and its members must manually "activate" the role before using its permissions.
2. **Key Decision Point:** Group configuration property + PIM assignment type.
3. **Decision Path:**
   - IF `group must receive an Entra ID role` -> SELECT `role-assignable group` — set **"Azure AD roles can be assigned to the group" = Yes** at creation (Reason: standard security groups cannot hold directory roles).
   - IF `members must perform an activation step` -> SELECT `PIM Eligible assignment` (Reason: Eligible = candidate status; permissions are dormant until activation workflow completes).
   - IF `members should have standing permissions with no activation` -> REJECT `Active assignment` (Reason: Active grants immediate, always-on permissions — violates the "must activate" requirement).
4. **SC-300 Golden Rule:** *Role-assignable group + Eligible assignment = the group holds the role, but nobody wields it until they activate.*

### 1.2 PIM Activation Mechanics: Approval, Duration & MFA
1. **Exam Scenario:** Eligible users request role activation; approvers are defined in a group; activation max duration is configured; MFA-on-activation is required.
2. **Key Decision Point:** How eligibility windows, approval rules, max duration, and MFA claims interact.
3. **Decision Path:**
   - IF `assignment type = Eligible` -> SELECT `user does NOT have the role automatically` (Reason: activation workflow is mandatory; "Eligible" ≠ "Assigned").
   - IF `approver group contains the requester` -> SELECT `only the OTHER members can approve` (Reason: PIM guardrails block self-approval — a member can never approve their own request).
   - IF `activation approved BEFORE eligibility expiration` -> SELECT `active session runs its FULL max duration` (Reason: the eligibility window gates the *start* of activation; it does not truncate an already-instantiated active assignment).
   - IF `activation requested AFTER eligibility expiration` -> REJECT `the request` (Reason: expired eligibility = no activation possible).
   - IF `user already completed MFA at sign-in` -> SELECT `PIM does NOT prompt for MFA again` (Reason: PIM detects the valid MFA claim already present in the session token and satisfies the requirement automatically).
   - IF `requested duration ≤ Activation maximum duration` -> SELECT `custom duration is allowed` (Reason: users can pick any value up to the configured maximum).
4. **SC-300 Golden Rule:** *Eligibility expiration gates the START; max duration caps the LENGTH; an MFA claim in the token satisfies the re-check; nobody approves themselves.*

### 1.3 Which Identities Support Eligible Assignments
1. **Exam Scenario:** Determine which identity types (user, guest, managed identity, service principal) can receive an Eligible PIM assignment.
2. **Key Decision Point:** Can the identity authenticate **interactively**?
3. **Decision Path:**
   - IF `standard user` -> SELECT `Eligible assignment supported` (Reason: can sign in, complete MFA, and provide justification).
   - IF `B2B guest user` -> SELECT `Eligible assignment supported` (Reason: guests perform interactive sign-ins and complete the full activation workflow).
   - IF `managed identity OR service principal` -> REJECT `Eligible assignment` — only **Active** is possible (Reason: programmatic, non-interactive identities cannot click "Activate," answer MFA, or type a justification).
4. **SC-300 Golden Rule:** *If it can't log in and click "Activate," it can't be Eligible — service principals and managed identities are Active-only.*

---

## SECTION 2: ACCESS REVIEWS & GOVERNANCE SCOPE

### 2.1 What Resources Support Access Reviews
1. **Exam Scenario:** Admin must determine which resources can be targeted by access reviews: groups, enterprise app assignments, Azure resource roles, and Entra ID roles.
2. **Key Decision Point:** Resource type eligibility for review creation.
3. **Decision Path:**
   - IF `security / Microsoft 365 group membership` -> SELECT `access review supported` (Reason: group membership review is the core use case).
   - IF `enterprise application assignment` -> SELECT `access review supported` (Reason: reviewing app assignments is a primary, built-in scenario).
   - IF `Azure resource role (e.g., Contributor on a subscription)` -> SELECT `supported via PIM integration` (Reason: PIM for Azure resources enables role assignment reviews).
   - IF `Entra ID directory role` -> SELECT `supported via PIM integration` (Reason: PIM for Entra roles supports assignment reviews).
   - IF `answer option excludes apps or roles` -> REJECT `that option` (Reason: all four resource types are valid — the correct answer is the most inclusive one).
4. **SC-300 Golden Rule:** *Access reviews see everything governance touches: Groups, Apps, Azure roles, and Entra roles — never pick the option that arbitrarily excludes one.*
   - **License note:** Access Reviews require **Entra ID P2**.

---

## SECTION 3: ENTITLEMENT MANAGEMENT

### 3.1 Connected Organizations — Counting Them Correctly
1. **Exam Scenario:** Two external partners need to request an access package: Fabrikam (an Entra tenant with two verified domains) and Litware (a third-party IdP with two distinct domains). What is the *minimum* number of connected organizations?
2. **Key Decision Point:** Connected organizations are created per **partner entity**, not per domain.
3. **Decision Path:**
   - IF `partner is an Entra/Azure AD tenant` -> SELECT `ONE connected organization` (Reason: it automatically encompasses **all verified domains** of that tenant — no per-domain entries needed).
   - IF `partner uses a third-party IdP with multiple domains` -> SELECT `ONE connected organization bundling all domains` (Reason: entitlement management allows multiple domains inside a single connected organization).
   - IF `two partners have different identity models (tenant vs. IdP)` -> REJECT `merging them into one connected organization` (Reason: separate external entities require separate connected organizations).
   - IF `answer suggests one connected org per domain` -> REJECT `that count` (Reason: per-domain creation is unnecessary and inflates the count).
4. **SC-300 Golden Rule:** *One connected organization per logical partner — tenant domains come free, IdP domains get bundled. Count partners, not domains.*

### 3.2 Blocking a Specific Domain Inside a Partner Tenant
1. **Exam Scenario:** Users from `fabrikam.com` may request an access package, but users from `litwareinc.com` (a verified domain of the *same* partner tenant) must be blocked.
2. **Key Decision Point:** Where domain-level blocking is actually configured.
3. **Decision Path:**
   - IF `defining who can request the package` -> SELECT `access package policy scoped to the Connected Organization` (Reason: policies define the request audience).
   - IF `connected org is an Entra tenant` -> SELECT `ALL verified domains are automatically included` (Reason: you cannot selectively remove one verified domain from the connected organization or the policy).
   - IF `a specific domain must be denied` -> SELECT `External collaboration settings → B2B collaboration Deny list` (Reason: this is the only mechanism that blocks a single domain while permitting its sibling domain in the same tenant).
   - IF `answer claims you can exclude a domain within the access package policy` -> REJECT `it` (Reason: the policy has no per-domain exclusion for connected tenant domains).
4. **SC-300 Golden Rule:** *Connected orgs are all-or-nothing per tenant; surgical domain blocking lives in the B2B Deny list under External collaboration settings.*

---

## SECTION 4: CONDITIONAL ACCESS

### 4.1 Mapping Settings to CA Policy Sections
1. **Exam Scenario:** Configure country restrictions, sign-in frequency, and MFA requirements — identify which CA policy block each belongs to.
2. **Key Decision Point:** Conditions vs. Grant controls vs. Session controls.
3. **Decision Path:**
   - IF `restrict by country/IP range (e.g., HighRiskCountries)` -> SELECT `Conditions → Locations` (Reason: named locations are referenced under the Locations condition).
   - IF `limit how long a session stays authenticated (sign-in frequency)` -> SELECT `Access controls → Session controls → Sign-in frequency` (Reason: session lifetime management is a Session control).
   - IF `require MFA / require device compliance / require app-enforced restrictions` -> SELECT `Access controls → Grant` (Reason: MFA is an enforcement (Grant) mechanism, not a session setting).
   - IF `real-time monitoring/blocking of session activities` -> SELECT `Session controls → Conditional Access App Control` (Reason: reverse-proxy session control, not a Grant).
4. **SC-300 Golden Rule:** *Conditions = WHO/WHERE; Grant = WHAT must be proven; Session = HOW LONG and HOW monitored.*

### 4.2 Security Defaults vs. Conditional Access — Mutual Exclusivity
1. **Exam Scenario:** All Conditional Access policy options are grayed out / unavailable in a new tenant.
2. **Key Decision Point:** The state of tenant-wide Security defaults.
3. **Decision Path:**
   - IF `Security defaults are enabled (default for new tenants)` -> SELECT `CA policy creation is blocked` (Reason: the two features are mutually exclusive by design).
   - IF `granular CA control is required` -> SELECT `explicitly disable Security defaults FIRST` (Reason: this unlocks the CA configuration interface).
   - IF `considering user consent settings, MFA registration policy, or password protection as the fix` -> REJECT `all of them` (Reason: none are prerequisites for CA; the MFA registration policy is itself typically built *with* CA after defaults are disabled).
4. **SC-300 Golden Rule:** *Security defaults and Conditional Access never coexist — to go granular, first flip Security defaults OFF.*

### 4.3 Blocking Legacy Authentication
1. **Exam Scenario:** Enforce Modern authentication for Exchange Online; block POP3/IMAP4/old Outlook (Basic auth) clients.
2. **Key Decision Point:** Which tool can block authentication *protocols*.
3. **Decision Path:**
   - IF `block legacy authentication clients` -> SELECT `CA policy → Conditions → Client apps → Legacy authentication clients → Block` (Reason: the Client apps condition targets protocol-level auth, e.g., Basic auth for Exchange).
   - IF `considering an Endpoint Manager compliance policy` -> REJECT `it` (Reason: compliance policies evaluate device posture — OS, encryption — not auth protocols).
   - IF `considering an app control profile` -> REJECT `it` (Reason: it restricts which apps *run on a device*, not how they authenticate to cloud services).
   - IF `considering a Defender for Cloud Apps OAuth policy` -> REJECT `it` (Reason: OAuth policies govern third-party app *permissions*, not first-party legacy protocols).
4. **SC-300 Golden Rule:** *Legacy authentication dies by the Conditional Access "Client apps" condition — device tools and OAuth policies can't kill protocols.*

### 4.4 Conditional Access App Control (CAAC) Onboarding
1. **Exam Scenario:** Enable real-time session monitoring for an app via reverse proxy.
2. **Key Decision Point:** Correct deployment sequence across Entra ID and Defender for Cloud Apps.
3. **Decision Path:**
   - IF `app must be identity-integrated` -> SELECT `publish the app in Entra ID` (Reason: Entra ID acts as the identity provider).
   - IF `traffic must route through the proxy` -> SELECT `CA policy targeting the app with "Use Conditional Access App Control" session control` (Reason: CA is the traffic router).
   - IF `app must be enabled for session control` -> SELECT `onboard under Connected apps in Defender for Cloud Apps` (Reason: official onboarding activates the reverse proxy).
   - IF `monitor/block real-time activities (e.g., sensitive downloads)` -> SELECT `create a Session policy in Defender for Cloud Apps` (Reason: session policies enforce real-time behavior).
4. **SC-300 Golden Rule:** *Conditional Access routes the traffic; Defender for Cloud Apps watches the session.*

### 4.5 Analyzing CA Policy Usage in a SIEM
1. **Exam Scenario:** Export logs to a SIEM to analyze which Conditional Access policies were applied during sign-ins.
2. **Key Decision Point:** Log type (sign-in vs. audit) + export format (JSON vs. CSV).
3. **Decision Path:**
   - IF `need policy *evaluation results* during authentication` -> SELECT `Sign-in logs` (Reason: they capture which CA policies were evaluated and the outcome per attempt).
   - IF `need policy *creation/modification* events` -> REJECT `Audit logs for this purpose` (Reason: audit logs record admin changes to policies, not their application at sign-in).
   - IF `need nested CA evaluation detail preserved` -> SELECT `JSON format` (Reason: JSON retains the hierarchical structure of nested policy data).
   - IF `considering CSV` -> REJECT `it` (Reason: CSV flattens nested CA detail, losing data critical for SIEM analysis).
4. **SC-300 Golden Rule:** *Sign-in logs show policies APPLIED; audit logs show policies CHANGED — and always export JSON to keep the nesting.*

---

## SECTION 5: IDENTITY PROTECTION & MFA

### 5.1 Sign-in Risk Remediation
1. **Exam Scenario:** Automatically remediate risky sign-ins in real time **without blocking** access.
2. **Key Decision Point:** Which control remediates sign-in risk.
3. **Decision Path:**
   - IF `sign-in risk detected + must not block` -> SELECT `require MFA (sign-in risk policy)` (Reason: the MFA challenge proves identity and remediates the risk; access continues on success).
   - IF `prerequisite for the policy to work` -> SELECT `users registered for MFA` (Reason: unregistered users cannot satisfy the challenge).
   - IF `considering access reviews` -> REJECT `it` (Reason: periodic governance, not real-time remediation).
   - IF `considering Password Protection` -> REJECT `it` (Reason: preventative control against weak passwords, not a risk remediation).
   - IF `considering SSPR as the fix for SIGN-IN risk` -> REJECT `it` (Reason: SSPR is the prerequisite for the **user** risk policy's password change, not for sign-in risk MFA).
4. **SC-300 Golden Rule:** *Sign-in risk = MFA challenge; User risk = password change (needs SSPR).*
   - **License note:** Identity Protection risk policies require **Entra ID P2**.

### 5.2 Leaked Credentials — Risk Classification & Mitigation
1. **Exam Scenario:** Credentials exposed on the dark web; risk must be mitigated while the user retains app access.
2. **Key Decision Point:** Risk classification (user vs. sign-in) + remediation control.
3. **Decision Path:**
   - IF `username/password pair exposed publicly` -> SELECT `User risk` (Reason: the identity itself is compromised regardless of any current session — not a sign-in risk).
   - IF `mitigate without blocking productivity` -> SELECT `CA user risk policy → require secure password change (SSPR-driven)` (Reason: completing the password change fully remediates leaked-credential risk while access continues).
   - IF `considering "sign-in risk"` -> REJECT `it` (Reason: sign-in risk is per-attempt anomaly; leaked credentials taint the user account itself).
4. **SC-300 Golden Rule:** *Leaked credentials = USER risk; cure it with a secure password change, not a block.*

### 5.3 MFA Account Lockout vs. Smart Lockout
1. **Exam Scenario:** Interpret the MFA account lockout settings page (denials, auto-unblock minutes).
2. **Key Decision Point:** Which lockout mechanism governs which failure type.
3. **Decision Path:**
   - IF `wrong password entered` -> SELECT `Entra ID Smart Lockout applies` (Reason: password failures are primary-authentication events, outside MFA lockout scope).
   - IF `wrong MFA verification code / denied second factor` -> SELECT `MFA account lockout settings apply` (Reason: these settings govern only secondary-factor denials).
   - IF `reading "Minutes until account is automatically unblocked"` -> SELECT `the configured value (e.g., 30) is the auto-unblock time` (Reason: the setting is literal).
   - IF `question lists password/email as the trigger for MFA lockout` -> REJECT `it` (Reason: only MFA factors — e.g., Authenticator app codes — trigger MFA lockout).
4. **SC-300 Golden Rule:** *Smart Lockout owns the password; MFA lockout owns the second factor — never the twain shall meet.*

### 5.4 Per-User MFA State Transitions
1. **Exam Scenario:** Predict per-user MFA states (Disabled / Enabled / Enforced) after sign-ins and registration events.
2. **Key Decision Point:** Current state + whether a sign-in triggers registration.
3. **Decision Path:**
   - IF `state = Disabled and user signs in` -> SELECT `state remains Disabled` (Reason: no registration is triggered for Disabled users).
   - IF `state = Enabled and user signs in` -> SELECT `user must register → state auto-transitions to Enforced` (Reason: Enabled prompts registration at next sign-in; completion promotes to Enforced).
   - IF `state = Enforced` -> SELECT `state remains Enforced` (Reason: inactivity or "Remember MFA on trusted device" settings never revert the directory status).
   - IF `"Remember MFA on trusted device (X days)" is mentioned` -> REJECT `any answer claiming it changes the per-user state` (Reason: it only controls prompt frequency per device, not the administrative state).
4. **SC-300 Golden Rule:** *Enabled + sign-in = Enforced. Disabled stays Disabled. Enforced stays Enforced. Trusted-device memory never touches the state.*

---

## SECTION 6: ENTERPRISE APPLICATIONS & MY APPS

### 6.1 My Apps Collections
1. **Exam Scenario:** Present a custom set of apps (e.g., LinkedIn + Box) as a dedicated tab for a specific group (sg-Operations).
2. **Key Decision Point:** Collections vs. direct app assignment.
3. **Decision Path:**
   - IF `logical grouping of apps as a portal tab` -> SELECT `create a My Apps collection, add the apps, assign the group` (Reason: collections render as separate tabs for assigned users).
   - IF `granting access to the app itself` -> SELECT `Users and groups assignment on the enterprise app` (Reason: **collection membership does NOT grant app permissions**).
4. **SC-300 Golden Rule:** *Collections organize visibility; app assignments grant access — one never implies the other.*

### 6.2 Enterprise App Properties: "User assignment required" & "Visible to users"
1. **Exam Scenario:** Predict who can access and see an app based on these two toggles; troubleshoot a group that cannot see an app.
2. **Key Decision Point:** The interaction of assignment gating and visibility.
3. **Decision Path:**
   - IF `User assignment required = No` -> SELECT `ALL tenant users can sign in and access the app` (Reason: explicit assignment gating is bypassed).
   - IF `Visible to users = Yes (and access is tenant-wide)` -> SELECT `app icon appears in My Apps + M365 launcher for everyone with access` (Reason: visibility follows access rights).
   - IF `User assignment required = Yes and group is NOT in the assignment list` -> SELECT `group members cannot see or access the app` (Reason: assignment is the gate).
   - IF `group was added to a My Apps collection but not to the app` -> SELECT `still no access/visibility` (Reason: collections don't confer permissions).
   - IF `fix requires making the app visible to a specific group only` -> SELECT `add the group under Users and groups on the app` — REJECT `setting User assignment required = No` (Reason: that exposes the app to the entire tenant, violating least privilege).
   - IF `Visible to users = No` -> SELECT `app hidden from My Apps and M365 launcher for ALL users, even assigned ones` (Reason: the toggle hides it universally).
4. **SC-300 Golden Rule:** *Assignment required = the gate; Visible to users = the signpost; a hidden gate blocks everyone, an open gate needs no key.*

### 6.3 Self-Service Application Access Requests
1. **Exam Scenario:** Company policy requires users to request access to applications, with approval before access is granted.
2. **Key Decision Point:** Which enterprise app feature implements the request/approval workflow.
3. **Decision Path:**
   - IF `users must request access + optional approver` -> SELECT `Self-service settings on the enterprise app` (Reason: enables the My Apps request workflow with an approval step that assigns the user on approval).
   - IF `considering Provisioning` -> REJECT `it` (Reason: provisioning maintains accounts *after* access is granted; it doesn't handle requests).
   - IF `considering Roles and administrators` -> REJECT `it` (Reason: defines app-specific roles/admin delegation, not access requests).
   - IF `considering Application Proxy` -> REJECT `it` (Reason: on-prem connectivity publishing, unrelated to access requests).
4. **SC-300 Golden Rule:** *Self-service settings = the request-and-approve front door; everything else is plumbing behind it.*

### 6.4 Group Types Eligible for App Assignment
1. **Exam Scenario:** Which group type can be assigned to an enterprise application: security, distribution, Microsoft 365, or mail-enabled security?
2. **Key Decision Point:** Is the group a fully native, security-enabled principal?
3. **Decision Path:**
   - IF `standard Security group` -> SELECT `fully supported for app assignment` (Reason: native security principal, built for this purpose).
   - IF `Distribution group` -> REJECT `it` (Reason: securityEnabled = false; email routing only).
   - IF `Microsoft 365 group` -> REJECT `it` (Reason: not security-enabled by default; collaboration-focused).
   - IF `Mail-enabled security group` -> REJECT `it` (Reason: managed via Exchange Online, read-only in Microsoft Graph, lacks full native Entra assignment capability).
4. **SC-300 Golden Rule:** *For app assignment, only the plain, native Security group is the safe exam answer — everything else carries a caveat.*

### 6.5 App-Only Access to Microsoft Graph (Client Credentials)
1. **Exam Scenario:** An autonomous web service must read directory data via Graph with **no signed-in user**.
2. **Key Decision Point:** Permission type + consent requirement.
3. **Decision Path:**
   - IF `no user context (daemon/service)` -> SELECT `OAuth 2.0 client credentials flow + Application permissions (e.g., Directory.Read.All)` (Reason: the app authenticates as itself; Delegated permissions require a signed-in user).
   - IF `Application permissions are used` -> SELECT `tenant administrator MUST grant admin consent` (Reason: broad directory access without a user always requires admin authorization).
   - IF `considering Delegated permissions` -> REJECT `them` (Reason: they act on behalf of a user, which doesn't exist in this scenario).
4. **SC-300 Golden Rule:** *No user = Application permissions + admin consent. Delegated is for "on behalf of," Application is for "as itself."*

---

## SECTION 7: HYBRID IDENTITY

### 7.1 Pass-through Authentication (PTA) Agent Placement
1. **Exam Scenario:** Deploy an additional PTA agent for high availability; Domain Controllers are forbidden internet access; AAD Connect server already hosts the primary agent.
2. **Key Decision Point:** Internet reachability (outbound 443) + separation from the existing agent.
3. **Decision Path:**
   - IF `candidate server is a Domain Controller` -> REJECT `it` (Reason: security policy forbids DC internet access; PTA agents require outbound 443).
   - IF `candidate server already runs AAD Connect + the primary agent` -> REJECT `it` (Reason: same-server install provides zero redundancy against a full server failure).
   - IF `candidate is a separate member server with internet access` -> SELECT `install the additional PTA agent there` (Reason: satisfies both HA and the security policy).
4. **SC-300 Golden Rule:** *PTA agents need outbound 443 and their own server — never a DC, never the AAD Connect box twice.*

### 7.2 PTA Outage Behavior — Is There Automatic PHS Fallback?
1. **Exam Scenario:** PTA is the primary sign-in method (PHS also configured). On-premises internet connectivity is lost. Which users can still sign in?
2. **Key Decision Point:** Does PTA auto-failover to PHS? Which users bypass on-prem entirely?
3. **Decision Path:**
   - IF `user is cloud-only (Directory synced = No)` -> SELECT `can sign in` (Reason: authentication happens entirely in Entra ID; unaffected by on-prem outage).
   - IF `user is synced and PTA is primary` -> SELECT `CANNOT sign in during the outage` (Reason: Entra ID cannot reach the unreachable on-prem PTA agents).
   - IF `assuming PHS automatically takes over` -> REJECT `that assumption` (Reason: PTA does **NOT** automatically fail over to PHS — an administrator must **manually** switch the sign-in method).
   - IF `answer claims all users can sign in` -> REJECT `it` (Reason: it falsely presumes dynamic PHS failover).
4. **SC-300 Golden Rule:** *PHS is a parachute, not an autopilot — PTA outages require a MANUAL switch; cloud-only users never notice the storm.*

### 7.3 Azure AD Connect High Availability — Staging Mode
1. **Exam Scenario:** Deploy a standby sync server that minimizes failover time when the primary AAD Connect server fails.
2. **Key Decision Point:** Staging Mode configuration + pre-population steps.
3. **Decision Path:**
   - IF `standby server required` -> SELECT `install AAD Connect on Server2 with Staging Mode enabled` (Reason: it processes data but exports nothing — a true warm standby).
   - IF `pre-populating the standby` -> SELECT `run full import + delta synchronization for ALL connectors` (Reason: keeps the metaverse and connector spaces current; pending exports are pre-computed).
   - IF `primary server fails` -> SELECT `disable Staging Mode on Server2 → only a final export is needed` (Reason: drastically minimizes failover time).
   - IF `answer suggests a second active sync server without staging` -> REJECT `it` (Reason: two active exporters cause conflicts; staging exists precisely for this).
4. **SC-300 Golden Rule:** *Staging Mode = warm standby: import + delta sync in advance; on failure, flip staging off and export.*

### 7.4 Attribute Authority for Synced vs. Cloud-Only Users
1. **Exam Scenario:** Determine which users' attributes (Job title, Usage location) can be edited in Entra ID.
2. **Key Decision Point:** Source of authority (on-prem AD vs. Entra ID) per attribute.
3. **Decision Path:**
   - IF `user is directory-synced (Directory synced = Yes)` -> SELECT `core attributes (Job title, Department, Manager) are READ-ONLY in the cloud` (Reason: on-prem AD is the source of authority via Entra Connect).
   - IF `user is cloud-only (member or guest, synced = No)` -> SELECT `attributes freely editable in Entra ID` (Reason: the cloud is the authority).
   - IF `attribute is Usage location` -> SELECT `editable in Entra ID for ALL users — synced, member, or guest` (Reason: it's an Entra-specific licensing-compliance attribute not mapped from standard AD schemas by default).
   - IF `answer claims Job title is editable for a synced user` -> REJECT `it` (Reason: violates source-of-authority).
4. **SC-300 Golden Rule:** *On-premises owns the synced profile; the cloud owns Usage location — always.*

---

## SECTION 8: LICENSING, B2B & GROUP-BASED LICENSING

### 8.1 Entra ID Sign-in Log Retention by License Tier
1. **Exam Scenario:** Determine the default retention period for sign-in logs.
2. **Key Decision Point:** Tenant license tier.
3. **Decision Path:**
   - IF `Entra ID Free` -> SELECT `7 days retention` (Reason: free-tier baseline).
   - IF `Entra ID P1 or P2` -> SELECT `30 days retention` (Reason: premium tiers both retain 30 days — P1 vs. P2 makes no difference here).
   - IF `longer retention required (e.g., 90/365 days)` -> SELECT `archive to Azure Monitor Log Analytics / Azure Storage` (Reason: default retention cannot be extended in-place).
   - IF `options include 14, 90, or 365 days as "default"` -> REJECT `all of them` (Reason: none are default retention periods; 90/365 require external archiving).
4. **SC-300 Golden Rule:** *Free = 7, Premium (P1/P2) = 30; anything longer lives in Azure Monitor, not the tenant.*
   - **Related thresholds to memorize:** Conditional Access → **P1**; PIM, Identity Protection, Access Reviews, Entitlement Management → **P2**.

### 8.2 B2B External Identities Pricing Model
1. **Exam Scenario:** Share SharePoint Online with users from another tenant at no extra license cost; enable MAU-based billing.
2. **Key Decision Point:** B2B collaboration + MAU billing activation.
3. **Decision Path:**
   - IF `external users need access to M365/SharePoint resources` -> SELECT `invite as B2B guest users via Entra External Identities` (Reason: SharePoint natively supports external sharing with guests).
   - IF `cost must be zero/minimal` -> SELECT `MAU billing model — first 50,000 Monthly Active Users are FREE` (Reason: usage-based pricing replaces per-license costs).
   - IF `enabling MAU billing` -> SELECT `link an Azure subscription to the Entra tenant` (Reason: the subscription provides the billing mechanism; linkage auto-enables MAU billing).
   - IF `considering Entra Connect syncing to multiple tenants` -> REJECT `it` (Reason: one sync server maps 1:1 to one tenant).
   - IF `considering Application Proxy for cross-tenant SharePoint` -> REJECT `it` (Reason: it publishes on-prem web apps, not cloud services).
   - IF `considering access reviews / terms of use / user flows for billing setup` -> REJECT `all` (Reason: none establish the billing relationship).
4. **SC-300 Golden Rule:** *B2B guests ride the MAU meter — first 50K free; flip the meter by linking an Azure subscription.*

### 8.3 Group-Based Licensing & Nested Groups
1. **Exam Scenario:** An E5 license is assigned to Group1, which directly contains User1, User3, and nested Group2/Group4 (whose members are User2/User5). How many licenses are consumed?
2. **Key Decision Point:** Does group-based licensing support nesting?
3. **Decision Path:**
   - IF `user is a DIRECT member of the licensed group` -> SELECT `license is provisioned` (Reason: the licensing engine evaluates only immediate members).
   - IF `user is inside a NESTED group` -> SELECT `license is NOT inherited` (Reason: group-based licensing does not traverse nested groups — nested members are ignored).
   - IF `answer counts nested users` -> REJECT `it` (Reason: it falsely assumes nested inheritance).
4. **SC-300 Golden Rule:** *Group licensing is FLAT — direct members only; nesting is invisible to the license engine.*

---

## SECTION 9: SSPR, TENANT SETTINGS & LOG STREAMING

### 9.1 Valid SSPR Authentication Methods
1. **Exam Scenario:** Select valid methods users can provide to reset their password.
2. **Key Decision Point:** The official SSPR method list.
3. **Decision Path:**
   - IF `method is Mobile app notification (push)` -> SELECT `valid` (Reason: standard configurable SSPR method).
   - IF `method is Mobile app code (OTP from authenticator)` -> SELECT `valid` (Reason: standard configurable SSPR method).
   - IF `method is Mobile phone / Office phone` -> SELECT `valid` (Reason: SMS and voice call are supported).
   - IF `method is "email to an address in your organization"` -> REJECT `it` (Reason: corporate email is invalid — a locked-out user can't read it; only an *external* alternate email works).
   - IF `method name doesn't match the official list` -> REJECT `it` (Reason: e.g., "home phone" is not a listed method).
4. **SC-300 Golden Rule:** *SSPR methods: app push, app code, mobile phone, office phone, external email, security questions — NEVER corporate email.*

### 9.2 Blocking Self-Service Sign-Up
1. **Exam Scenario:** Prevent users from creating new accounts in the tenant via self-service sign-up for Microsoft 365 services.
2. **Key Decision Point:** Tenant-level setting + correct cmdlet/parameter.
3. **Decision Path:**
   - IF `block ad-hoc/self-service sign-ups` -> SELECT `Set-MsolCompanySettings -AllowAdHocSubscriptions $false` (Reason: this is the tenant-wide company-level control).
   - IF `considering Set-MsolDomainFederationSettings / Update-MsolFederatedDomain` -> REJECT `them` (Reason: federation certificate/metadata management, unrelated to sign-up).
   - IF `considering Set-MsolDomain` -> REJECT `it` (Reason: switches managed/federated auth type per domain, not sign-up policy).
4. **SC-300 Golden Rule:** *Ad-hoc subscriptions OFF = self-service sign-up dead — CompanySettings, not DomainSettings.*

### 9.3 Streaming Entra Logs to Azure Monitor
1. **Exam Scenario:** View Entra ID audit/sign-in logs inside Azure Monitor for querying and alerting.
2. **Key Decision Point:** The routing mechanism into Log Analytics.
3. **Decision Path:**
   - IF `logs must appear in Azure Monitor` -> SELECT `configure Diagnostic settings on the tenant → select log categories (AuditLogs, SignInLogs) → destination Log Analytics workspace` (Reason: diagnostic settings are the foundational pipe for platform logs).
   - IF `considering Get-AzureADAuditDirectoryLogs` -> REJECT `it` (Reason: one-time PowerShell query, not continuous streaming).
   - IF `considering creating a workbook first` -> REJECT `it` (Reason: workbooks only visualize data already in the workspace — the pipe must come first).
   - IF `considering Set-AzureADTenantDetail` -> REJECT `it` (Reason: tenant properties like technical contact; unrelated to logs).
4. **SC-300 Golden Rule:** *Diagnostic settings are the pipe; Log Analytics is the tank; workbooks are just the window.*

---

## QUICK-REFERENCE: DISTRACTOR PATTERNS IN PART 2

| Distractor Pattern | Where It Appears | Why It's Always Wrong |
|---|---|---|
| Per-domain connected organizations | Entitlement Management | Connected orgs are per partner entity; tenant domains auto-include |
| Audit logs for CA policy *usage* | Log analysis | Audit = policy changes; Sign-in = policy application |
| CSV for nested log data | SIEM export | CSV flattens hierarchical CA detail |
| Automatic PHS failover | PTA outage | Fallback to PHS is manual only |
| Nested groups in licensing/app assignment | Licensing & app access | Neither feature traverses nesting |
| Managed identity as Eligible | PIM | Non-interactive identities = Active only |
| Self-approval in PIM | Activation workflows | Approvers can never approve themselves |
| Security defaults + CA coexisting | CA grayed out | Mutually exclusive — disable defaults first |
| Collection membership = app access | My Apps | Collections organize; assignments authorize |
| Corporate email for SSPR | SSPR methods | Only external alternate email is valid |# SC-300 Decision Trees - Part 3

# SC-300 Decision Trees & Logic Rules — Part 3 (Questions 73–108)

> **Scope:** This decision map covers group nesting mechanics, sign-in methods (Seamless SSO / PTA / PHS), B2B guest access, MFA configuration, Conditional Access, Identity Protection, Administrative Units, and emergency access accounts.

---

## 1. Group Nesting & Membership Rules (Security vs. Microsoft 365 Groups)

1. **Exam Scenario:** You must determine which users/groups can be added as members of a target group (e.g., GroupA is a Security group; GroupB is a Microsoft 365 group; a mail-enabled security group requires licensed members).
2. **Key Decision Point:** The **type of the TARGET (parent) group** — Security, Mail-enabled Security, or Microsoft 365 — and the **license/membership type** of the candidate member.
3. **Decision Path:**
   - IF target is a **Security Group (Assigned or Dynamic)** -> MAY contain **users, devices, and other Security groups** (nesting allowed, regardless of child's assigned/dynamic state).
     - IF candidate is a **Microsoft 365 group** -> REJECT it (Reason: M365 groups can NEVER be nested inside any group type).
   - IF target is a **Microsoft 365 Group** -> MAY contain **only direct user accounts (and guests)**.
     - IF candidate is any group (Security or M365) -> REJECT (Reason: Microsoft 365 groups support **zero nesting**).
   - IF target is a **Mail-enabled Security Group** -> members must be **mail-ready and licensed** (e.g., E3/E5).
     - IF user is **unlicensed** -> REJECT (Reason: cannot be resolved as a mail-active member).
     - IF candidate is a nested group -> REJECT (Reason: nesting is not supported for mail-enabled delivery workflows).
4. **SC-300 Golden Rule:** *Security groups nest; Microsoft 365 groups never nest; mail-enabled groups require licensed, mail-ready members.*

---

## 2. Seamless Single Sign-On (SSO) Browser Configuration

1. **Exam Scenario:** Users on domain-joined, corporate-network devices must be signed in to cloud resources automatically without credential prompts in the browser.
2. **Key Decision Point:** Which component enables Kerberos-based automatic sign-in for browsers.
3. **Decision Path:**
   - IF goal is **automatic browser sign-in via Seamless SSO** -> SELECT adding `https://autologon.microsoftazuread-sso.com` to the **Local Intranet zone** via Group Policy (Reason: browsers only auto-send Kerberos tickets to intranet-zone URLs).
   - IF question mentions **Authentication Agent** -> REJECT (Reason: that belongs to **Pass-through Authentication (PTA)**, a different sign-in method).
   - IF question mentions **Enterprise State Roaming** -> REJECT (Reason: syncs user settings/app data, unrelated to SSO).
   - IF question mentions **Sign-in options (Windows Hello/PIN)** -> REJECT (Reason: local device authentication, not cloud SSO).
4. **SC-300 Golden Rule:** *Seamless SSO = Kerberos + Local Intranet zone; the Authentication Agent = PTA, not Seamless SSO.*

---

## 3. Custom Domain Validation & Primary Domain Sequence

1. **Exam Scenario:** A company wants `contoso.com` to be the default UPN suffix for all newly created users in Microsoft 365.
2. **Key Decision Point:** Correct **ordering** of domain onboarding steps.
3. **Decision Path:**
   - Step 1: **Register/Add** the domain in the Microsoft 365 admin center.
   - Step 2: **Create a TXT record** at the DNS hosting provider (proof of ownership).
   - Step 3: **Verify** the domain in the admin center after DNS propagation.
   - Step 4: **Set the domain as primary/default** so new users receive `user@contoso.com`.
   - IF a question option says **"Delete the .onmicrosoft.com domain"** -> REJECT (Reason: the initial tenant domain is **permanently required** for backend routing).
4. **SC-300 Golden Rule:** *Add → TXT → Verify → Set as Primary. The .onmicrosoft.com domain can never be deleted.*

---

## 4. Redirecting Azure Monitor Alerts (Role Assignment vs. Action Groups)

1. **Exam Scenario:** A new security administrator must now **receive** email alerts for failed sign-ins; a series of Yes/No solutions is proposed (assign a role, create a DCR, modify an alert rule, etc.).
2. **Key Decision Point:** Where alert **notification recipients** are actually defined — the **Action Group** attached to the alert rule.
3. **Decision Path:**
   - IF solution = **assign an admin role** (e.g., Insights administrator) -> REJECT (Reason: grants *view/manage* permissions; does **not** change alert recipients — Azure AD roles and Azure Monitor are independent).
   - IF solution = **create a Data Collection Rule (DCR)** -> REJECT (Reason: DCRs govern telemetry *ingestion/transform/destination*, not alerting or recipients).
   - IF solution = **modify the alert rule's Action Group** (remove old email, add new admin's email) -> SELECT (Reason: Action Groups are the only store of notification preferences — email, SMS, push).
4. **SC-300 Golden Rule:** *Alerts go where the Action Group points — never where the RBAC role points. DCR = data pipeline; Action Group = notification target.*

---

## 5. MFA Fraud Alert vs. Block/Unblock vs. Account Lockout

1. **Exam Scenario:** Users must be **automatically blocked** when they report an unexpected MFA prompt as fraudulent via the Microsoft Authenticator app.
2. **Key Decision Point:** Which MFA service setting matches the **trigger** (user-reported fraud vs. denied attempts vs. manual admin action).
3. **Decision Path:**
   - IF trigger = **user reports fraud in Authenticator** -> SELECT **Fraud alert** with *"Automatically block users who report fraud"* enabled (Reason: purpose-built for user-initiated fraud reports; immediately blocks the account, mitigating password-compromise attacks).
   - IF trigger = **too many denied MFA attempts** -> SELECT **MFA Account Lockout** (Reason: protects against brute-force; triggers on *count of denied attempts*, not user reports).
   - IF solution = **Block/unblock users** -> REJECT for automatic scenarios (Reason: this is a **manual** administrative action only).
4. **SC-300 Golden Rule:** *Fraud report → Fraud Alert; brute force → Account Lockout; manual ban → Block/unblock. Never interchange them.*

---

## 6. Group-Based Licensing Eligibility

1. **Exam Scenario:** Identify which groups can receive a license (e.g., Microsoft 365 E5) directly via group-based licensing, across Security/M365 and Assigned/Dynamic/Device membership types.
2. **Key Decision Point:** Group **type** (Security or M365) × **membership type** (Assigned, Dynamic **User**, Dynamic **Device**).
3. **Decision Path:**
   - IF group = Security **Assigned** -> SELECT (Reason: fully supported).
   - IF group = Security **Dynamic User** -> SELECT (Reason: dynamic *user* groups are supported).
   - IF group = M365 **Assigned** or M365 **Dynamic User** -> SELECT (Reason: M365 groups natively support group-based licensing).
   - IF group = **Dynamic Device** -> REJECT (Reason: licenses attach to *users*; device-based dynamic groups cannot drive user license assignment).
   - IF group contains **nested groups** -> STILL SELECT the parent group (Reason: licensing is valid on the parent; only **first-level immediate user members** inherit licenses).
4. **SC-300 Golden Rule:** *Any Security or M365 group with Assigned or Dynamic-USER membership is licensable; Dynamic-DEVICE groups never are. Nested groups block inheritance beyond level 1.*

---

## 7. B2B Guest Authentication — Email One-Time Passcode (OTP)

1. **Exam Scenario:** Determine which external users will authenticate with an email one-time passcode when accessing shared SharePoint content.
2. **Key Decision Point:** The guest's **identity state**: brand-new (never redeemed), existing/redeemed guest, or internal user.
3. **Decision Path:**
   - IF user is a **new external guest** with **no Microsoft account, no Entra account, no federation** -> SELECT **Email OTP** (Reason: OTP is the fallback for first-time invitation redemption).
   - IF user is an **already-redeemed existing guest** -> REJECT OTP (Reason: they authenticate with their established credentials).
   - IF user is an **internal member** of the tenant domain -> REJECT OTP (Reason: internal users authenticate natively with company credentials).
4. **SC-300 Golden Rule:** *Email OTP applies only to NEW, unredeemed guests with no other identity — never to existing guests or internal members.*

---

## 8. Bulk License Removal (Least Administrative Effort)

1. **Exam Scenario:** Remove a license (e.g., E3) from 2,500 users with the **least administrative effort**.
2. **Key Decision Point:** GUI bulk operation vs. per-object scripting vs. wrong tool.
3. **Decision Path:**
   - IF requirement = bulk action with least effort -> SELECT **Licenses blade** → select product → select all users → **Remove license** (Reason: single GUI bulk operation).
   - IF option = `Set-AzureADUser` cmdlet -> REJECT (Reason: modifies **one user at a time**; requires a script for 2,500 users).
   - IF option = Identity Governance blade -> REJECT (Reason: lifecycle/reviews/entitlement, not one-time bulk license ops).
   - IF option = `Set-WindowsProductKey` -> REJECT (Reason: OS product keys, unrelated).
4. **SC-300 Golden Rule:** *"Least effort" + bulk license change = Licenses blade bulk select; per-user cmdlets are always more effort.*

---

## 9. External Collaboration Domain Restrictions (Allowlist Timing)

1. **Exam Scenario:** Tenant allows invitations only to specific domains (e.g., outlook.com). Determine whether a pending invitation can be accepted, whether an already-redeemed guest retains access after the allowlist changes, and whether a new invitation from a blocked domain succeeds.
2. **Key Decision Point:** **When** domain restrictions are evaluated (invitation send + redemption time only — never retroactively).
3. **Decision Path:**
   - IF guest's domain **IS on the allowlist** at redemption time -> guest **CAN accept** the invitation.
   - IF guest **already redeemed** the invitation **before** the domain was removed from the allowlist -> guest **RETAINS access** (Reason: restrictions are not retroactive to redeemed guests).
   - IF guest's domain is **NOT on the allowlist** -> invitation is **blocked at send time**; the guest can never accept (Reason: directory-level check occurs before the invite is even delivered).
4. **SC-300 Golden Rule:** *Domain allowlists gate the INVITATION (send + redemption), not ongoing access of already-redeemed guests.*

---

## 10. B2B Bulk Invitation CSV Requirements

1. **Exam Scenario:** Complete the mandatory fields of the CSV template used for bulk-inviting B2B collaboration users.
2. **Key Decision Point:** Which CSV fields are **required** by the bulk invite template.
3. **Decision Path:**
   - IF field = **Invited user's email address** -> SELECT (Reason: the guest's external identity).
   - IF field = **Redirect URL** -> SELECT (Reason: destination after invitation acceptance, e.g., My Apps portal).
   - IF field = **username/UPN** -> REJECT (Reason: auto-generated from the email).
   - IF field = **password** -> REJECT (Reason: guests authenticate via their **home** IdP; inviting tenant never sets passwords).
   - IF field = **shared key** -> REJECT (Reason: service-principal credential concept, irrelevant to B2B).
4. **SC-300 Golden Rule:** *Bulk invite CSV = Email + Redirect URL. Nothing else — guests bring their own credentials.*

---

## 11. Internal Admin Takeover of an Unmanaged Tenant

1. **Exam Scenario:** Employees self-signed up to an unmanaged `contoso.com` tenant; an administrator must take over as Global Administrator.
2. **Key Decision Point:** The forced **DNS proof-of-ownership** sequence.
3. **Decision Path:**
   - Step 1: Perform **self-service sign-up** with an email matching the unmanaged domain.
   - Step 2: Sign in to the **Microsoft 365 admin center**; the unmanaged tenant prompts **"Become the admin."**
   - Step 3: Create the required **TXT record** in the `contoso.com` DNS zone.
   - Step 4: After DNS verification -> account is elevated to **Global Administrator**.
4. **SC-300 Golden Rule:** *Self-sign-up → "Become admin" prompt → TXT record → Global Admin. DNS proof is always the takeover gate.*

---

## 12. Disabled-Account Sync Latency: PHS vs. PTA vs. Other Controls

1. **Exam Scenario:** When an on-premises account is disabled, the disabled state takes ~30 minutes (default Entra Connect sync interval) to reach the cloud, so sign-ins still succeed during the gap. Evaluate proposed fixes.
2. **Key Decision Point:** Whether the proposed mechanism performs **real-time** on-premises validation or merely alters sync/latency behavior.
3. **Decision Path:**
   - IF solution = **Pass-through Authentication (PTA)** -> SELECT (Reason: PTA validates credentials **live against on-premises AD** at each sign-in — a disabled account fails instantly, no sync delay).
   - IF solution = **Password writeback** -> REJECT (Reason: syncs passwords *cloud → on-prem* for SSPR; does not affect account **enabled/disabled** attribute sync).
   - IF solution = **Conditional Access policy** -> REJECT (Reason: CA evaluates the user state **as known to Entra ID** after first-factor success; it cannot force real-time on-prem checks).
   - IF question = PHS default context -> note the **30-minute default sync interval** as the root cause.
4. **SC-300 Golden Rule:** *Real-time revocation = PTA. PHS = up to 30 min stale. Password writeback and Conditional Access cannot fix sync latency.*

---

## 13. Administrative Units — Object Scope vs. Membership Scope

1. **Exam Scenario:** Admin1 has User Administrator scoped to Department1 AU. Group2 is placed in the AU, but its members (User3, User4) are not. Can Admin1 reset their passwords? Manage the group? What can a directory-scoped Admin2 do?
2. **Key Decision Point:** **AU scope** applies to objects *placed in* the AU — placing a **group** in an AU does **not** place its **members** there.
3. **Decision Path:**
   - IF role is AU-scoped AND target object (group) **is in the AU** -> role holder CAN manage the **group object and its membership** (e.g., add User1 to Group2 → **Yes**).
   - IF target user is only a **member of a group inside the AU** but not directly in the AU's Users list -> role holder CANNOT manage that user (reset passwords of User3/User4 → **No**).
     - Fix: **add the user objects directly to the AU**.
   - IF role is scoped to the **entire directory** -> role holder manages ALL non-admin users tenant-wide (reset User1's password → **Yes**).
4. **SC-300 Golden Rule:** *AU = object container, not membership propagator. Group in AU ≠ users in AU. Directory scope beats AU scope.*

---

## 14. Password Reset Role Selection (Least Privilege)

1. **Exam Scenario:** A security admin (SecAdmin1) must reset passwords and force reauthentication (revoke refresh tokens) for **non-administrative users only**.
2. **Key Decision Point:** Match the role's **scope of resettable users** to the requirement without over-privileging.
3. **Decision Path:**
   - IF target = non-admin users only, plus revoke sessions -> SELECT **Helpdesk administrator** (Reason: resets passwords + invalidates refresh tokens for non-admins; exact least-privilege fit).
   - IF option = **Authentication administrator** -> REJECT (Reason: can also reset credentials of *other admins* — exceeds need).
   - IF option = **Privileged Authentication administrator** -> REJECT (Reason: can reset even **Global Administrators** — far beyond need).
   - IF current role = **Security administrator** -> note it CANNOT reset passwords (monitoring/Identity Protection only).
   - IF option = **Security operator** -> REJECT (Reason: read-only alert monitoring).
4. **SC-300 Golden Rule:** *Helpdesk admin = passwords for non-admins + revoke sessions. Any "Authentication" variant = over-privileged for non-admin resets.*

---

## 15. Entra Password Protection — Fuzzy Matching & Normalization

1. **Exam Scenario:** A custom banned-password list contains words (e.g., "Litware," "Contoso," "Project"). Determine which "leet-speak" passwords are blocked.
2. **Key Decision Point:** The **normalization algorithm** applied before matching.
3. **Decision Path:**
   - Normalize candidate: lowercase everything → substitute leet characters (`0→o`, `1→i/l`, `@→a`, etc.).
   - IF normalized string **contains** any banned substring -> **BLOCKED** (substring matching, not exact match).
     - `Pr0jectlitw@re` → `projectlitware` → contains "project" + "litware" → blocked.
     - `T@ilw1nd` → `tailwind` → blocked.
     - `C0nt0s0` → `contoso` → blocked.
   - IF no normalized substring matches the list -> allowed.
4. **SC-300 Golden Rule:** *Normalize first (lowercase + de-leet), then substring-match. If the banned word hides anywhere inside, it's blocked.*

---

## 16. Authentication Method Selection Under Environmental Constraints

1. **Exam Scenario:** (a) Call-center users share computers, cannot use mobile phones, and lack biometric hardware — choose an MFA method. (b) A user's phone has no signal but the laptop has internet — which method works for a legacy/non-modern-auth app?
2. **Key Decision Point:** Match the method to the **available hardware/connectivity** and the **application type**.
3. **Decision Path:**
   - **Scenario A (shared PCs, no phones, no biometrics):**
     - IF phones prohibited AND biometrics unavailable -> SELECT **FIDO2 security keys** (Reason: portable, phishing-resistant, user-bound not device-bound — ideal hot-desking).
     - IF option = Authenticator app -> REJECT (requires phone).
     - IF option = Windows Hello for Business -> REJECT (device-tied; breaks on shared machines).
     - IF option = named network location -> REJECT (a CA **condition**, not an auth method).
   - **Scenario B (phone offline, legacy app):**
     - IF application doesn't support modern auth -> SELECT **App password** (Reason: pre-configured second-factor surrogate usable from the internet-connected laptop).
     - IF option = Push notification -> REJECT (phone needs internet).
     - IF option = Voice call -> REJECT (phone needs cellular).
     - IF option = Security questions -> REJECT (an **SSPR** method, not an MFA sign-in factor).
4. **SC-300 Golden Rule:** *Shared PCs + no phones = FIDO2 keys. Legacy app + dead phone = App password. Security questions live in SSPR, never MFA sign-in.*

---

## 17. Conditional Access — Prerequisites, Legacy Auth Blocking, and Session Controls

1. **Exam Scenario:** Three sub-scenarios: enabling CA for the first time; blocking legacy authentication protocols; restricting unmanaged-device behavior in SharePoint in real time.
2. **Key Decision Point:** CA prerequisites, the correct **condition**, and the correct **control type**.
3. **Decision Path:**
   - **First-time CA enablement:**
     - IF tenant has **Security defaults enabled** -> FIRST **disable Security defaults** (Reason: Security defaults and Conditional Access are **mutually exclusive**).
     - IF option = user consent settings / password protection / MFA registration policy "first" -> REJECT (Reason: not prerequisites; MFA registration is itself built *inside* CA).
   - **Blocking legacy authentication:**
     - IF requirement = block POP/IMAP/SMTP clients -> use the **Client apps condition → "Legacy authentication clients"** + Block control.
     - IF option = Cloud apps condition -> REJECT (targets apps, not protocols).
     - IF option = user risk / sign-in risk condition -> REJECT (risk-based, not protocol-based).
   - **Real-time in-session restriction (view but not download on unmanaged devices):**
     - IF requirement = real-time, session-level, device-state-based control -> SELECT CA **Session control → "Use Conditional Access App Control"** (Defender for Cloud Apps) (Reason: blocks download/sync in-session for Azure AD *registered* devices without affecting *joined* devices).
     - IF option = activity policy -> REJECT (Reason: **reactive** — acts *after* mass download, not proactive).
     - IF option = client apps condition -> REJECT (Reason: distinguishes browser vs. desktop client, not device management state).
     - IF option = app discovery policy -> REJECT (Reason: shadow-IT discovery only).
4. **SC-300 Golden Rule:** *Disable Security defaults before CA. Legacy auth = Client apps condition. Real-time download blocking = Session control + Cloud Apps — activity policies are always too late.*

---

## 18. Identity Protection — User Risk vs. Sign-in Risk Detection Types

1. **Exam Scenario:** Classify "Leaked credentials" and distinguish it from other detection types.
2. **Key Decision Point:** Whether the detection reflects **identity compromise state** (offline) or a **live sign-in anomaly** (real time).
3. **Decision Path:**
   - IF detection = **Leaked credentials** -> classify as **USER risk** (Reason: credentials found in breach data = account compromised, independent of any sign-in; calculated offline from threat intelligence).
   - IF detection = **Impossible travel / Atypical travel / Anonymous IP address** -> classify as **SIGN-IN risk** (Reason: evaluated in real time during the authentication request).
   - IF exam pairs "user risk" with a CA policy -> the "User risk policy" blocks/resets based on this offline state.
4. **SC-300 Golden Rule:** *Anything about WHERE/HOW a sign-in happens = sign-in risk. Anything about WHO is compromised (leaked creds, detached from sign-ins) = user risk.*

---

## 19. Extending MFA to On-Premises RADIUS Infrastructure (VPN)

1. **Exam Scenario:** An on-premises VPN (RADIUS client) must enforce cloud-based Azure MFA as a second factor.
2. **Key Decision Point:** The correct integration component between RADIUS and Entra ID.
3. **Decision Path:**
   - IF VPN/RADIUS + MFA required -> SELECT **NPS with the Azure AD MFA extension** (Reason: VPN → NPS (RADIUS) → MFA extension → Entra ID; primary auth stays on-prem AD).
   - IF option = **Azure AD Application Proxy** -> REJECT (Reason: publishes on-prem **web apps** for remote access, not RADIUS MFA).
   - IF option = **Password Protection proxy** -> REJECT (Reason: extends banned-password lists on-prem; no MFA).
   - IF option = **PTA agent** -> REJECT (Reason: sign-in password validation agent, not a RADIUS proxy).
4. **SC-300 Golden Rule:** *RADIUS + MFA = NPS extension. App Proxy = web apps only.*

---

## 20. Password Protection Proxy Redundancy (On-Premises)

1. **Exam Scenario:** DC agents on internet-blocked domain controllers (Server1, Server2) rely on a Proxy (Server3) to reach Entra ID; add fault tolerance.
2. **Key Decision Point:** How redundancy is achieved for the Password Protection **Proxy** component.
3. **Decision Path:**
   - IF goal = survive proxy failure -> SELECT **deploy the Proxy service on an additional domain-joined server (Server4)** (Reason: proxy is stateless; multiple instances = automatic fault tolerance; no load balancer needed).
   - IF option = Azure AD Connect -> REJECT (Reason: directory sync, not password-policy proxying).
   - IF option = Application Proxy -> REJECT (Reason: remote web-app access, unrelated).
   - IF option = PCNS -> REJECT (Reason: legacy MIM password sync component).
4. **SC-300 Golden Rule:** *DC agent on DCs, Proxy anywhere domain-joined with internet; more proxies = more redundancy, zero configuration.*

---

## 21. Shadow IT Discovery & OAuth App Governance

1. **Exam Scenario:** (a) Identify unmanaged external cloud apps and their users using **firewall logs**. (b) Alert when third-party apps request excessive permissions (e.g., Mail.ReadWrite).
2. **Key Decision Point:** Cloud Discovery (traffic-log analysis) vs. OAuth app policies vs. other governance tools.
3. **Decision Path:**
   - **Scenario A (discover unmanaged apps from firewall logs):**
     - IF logs come from firewalls/proxies -> SELECT **Cloud App Discovery** in **Microsoft Defender for Cloud Apps** (Reason: analyzes traffic logs to profile shadow IT usage and risk).
     - IF option = Application Insights -> REJECT (Reason: APM for developer-owned apps).
     - IF option = Access reviews -> REJECT (Reason: reviews access to *known* resources).
     - IF option = Enterprise applications blade -> REJECT (Reason: manages already-registered apps only).
   - **Scenario B (app requests excessive OAuth permissions):**
     - IF requirement = alert on dangerous API permissions (Graph Mail.Read/ReadWrite) -> SELECT **OAuth app policy** in Defender for Cloud Apps.
     - IF option = Identity Protection -> REJECT (Reason: user/sign-in risk, not app permissions).
     - IF option = Identity Governance -> REJECT (Reason: entitlement/access reviews/privileged workflows).
     - IF option = Intune (Endpoint Manager) -> REJECT (Reason: device compliance and MAM, not cloud API permissions).
4. **SC-300 Golden Rule:** *Firewall logs → Cloud Discovery. Dangerous OAuth permissions → OAuth app policy. Both live in Defender for Cloud Apps.*

---

## 22. Publishing On-Premises Web Apps — Application Proxy Architecture

1. **Exam Scenario:** Publish an internal ASP.NET app (App1 on Server3) to Entra ID users; determine connector placement and firewall rules.
2. **Key Decision Point:** Connector location and **traffic direction** through the firewall.
3. **Decision Path:**
   - IF publishing an on-prem web app -> install the **Application Proxy connector** on a domain-joined server (Server4) with line of sight to App1.
   - IF question asks about **Firewall1 inbound ports** -> configure **NONE** (Reason: connectors make **outbound-only** HTTPS/443 and HTTP/80 connections to Entra; no inbound rules required).
4. **SC-300 Golden Rule:** *Application Proxy = outbound-only connector. If an exam option demands inbound firewall openings, it's wrong.*

---

## 23. App Registration & Assignment Permissions by Role

1. **Exam Scenario:** Who can **assign users** to an enterprise app registered by a Global Admin, and who can **register** a new app under default tenant settings?
2. **Key Decision Point:** Distinguish **directory-wide app management roles** vs. **ownership-based** rights vs. the tenant-wide **"Users can register applications"** switch.
3. **Decision Path:**
   - **Assign users to an existing enterprise app:**
     - IF role = **Application administrator** or **Cloud application administrator** -> CAN assign (Reason: directory-wide app management rights).
     - IF role = **Application developer** who did NOT create/own the app -> CANNOT (Reason: ownership-scoped only).
     - IF role = standard user -> CANNOT.
   - **Register a new app (default settings):**
     - IF tenant keeps defaults → "Users can register applications" = **Yes** -> **ALL member users** (admins and standard users alike) CAN register.
4. **SC-300 Golden Rule:** *App admins assign any app; app developers only their own; default tenants let every member register apps.*

---

## 24. Emergency Access (Break-Glass) Account Design

1. **Exam Scenario:** Determine which management action for a break-glass Global Admin account **minimizes the likelihood of sign-in failure** during an outage.
2. **Key Decision Point:** Whether the action **adds a dependency** (potential failure point) or not.
3. **Decision Path:**
   - IF action = **monitoring/sign-in log review** of the account -> SELECT (Reason: does not add any sign-in dependency; aligns with best practice).
   - IF action = **require PIM activation** of the Global Admin role -> REJECT (Reason: if PIM is down during the emergency, the role cannot activate — a fatal dependency).
   - IF action = **CA policy restricting to corporate network** -> REJECT (Reason: blocks sign-in during on-premises failure or off-network emergency access).
   - IF action = **CA policy requiring MFA** -> REJECT (Reason: if the MFA service fails, the break-glass account is locked out — contradicting its purpose).
   - Supporting practice: exclude break-glass accounts from CA/MFA, use exclusion lists.
4. **SC-300 Golden Rule:** *Break-glass = zero dependencies. If the action adds PIM, MFA, or network conditions to the emergency account, it breaks the glass.*

---

## 25. Enforcing Terms of Use

1. **Exam Scenario:** Users must accept a Terms of Use document before accessing tenant resources.
2. **Key Decision Point:** The enforcement **mechanism** for Entra ID ToU.
3. **Decision Path:**
   - IF ToU must gate **resource access** -> SELECT a **Conditional Access policy** with a **Grant control = "Require Terms of Use acceptance"** (Reason: ToU is delivered exclusively through CA).
   - IF option = **Cloud Apps access/session policy** -> REJECT (Reason: session/threat control, not ToU enforcement).
   - IF option = **Intune Terms and Conditions** -> REJECT (Reason: applies to **device enrollment** only).
   - IF option = **Intune compliance policy** -> REJECT (Reason: defines device compliance rules; compliance can be a CA *condition*, but never delivers the ToU).
4. **SC-300 Golden Rule:** *Entra Terms of Use = Conditional Access Grant control. Intune T&C = enrollment only. Never confuse the two.*

---

## Quick Reference — Cross-Exam Cheat Table

| Trigger Phrase in Question | Correct Answer Pattern |
|---|---|
| "Automatically block user who reports fraud" | **MFA Fraud alert** + auto-block |
| "Many denied MFA attempts" | **MFA Account Lockout** |
| "Real-time check against on-prem AD" | **Pass-through Authentication** |
| "Alert email to new admin" | **Action Group** edit |
| "License assignment to dynamic groups" | **Dynamic User = yes; Dynamic Device = no** |
| "Least effort bulk license change" | **Licenses blade bulk select** |
| "New guest, no account" | **Email One-Time Passcode** |
| "Shared computers, no phones, no biometrics" | **FIDO2 security keys** |
| "Block legacy authentication" | **CA Client apps condition → Legacy auth clients** |
| "View but not download on unmanaged devices" | **CA Session control + Defender for Cloud Apps** |
| "Leaked credentials" | **User risk** (offline) |
| "VPN + MFA" | **NPS + Azure AD MFA extension** |
| "Break-glass account" | **No PIM / no MFA / no network CA dependency** |
| "Terms of Use enforcement" | **Conditional Access grant control** |# SC-300 Decision Trees - Part 4

# SC-300 Decision Trees & Logic Rules — Part 4 (Questions 109–144)

---

## SECTION 1: ACCESS REVIEWS (Identity Governance)

### 1.1 Which Objects Support Access Reviews?
1. **Exam Scenario:** You must determine which groups in the tenant can be targeted by an access review (security groups, Microsoft 365 groups, dynamic user groups, dynamic device groups).
2. **Key Decision Point:** The **object type contained in the group** (users/service principals vs. devices).
3. **Decision Path:**
   - IF group is a **Security group** (Assigned or Dynamic User) -> SELECT it as reviewable (Reason: contains user/service principal identities).
   - IF group is a **Microsoft 365 group** (Assigned or Dynamic User) -> SELECT it as reviewable (Reason: M365 groups fully support access reviews).
   - IF group is a **Dynamic User group** -> SELECT it for review, BUT note decisions will **not auto-remove members** (Reason: membership is attribute-driven, not review-driven).
   - IF group is a **Dynamic Device group** -> REJECT it (Reason: devices are not user/service principal identities; access reviews cannot target device objects).
4. **SC-300 Golden Rule:** **"Access reviews review PEOPLE, not machines — if the group holds devices, it's out of scope."**

---

### 1.2 Access Review Reviewer Types — Who Reviews Whom?
1. **Exam Scenario:** An access review is created with a specific reviewer type; you must identify which user performs the review for a given target user.
2. **Key Decision Point:** The **reviewer setting** (Members (self), Group owners, Selected reviewers, Manager).
3. **Decision Path:**
   - IF reviewer type = **Members (self)** AND scope = Everyone -> SELECT **each user reviews their own access** (Reason: self-attestation applies to every scoped user, Member or Guest alike).
   - IF reviewer type = **Group owners** -> SELECT owners as reviewers (Reason: only this setting routes reviews to owners).
   - IF reviewer type = **Selected users** -> SELECT only explicitly named reviewers.
   - IF reviewer type = **Manager** AND user's manager attribute is populated -> SELECT the user's manager.
   - IF reviewer type = **Manager** AND manager attribute is **empty/undefined** -> SELECT the **fallback reviewer** (Reason: Entra ID cannot resolve the manager relationship and redirects to the fallback).
   - IF the proposed fix is "create separate reviews per role" -> REJECT (Reason: does not fix missing manager attributes or change routing logic).
   - IF the proposed fix is "add all managers to the fallback reviewer list" -> REJECT (Reason: fallback is a single catch-all, not a dynamic departmental mapping).
   - IF the proposed fix is "populate the manager property on each user object" -> SELECT it (Reason: this resolves the root cause and routes reviews to the correct managers).
   - IF the proposed fix is "set reviewers to Member (self)" -> REJECT (Reason: bypasses management oversight; contradicts the requirement that managers perform reviews).
4. **SC-300 Golden Rule:** **"Manager reviews fail when the manager attribute is empty — fix the attribute, not the review. Fallback reviewer = the safety net for missing managers."**

---

### 1.3 Access Review Timing — Can a Reviewer Answer/Change on Date X?
1. **Exam Scenario:** A review starts January 15 with a 14-day duration, recurring monthly. Determine if a reviewer can submit or modify answers on a given date.
2. **Key Decision Point:** Whether the date falls **inside the active review window**.
3. **Decision Path:**
   - IF date is within [Start Date, Start Date + Duration] -> SELECT Yes (Reason: reviewers can submit and change answers any time before the period ends).
   - IF date is after the window closes AND before the next recurrence starts -> SELECT No (Reason: no active review instance exists).
   - IF the user is not in the reviewer scope (e.g., is an Owner but reviewers = Members (self)) -> SELECT No (Reason: owners are not reviewers unless explicitly configured).
4. **SC-300 Golden Rule:** **"Reviews are only answerable while the window is open — compute Start + Duration first, then check the reviewer's scope."**

---

### 1.4 Periodic Review of Guest Access to an Application
1. **Exam Scenario:** External contractors (guests) have access to an enterprise application (e.g., Exchange admin center). You must review their access monthly and automatically revoke denied access.
2. **Key Decision Point:** **Access review scope target** (application vs. group vs. access package).
3. **Decision Path:**
   - IF requirement = recurring review of existing access to a **specific app** for guests -> SELECT an **application-based access review** scoped to "Guest users only" with auto-remove on deny (Reason: directly targets the app's assignment list and covers all assignment methods).
   - IF proposed answer = "access package targeting users outside your directory" -> REJECT (Reason: access packages manage initial requests/bundling, not recurring reviews of existing access).
   - IF proposed answer = "group-based access review" -> REJECT as less precise (Reason: assumes access flows via group membership; app-scoped review is more direct).
4. **SC-300 Golden Rule:** **"Review the resource where access lives — app access gets an app review, group access gets a group review."**

---

### 1.5 Least-Privilege Roles for Access Review Administration
1. **Exam Scenario:** Assign roles so one user can create/manage access reviews and another can only view access review history reports.
2. **Key Decision Point:** **Role privilege tier** for Identity Governance tasks.
3. **Decision Path:**
   - IF user must create/manage access reviews -> SELECT **User Administrator** (Reason: sufficient for group/app reviews; avoids Global Administrator over-privilege).
   - IF user must view access review history reports -> SELECT **Security Reader** (Reason: explicitly grants review history read access).
   - IF proposed role = Global Reader -> REJECT as over-privileged (Reason: tenant-wide read exceeds the requirement).
   - IF proposed role = Reports Reader -> REJECT (Reason: lacks identity governance permissions for access review histories).
4. **SC-300 Golden Rule:** **"User Administrator builds reviews; Security Reader reads them — never default to Global roles."**

---

## SECTION 2: PRIVILEGED IDENTITY MANAGEMENT (PIM)

### 2.1 PIM Activation Settings — MFA Frequency & Default Approvers
1. **Exam Scenario:** A PIM role requires MFA on activation with an 8-hour maximum activation duration, and approval is required but no approvers are listed. Determine MFA frequency and who approves.
2. **Key Decision Point:** **Activation max duration** and **empty approver list behavior**.
3. **Decision Path:**
   - IF MFA is required on activation AND max activation duration = 8 hours -> SELECT **MFA every 8 hours** (Reason: each re-activation triggers a fresh MFA challenge).
   - IF "Require approval" = Yes AND approvers list = None/empty -> SELECT **Global Administrators and Privileged Role Administrators** as default approvers (Reason: PIM falls back to these tenant roles automatically).
   - IF the question asks about a role with no approval requirement -> SELECT no approver step; MFA/justification only.
4. **SC-300 Golden Rule:** **"MFA rides on every activation; empty approver list = Global Admins + Privileged Role Admins step in."**

---

### 2.2 Converting Permanent (Standing) Access to Just-in-Time
1. **Exam Scenario:** Users hold permanent role assignments; the business requires access only when needed.
2. **Key Decision Point:** **Assignment type** (Eligible vs. Active) in PIM.
3. **Decision Path:**
   - IF requirement = "access only when required" / JIT -> SELECT **Assignment type = Eligible** (Reason: user must activate — with justification/MFA/approval — to temporarily gain permissions).
   - IF proposed answer = "Expire eligible assignments after" -> REJECT (Reason: caps eligibility duration, does not create JIT).
   - IF proposed answer = "Expire active assignments after" -> REJECT (Reason: limits duration but user still has standing permissions for that window).
   - IF proposed answer = "Assignment type = Active" -> REJECT (Reason: this IS the current permanent model — the opposite of JIT).
4. **SC-300 Golden Rule:** **"Eligible = JIT. Active = standing. Expiration settings trim duration, they don't create just-in-time."**

---

## SECTION 3: TERMS OF USE & PER-DEVICE CONSENT

### 3.1 Terms of Use Consent Behavior
1. **Exam Scenario:** "Require users to consent on every device" is enabled with an expiration schedule; determine when a user is re-prompted on various devices.
2. **Key Decision Point:** **Device registration state** + **expiration start date**.
3. **Decision Path:**
   - IF consent is per-device AND user accesses from a **new registered device** never used for consent -> SELECT Yes, user is prompted (Reason: consent is tracked per device).
   - IF device is **not registered** -> SELECT Yes, user is prompted **every time** (Reason: no persistent device record exists to store consent).
   - IF user previously consented on that device AND the date is **before** the "Expire starting on" date -> SELECT No, no re-prompt (Reason: existing consent remains valid until expiration begins).
   - IF date is on/after the expiration start date -> SELECT Yes, re-consent required.
4. **SC-300 Golden Rule:** **"Per-device consent = one consent per registered device; unregistered devices prompt forever; consent lives until the expiration date."**

---

## SECTION 4: B2B COLLABORATION & EXTERNAL USERS

### 4.1 Inviting an External Contractor with an MSA (outlook.com)
1. **Exam Scenario:** Grant an external contractor using user1@outlook.com access to an internal enterprise application while they keep their own credentials.
2. **Key Decision Point:** **Identity acquisition method** for the external user.
3. **Decision Path:**
   - IF external user has an existing identity (MSA, Google, other IdP) -> SELECT **B2B invitation** (e.g., `New-AzureADMSInvitation` cmdlet or portal invite) (Reason: creates a guest object linked to their external identity; they authenticate with their own credentials).
   - IF proposed answer = `New-AzADUser` cmdlet -> REJECT (Reason: creates an internal **member** account with tenant-managed credentials).
   - IF proposed answer = "Configure External collaboration settings" -> REJECT (Reason: defines guest invite *policies*, does not invite a specific user).
   - IF proposed answer = "Add a WS-Fed identity provider" -> REJECT (Reason: federation is for whole organizations/ADFS, not individual MSAs).
   - IF proposed answer = "Implement Azure AD Connect" -> REJECT (Reason: syncs on-premises identities, unrelated to guests).
4. **SC-300 Golden Rule:** **"External user + own credentials = B2B invite. New-AzADUser makes members, not guests."**

---

### 4.2 External User Lifecycle After Access Package Assignment Expires
1. **Exam Scenario:** Settings like "Block external user from signing in," "Remove external user," and "Number of days before removing external user" must be located.
2. **Key Decision Point:** **Where the guest lifecycle settings live** in Entitlement Management.
3. **Decision Path:**
   - IF the settings govern what happens to a guest after losing their **last access package assignment** -> SELECT **Entitlement Management (main/global settings)** (Reason: tenant-wide external user lifecycle is configured centrally, not per package).
   - IF proposed location = individual Access packages -> REJECT (Reason: packages define resources/policies, not account lifecycle).
   - IF proposed location = Terms of use -> REJECT (Reason: presents policy documents, doesn't remove accounts).
   - IF proposed location = Access reviews settings -> REJECT (Reason: periodic review process, not lifecycle removal).
4. **SC-300 Golden Rule:** **"Guest lifecycle after the last package expires = Entitlement Management global settings, not the package itself."**

---

## SECTION 5: AZURE AD CONNECT & SYNCHRONIZATION

### 5.1 Filtering Users from Sync (Attribute-Based)
1. **Exam Scenario:** Prevent users with `extensionAttribute15 = NoSync` in on-premises AD from synchronizing to Azure AD.
2. **Key Decision Point:** **Rule direction + connector** in Azure AD Connect Sync (MIM engine).
3. **Decision Path:**
   - IF data flows **from on-premises AD into** the metaverse -> SELECT an **inbound synchronization rule on the Active Directory (AD DS) connector** with a scoping filter (Reason: filtering happens as data enters the metaverse from the source).
   - IF proposed answer = inbound rule on the **Azure AD connector** -> REJECT (Reason: inbound on the AAD connector means data flowing *from* Azure AD — wrong direction).
   - IF proposed answer = "Configure a Full Import run profile" -> REJECT (Reason: run profiles *execute* sync; they don't define filtering logic).
   - IF proposed answer = "Configure an Export run profile" -> REJECT (Reason: export pushes changes to the target; filtering must occur before this step).
4. **SC-300 Golden Rule:** **"Filter at the source: inbound rule on the AD connector. Run profiles run rules — they don't write them."**

---

### 5.2 Domain/OU Filtering + Group-Based Filtering (Pilot)
1. **Exam Scenario:** Azure AD Connect uses both Domain/OU filtering AND group-based filtering; determine which objects sync.
2. **Key Decision Point:** An object must satisfy **BOTH filters simultaneously**.
3. **Decision Path:**
   - IF object is in a selected OU **AND** is a **direct member** of the pilot group -> SELECT Yes, it syncs.
   - IF object is in a selected OU but **NOT** a member of the pilot group -> SELECT No (Reason: group-based filtering requires direct membership).
   - IF object is a **nested group** (member of the pilot group, not direct member) -> SELECT No (Reason: nested groups are explicitly ignored by group-based filtering).
4. **SC-300 Golden Rule:** **"Both filters must pass, and group filtering means DIRECT membership only — nesting is ignored."**

---

### 5.3 Authentication During On-Premises Outage (PTA vs. PHS vs. Cloud-Only)
1. **Exam Scenario:** On-premises domain loses internet connectivity; determine which users can still sign in (cloud-only user, synced user with PTA, guest user).
2. **Key Decision Point:** **Where authentication is validated** for each user type.
3. **Decision Path:**
   - IF user is **cloud-only** -> SELECT Yes, can sign in (Reason: authentication is entirely in Entra ID; no on-prem dependency).
   - IF user is a **guest (B2B)** -> SELECT Yes, can sign in (Reason: authenticated by Entra ID or their home IdP, not your on-prem).
   - IF user is **synced** AND tenant uses **Pass-through Authentication (PTA)** AND on-prem is offline -> SELECT No, cannot sign in (Reason: PTA agents on-prem cannot reach Azure AD to validate credentials).
   - IF tenant uses **Password Hash Sync (PHS)** instead -> SELECT Yes, can sign in (Reason: validation happens in the cloud).
   - IF claim is "PHS will automatically fail over for PTA users" -> REJECT (Reason: PHS is a backup option requiring manual switch, not automatic instant failover).
4. **SC-300 Golden Rule:** **"PTA dies when the on-prem dies; PHS and cloud-only survive. PHS is a manual backup, not an auto-failover."**

---

### 5.4 Sync Delay of Disabled Accounts
1. **Exam Scenario:** Disabled on-prem accounts take time to reflect in Azure AD; a proposed fix is Azure AD Password Protection.
2. **Key Decision Point:** **Feature purpose vs. sync schedule**.
3. **Decision Path:**
   - IF the problem is sync **latency** (default 30-min delta cycle) -> REJECT Azure AD Password Protection (Reason: it enforces password strength/banned passwords; zero impact on sync schedule or AccountEnabled replication).
   - IF the problem were weak/compromised passwords -> SELECT Azure AD Password Protection.
4. **SC-300 Golden Rule:** **"Password Protection polices passwords, not pipelines — sync delay is a delta-cycle issue, not a password issue."**

---

## SECTION 6: SELF-SERVICE PASSWORD RESET (SSPR)

### 6.1 SSPR Configuration — Methods & Writeback
1. **Exam Scenario:** Users must reset passwords using mobile app notification or security questions; cloud resets must sync back to on-prem AD (PHS already enabled via Express settings).
2. **Key Decision Point:** **SSPR blade sections** (Authentication methods vs. Password writeback).
3. **Decision Path:**
   - IF requirement = choose available methods (mobile app notification, security questions) and number required -> SELECT **Authentication methods** section under the Password reset blade.
   - IF requirement = cloud-initiated resets propagate to on-prem AD -> SELECT **enable Password writeback** in Azure AD Connect (Reason: PHS alone syncs passwords on-prem → cloud; writeback handles cloud → on-prem).
4. **SC-300 Golden Rule:** **"Methods live in the SSPR blade; cloud-to-on-prem password flow requires Password writeback in AD Connect."**

---

### 6.2 SSPR vs. MFA Methods — Don't Cross the Streams
1. **Exam Scenario:** Identify which methods are valid for MFA during sign-in vs. SSPR.
2. **Key Decision Point:** **Method purpose** (sign-in MFA vs. identity verification for reset).
3. **Decision Path:**
   - IF method = security questions OR email -> SELECT **SSPR only** (Reason: not supported as MFA methods during sign-in).
   - IF method = Authenticator push, TOTP code, SMS, FIDO2, Windows Hello -> SELECT valid for **MFA**.
   - IF method = app password -> REJECT entirely (Reason: legacy workaround for old clients that can't do MFA; not a primary MFA method).
4. **SC-300 Golden Rule:** **"Security questions and email are for password resets, never for MFA sign-in."**

---

## SECTION 7: MULTI-FACTOR AUTHENTICATION — OFFLINE SCENARIOS

### 7.1 MFA When the Phone Has No Connectivity
1. **Exam Scenario:** Laptop has wired internet; mobile phone has no Wi-Fi and no cellular. Which MFA method works?
2. **Key Decision Point:** **Does the method require phone connectivity?**
3. **Decision Path:**
   - IF phone is fully offline AND method = Authenticator **push notification** -> REJECT (Reason: push requires internet on the phone).
   - IF phone is fully offline AND method = **SMS** -> REJECT (Reason: requires cellular network).
   - IF phone is fully offline AND method = **Authenticator TOTP verification code** -> SELECT it (Reason: time-based codes are generated locally on the device; no connectivity needed — user types the code on the online laptop).
   - IF phone is fully offline AND method = **Windows Hello for Business** -> SELECT it (Reason: MFA satisfied on the laptop itself — something you have [device/TPM key] + something you know [PIN] or are [biometric]; uses the laptop's internet).
   - IF method = app password -> REJECT (Reason: legacy workaround, not a primary MFA method).
4. **SC-300 Golden Rule:** **"Offline phone? Think TOTP code or Windows Hello — push and SMS need a connected phone."**

---

### 7.2 MFA Fraud Reporting & Automatic Blocking
1. **Exam Scenario:** Enable "Report suspicious activity" in MFA notification settings; does this automatically block users who report fraud?
2. **Key Decision Point:** **Risk signal vs. enforcement action**.
3. **Decision Path:**
   - IF user reports a fraudulent MFA prompt -> SELECT risk level is set to **High** in Identity Protection (Reason: the report creates a risk event).
   - IF claim is "reporting alone blocks the user" -> REJECT (Reason: a separate **Identity Protection user risk policy** with "Block access" action is required for enforcement).
4. **SC-300 Golden Rule:** **"Reporting raises risk; a user risk policy acts on it. Signal ≠ enforcement."**

---

## SECTION 8: IDENTITY PROTECTION & ROLES

### 8.1 Who Can Configure Identity Protection Policies vs. View Reports?
1. **Exam Scenario:** Determine which admin can create/modify user risk policies and who can view the risky users report.
2. **Key Decision Point:** **Role permission boundaries** for Identity Protection.
3. **Decision Path:**
   - IF task = create/change dedicated Identity Protection risk policies -> SELECT **Security Administrator** (Reason: least-privileged role with policy write access).
   - IF user is a **Conditional Access Administrator** -> REJECT for risk policies (Reason: can create standard CA policies using risk as a condition, but cannot configure native Identity Protection risk policies).
   - IF user is a **Security Operator** -> REJECT for policy changes (Reason: can view reports and remediate/dismiss risk, but not configure policies).
   - IF task = view risky users report -> SELECT **Security Administrator OR Security Operator** (Reason: both have read access to Identity Protection reports; CA/Authentication admins do not).
4. **SC-300 Golden Rule:** **"Security Admin writes risk policies; Security Operator reads reports and remediates; CA Admin only touches Conditional Access."**

---

### 8.2 Streaming Identity Protection Alerts into Azure Sentinel
1. **Exam Scenario:** Enable Sentinel to generate incidents from Azure AD Identity Protection risk alerts.
2. **Key Decision Point:** **Data ingestion path** into Sentinel.
3. **Decision Path:**
   - IF goal = Sentinel incidents from Identity Protection -> SELECT **enable the "Azure AD Identity Protection" data connector** (Reason: streams risk alerts into the SecurityAlert table that Sentinel's built-in analytics rules query).
   - IF proposed answer = "Configure Notify settings in Identity Protection" -> REJECT (Reason: email notifications to admins, not SIEM integration).
   - IF proposed answer = "Create a Sentinel playbook" -> REJECT (Reason: playbooks are automated *response* after incidents exist; ingestion comes first).
   - IF proposed answer = "Modify Azure AD Diagnostic settings" -> REJECT (Reason: populates different tables like AADRiskyUsers; the built-in Sentinel rules rely on the dedicated connector's SecurityAlert table).
4. **SC-300 Golden Rule:** **"Sentinel needs the connector first — playbooks respond, diagnostics stream other tables."**

---

## SECTION 9: CONDITIONAL ACCESS & APPLICATIONS

### 9.1 Publishing a Gallery SaaS App + Device-Based Restriction
1. **Exam Scenario:** A third-party SaaS app is in the Azure AD gallery; users must not be prompted for credentials, and access must be restricted to Azure AD-joined devices.
2. **Key Decision Point:** **App integration method** + **access enforcement mechanism**.
3. **Decision Path:**
   - IF app exists in the **gallery** -> SELECT add it as an **Enterprise application** from the gallery (Reason: template-based SSO/federation setup = least administrative effort, no credential prompts).
   - IF requirement = restrict by device state (e.g., trustType = AzureAD) -> SELECT a **Conditional Access policy** using the **"Filter for devices"** condition (Reason: device.trustType = AzureAD enforces joined-device access).
   - IF proposed answer = custom app registration/manual federation -> REJECT when a gallery template exists (Reason: violates least administrative effort).
4. **SC-300 Golden Rule:** **"Gallery app = Enterprise application; device restrictions = Conditional Access 'Filter for devices'."**

---

### 9.2 Changing Alert Notification Recipients
1. **Exam Scenario:** Change who receives Azure Monitor alert notifications for Azure AD logs.
2. **Key Decision Point:** **Where notification routing is configured**.
3. **Decision Path:**
   - IF goal = change alert recipients -> SELECT modify the **Action Group** attached to the alert rule in Azure Monitor (Reason: action groups define who gets notified).
   - IF proposed answer = "Modify Azure AD Diagnostics settings" -> REJECT (Reason: diagnostics control *where logs are sent* (Log Analytics/storage/event hub), not alert recipients).
4. **SC-300 Golden Rule:** **"Diagnostics move logs; Action Groups move alerts. Recipients live in the Action Group."**

---

### 9.3 Streaming Azure AD Logs to Azure Monitor
1. **Exam Scenario:** Make Azure AD audit logs available for analysis in Azure Monitor.
2. **Key Decision Point:** **The prerequisite data pipeline**.
3. **Decision Path:**
   - IF logs must be analyzed in Azure Monitor -> SELECT **create a Diagnostic setting in Azure AD** streaming AuditLogs/SignInLogs to a Log Analytics workspace (Reason: this is the required first step; analysis tools only work on data already in the workspace).
   - IF proposed answer = "Create an Azure AD workbook" -> REJECT as a first step (Reason: workbooks visualize data already in the workspace — prerequisite must exist first).
   - IF proposed answer = `Set-AzureADTenantDetail` -> REJECT (Reason: updates tenant company info, not log streaming).
   - IF proposed answer = `Get-AzureADAuditDirectoryLogs` -> REJECT (Reason: one-time retrieval, not continuous streaming).
4. **SC-300 Golden Rule:** **"No Diagnostic Setting = no logs in the workspace = nothing to query or visualize."**

---

### 9.4 Sign-In Log Retention by License
1. **Exam Scenario:** Determine how long sign-in logs are retained based on tenant licensing.
2. **Key Decision Point:** **License tier** (Free vs. Premium P1/P2).
3. **Decision Path:**
   - IF tenant has **Entra ID Premium P1 or P2** -> SELECT **30 days** for sign-in logs.
   - IF tenant has **Free** license -> SELECT **7 days** for sign-in logs.
   - IF the question asks about **Audit logs** on Premium -> SELECT **90 days** (Reason: audit logs retain longer than sign-in logs).
   - IF proposed answer = 14 days -> REJECT (Reason: not a standard built-in retention period).
   - IF proposed answer = 365 days -> REJECT as native retention (Reason: only achievable by exporting to Log Analytics/storage).
4. **SC-300 Golden Rule:** **"Sign-ins: 7 (Free) / 30 (Premium). Audit: 90 (Premium). Longer = export it."**

---

## SECTION 10: DEVICE MANAGEMENT

### 10.1 Device Quota, Registration MFA, and Local Admin Rights
1. **Exam Scenario:** Tenant device limit is 5; a user has 4 devices; determine remaining capacity, which admin can change registration MFA requirements, and which devices get local admin rights from Entra roles.
2. **Key Decision Point:** **Device count vs. quota**, **role permissions**, and **join type** (joined vs. registered).
3. **Decision Path:**
   - IF user has N devices and quota = 5 -> remaining capacity = **5 − N** (e.g., 4 devices = only 1 more, not 4).
   - IF task = toggle MFA requirement for device registration/join -> SELECT **Cloud Device Administrator** (Reason: holds `microsoft.directory/deviceRegistrationPolicy/basic/update`).
   - IF admin = **Device Administrator (local)** AND device is **Azure AD JOINED** -> SELECT Yes, they get local admin rights (Reason: role members are added to the local Administrators group on AAD-joined Windows devices).
   - IF device is **Azure AD REGISTERED** (BYOD) -> SELECT No, no local admin rights (Reason: Entra admin roles are not pushed to registered devices).
4. **SC-300 Golden Rule:** **"Cloud Device Admin sets registration policy; Device Admin gets local admin only on JOINED devices — registered (BYOD) devices get nothing."**

---

## SECTION 11: ADMIN PORTALS & ROLE-TO-TASK MAPPING

### 11.1 Mapping Roles to the Correct Admin Center
1. **Exam Scenario:** Assign roles spanning identity, compliance, and endpoint management; determine which portal hosts each role.
2. **Key Decision Point:** **Role domain** (directory vs. compliance vs. endpoint).
3. **Decision Path:**
   - IF role = User Administrator, Device Administrators, Identity Governance Administrator -> SELECT **Microsoft Entra admin center** (Reason: native directory roles).
   - IF role = Records Management / compliance / information protection -> SELECT **Microsoft Purview (M365 compliance center)**.
   - IF role = Endpoint Security Manager, Intune Role Administrator -> SELECT **Microsoft Intune (Endpoint Manager) admin center** (Reason: Intune RBAC roles for device compliance and baselines).
4. **SC-300 Golden Rule:** **"Directory roles live in Entra, compliance in Purview, devices in Intune — match the role to its home portal."**

---

### 11.2 Scoped Administration with Administrative Units
1. **Exam Scenario:** Delegate management of 100 executives to a specific admin without granting tenant-wide rights.
2. **Key Decision Point:** **Administrative scope container** + **least-privileged role**.
3. **Decision Path:**
   - IF requirement = restrict admin scope to a subset of users -> SELECT an **Administrative Unit** (Reason: container that scopes role assignments; groups cannot restrict administrative scope this way).
   - IF task = manage MFA settings, re-register MFA, revoke sessions, reset passwords for non-admins -> SELECT **Authentication Administrator** (Reason: covers authentication method management; Helpdesk/Password admins can reset passwords but cannot manage MFA methods).
4. **SC-300 Golden Rule:** **"Administrative Unit = scope fence; Authentication Administrator = the MFA methods role."**

---

### 11.3 Restoring Deleted Users
1. **Exam Scenario:** A user was deleted; determine the recovery window and least-privileged role for restoration.
2. **Key Decision Point:** **Soft-delete window** + **role capability**.
3. **Decision Path:**
   - IF user was recently deleted -> SELECT **30 days** soft-delete recovery window (Reason: after 30 days the object is permanently unrecoverable).
   - IF task = restore deleted user objects -> SELECT **User Administrator** (Reason: least-privileged role managing all user account aspects including restore).
   - IF proposed role = Helpdesk Administrator -> REJECT (Reason: limited to password resets/service health; cannot restore deleted accounts).
4. **SC-300 Golden Rule:** **"Deleted users live 30 days in soft-delete; User Administrator holds the restore key."**

---

## SECTION 12: LICENSING, APP PORTAL UX & SMART LOCKOUT

### 12.1 Organizing Apps in the My Apps Portal
1. **Exam Scenario:** Group applications into logical categories for users in myapps.microsoft.com.
2. **Key Decision Point:** **Feature designed for My Apps organization**.
3. **Decision Path:**
   - IF requirement = categorize apps for users in the My Apps portal -> SELECT **Collections** (Reason: purpose-built to group apps and assign them to users/groups; requires Premium P1/P2).
   - IF proposed answer = tags -> REJECT (Reason: Azure resource management/billing metadata, not portal UX).
   - IF proposed answer = naming policies -> REJECT (Reason: enforce M365 group naming conventions).
   - IF proposed answer = dynamic groups -> REJECT (Reason: manage membership by rules; don't create portal categories).
4. **SC-300 Golden Rule:** **"Collections categorize My Apps; tags tag resources, not tiles."**

---

### 12.2 Smart Lockout Bad-Password Buffer
1. **Exam Scenario:** A user types a sequence of wrong passwords; determine how many attempts are counted toward lockout.
2. **Key Decision Point:** **Smart Lockout's rolling buffer of the last 3 bad password hashes**.
3. **Decision Path:**
   - IF entered password's hash matches any of the **last 3 tracked bad hashes** -> SELECT the attempt is **ignored** (lockout counter does not increment).
   - IF entered password is a **new** bad password -> SELECT it is **tracked** (counter increments; oldest hash drops out of the buffer).
   - IF account is locked out AND user completes **SSPR ("I forgot my password")** -> SELECT lockout timer resets to **0 seconds** (Reason: SSPR immediately clears the block without waiting).
4. **SC-300 Golden Rule:** **"Smart Lockout remembers the last 3 bad passwords — repeats are free; a fresh wrong password counts. SSPR clears the lock instantly."**

---

### 12.3 Bulk License Removal After Group-Based Licensing
1. **Exam Scenario:** 2,500 users have directly assigned E3 licenses; new licenses were assigned via a group; remove the direct assignments with least effort.
2. **Key Decision Point:** **Tool for bulk direct-license removal**.
3. **Decision Path:**
   - IF bulk removal of directly assigned licenses -> SELECT **`Set-MsolUserLicense` with `-RemoveLicenses`** in a script (Reason: iterates users and removes specific SKUs efficiently).
   - IF proposed answer = `Set-MsolProductKey` -> REJECT (Reason: sets subscription product keys, not user licenses).
   - IF proposed answer = `Update-MgGroup` -> REJECT (Reason: modifies group properties; cannot remove direct user licenses).
   - IF proposed answer = `Update-MgUser` -> REJECT (Reason: general user property updates; the license-specific Graph cmdlet is `Set-MgUserLicense`).
4. **SC-300 Golden Rule:** **"Direct licenses come off with Set-MsolUserLicense -RemoveLicenses — group assignment replaces, the script cleans up."**

---

## MASTER CHEAT SHEET — PART 4

| Topic | Trigger in Question | Correct Answer Pattern |
|---|---|---|
| Access reviews | Dynamic Device group | ❌ Not reviewable |
| Access reviews | Manager reviewer + empty manager attr | Fallback reviewer; fix = populate manager property |
| PIM | "Access only when required" | Assignment type = **Eligible** |
| PIM | Approval required, no approvers | Global Admins + Privileged Role Admins |
| Terms of Use | Per-device consent | Prompt per registered device; unregistered = every time |
| B2B | outlook.com contractor | B2B invitation (`New-AzureADMSInvitation`) |
| AD Connect | Filter by attribute | Inbound rule on **AD DS connector** |
| AD Connect | Group-based filtering | Direct members only; nested groups ignored |
| Auth outage | PTA + on-prem offline | Synced users CANNOT sign in; cloud-only & guests CAN |
| SSPR | Cloud reset → on-prem | Password writeback |
| MFA | Phone offline | TOTP code or Windows Hello (never push/SMS) |
| Identity Protection | Fraud report | High risk; blocking needs a separate user risk policy |
| Sentinel | Identity Protection incidents | Enable the dedicated data connector |
| Logs | Retention | Sign-in 30d (Premium) / 7d (Free); Audit 90d |
| Devices | Local admin rights | AAD-joined only; registered (BYOD) gets none |
| Deleted user | Recovery | 30-day soft delete; User Administrator restores |
| Smart Lockout | Repeated wrong passwords | Last 3 hashes buffered; repeats ignored; SSPR resets timer |# SC-300 Decision Trees - Part 5

# SC-300 Decision Trees & Logic Rules — Part 5 (Questions 145–180)

---

### 1. Defender for Cloud Apps — Policy Type Selection (Q145, Q156, Q158)

1. **Exam Scenario:** You must block users from printing PDFs in real time, alert when a high-permission OAuth app is consented by >20 users, or deploy session controls for SharePoint Online.
2. **Key Decision Point:** The *timing and target* of the control — real-time in-session action, app authorization state, post-activity alert, or data-at-rest.
3. **Decision Path:**
   - IF the requirement is to **block/monitor a real-time in-session action** (print, download, copy-paste) -> SELECT **Session policy** (Reason: Proxies the live session via Conditional Access App Control)
     - IF deploying session control -> FIRST create a **Conditional Access policy** with Session control = **"Conditional Access App Control"** (Reason: This routes sessions through the Defender for Cloud Apps reverse proxy — mandatory first step)
   - IF the requirement is to **alert on app attributes** (permission level + number of consenting users) -> SELECT **OAuth app policy** (Reason: Filters specifically on app permission and consent-user count)
   - IF the requirement is to **alert/govern after an activity occurred** -> SELECT **Activity policy** (Reason: Reactive; cannot block in real time)
   - IF the requirement is to **scan/classify files at rest** -> SELECT **File policy** (Reason: Cannot control live session actions like printing)
   - IF the requirement is to **gate whether a user can access the app at all** -> SELECT **Access policy** (Reason: Sign-in level control, no granular in-session actions)
   - REJECT "User monitoring" / "Continuous report" (Reason: Investigation/export features, not policy creation)
4. **SC-300 Golden Rule:** *Session = real-time in-session (CA App Control first); Access = gate at sign-in; Activity = alert after the fact; File = data at rest; OAuth app = consent alerts.*

---

### 2. Access Reviews — Scope, Nested Groups & Source of Authority (Q146, Q177)

1. **Exam Scenario:** A review targets Group3 (which contains nested Group1); another group is synced from on-prem AD. Will users be auto-removed? Separately: auto-remove guests inactive for 30 days from App1.
2. **Key Decision Point:** Is the target **directly in scope**, and is **Entra ID the source of authority** for the group's membership?
3. **Decision Path:**
   - IF the group is **not in the review's defined scope** -> Answer **No** (Reason: Reviews evaluate only explicitly targeted groups)
   - IF the user is a **nested member** (member of a member group) -> Answer **No**, user is NOT removed (Reason: Access reviews evaluate **direct members only** — no drill-down into nested groups)
   - IF the group's source = **Cloud (Microsoft Entra ID)** -> Answer **Yes**, auto-removal works (Reason: Entra ID is the source of authority and can modify membership)
   - IF the group is **synced from an on-prem AD domain** -> Answer **No**, auto-removal fails (Reason: Entra ID cannot write back membership deletions for groups it doesn't own)
   - IF auto-removing **inactive guests from an application** -> SELECT **Access review for application access**, scope = **Guest users only**, criterion = **last sign-in (inactivity, 30 days)**, enable **Auto-apply results to resource** (Reason: Minimal effort, automatic revocation)
   - REJECT Conditional Access for inactivity removal (Reason: Evaluates sign-in conditions, never removes historical access)
4. **SC-300 Golden Rule:** *Access reviews see direct members only and can only remove members from groups Entra ID owns — synced/on-prem groups are read-only to reviews.*

---

### 3. Access Review Licensing — P2 vs Guest MAU Billing (Q160)

1. **Exam Scenario:** Group1 review: 500 members + 25 guests performing self-reviews. Group2 review: 95 guests reviewed by one Group Owner. How many P2 licenses?
2. **Key Decision Point:** Is the individual a **member user** or a **guest (B2B) user**?
3. **Decision Path:**
   - IF a **member (internal) user** performs or is subject to a review -> Requires **Entra ID Premium P2** (one per member)
   - IF a **guest user** is being reviewed -> **No P2 license** (Reason: Guest governance is billed under the **External ID Monthly Active Users (MAU)** model)
   - IF a **reviewer** (e.g., Group Owner) performs the review -> Requires **P2** (Reason: Reviewers are governance actors, not MAU-covered guests)
   - Calculation logic: Group1 = 500 P2 (members only); Group2 = 1 P2 (the owner only)
4. **SC-300 Golden Rule:** *Members pay P2; guests ride free on MAU billing — count licenses for internal users only (both reviewers and reviewed members).*

---

### 4. Identity Protection — User Risk vs Sign-in Risk (Q147, Q161, Q163, Q174)

1. **Exam Scenario:** Leaked credentials detected; sign-in from anonymous IP (Tor); threat intelligence detection; users with varying MFA registration status.
2. **Key Decision Point:** Does the detection indicate a **compromised identity** (User risk) or a **suspicious authentication attempt** (Sign-in risk)?
3. **Decision Path:**
   - IF detection = **Leaked credentials** or **Azure AD threat intelligence** -> Classify as **User risk** (Reason: Account is compromised regardless of sign-in context — offline detection)
     - Remediation policy control -> **Grant access but require password change** (self-remediation via SSPR)
   - IF detection = **Anonymous IP address** or **Unfamiliar sign-in properties** -> Classify as **Sign-in risk** (Reason: Real-time evaluation of a single authentication attempt)
     - Enforcement -> **Sign-in risk policy** requiring MFA at threshold
   - IF user is covered only by a **User risk policy** but trigger is a **sign-in risk** -> User signs in **successfully** (Reason: Policy scope mismatch — anonymous IP never triggers the user risk policy)
   - IF user is covered by a **Sign-in risk policy** requiring MFA:
     - IF MFA status = **Registered/Enforced** -> Sign-in succeeds after MFA remediation
     - IF MFA status = **Disabled/Not registered** -> Sign-in **blocked** (Reason: Cannot self-remediate without registered methods)
   - REJECT **User risk policy** for anonymous IP (Reason: Evaluates cumulative user risk, not a single real-time sign-in)
   - REJECT **MFA registration policy** for enforcing MFA at sign-in (Reason: It only compels registration)
4. **SC-300 Golden Rule:** *Leaked credentials/threat intel = User risk (fix: password change); Anonymous IP/unfamiliar properties = Sign-in risk (fix: MFA) — and the user must be MFA-registered to remediate.*

---

### 5. Identity Protection — Risk State Actions: Confirm vs Dismiss (Q166, Q173)

1. **Exam Scenario:** Admin confirms a user compromised or dismisses a risk; existing user risk policy blocks "Low and above" or requires password reset for High.
2. **Key Decision Point:** Which **administrative action** was taken, and what does the **existing policy** enforce?
3. **Decision Path:**
   - IF admin selects **"Confirm user compromised"** -> Risk state = confirmed compromised, user risk = **High (permanent)** (Reason: Explicit admin override)
     - IF policy blocks "Low and above" -> User is **blocked** (cannot remediate via MFA)
     - IF policy requires password reset for High -> User forced to **reset password at next sign-in** (Reason: Leverages existing policy = least administrative effort)
   - IF admin selects **"Dismiss risk"** -> User risk resets to **None** (Reason: Risk deemed benign)
     - BUT IF the user then triggers a **sign-in risk** (e.g., anonymous IP = Medium) -> Active sign-in risk can elevate overall risk; IF user risk policy blocks "Low and above" -> User may still be **blocked**
   - REJECT reconfiguring the global user risk policy to Medium/Low (Reason: Tenant-wide impact — violates least administrative effort)
   - REJECT resetting MFA registration (Reason: Forces re-enrollment, not a password reset)
4. **SC-300 Golden Rule:** *Confirm compromised = risk locked at High (block or force reset via existing policy); Dismiss = back to None — never rewrite global policies for a single user.*

---

### 6. Entitlement Management — Catalogs, Delegation & Access Packages (Q148, Q151, Q180)

1. **Exam Scenario:** Bundle Site1 + Team1 into an access package; delegate catalog creation to a non-admin; enable self-service requests with auto-approval and 30-day expiry.
2. **Key Decision Point:** Workflow stage — **resource organization**, **delegation**, or **access lifecycle**?
3. **Decision Path:**
   - IF creating an **access package** -> FIRST create a **catalog** (Reason: Catalog is the container; packages must exist inside one)
   - IF a **non-admin** must create new catalogs -> Identity Governance blade → Entitlement Management **settings** → add user to **"Catalog creators"** (Reason: Global delegation setting, not a directory role; creator auto-becomes catalog owner and can add owned resources)
     - REJECT Service support administrator / Groups administrator roles (Reason: Unrelated to Identity Governance)
     - REJECT modifying roles on the existing "General" catalog (Reason: Grants Catalog owner *within that catalog only* — cannot create NEW catalogs)
   - IF users must **request access** with auto-approval for group members + **30-day expiration** -> SELECT **Entitlement Management access package** with request policy + lifecycle expiry (Reason: Purpose-built request/approve/expire lifecycle)
     - REJECT Conditional Access (Reason: Sign-in enforcement, no request/lifecycle)
     - REJECT PIM (Reason: Privileged admin roles only, not general resource access)
     - REJECT Defender for Cloud Apps access policy (Reason: Real-time session control)
   - REJECT Administrative units (Reason: Delegate admin over users/groups, not an entitlement prerequisite)
   - REJECT App registrations (Reason: For app integration, not internal resource access)
4. **SC-300 Golden Rule:** *Catalog first, package second; catalog creators live in Identity Governance settings (not directory roles); request + approve + expire = Entitlement Management, nothing else.*

---

### 7. Azure RBAC vs Entra ID Roles — Two Separate Control Planes (Q149, Q155)

1. **Exam Scenario:** Create a custom Azure subscription role by cloning; enable VM sign-in with Entra credentials; delegate VM creation.
2. **Key Decision Point:** Which **control plane** hosts the target — Azure RBAC (subscriptions/resources) or Microsoft Entra ID (directory objects)?
3. **Decision Path:**
   - IF cloning to create a **custom Azure subscription role** -> CAN clone: **built-in Azure subscription roles** + **existing custom Azure subscription roles** (Role2) (Reason: Same authorization system)
     - CANNOT clone: **Entra ID roles** (custom Role1 or built-in Azure AD roles) (Reason: Entra ID and Azure RBAC use separate authorization systems)
   - IF users must **sign in to Azure VMs with Entra ID credentials** -> Assign **Virtual Machine Administrator Login** or **Virtual Machine User Login** (Azure RBAC) at resource level
   - IF delegating **VM creation/management** -> Assign **Virtual Machine Contributor** (Azure RBAC)
   - REJECT Entra ID built-in roles for Azure infrastructure (Reason: They govern directory objects — users, licenses — not Azure resources)
   - REJECT Managed identities for user logins (Reason: Service-to-service authentication only)
4. **SC-300 Golden Rule:** *Azure RBAC roles govern Azure resources; Entra ID roles govern the directory — never the twain shall clone.*

---

### 8. Cloud App Discovery — Shadow IT (Q150)

1. **Exam Scenario:** Identify external cloud apps employees use by uploading firewall logs.
2. **Key Decision Point:** Is the app **known/integrated** or **unknown/unsanctioned**?
3. **Decision Path:**
   - IF discovering **unknown cloud apps from network traffic logs** -> SELECT **Cloud App Discovery** (Reason: Ingests firewall/proxy logs; reports apps, users, traffic volume)
   - REJECT Enterprise applications in Entra ID (Reason: Manages already-integrated SSO apps)
   - REJECT Access reviews (Reason: Governance of existing access, not discovery)
   - REJECT Application Insights (Reason: APM for developer-built custom apps)
4. **SC-300 Golden Rule:** *Shadow IT discovery = upload firewall logs to Cloud App Discovery — not Entra enterprise apps, not App Insights.*

---

### 9. MFA Registration Policy & Grace Period (Q152)

1. **Exam Scenario:** Ensure users register for MFA with a grace period before being blocked.
2. **Key Decision Point:** Is the goal **registration enforcement** or **sign-in enforcement**?
3. **Decision Path:**
   - IF compelling users to **register MFA methods** -> SELECT **Entra ID Protection MFA registration policy** (Reason: Built-in registration flow with grace period)
     - Default grace period = **14 days**; after expiry, users are **blocked** until registered
   - IF enforcing MFA **at sign-in** -> Use **Conditional Access** (Reason: CA does not manage registration grace periods)
4. **SC-300 Golden Rule:** *Registration = Identity Protection MFA registration policy (14-day grace); enforcement at sign-in = Conditional Access.*

---

### 10. Log Selection — Sign-in vs Audit vs Secure Score (Q153, Q178)

1. **Exam Scenario:** Find IP/location of sign-ins, track admin changes, get posture recommendations, or identify who accepted a Terms of Use.
2. **Key Decision Point:** Event type — **authentication**, **administrative change**, **posture**, or **consent action**?
3. **Decision Path:**
   - IF need **IP address / geographic location** of authentication attempts -> SELECT **Sign-in logs** (Reason: Capture client IP and location per attempt)
   - IF need **administrative/system changes** (user, group, service principal modifications) -> SELECT **Audit logs** (Reason: Traceability for all tenant admin actions)
     - IF need **who accepted/declined a Terms of Use** -> SELECT **Audit logs**, filter category **UserManagement**, activities "Accept/Decline terms of use policy" (Reason: ToU acceptance is an audit event, not a sign-in detail)
   - IF need **security posture + improvement recommendations** -> SELECT **Identity Secure Score** (Reason: Continuous evaluation against best practices)
   - REJECT Sign-in logs for ToU acceptance (Reason: Focus is the sign-in itself, not the consent action)
   - REJECT Provisioning logs (Reason: Automated provisioning events only)
   - REJECT Usage & Insights report (Reason: Aggregated trends, not event-level detail)
4. **SC-300 Golden Rule:** *Who signed in from where = Sign-in logs; what changed = Audit logs; how healthy = Secure Score; who accepted ToU = Audit logs (UserManagement).*

---

### 11. Role-Assignable Groups (Q154)

1. **Exam Scenario:** Create a group that can receive Entra ID role assignments AND allows manual addition of specific users.
2. **Key Decision Point:** Group **type** (Security vs Microsoft 365) and **membership type** (Assigned vs Dynamic).
3. **Decision Path:**
   - IF group must receive **Entra ID role assignments** -> Enable **"Azure AD roles can be assigned to the group"** — available for BOTH Security and Microsoft 365 types
   - IF users must be **manually added** -> Membership type = **Assigned** (Reason: Dynamic types are rule-based, no manual adds)
   - VALID answers: **Security + Assigned** OR **Microsoft 365 + Assigned**
   - REJECT Dynamic User (Reason: Rule-based — contradicts manual addition)
   - REJECT Dynamic Device (Reason: Populates device objects, not users)
4. **SC-300 Golden Rule:** *Role-assignable works for both group types; manual membership = Assigned — Dynamic anything means no hand-picked users.*

---

### 12. Conditional Access What If Tool (Q162, Q175)

1. **Exam Scenario:** Simulate a sign-in from another country with a specific risk level without affecting real users.
2. **Key Decision Point:** Is the evaluation **simulated/proactive** or **historical/actual**?
3. **Decision Path:**
   - IF simulating a sign-in (user, IP, risk level, device platform, client app) to see which CA policies apply -> SELECT **Conditional Access What If tool** (Reason: Safe simulation of policy logic and grant/session controls)
   - REJECT Sign-in logs (Reason: Report actual past events, not simulation)
   - REJECT Access reviews (Reason: Access recertification, not policy testing)
   - REJECT Identity Secure Score (Reason: Posture metric, not policy evaluation)
   - REJECT Defender for Cloud Apps activity logs (Reason: Post-sign-in app activity, not CA logic)
   - REJECT M365 network connectivity test tool (Reason: Network performance, not identity policy)
4. **SC-300 Golden Rule:** *"Simulate a sign-in" = What If tool — logs look backward, What If looks forward.*

---

### 13. Tenant Restrictions & Modern Authentication (Q164)

1. **Exam Scenario:** Enforce tenant restrictions; some clients run Outlook 2013 (no native modern auth).
2. **Key Decision Point:** Does the client support **modern authentication (OAuth 2.0)**?
3. **Decision Path:**
   - IF clients lack modern authentication (Outlook 2013) -> FIRST **upgrade to a natively modern-auth client** (Outlook 2016+) (Reason: Tenant restrictions intercept auth requests requiring OAuth 2.0; Outlook 2013 is past support lifecycle)
   - REJECT configuring Outlook 2013 via registry (Reason: Temporary fix on an unsupported client — unreliable)
   - REJECT upgrading ALL clients including compliant Outlook 2016 (Reason: Unnecessary effort — violates the stated requirement)
   - REJECT Organization Sharing (Reason: Exchange calendar sharing, unrelated to tenant restrictions)
4. **SC-300 Golden Rule:** *Tenant restrictions demand modern auth — upgrade the legacy client first; don't patch dead software.*

---

### 14. Authentication Methods — Number Matching (Q165)

1. **Exam Scenario:** Determine which users see number matching during sign-in.
2. **Key Decision Point:** Which **authentication method** is assigned to the user's group?
3. **Decision Path:**
   - IF method = **Microsoft Authenticator (push notification)** -> YES, number matching applies (Reason: Number matching is standard for all Authenticator push notifications — anti-MFA-fatigue protection)
   - IF method = **FIDO2 security key** -> NO (Reason: Physical key + PIN/biometric, no app push)
   - IF method = **SMS** -> NO (Reason: One-time passcode via text, no interactive push)
4. **SC-300 Golden Rule:** *Number matching lives only in Authenticator push — FIDO2 and SMS never number-match.*

---

### 15. Application Administration Roles — Least Privilege (Q167, Q168)

1. **Exam Scenario:** User1 must manage App Proxy settings without owning apps or managing credentials; a designated reviewer must approve admin consent requests.
2. **Key Decision Point:** What is actually required — **proxy management**, **credential management**, or **consent approval**?
3. **Decision Path:**
   - IF managing **Application Proxy settings** without ownership/credential management -> SELECT **Cloud Application Administrator** (Reason: Manages proxy + registration but NOT credentials/secrets — least privilege)
     - REJECT **Application Administrator** (Reason: Overly permissive — includes credential management not required)
     - REJECT **Application developer** (Reason: Registering apps makes the user an owner — violates requirement)
     - REJECT **Service support administrator** (Reason: Support tickets/health only, no app permissions)
   - IF approving **admin consent requests** -> Reviewer must hold a consent-capable role: **Global Administrator, Cloud Application Administrator, or Application Administrator** (Reason: Workflow reviewer designation does NOT elevate directory privileges)
     - REJECT designated reviewers without consent-capable roles (e.g., plain users, Authentication administrator) (Reason: Directory role privileges govern, not workflow assignment)
4. **SC-300 Golden Rule:** *Proxy-only = Cloud Application Administrator (no credentials); consent approval requires a consent-capable directory role — workflow designation grants nothing.*

---

### 16. B2B Guest Authentication — Email OTP & Redemption (Q169, Q172)

1. **Exam Scenario:** Guests with Entra ID, outlook.com, and gmail.com accounts sign in; collect attributes at self-service registration; OTP validity window.
2. **Key Decision Point:** What **identity source** does the guest have?
3. **Decision Path:**
   - IF guest has an **Entra ID (Azure AD) account** -> Authenticates with **home tenant credentials**
   - IF guest has an **outlook.com address** -> Authenticates as **Microsoft account (MSA)**
   - IF guest has a **gmail.com address** (no Google federation configured) -> Falls back to **Email one-time passcode (OTP)** (Reason: OTP is the fallback when no other method applies)
     - OTP validity = **30 minutes**
   - IF enabling OTP -> Configure under **External Identities → All identity providers** (Reason: Email OTP is managed as an identity provider)
   - IF collecting **user attributes (name, city, email) at sign-up** -> Configure a **user flow** with the User attributes section (Reason: Self-service sign-up flows define collected attributes)
4. **SC-300 Golden Rule:** *Entra account = home creds; outlook.com = MSA; gmail without federation = email OTP (valid 30 min); attributes at sign-up = user flow.*

---

### 17. Security Defaults (Q170)

1. **Exam Scenario:** User2 must use alternate MFA methods, but security defaults force Authenticator-only.
2. **Key Decision Point:** Are **security defaults** enabled, and who can modify them?
3. **Decision Path:**
   - IF security defaults enabled -> ALL users restricted to the **Microsoft Authenticator app only** for MFA (Reason: Security defaults mandate Authenticator)
     - IF alternate MFA methods needed -> **Disable/modify security defaults** first
   - IF modifying security defaults -> Requires **Global Administrator, Conditional Access Administrator, or Security Administrator** role
   - Cross-check the user table: only the user holding one of these roles can perform the action
4. **SC-300 Golden Rule:** *Security defaults = Authenticator-only MFA; to change them you need Global, Conditional Access, or Security Administrator.*

---

### 18. Seamless SSO — Local Intranet Zone (Q171)

1. **Exam Scenario:** Domain-joined Windows computers must sign in to Entra ID without credential prompts.
2. **Key Decision Point:** Where must the Entra SSO URL be **trusted on the client**?
3. **Decision Path:**
   - IF enabling Seamless SSO in browsers -> Add **https://autologon.microsoftazuread-sso.com** to the **Local intranet zone** (Reason: Browser forwards the Kerberos ticket without prompting; deploy via Group Policy)
   - REJECT Sign-in options in Settings app (Reason: Local device auth — Windows Hello/PIN)
   - REJECT Enterprise State Roaming (Reason: Settings sync, not SSO)
   - REJECT PTA Authentication Agent on clients (Reason: Server-side component of Pass-through Authentication, not client config)
4. **SC-300 Golden Rule:** *Seamless SSO = Kerberos + autologon.microsoftazuread-sso.com in the Local intranet zone via GPO.*

---

### 19. Emergency Access (Break-Glass) Accounts (Q176)

1. **Exam Scenario:** Create an emergency account that survives federation failure or sync outage.
2. **Key Decision Point:** Account **origin** (cloud-only vs synced/federated) and **role**.
3. **Decision Path:**
   - IF creating a break-glass account -> Create **cloud-only in Entra ID** using the ***.onmicrosoft.com** domain (Reason: Must not depend on on-prem AD, federation, or sync)
     - REJECT accounts in on-prem OUs (OU1/OU2) (Reason: On-prem dependency defeats the purpose)
   - IF assigning the role -> Assign **Global Administrator** permanently (Reason: Break-glass needs full tenant access to resolve catastrophic lockouts)
4. **SC-300 Golden Rule:** *Break-glass = cloud-only *.onmicrosoft.com + permanent Global Administrator — zero on-prem dependency.*

---

### 20. App Registration Soft Delete (Q179)

1. **Exam Scenario:** An app registration was deleted — how long is the recovery window?
2. **Key Decision Point:** **Retention period** for soft-deleted app registrations.
3. **Decision Path:**
   - IF app registration deleted -> Moves to **soft-deleted state for 30 days** (Reason: Full restore of properties, service principals, and permissions possible within the window)
   - REJECT 14 / 60 / 180 days (Reason: Official retention is exactly 30 days)
4. **SC-300 Golden Rule:** *Deleted app registration = 30-day soft-delete recovery window — then it's gone forever.*

---

### 21. Microsoft Graph Permissions — Application vs Delegated (Q157)

1. **Exam Scenario:** Which user can create calendar appointments for ALL users in the organization via their app?
2. **Key Decision Point:** Permission **type** (Application vs Delegated) and **scope** (ReadWrite vs Read).
3. **Decision Path:**
   - IF the app must act **without a signed-in user / tenant-wide** -> Requires **Application** permission (Reason: App permissions run app-only with tenant-wide access)
   - IF creating/modifying data -> Requires **ReadWrite** (Read = read-only)
   - IF only **Delegated Calendars.Read** -> REJECT (Reason: Read-only, limited to the signed-in user's context)
   - IF only **Application Calendars.Read** -> REJECT (Reason: Tenant-wide but read-only — cannot create appointments)
4. **SC-300 Golden Rule:** *Org-wide writes need Application + ReadWrite; Delegated = user context; Read = read-only.*

---

### 22. Hybrid Authentication — PTA & Password Writeback (Q159)

1. **Exam Scenario:** Users must sign in with the same passwords validated by on-prem domain controllers, and use cloud-based SSPR.
2. **Key Decision Point:** Which method validates against the **domain controller**, and what enables **SSPR writeback**?
3. **Decision Path:**
   - IF passwords must be validated **by the on-prem DC** -> SELECT **Pass-through Authentication (PTA)** (Reason: Lightweight on-prem agent validates credentials directly against the DC)
     - REJECT **PHS** (Reason: Authentication handled entirely in the cloud)
     - REJECT **Federation** (Reason: Separate AD FS infrastructure, not a direct DC validation agent)
   - IF on-prem users must use **cloud SSPR** -> Enable **Password writeback** in Microsoft Entra Connect (Reason: Writes cloud-initiated resets back to on-prem AD in real time)
4. **SC-300 Golden Rule:** *DC-validated passwords = PTA; cloud SSPR for on-prem users = Password writeback in Entra Connect.*

---

## 📋 Quick-Reference Cheat Sheet — Part 5

| Trigger Phrase in Question | Correct Answer Instinct |
|---|---|
| "Block printing / real-time in-session" | Defender for Cloud Apps **Session policy** (+ CA App Control first) |
| "Alert on high-permission app consented by N users" | **OAuth app policy** |
| "Nested group member in access review" | **Not removed** (direct members only) |
| "Group synced from on-prem AD + auto-remove" | **Fails** (not source of authority) |
| "Leaked credentials / threat intelligence" | **User risk** → require password change |
| "Anonymous IP / unfamiliar properties" | **Sign-in risk** → require MFA |
| "Confirm user compromised" | Risk = **High permanently** |
| "Dismiss risk" | Risk resets to **None** |
| "First step for access package" | **Create a catalog** |
| "Non-admin creates catalogs" | Identity Governance **settings → Catalog creators** |
| "Request + auto-approve + expire" | **Entitlement Management access package** |
| "Clone role for Azure subscription" | Built-in + custom **Azure subscription roles only** (never Entra roles) |
| "VM sign-in with Entra credentials" | **Virtual Machine (Administrator/User) Login** (Azure RBAC) |
| "Discover shadow IT from firewall logs" | **Cloud App Discovery** |
| "MFA registration grace period" | **14 days** (Identity Protection registration policy) |
| "Who accepted Terms of Use" | **Audit logs** (UserManagement category) |
| "Simulate sign-in / test CA policy" | **What If tool** |
| "Outlook 2013 + tenant restrictions" | **Upgrade to Outlook 2016+** (modern auth) |
| "Number matching" | **Microsoft Authenticator push only** |
| "Manage App Proxy, no credentials" | **Cloud Application Administrator** |
| "Approve admin consent requests" | Global Admin / Cloud App Admin / App Admin |
| "gmail guest, no federation" | **Email OTP — valid 30 minutes** |
| "Security defaults enabled" | **Authenticator-only MFA** |
| "Seamless SSO client config" | **autologon.microsoftazuread-sso.com → Local intranet zone** |
| "Break-glass account" | **Cloud-only *.onmicrosoft.com + Global Administrator** |
| "Deleted app registration restore" | **30 days** |
| "Create appointments for all users" | Graph **Application + ReadWrite** permission |
| "Password validated by on-prem DC" | **PTA** (+ Password writeback for SSPR) |
| "Guests in access reviews" | **No P2 needed — MAU billing** |# SC-300 Decision Trees - Part 6

# SC-300 Decision Trees — Part 6 Analysis (Questions 181–216)

*Structured logic rules extracted from explanations. Licensing gates noted where relevant: **PIM, Access Reviews, and Entitlement Management all require Microsoft Entra ID P2**; MDCA/App Control requires MDCA licensing (M365 E5 or add-on).*

---

### 1. Managed Identity: Region Rules & Type Selection
**Source: Q181, Q195**

1. **Exam Scenario:** A VM or App Service must authenticate to Entra ID and access resources without stored credentials; determine if identity-to-resource attachments and RBAC assignments are valid across regions.
2. **Key Decision Point:** *Attaching* an identity (region-bound) vs. *assigning RBAC* (region-agnostic) — then lifecycle ownership (system vs. user-assigned).
3. **Decision Path:**
   - IF attaching a **user-assigned** managed identity to a resource -> REQUIRE **same region** (Managed1 East US + VM1 East US = valid; Managed3 West US + VM1 East US = INVALID)
   - IF assigning an **Azure RBAC role** (e.g., Owner on RG1) to a managed identity -> SELECT **Yes** regardless of region (role assignments are global, not regional)
   - IF requirement = credentials available **ONLY to App1** + minimal admin effort -> SELECT **system-assigned managed identity** (created/deleted with the resource; Azure rotates credentials automatically)
   - IF requirement = identity **shared across multiple resources** -> SELECT **user-assigned managed identity** (standalone resource)
   - IF distractor = user account / SQL account -> REJECT (both require **manual password/secret rotation** — fails "minimize administrative effort")
4. **SC-300 Golden Rule:** "Attach = same region; Assign (RBAC) = any region. System-assigned lives and dies with the resource; user-assigned is a reusable standalone."

---

### 2. Key Vault Deletion States: Recover vs. Purge
**Source: Q182**

1. **Exam Scenario:** An object was deleted from a vault on June 1; determine whether an admin can **recover** or **purge** it on later dates, given retention periods and purge protection.
2. **Key Decision Point:** Action verb (Recover vs. Purge) + Purge Protection flag + retention window arithmetic.
3. **Decision Path:**
   - IF action = **RECOVER** within retention window AND role has data-plane rights (Key Vault Administrator) -> SELECT **Yes** (recovery is a permitted data-plane operation)
   - IF action = **PURGE** and Purge Protection is enabled -> SELECT **No** for **everyone** (no role overrides purge protection)
   - IF purge date falls before retention expiry (deleted June 1, 15-day retention → expires June 16; attempt on June 14) -> BLOCKED; after expiry, Azure purges **automatically**
   - IF distractor = "Administrator can force-purge anyway" -> REJECT (purge protection is role-transcendent)
4. **SC-300 Golden Rule:** "Recover = permission + within the soft-delete window. Purge protection blocks ALL humans until the timer expires — no role is an exemption card."

---

### 3. Key Vault Data-Plane Role Matching
**Source: Q215**

1. **Exam Scenario:** Assign least-privilege vault roles for key creation and certificate access.
2. **Key Decision Point:** Which object type (keys / secrets / certificates) the task targets.
3. **Decision Path:**
   - IF task = create/manage **keys** -> SELECT **Key Vault Crypto Officer**
   - IF task = access/manage **certificates** -> SELECT **Key Vault Certificates Officer**
   - IF task = manage **secrets** -> SELECT **Key Vault Secrets Officer**
   - IF task = manage vault permissions/control plane -> use control-plane roles (data-plane Officer roles explicitly cannot manage permissions)
4. **SC-300 Golden Rule:** "Match the Officer to the object: Keys = Crypto, Certs = Certificates, Secrets = Secrets."

---

### 4. MDCA Policy Type Selection
**Source: Q192, Q202, Q209**

1. **Exam Scenario:** Choose the correct Defender for Cloud Apps policy: block sign-ins from risky locations, alert on 50+ downloads/minute, or identify who is accessing an app.
2. **Key Decision Point:** *When* enforcement occurs (sign-in vs. in-session vs. after-the-fact) and *what* triggers it (condition vs. threshold vs. ML baseline).
3. **Decision Path:**
   - IF requirement = real-time control/monitoring of **sign-ins** by condition (app + location/IP tags) -> SELECT **Access policy** (e.g., Block access for Anonymous proxy/Satellite locations; or Monitor-only to identify Facebook users)
   - IF requirement = real-time control **inside a session** (block downloads, protect on download) -> SELECT **Session policy** (requires CA App Control onboarding — see Tree 5)
   - IF requirement = alert when an action repeats **N times in a time window** (50 downloads/min from Site1) -> SELECT **Activity policy** with the **"Repeated activity"** trigger
   - IF requirement = detect behavior deviating from a learned baseline -> SELECT **Anomaly detection policy** (alerts only; does not block)
   - IF requirement = scan stored files for sensitive content -> **File policy**
   - IF distractor = **"Unsanction the app"** -> REJECT for real-time conditional control (it only tags the app to generate block scripts for edge devices)
   - IF distractor = Conditional Access policy for monitoring-only identification -> REJECT (CA enforces; a direct MDCA access policy is simpler for monitor-only)
4. **SC-300 Golden Rule:** "Sign-in = **Access**, In-session = **Session**, Repeated-action threshold = **Activity**, Baseline deviation = **Anomaly**, Data at rest = **File**."

---

### 5. Onboarding an App to MDCA Session Control
**Source: Q183**

1. **Exam Scenario:** Enable MDCA to enforce real-time access/session policies on App1.
2. **Key Decision Point:** The onboarding mechanism — a Conditional Access **Session control**, not an app setting.
3. **Decision Path:**
   - IF app must be routed through the MDCA reverse proxy -> CREATE a **Conditional Access policy targeting App1** -> Session controls -> **"Use Conditional Access App Control"** (this redirects sessions through MDCA = onboarding)
   - IF distractor = **Token configuration** -> REJECT (defines token claims; unrelated to proxying)
   - IF distractor = **API permission** -> REJECT (grants the app rights to call APIs like Graph)
   - IF distractor = **"Use app-enforced restrictions"** -> REJECT (the cloud app itself, e.g., SharePoint, enforces limits — not the MDCA proxy)
4. **SC-300 Golden Rule:** "MDCA real-time control begins in Conditional Access: Session → 'Use Conditional Access App Control' = app onboarded."

---

### 6. Security Defaults vs. Conditional Access Exclusivity
**Source: Q184**

1. **Exam Scenario:** Enable security defaults in a tenant containing an existing CA policy (CAPolicy1).
2. **Key Decision Point:** Mutual exclusivity — the tenant blocks security defaults whenever **any** CA policy exists.
3. **Decision Path:**
   - IF any Conditional Access policies exist in the directory -> security defaults CANNOT be enabled -> **delete/disable ALL CA policies first**
   - IF distractor = delete access packages / Identity Governance objects -> REJECT (zero interaction with baseline security defaults)
   - IF distractor = scope an **Authentication administrator** to AU1 -> REJECT (wrong permission AND wrong scope — security defaults are tenant-wide)
4. **SC-300 Golden Rule:** "Security defaults and Conditional Access never coexist — wipe the CA policies before flipping the switch."

---

### 7. External Collaboration Settings (Guest Hardening)
**Source: Q185**

1. **Exam Scenario:** Guests must be unable to read staff email addresses, must only be invitable by User1 (a User Administrator), and must never self-register.
2. **Key Decision Point:** Restriction level per setting in External collaboration settings.
3. **Decision Path:**
   - IF requirement = guests cannot query the directory -> SELECT the **most restrictive** guest access option; REJECT **"limited access"** (it still permits querying certain directory properties → leaks emails)
   - IF requirement = only User1 invites -> SELECT **"Only users assigned to specific admin roles can invite guest users"** (blocks members and other guests)
   - IF requirement = access only via invitation -> SET **self-service sign-up via user flows = No** (Yes would let externals bypass invitations)
4. **SC-300 Golden Rule:** "'Limited' still leaks directory data — go most restrictive, lock invitations to admin roles, and disable self-service sign-up."

---

### 8. Named Locations, Public NAT IPs & per-user MFA Trusted IPs
**Source: Q187**

1. **Exam Scenario:** Users connect via private IPs (10.x / 192.168.x) behind NAT; decide if MFA prompts occur, given a misconfigured named location, a CA policy scoped to trusted locations, and per-user MFA states.
2. **Key Decision Point:** Which IP Entra ID actually evaluates (**public NAT**, never the private IP) and **which engine** evaluates it (CA named locations vs. legacy MFA Trusted IPs).
3. **Decision Path:**
   - IF evaluation is by **Conditional Access** -> compare the **public NAT IP** against named locations; a named location built on a **private subnet (10.10.0.0/16) never matches** → policy conditions fail → CA policy not applied
   - IF CA policy doesn't apply AND per-user MFA = **Disabled** -> NO MFA prompt
   - IF per-user MFA = **Enforced** AND public IP is **outside** the MFA Trusted IP range -> MFA prompt fires (CA targeting is irrelevant to per-user MFA)
   - IF per-user MFA = **Enforced** AND public IP **matches** the MFA Trusted IP range -> MFA is **BYPASSED** (no prompt)
   - IF distractor = matching private/internal IPs against named locations or trusted IPs -> REJECT (Entra only sees the NAT address)
4. **SC-300 Golden Rule:** "Entra sees the NAT, not the NIC. CA reads **named locations**; per-user MFA reads **trusted IPs** — and a trusted IP silences even an Enforced per-user MFA."

---

### 9. Application Consent Governance & Admin Consent Workflow
**Source: Q188, Q189**

1. **Exam Scenario:** (a) Let users consent only to apps requesting low-impact permissions; (b) Determine which designated consent-request reviewers can actually **approve** tenant-wide consent.
2. **Key Decision Point:** Permission **classifications** (pre-step) and the reviewer's **directory role** (approval power).
3. **Decision Path:**
   - IF users should consent only to low-impact apps -> STEP 1: configure **permission classifications** (mark e.g., User.Read as "low impact") → STEP 2: set user consent to allow only apps requesting **low-impact classified** permissions
   - IF distractor = access package / enterprise app collection / access review -> REJECT (resource bundling, My Apps organization, and access recertification do **not** touch consent)
   - IF user is a **designated reviewer** in the admin consent workflow -> they may **view** requests, but approval requires a role authorized to grant tenant-wide admin consent
   - IF reviewer holds **Cloud Application Administrator** or **Application Administrator** -> CAN approve (Admin1 + Admin2)
   - IF reviewer holds **Security Administrator** or **no admin role** (even app owner) -> CANNOT approve (Admin3, User1)
4. **SC-300 Golden Rule:** "Classify first, consent second. Designation ≠ approval: Cloud App Admin & App Admin approve; Security Admin and plain users can only look."

---

### 10. App Registration Platform Requirements
**Source: Q213**

1. **Exam Scenario:** Register a .NET web application for authentication with the Microsoft identity platform.
2. **Key Decision Point:** Platform type → the mandatory registration identifier.
3. **Decision Path:**
   - IF **web application** -> **Redirect URI** is MANDATORY (Entra sends tokens only to pre-registered redirect endpoints — anti-token-theft core of OAuth 2.0/OIDC)
   - IF iOS/macOS app -> bundle ID; IF Android app -> package name
   - IF distractor = executable name for a web app -> REJECT (desktop/native client concept)
4. **SC-300 Golden Rule:** "Web = Redirect URI; iOS/macOS = Bundle ID; Android = Package name. Tokens fly only to registered redirect endpoints."

---

### 11. Least-Privilege Built-in Role Selection Matrix
**Source: Q190, Q196, Q197–200, Q207, Q208**

1. **Exam Scenario:** Repeated "assign the least-privileged role" tasks: access reviews, App Proxy connectors, app registrations, Secure Score updates, password resets, access package management.
2. **Key Decision Point:** Match the **exact capability** to the **narrowest** built-in role; eliminate both over-privileged and under-privileged options.
3. **Decision Path:**
   - IF task = create/manage **access reviews for Azure AD roles** -> SELECT **Identity Governance Administrator** (P2 feature)
     - REJECT **Privileged Role Administrator** (works, but over-privileged — manages all PIM/role assignments)
     - REJECT **User Administrator** (lifecycle of users/groups only — no governance perms)
     - REJECT **User Access Administrator** (that's **Azure RBAC** resource permissions — wrong plane for Azure AD role reviews)
   - IF task = install the **Application Proxy connector** -> SELECT **Application Administrator** or Global Administrator; REJECT **Cloud Application Administrator** (explicitly excluded from App Proxy management)
   - IF task = **register an application** (least privilege) -> SELECT **Application Developer**
   - IF task = **update Secure Score improvement actions** -> SELECT **Security Administrator** (or Global Admin); REJECT **Security Operator** (Secure Score is **read-only**)
     - REJECT SharePoint Administrator / Exchange Administrator / User Administrator (service-scoped roles; no security-posture write access)
   - IF task = **reset passwords** (Password Administrator, org-scoped) -> CAN reset: non-admins **and** other Password Administrators; CANNOT reset: Global Reader and higher admins
   - IF Password Administrator is **scoped to an Administrative Unit** -> CAN manage only AU members AND can never reset org-wide admins (anti-privilege-escalation guard)
   - IF task = manage **access package** settings (e.g., review frequency) -> SELECT **User Administrator** among the offered options (least-privilege fit for entitlement management policy edits)
4. **SC-300 Golden Rule:** "Capability first, privilege second: **Identity Governance Admin** for reviews, **Application Admin (never Cloud App Admin)** for App Proxy, **App Developer** to register, **Security Admin writes / Security Operator reads** Secure Score, and AU-scoped admins can never touch org-wide admins."

---

### 12. PIM: Portals, Activation & Approval Workflows
**Source: Q191, Q201, Q204, Q205** *(License gate: Entra ID P2)*

1. **Exam Scenario:** A user is "eligible" for a role and must activate it; an admin must locate eligible assignments; evaluate MFA/approval/justification settings in PIM configuration.
2. **Key Decision Point:** The keyword **"eligible" → PIM**; role type (Azure AD role vs. Azure resource) → correct PIM blade; workflow settings → per-role configuration exhibit.
3. **Decision Path:**
   - IF question says user is **eligible** for a role -> PIM activation required → navigate to the **Azure AD / Entra admin center** (REJECT Defender portals, M365 admin center — they host no role activation)
   - IF viewing **eligible/active assignments for an Azure AD role** (e.g., Cloud Application Administrator) -> PIM → **Azure AD roles** blade
   - IF managing **Azure resource roles** (Owner/Contributor on subscriptions/RGs) -> PIM → **Azure resources** blade (REJECT for tenant-wide Azure AD roles)
   - IF JIT **group membership/ownership** -> Privileged access groups blade; IF periodic audits -> Review access blade
   - IF PIM setting **"Require MFA on activation" = Yes** -> activation triggers an MFA prompt (Statement = Yes)
   - IF **"Require approval" = No / Approvers = None** -> NO approval request is generated for anyone (Statement = No)
   - IF the **requester is also the designated approver** -> REJECT "approved" — PIM **forbids self-approval**
   - IF Role2 requires approval = No -> requester needs only a justification; the designated approver has no authority over it
   - IF an **approver processes any request** -> the approver MUST supply a justification (even when requester justification was not required)
   - IF user must **edit** Global Administrator PIM assignments -> requires **Privileged Role Administrator or Global Administrator** (REJECT Privileged Authentication Administrator — password/auth-method manager only)
4. **SC-300 Golden Rule:** "Eligible = PIM = P2 = Entra admin center. Azure AD roles → 'Azure AD roles' blade. You can never approve your own activation, and approvers always justify."

---

### 13. Dynamic Membership Rules: Precedence & Null Syntax
**Source: Q193, Q210**

1. **Exam Scenario:** Predict which users join a dynamic group from a mixed-operator rule; write a rule targeting users with no department.
2. **Key Decision Point:** Operator precedence (`-and` > `-or`) and the correct null token.
3. **Decision Path:**
   - IF rule mixes `-and` and `-or` -> evaluate as: **(A -and B -and -not C) -or (D)** — the `-and` block groups first
   - IF user satisfies the entire `-and` block -> member (User1: US + Sales + not Manager)
   - IF user fails the `-and` block but matches the `-or` operand -> member (User2: Finland but Title = SalesRep)
   - IF user fails both branches (Manager + not SalesRep) -> excluded (User3)
   - IF requirement = users with a **missing attribute** -> write `user.department -eq null`
   - IF distractor = `$null` -> REJECT (PowerShell syntax — **invalid** in Entra dynamic rules)
   - IF distractor = `"null"` or `""` -> REJECT (literal strings, not the null state)
4. **SC-300 Golden Rule:** "-and outranks -or; missing attributes match bare `null` — never `$null`, never quotes."

---

### 14. Azure RBAC Assignable Security Principals
**Source: Q194**

1. **Exam Scenario:** Determine which directory objects can receive the Contributor role at resource-group scope.
2. **Key Decision Point:** Is the object a **security principal** (possesses a principal ID)?
3. **Decision Path:**
   - IF object = **user**, **security group** (dynamic membership included), **managed identity** (system-assigned auto-creates a companion service principal), or **enterprise application** (fundamentally a service principal) -> ALL can receive the role assignment
   - IF distractor = "dynamic groups can't hold roles" -> REJECT (membership type does not affect assignability)
   - IF distractor options omit any of the four -> REJECT (all four are valid principals)
4. **SC-300 Golden Rule:** "If it has a principal ID, it can hold a role — users, groups (even dynamic), managed identities, and enterprise apps all count."

---

### 15. Bulk User Creation — Mandatory CSV Properties
**Source: Q203**

1. **Exam Scenario:** Bulk-provision user accounts using the Entra ID CSV upload template.
2. **Key Decision Point:** Mandatory vs. optional attributes.
3. **Decision Path:**
   - IF bulk upload -> MUST include: **displayName**, **userPrincipalName**, **passwordProfile**, **accountEnabled**
   - IF distractor lists givenName / surname as required -> REJECT (optional)
   - IF distractor omits UPN or passwordProfile -> REJECT (a functional, sign-in-able identity cannot be created without them)
   - IF distractor lists usageLocation as required -> REJECT (optional at creation)
4. **SC-300 Golden Rule:** "Name it, log in with it, password it, switch it on: displayName + UPN + passwordProfile + accountEnabled — the four mandatory bulk-create fields."

---

### 16. Log Type Selection for Troubleshooting
**Source: Q206**

1. **Exam Scenario:** Diagnose why User1 cannot access the MDCA portal with **minimal administrative effort**.
2. **Key Decision Point:** Which log category records the event type.
3. **Decision Path:**
   - IF a **user sign-in/access failure** -> SELECT **Sign-in logs** (filter by user + app → error code and failure reason; zero setup)
   - IF administrative/config changes -> **Audit logs** (REJECT for sign-in issues)
   - IF automated account creation/updates -> **Provisioning logs** (REJECT for interactive sign-in issues)
   - IF distractor = **Log Analytics** for a one-off issue -> REJECT (requires prior configuration + KQL — not minimal effort)
4. **SC-300 Golden Rule:** "User can't get in? **Sign-in logs.** Admin changed something? Audit logs. Automation wrote something? Provisioning logs."

---

### 17. Entitlement Management: Custom Extensions & Catalog Distribution
**Source: Q211** *(License gate: Entra ID P2 / Governance licensing)*

1. **Exam Scenario:** Automate actions during access package lifecycle events and make Catalog1's resources available to users.
2. **Key Decision Point:** Building-block mapping within Entitlement Management.
3. **Decision Path:**
   - IF custom extension / automated lifecycle workflow -> CREATE **an Azure Logic App** (custom extensions are powered by Logic Apps)
   - IF distributing a catalog's resources to users -> CREATE/PUBLISH **an access package** (its policies govern requests; extensions hook into lifecycle stages: request created/granted/removed)
   - IF distractor = assigning the catalog directly -> REJECT (catalogs are containers; users receive access only **through access packages**)
4. **SC-300 Golden Rule:** "Catalog = the box, Access package = the product, Policy = the rules, **Logic App = the automation engine**."

---

### 18. Azure AD Connect: OU Filtering, Password Writeback & PTA
**Source: Q214**

1. **Exam Scenario:** Evaluate what synced users can do given OU filtering selections, password writeback, and pass-through authentication settings.
2. **Key Decision Point:** Is the object **synced at all**? + which authentication path is configured?
3. **Decision Path:**
   - IF user's OU is **selected** in Domain/OU filtering -> object syncs → IF "Enable Password writeback" is checked -> user can **SSPR in the cloud and write the change back to on-prem AD** (Statement = Yes)
   - IF "Enable Pass-through authentication" is checked -> cloud sign-ins (e.g., Exchange Online) are validated by an **on-prem agent against a domain controller** (Statement = Yes)
   - IF user's OU is **filtered out** (checkbox cleared) -> object **never syncs** → no Entra object exists → cannot be added to cloud resources like SharePoint Online (Statement = No)
   - IF distractor = "filtered-out user can still be assigned to SPO" -> REJECT (no cloud identity = no assignment)
4. **SC-300 Golden Rule:** "Filtered OU = invisible to the cloud. **Writeback** sends SSPR home; **PTA** validates at the DC; no sync = no cloud access."

---

### 19. SSPR Administrator Policy: Security Questions Ban
**Source: Q212**

1. **Exam Scenario:** Determine which users may use **security questions** for self-service password reset.
2. **Key Decision Point:** Whether the user falls under the unchangeable **administrator SSPR policy**.
3. **Decision Path:**
   - IF user holds a high-privilege admin role (**User Administrator, Password Administrator**, etc.) -> admin SSPR policy applies → **two gates required, security questions FORBIDDEN**
   - IF user is a standard user, or holds a read-only role not bound by the admin reset policy (**Security Reader**) -> custom/user SSPR policy applies → **security questions permitted**
   - IF distractor includes any User/Password Administrator in the "can use security questions" list -> REJECT (admins are structurally excluded)
4. **SC-300 Golden Rule:** "Admins reset with **two gates and NO security questions** — this policy is secure-by-default and cannot be changed."

---

### 20. App Governance Enablement Portal
**Source: Q216**

1. **Exam Scenario:** Enable the app governance add-on for OAuth-enabled apps after licensing.
2. **Key Decision Point:** Which portal hosts the feature toggle (post-consolidation).
3. **Decision Path:**
   - IF enabling/configuring app governance -> **Microsoft 365 Defender portal** (Settings → Cloud Apps → App governance)
   - IF distractor = standalone MDCA portal -> REJECT (management consolidated into the unified Defender portal)
   - IF distractor = M365 admin center / Entra admin center / Purview -> REJECT (licensing, identities, compliance — not app governance toggles)
4. **SC-300 Golden Rule:** "App governance flips on inside **Microsoft 365 Defender** — MDCA's feature, Defender's address."

---

### 21. KQL Aggregation & Visualization
**Source: Q186**

1. **Exam Scenario:** Graphically display sign-in counts grouped by user from Entra logs.
2. **Key Decision Point:** Operator chain order — aggregate first, render second.
3. **Decision Path:**
   - IF aggregation/grouping needed -> use **`summarize`** with `count()` **grouped by** the Identity column
   - IF graphical output required -> use **`render columnchart`**
   - IF distractor = render without summarize -> REJECT (render only draws an existing table; the aggregation must precede it)
4. **SC-300 Golden Rule:** "summarize crunches, render draws — `count()` by Identity, then `columnchart`."# SC-300 Decision Trees - Part 7

# SC-300 Decision Trees & Logic Rules — Part 7 (Questions 217–252)

---

### 1. Least-Privilege Administrative Role Selection (User / License / Helpdesk / Billing Admin)
1. **Exam Scenario:** A single role must be assigned to User1 who must *both* reset user passwords *and* manage license assignments (Q217); another user must only *purchase* a license (Q241).
2. **Key Decision Point:** Match the exact permission bundle to the task set — never over-grant (least privilege).
3. **Decision Path:**
   - IF `task = reset passwords (non-admins) + manage licenses` -> SELECT `User Administrator` (Reason: It is the *only* built-in role covering both password resets for non-admins and license assignment management.)
   - IF `task = password resets only` -> SELECT `Helpdesk Administrator` — REJECT `License Administrator` (Reason: License Administrator cannot reset passwords; Helpdesk Administrator cannot manage licenses.)
   - IF `task = license assignments only` -> SELECT `License Administrator` (Reason: Dedicated license management, no password rights.)
   - IF `task = purchases, subscriptions, invoices` -> SELECT `Billing Administrator` (Reason: Purpose-built for commerce tasks; has zero user/password permissions.)
   - IF `task = purchase a license with least privilege` -> SELECT `Billing Administrator` — REJECT `Global Administrator` (Reason: Global Admin *can* purchase but violates least privilege; REJECT `Permissions Management Administrator` — it manages the service *after* licensing, not the purchase.)
4. **SC-300 Golden Rule:** *"Passwords + Licenses = User Administrator. Purchases = Billing Administrator. If a specialized role exists, Global Administrator is always wrong."*

---

### 2. Combined Registration (MFA + SSPR) & Security Defaults — First Method Logic
1. **Exam Scenario:** A new user hits the combined registration portal in interrupt mode (Q218); a tenant created after October 2019 has Security Defaults enabled (Q221).
2. **Key Decision Point:** Is the method MFA-capable as the **first** registered method?
3. **Decision Path:**
   - IF `first-time registration / Security Defaults active` -> SELECT `Microsoft Authenticator app` (Reason: The only method enforced for initial MFA registration; Security Defaults mandate it out-of-the-box.)
   - IF `method = Email OTP` -> REJECT `as an MFA method` (Reason: Email OTP is SSPR-only; valid as an *additional* SSPR method *after* one MFA-compliant method exists.)
   - IF `method = FIDO2 key or hardware OATH token` -> REJECT `for initial setup` (Reason: Both require an existing MFA method to register — impossible for a brand-new user.)
   - IF `method = Windows Hello for Business` -> REJECT (Reason: Registered via the Windows sign-in experience on a device, not the web-based combined registration portal.)
   - IF `SMS/voice call desired` -> REJECT `under Security Defaults` (Reason: Configurable only after disabling Security Defaults and moving to Conditional Access.)
4. **SC-300 Golden Rule:** *"First method must be MFA-strong: Authenticator app only. Email OTP = SSPR. FIDO2/tokens need MFA first. WHfB lives in Windows, not the web portal."*

---

### 3. Conditional Access Template Deployment Behavior
1. **Exam Scenario:** Admin1 deploys the "Require password change for high-risk users" CA template (Q219).
2. **Key Decision Point:** Who is included/excluded by default when a user-targeting template is deployed?
3. **Decision Path:**
   - IF `CA template targeting user identities is deployed` -> ASSUME `All users included by default` (Reason: Templates default to comprehensive coverage.)
   - IF `the admin creating the policy` -> AUTOMATICALLY `excluded` (Reason: Built-in anti-tenant-lockout safety mechanism.)
   - IF `break-glass/emergency accounts need exclusion` -> MANUALLY `add to exclusion list after deployment` (Reason: No automatic exclusion exists for them.)
4. **SC-300 Golden Rule:** *"Templates = All users by default; the creator is auto-excluded; break-glass exclusions are always manual."*

---

### 4. Conditional Access Device Filters & Null Properties
1. **Exam Scenario:** Block policy with a device filter `displayName -startsWith "Device"` set to *Exclude* filtered devices; devices have mixed join types (Q223).
2. **Key Decision Point:** How do null device properties evaluate against filter operators?
3. **Decision Path:**
   - IF `device is unregistered (Entra join type: None)` -> ALL `device filter properties return null` (Reason: Unregistered devices expose no device data to Conditional Access.)
   - IF `filter uses a positive operator (e.g., -startsWith) on a null property` -> EVALUATES `False (no match)` -> device is **NOT excluded** -> `Block control applies` (Reason: Null never matches a positive expression.)
   - IF `device is Entra-registered AND displayName matches the filter` -> device IS `excluded from the Block policy` -> falls through to the MFA policy -> `access granted after MFA`
   - IF `user is not a member of the policy's assigned group` -> POLICY `skipped entirely` (Reason: Assignment scope is evaluated before conditions.)
4. **SC-300 Golden Rule:** *"Null properties never match positive filters. Unregistered device + positive filter = NOT excluded = policy hits it. Exclusions always override inclusions."*

---

### 5. Microsoft Defender for Cloud Apps — Policy Type Selection
1. **Exam Scenario:** Block a user's *initial* access to a cloud app based on high-risk status detected by Conditional Access (Q220).
2. **Key Decision Point:** Which policy type acts at which stage of the session lifecycle?
3. **Decision Path:**
   - IF `goal = block/allow the initial sign-in in real time (e.g., risky user redirected from CA)` -> SELECT `Access policy` (Reason: Evaluates and enforces controls at sign-in, before session establishment.)
   - IF `goal = govern third-party OAuth app permissions` -> SELECT `OAuth app policy` — REJECT `for sign-in control` (Reason: OAuth policies govern apps, not user sessions.)
   - IF `goal = detect unusual activity and alert` -> SELECT `Anomaly detection policy` — REJECT `as an enforcement tool` (Reason: Detection/alerting only; no pre-defined access control.)
   - IF `goal = act on behavior AFTER access is granted (e.g., mass download)` -> SELECT `Activity policy` (Reason: Acts mid-session, not at the gate.)
4. **SC-300 Golden Rule:** *"Access = the gate at sign-in. Activity = what they do inside. OAuth = app permissions. Anomaly = alerts only, never blocks."*

---

### 6. Defender for Cloud Apps App Connectors (Multi-Cloud OAuth Monitoring)
1. **Exam Scenario:** Monitor OAuth authentication requests across Google Workspace, AWS, and GitHub; a proposed solution adds *only* the Azure connector (Q224, Q225, Q226, Q231).
2. **Key Decision Point:** Does the specific platform's connector exist for the target cloud?
3. **Decision Path:**
   - IF `visibility needed into a SaaS/cloud platform's OAuth apps or auth activity` -> DEPLOY `that platform's dedicated app connector` (Reason: Connectors are the *only* mechanism for API-based visibility.)
   - IF `Google Workspace connector added` -> YES, `Google OAuth apps become monitorable`
   - IF `AWS connector added` -> YES, `CloudTrail logs ingested → auth requests monitorable`
   - IF `GitHub connector added` -> YES, `GitHub OAuth app authorizations monitorable`
   - IF `only the Azure connector is added but target = AWS/Google/GitHub` -> REJECT `the solution` (Reason: The Azure connector provides visibility *only* into Azure — zero cross-cloud coverage.)
4. **SC-300 Golden Rule:** *"One connector per cloud — no connector, no visibility. The Azure connector never covers AWS, Google, or GitHub."*

---

### 7. Admin Consent Request Workflow — Sequencing
1. **Exam Scenario:** Users cannot consent to an app requiring high-privilege permissions; they must be able to *request* admin consent (Q227).
2. **Key Decision Point:** Correct order of workflow configuration steps.
3. **Decision Path:**
   - IF `enabling the admin consent request workflow` -> STEP 1 = `turn ON the feature at tenant level` (Reason: No requests can be submitted or reviewed until enabled.)
   - IF `feature is enabled` -> THEN `designate reviewers` (Reason: Reviewer designation is dependent on the feature being active first.)
   - IF `option = grant admin consent directly from App1's permissions` -> REJECT (Reason: Bypasses the request workflow and violates the least-privilege review intent.)
   - IF `option = Conditional Access policy for App1` -> REJECT (Reason: CA governs *user sign-in* to apps, never app permission consent.)
4. **SC-300 Golden Rule:** *"Enable the workflow FIRST, reviewers SECOND. Consent workflows and Conditional Access live in different worlds."*

---

### 8. PIM Role Support Matrix — Classic Subscription Roles
1. **Exam Scenario:** Admin1 = Account Administrator, Admin2 = Service Administrator, Admin3 = SharePoint Administrator — who can activate via PIM? (Q228)
2. **Key Decision Point:** Is the role an Entra/M365 directory role or a classic Azure subscription role?
3. **Decision Path:**
   - IF `role = Entra directory role or M365 admin role (e.g., SharePoint Administrator)` -> SELECT `PIM-manageable / activatable` (Reason: PIM supports all Entra directory and M365 roles.)
   - IF `role = Account Administrator, Service Administrator, or Co-Administrator` -> REJECT `PIM entirely` (Reason: Classic Azure subscription administrator roles are explicitly unsupported by PIM.)
4. **SC-300 Golden Rule:** *"PIM governs Entra + M365 roles only. Account Administrator and Service Administrator are invisible to PIM — classic subscription roles are always the trap."*

---

### 9. PIM for Groups — Eligibility Rules
1. **Exam Scenario:** Determine which groups can be brought under PIM for Groups JIT membership (Q247).
2. **Key Decision Point:** Group type + membership type + origin (cloud vs. synced).
3. **Decision Path:**
   - IF `group = Security group OR Microsoft 365 group AND membership type = Assigned` -> SELECT `eligible for PIM for Groups`
   - IF `group uses dynamic membership` -> REJECT (Reason: Dynamic groups cannot be PIM-managed.)
   - IF `group is synchronized from on-premises Active Directory` -> REJECT (Reason: PIM for Groups is cloud-only.)
4. **SC-300 Golden Rule:** *"PIM for Groups = cloud-only + Assigned membership. Dynamic or on-prem synced = automatically ineligible."*

---

### 10. PIM vs. Other Entra Governance Services
1. **Exam Scenario:** Evaluate and remediate risk for privileged accounts with JIT access, approvals, MFA on activation, and automated reviews — minimize effort (Q242).
2. **Key Decision Point:** Which service owns the *privileged role lifecycle*?
3. **Decision Path:**
   - IF `requirement = govern/remediate privileged Entra roles (JIT, time-bound, approval, MFA, access reviews)` -> SELECT `Microsoft Entra Privileged Identity Management (PIM)`
   - IF `option = Microsoft Entra Verified ID` -> REJECT (Reason: Decentralized credential issuance/verification — not privileged access governance.)
   - IF `option = Global Secure Access` -> REJECT (Reason: SSE/network security — not identity role governance.)
   - IF `option = Microsoft Entra Permissions Management` -> REJECT `for Entra role governance` (Reason: CIEM rightsizes multicloud *resource* permissions; PIM owns directory role lifecycles.)
4. **SC-300 Golden Rule:** *"Privileged role lifecycle = PIM. Multicloud permission rightsizing = Permissions Management (CIEM). Never swap them."*

---

### 11. Deleted App Registration — Soft Delete & Restoration
1. **Exam Scenario:** Four apps deleted April 5; restoration attempted April 16; which properties return? (Q229)
2. **Key Decision Point:** Is the restore attempt within the 30-day soft-delete window measured from the **deletion date**?
3. **Decision Path:**
   - IF `deletion ≤ 30 days ago` -> SELECT `full restoration possible` (Reason: App object + service principal both recoverable; March update dates are irrelevant noise.)
   - IF `deletion > 30 days ago` -> REJECT `restoration` (Reason: Permanent deletion.)
   - IF `question asks what is restored` -> SELECT `App roles, client secrets (app object) + assigned users/groups, self-service settings (service principal)` (Reason: Both app-object and tenant-specific SP properties survive soft delete.)
4. **SC-300 Golden Rule:** *"Soft delete = 30 days from the DELETION date. Everything comes back — secrets, roles, assignments. Pre-deletion update dates are distractors."*

---

### 12. Entitlement Management — Guest Lifecycle & 30-Day Removal
1. **Exam Scenario:** Access packages expire April 1; will guests still exist in the directory on May 5? (Q230)
2. **Key Decision Point:** Is the guest's access governed *solely* by an expired access package?
3. **Decision Path:**
   - IF `guest's ONLY access = expired access package (no roles, no other packages)` -> `sign-in blocked → removed 30 days after expiration` (Reason: Standard lifecycle policy: April 1 + 30 days = May 1 → gone by May 5.)
   - IF `guest holds an active directory role (e.g., Reports reader)` -> `NOT removed` (Reason: Entitlement Management never removes a guest holding an active directory role, even with an expired package.)
   - IF `guest was added manually (no access package involved)` -> `NOT removed` (Reason: The lifecycle policy only governs access-package-managed external users.)
4. **SC-300 Golden Rule:** *"Expired package + nothing else = gone in 30 days. A directory role = stays forever. Manual add = lifecycle rules don't apply."*

---

### 13. Granting Time-Bound Access — Access Package vs. Alternatives
1. **Exam Scenario:** Automatically grant Group1 members access to a SharePoint site for exactly 90 days with minimal admin effort (Q232).
2. **Key Decision Point:** Is the requirement *granting* access with an expiration, or *reviewing/governing* existing access?
3. **Decision Path:**
   - IF `need to GRANT time-limited access automatically to a group` -> SELECT `Access package with an expiration policy (e.g., 90 days)` (Reason: Auto-assignment policy + automatic expiry = "set-and-forget.")
   - IF `option = access review` -> REJECT (Reason: Reviews *recertify existing* access; they never grant initial access.)
   - IF `option = lifecycle workflow` -> REJECT (Reason: Lifecycle workflows fire on joiner/mover/leaver events, not resource-access durations.)
   - IF `option = Conditional Access policy` -> REJECT (Reason: CA enforces sign-in conditions; it grants no permissions and manages no durations.)
4. **SC-300 Golden Rule:** *"Grant + expire = Access Package. Recertify = Access Review. Joiner/Leaver automation = Lifecycle Workflows. Sign-in rules = Conditional Access."*

---

### 14. Managed Identity Selection — System-Assigned vs. User-Assigned
1. **Exam Scenario:** Multiple App Service apps need storage access (Q233); a single VM needs access (Q236); an app runs on two VMs sharing one identity (Q245); PowerShell enablement (Q239).
2. **Key Decision Point:** How many Azure resources share the identity, and is lifecycle independence required?
3. **Decision Path:**
   - IF `identity needed by MULTIPLE resources/apps` -> SELECT `User-assigned managed identity` (Reason: Standalone resource, shared across resources, permissions configured once.)
   - IF `identity needed by a SINGLE resource` -> SELECT `System-assigned managed identity` (Reason: Tied to the resource lifecycle; simplest for one VM.)
   - IF `system-assigned identity must join an Entra group via PowerShell` -> USE `Get-AzVM` → enable identity → `Get-AzADServicePrincipal -DisplayName <VM name>` (Reason: Enabling creates a service principal sharing the VM's exact display name.)
   - IF `options involve SAS tokens or certificates` -> REJECT (Reason: RBAC on a managed identity eliminates all credential creation/rotation overhead.)
4. **SC-300 Golden Rule:** *"One resource = system-assigned. Many resources = user-assigned. The system-assigned SP inherits the VM's display name."*

---

### 15. Key Vault & Azure RBAC — Least-Privilege Scoping (Data Plane vs. Management Plane)
1. **Exam Scenario:** Managed identity must read *only one secret* (Q234); AKS must read one Cosmos DB account's keys (Q248); users' effective secret access across RGs (Q252).
2. **Key Decision Point:** Scope granularity + whether the role grants *data-plane* or only *management-plane* access.
3. **Decision Path:**
   - IF `access to ONE secret only` -> ASSIGN `Key Vault Secrets User at the individual secret's IAM scope` — REJECT `vault-level IAM` (Reason: Vault scope exposes all secrets; secret scope = strictest.)
   - IF `AKS needs Cosmos DB metadata + listKeys` -> ASSIGN `Cosmos DB Account Reader Role at the ACCOUNT scope` — REJECT `RG/subscription scope` (Reason: Broader scopes expose other Cosmos accounts; REJECT generic `Reader` — management-plane only, cannot list keys.)
   - IF `data-plane secret read required` -> SELECT `Key Vault Secrets User` (Reason: Data-plane role; `Reader`/`Owner` at management plane ≠ reading secret *values*.)
   - IF `role = Owner/Contributor at subscription scope` -> REJECT (Reason: Severe least-privilege violation.)
4. **SC-300 Golden Rule:** *"Scope to the smallest object: secret > vault > RG > subscription. Reader ≠ data access. Secrets User = data plane. Management-plane roles never read secret values."*

---

### 16. Azure ABAC Condition Eligibility
1. **Exam Scenario:** Add attribute-based conditions to role assignments for blob storage (Q235).
2. **Key Decision Point:** Does the role contain **storage data actions**?
3. **Decision Path:**
   - IF `role = Storage Blob Data Reader (or any Blob/Queue storage DATA role)` -> SELECT `ABAC conditions supported`
   - IF `role = Reader, Contributor, or Virtual Machine Contributor` -> REJECT `ABAC conditions` (Reason: All are Azure Resource Manager *management-plane* roles — conditions are unsupported.)
4. **SC-300 Golden Rule:** *"ABAC loves data actions: only Blob/Queue storage data roles qualify. Reader/Contributor = management plane = no conditions."*

---

### 17. API Permission Model — Daemon Apps (Application vs. Delegated)
1. **Exam Scenario:** An app runs as a service calling Microsoft Graph *without user interaction* (Q237).
2. **Key Decision Point:** Is a signed-in user present during the API call?
3. **Decision Path:**
   - IF `app runs without user interaction (daemon/background service)` -> SELECT `Application permissions` (Reason: Admin-consented, app-only token via client credentials — no user context.)
   - IF `app acts on behalf of a signed-in user` -> SELECT `Delegated permissions` — REJECT `for daemon scenarios` (Reason: Delegated requires an interactive user.)
   - IF `option = custom/built-in Entra RBAC role for the Graph call` -> REJECT (Reason: Entra RBAC governs directory administration; API authorization is governed by *API permissions*.)
4. **SC-300 Golden Rule:** *"No user = application permissions + client credentials. Signed-in user = delegated. Graph authorization = API permissions, never RBAC roles."*

---

### 18. Group Naming Policy — Role Exemptions
1. **Exam Scenario:** Which users bypass M365 group naming prefix/suffix and blocked-word policies? (Q238)
2. **Key Decision Point:** Does the user hold one of the two exempted roles?
3. **Decision Path:**
   - IF `user = Global Administrator OR User Administrator` -> `EXEMPT from naming policy across all endpoints` (Reason: These two roles bypass prefix/suffix rules and blocked words.)
   - IF `user = Groups Administrator OR standard user (no admin role)` -> `policy fully applies`
4. **SC-300 Golden Rule:** *"Only Global Admin and User Administrator bypass naming policies. Groups Administrator obeys them — the 'Groups' in the title is the trap."*

---

### 19. Administrative Units (AU) — Scoped Role Authorization
1. **Exam Scenario:** User Administrator scoped to AU Department1 — can they manage users/groups outside it? (Q240)
2. **Key Decision Point:** Is the target object a *direct member* of the AU, and what is the admin's assignment scope?
3. **Decision Path:**
   - IF `admin role scoped to an AU` -> `can manage ONLY objects that are members of that AU`
   - IF `a group is added to the AU` -> `its members do NOT inherit AU membership` (Reason: Adding a group to an AU never places members into the AU.)
   - IF `target object (user or group) is outside the AU` -> REJECT `the management action` (Reason: No authorization over out-of-scope objects.)
   - IF `admin role has Directory (tenant-wide) scope` -> `can manage ALL users regardless of AU membership`
4. **SC-300 Golden Rule:** *"AU scope = objects IN the AU only. Group in AU ≠ members in AU. Directory scope = everyone, everywhere."*

---

### 20. Microsoft Entra Permissions Management (CIEM) — Onboarding & Operations
1. **Exam Scenario:** Onboard additional clouds (Q243); grant User1 first access (Q250); replace User1's permissions with read-only (Q251).
2. **Key Decision Point:** Which clouds are supported, and which console action matches the task?
3. **Decision Path:**
   - IF `onboarding multicloud` -> SELECT `Azure + AWS + GCP only` — REJECT `Alibaba Cloud` (Reason: Only the three major providers are supported.)
   - IF `granting a user initial access to the service` -> ASSIGN `an app role (Reader/User/Administrator) on the Permissions Management enterprise application` — REJECT `creating a security group first` (Reason: Unnecessary extra step for one user; the role assignment is what grants access.)
   - IF `replacing a user's permissions with read-only` -> USE `Remediation tab → Permissions subtab → "Replace with Read-Only" quick action` (Reason: One action revokes everything and grants read-only.)
   - IF `option = Role/Policy Template creation` -> REJECT `for service access` (Reason: Templates define permissions on *monitored resources*, not access to the service itself.)
   - IF `option = My Requests` -> REJECT `for admin remediation` (Reason: My Requests is self-service JIT for users who *already* have access.)
4. **SC-300 Golden Rule:** *"CIEM = Azure/AWS/GCP only. Onboard access = app roles. Remediate = quick actions. My Requests = self-service JIT, never an admin tool."*

---

### 21. Entra Activity Log Retention by License Tier
1. **Exam Scenario:** How long are sign-in and audit logs retained natively in the Entra admin center? (Q244)
2. **Key Decision Point:** Tenant license tier.
3. **Decision Path:**
   - IF `tenant = Entra ID P1 or P2` -> SELECT `30 days (sign-in + audit logs)`
   - IF `tenant = Free edition` -> SELECT `7 days`
   - IF `retention > 30 days required` -> REJECT `native retention` — ROUTE `logs to Log Analytics workspace or Azure Storage via diagnostic settings`
4. **SC-300 Golden Rule:** *"Free = 7 days, P1/P2 = 30 days. Anything longer = export to Log Analytics or Storage. 14/90/365 are never native answers."*

---

### 22. Risk Policy Evaluation — Exclusion Dominance
1. **Exam Scenario:** User in both an included group (risk: High) and an excluded group (Q246) — does the risk policy fire?
2. **Key Decision Point:** Evaluation order: exclusion first, threshold second.
3. **Decision Path:**
   - IF `user is in an EXCLUDED group` -> POLICY `bypassed entirely` — EVEN IF `also included AND risk = High` (Reason: Exclusions always override inclusions.)
   - IF `user included, not excluded, risk ≥ threshold (e.g., "Medium and above")` -> POLICY `applies → control triggers (e.g., require password change)`
   - IF `user included, not excluded, risk < threshold` -> POLICY `does not trigger`
4. **SC-300 Golden Rule:** *"Check exclusion FIRST, threshold SECOND. One excluded group membership kills the policy regardless of risk level."*

---

### 23. Role Comparison Tooling
1. **Exam Scenario:** Compare permissions of up to three Entra built-in roles side-by-side with minimal effort (Q222).
2. **Key Decision Point:** Which portal hosts the native comparison feature?
3. **Decision Path:**
   - IF `side-by-side comparison of up to 3 built-in roles needed` -> SELECT `Microsoft 365 admin center → Role assignments → Compare roles`
   - IF `option = Microsoft Entra admin center` -> REJECT (Reason: Core directory portal, but no native multi-role tabular comparison.)
   - IF `option = Defender or Purview portal` -> REJECT (Reason: Security/compliance scope — no identity role comparison.)
4. **SC-300 Golden Rule:** *"Compare roles lives in the M365 admin center — max 3 roles, side-by-side, zero scripting."*

---

### 24. Application Sign-In Analytics — Usage & Insights Report
1. **Exam Scenario:** Report successful sign-ins, failed sign-ins, and success rate *per application* with minimal effort (Q249).
2. **Key Decision Point:** Pre-calculated app-centric metrics vs. raw log manipulation.
3. **Decision Path:**
   - IF `per-app success/failure counts + success rate needed` -> SELECT `Usage & insights report` (Reason: Pre-aggregated, app-centric — minimal effort.)
   - IF `option = Audit logs` -> REJECT (Reason: Administrative/config changes, not user sign-ins.)
   - IF `option = raw Sign-in logs` -> REJECT (Reason: Requires manual filtering and calculation — fails the "minimize effort" test.)
   - IF `option = Access reviews` -> REJECT (Reason: Access governance, not sign-in analytics.)
4. **SC-300 Golden Rule:** *"App sign-in stats = Usage & insights. Audit = changes. Raw sign-in logs = manual work. Access reviews = governance, not analytics."*

---

## Part 7 High-Frequency Trap Summary

| Trap | Correct Instinct |
|---|---|
| Global Administrator offered for a narrow task | Pick the specialized role (User Admin / Billing Admin) |
| Email OTP as an MFA method | SSPR-only — never MFA |
| PIM + Account/Service Administrator | Classic subscription roles = PIM-unsupported |
| Azure connector covering AWS/Google/GitHub | One connector per cloud — no cross-coverage |
| Reader role reading secrets/keys | Management plane ≠ data plane |
| Exclusion vs. inclusion conflict | Exclusion always wins |
| Access review to *grant* access | Reviews recertify; Access Packages grant |
| Group added to AU = members in AU | False — members never inherit AU membership |
| Permissions Management covering Alibaba | Azure + AWS + GCP only |
| Log retention 90/365 days natively | 30 days max (P1/P2); longer = export |# SC-300 Decision Trees - Part 8

# SC-300 Exam Decision Trees & Logic Rules — Part 8 (Questions 253–288)

---

### 1. Managed Identity — Owner Role Assignability & Cross-Resource Assignment
1. **Exam Scenario:** Determine which managed identities (user-assigned vs. system-assigned) can be assigned Azure RBAC roles, and whether a single user-assigned identity can attach to multiple VMs across regions.
2. **Key Decision Point:** Identity type (user-assigned vs. system-assigned) and whether the resource has its *own* identity or *borrows* one.
3. **Decision Path:**
   - IF the resource has a **system-assigned identity** (VM1, VM2) -> COUNT it as an independent security principal (Reason: it generates its own service principal in Entra ID).
   - IF the resource only *uses* a user-assigned identity (VM3 → Managed1) -> DO NOT count the resource itself; only the user-assigned identity is the principal.
   - IF asked whether a user-assigned identity can attach to multiple resources -> SELECT **Yes, all of them** (Reason: it is a standalone Azure resource; its service principal is global in Entra ID).
   - IF asked whether a user-assigned identity can attach to a VM in a **different region** -> SELECT **Yes** (Reason: created in a region, but usable tenant-wide; region does not constrain assignment).
   - IF asked whether user-assigned and system-assigned identities can coexist on the same VM -> SELECT **Yes** (Reason: VMs support one system-assigned + multiple user-assigned identities simultaneously).
   - REJECT any answer excluding a resource "because of region mismatch" (Reason: false assumption — user-assigned MI service principals are global).
4. **SC-300 Golden Rule:** *System-assigned = 1:1 with the resource; user-assigned = standalone, portable, multi-attach, any region. Only principals with their OWN identity can receive RBAC roles.*

---

### 2. Microsoft Entra Authentication on Windows Server VMs — Troubleshooting
1. **Exam Scenario:** Users cannot sign in to Windows Server Azure VMs with Entra credentials; identify the first troubleshooting action.
2. **Key Decision Point:** Is the root cause network-related (registration blocked) or user/session-related?
3. **Decision Path:**
   - IF Entra sign-in fails on the VMs -> FIRST verify outbound TCP 443 connectivity to `https://enterpriseregistration.windows.net` (Reason: device registration is a prerequisite for Entra logon).
   - IF the issue were user-specific SSO problems -> consider revoking the **Primary Refresh Token (PRT)** (Reason: PRT revocation is per-user, not systemic).
   - IF considering deleting device registrations -> REJECT as a *first* step (Reason: if NSG/Firewall blocks the endpoint, re-registration will fail too).
   - IF the scenario says **Windows Server** -> REJECT SSH/OpenSSH options (Reason: SSH is for Linux; Windows uses RDP).
4. **SC-300 Golden Rule:** *No registration = no Entra logon. Check enterpriseregistration.windows.net over 443 FIRST. SSH = Linux, RDP = Windows.*

---

### 3. Entra Connect — proxyAddresses Uniqueness & Sync Errors
1. **Exam Scenario:** Multiple users are synced claiming the same proxy address; predict which sync successfully and which fail.
2. **Key Decision Point:** Is the `proxyAddresses` value unique across **all objects in the tenant**?
3. **Decision Path:**
   - IF the updated proxy address is **unique** in the tenant -> SYNC succeeds (old alias is safely released).
   - IF **two or more objects** claim the *same* proxy address simultaneously -> BOTH fail with `AttributeValueMustBeUnique` error (Reason: Entra ID strictly enforces proxyAddresses uniqueness tenant-wide).
   - REJECT the idea that releasing an alias by one user automatically guarantees success for two simultaneous claimants (Reason: collision between the two claimants is still fatal).
4. **SC-300 Golden Rule:** *One proxy address = one object. Two objects, one address = AttributeValueMustBeUnique for both.*

---

### 4. Administrative Units — Which Roles Support AU Scope
1. **Exam Scenario:** Assign administrative roles scoped to an Administrative Unit (AU1) instead of the whole tenant.
2. **Key Decision Point:** Does the built-in role appear on the **AU-scoped supported roles list**?
3. **Decision Path:**
   - IF the role is **User Administrator** (also: Password Administrator, Helpdesk Administrator, Groups Administrator, etc.) -> ASSIGN at **both tenant and AU scope**.
   - IF the role is **Security Administrator** (also: Global Administrator, Exchange Administrator, etc.) -> ASSIGN at **tenant scope ONLY** (Reason: broad, tenant-wide security configuration roles are not AU-scopable).
   - REJECT any statement claiming Security Administrator can be scoped to AU1.
4. **SC-300 Golden Rule:** *User/Helpdesk/Password/Groups admins can be AU-scoped; Security/Global/Exchange admins are tenant-wide only.*

---

### 5. Microsoft Entra Permissions Management (CIEM) — Tool Selection
1. **Exam Scenario:** Assess privilege assignment risk across **Azure + AWS + GCP** with minimal administrative effort.
2. **Key Decision Point:** Multi-cloud entitlement visibility vs. identity risk vs. SaaS security vs. threat detection.
3. **Decision Path:**
   - IF the requirement is permission/entitlement analytics across **multi-cloud (Az/AWS/GCP)** -> SELECT **Microsoft Entra Permissions Management** (Reason: purpose-built CIEM with unified view).
   - IF the requirement is threat detection/log analytics -> REJECT **Microsoft Sentinel** (Reason: SIEM/SOAR, not permissions management).
   - IF the requirement is SaaS app/data flow security -> REJECT **Defender for Cloud Apps** (Reason: CASB).
   - IF the requirement is risky sign-ins/leaked credentials in Entra only -> REJECT **Entra ID Protection** (Reason: Entra-scoped identity risk, not multi-cloud permissions).
4. **SC-300 Golden Rule:** *CIEM = Permissions Management (Azure + AWS + GCP). SIEM = Sentinel. CASB = Defender for Cloud Apps. Entra-internal risk = ID Protection.*

---

### 6. SCIM 2.0 — Automated User Provisioning to SaaS Apps
1. **Exam Scenario:** Automatically create/update/delete user accounts in a third-party SaaS application from Entra ID.
2. **Key Decision Point:** Is the requirement **provisioning (lifecycle)** or **authentication/authorization (SSO)**?
3. **Decision Path:**
   - IF the requirement is automated account lifecycle (create/update/deprovision) -> SELECT **SCIM 2.0** (Reason: Entra is the SCIM *client*, the SaaS app is the SCIM *server*).
   - IF the requirement is SSO only -> REJECT **WS-Federation** (Reason: authentication protocol, not provisioning).
   - IF the target is on-premises AD directory queries -> REJECT **LDAP** (Reason: directory query protocol, not SaaS provisioning).
   - IF the requirement is delegated access to resources/tokens -> REJECT **OAuth 2.0** (Reason: authorization framework, not identity provisioning).
4. **SC-300 Golden Rule:** *Provisioning = SCIM 2.0. Sign-on = SAML/WS-Fed/OIDC. Delegated access = OAuth 2.0. Directory queries = LDAP.*

---

### 7. Granting a Managed Identity Access to Azure Resources (Storage / Key Vault)
1. **Exam Scenario:** WebApp1 or VM1 needs to access storage1 blobs or retrieve secrets from Vault1.
2. **Key Decision Point:** RBAC role assignment at the *resource's* scope vs. credential-based access.
3. **Decision Path:**
   - FOR Storage access -> Assign a data-plane RBAC role (e.g., **Storage Blob Data Contributor**) to the identity via **Access control (IAM)** on the storage account.
   - FOR Key Vault secret retrieval -> Assign **Key Vault Secrets User** (read-only) or **Key Vault Secrets Officer** (read + manage) via **Azure RBAC** at the Vault scope.
   - REJECT **access keys / SAS tokens** (Reason: managed identities exist precisely to eliminate credential management).
   - REJECT file share settings / data protection blades (Reason: quotas, soft delete, versioning — not access control).
   - IF the VM already has a system-assigned identity -> REJECT adding a user-assigned identity (Reason: redundant, increases admin effort).
   - IF the Key Vault permission model is already RBAC -> REJECT "configure resource access settings" as the answer (Reason: it's a prerequisite step, not the act of granting access).
4. **SC-300 Golden Rule:** *Managed identity + Azure RBAC role at the target resource's IAM blade. Secrets User = read; Secrets Officer = read/write. Never keys, never SAS.*

---

### 8. External Collaboration — Domain Restriction & Billing
1. **Exam Scenario:** Restrict guest invitations to specific approved external domains; also enable MAU-based billing for External Identities.
2. **Key Decision Point:** Allow/deny domain lists vs. tenant trust vs. federation vs. billing linkage.
3. **Decision Path:**
   - IF the requirement is an **allow-list of ANY external domains** (Entra or not) -> USE **External collaboration settings → Collaboration restrictions** → "Allow invitations only to the specified domains."
   - IF collaboration is only with *specific Entra tenants* -> use **Cross-tenant access settings** (Reason: tenant-to-tenant trust, not a general domain allow-list).
   - IF configuring *how guests authenticate* (Google, Facebook, SAML IdP) -> use **All identity providers** (Reason: federation config, not invitation scope).
   - IF enabling **MAU billing** for External Identities -> USE the **Linked subscriptions** blade (Reason: links the tenant's identity usage to an Azure subscription for billing).
4. **SC-300 Golden Rule:** *Who can be invited = External collaboration restrictions. How they sign in = Identity providers. Tenant trust = Cross-tenant access. B2C/B2B billing = Linked subscriptions.*

---

### 9. Azure RBAC — Least Privilege Role Assignment Counting
1. **Exam Scenario:** User1 needs read access everywhere in a subscription, plus create/restart VMs and create storage accounts **only in RG1**. How many assignments?
2. **Key Decision Point:** Match each *permission delta* to a *scoped* role assignment; minimize count without over-granting.
3. **Decision Path:**
   - Read across the whole subscription -> **Reader @ subscription scope**.
   - Create/restart VMs in RG1 only -> **Virtual Machine Contributor @ RG1**.
   - Create storage accounts in RG1 only -> **Storage Account Contributor @ RG1**.
   - RESULT: **3 assignments** is the minimum.
   - IF a proposed answer merges everything into one broad role (e.g., Contributor @ RG1 + Reader) -> REJECT (Reason: excessive permissions violate least privilege).
   - IF an answer uses 4+ assignments -> REJECT (Reason: not minimal).
4. **SC-300 Golden Rule:** *Count each distinct (role, scope) pair you need. Broad Reader at the top, narrow Contributors at the RG. Least privilege beats fewest clicks.*

---

### 10. Conditional Access — Terms of Use & Grant Controls
1. **Exam Scenario:** Require users to accept a Terms of Use (ToU) agreement before accessing resources.
2. **Key Decision Point:** Which section of the CA policy enforces the requirement vs. which defines targets/signals?
3. **Decision Path:**
   - To enforce acceptance of a document at sign-in -> Configure the ToU (uploaded as **PDF** in Identity Governance) and select it under **Grant controls** in a Conditional Access policy.
   - IF configuring *when* the policy triggers (location, device, risk) -> that is **Conditions** (REJECT as the enforcement point).
   - IF configuring sign-in frequency or App-enforced restrictions -> that is **Session** controls.
   - IF selecting which apps/users are protected -> that is **Target resources / Cloud apps**.
   - REJECT access packages (Entitlement Management bundles resources), lifecycle workflows (joiner/mover/leaver automation), and authentication methods (credentials) as ToU mechanisms.
4. **SC-300 Golden Rule:** *ToU lives in Identity Governance (PDF only!) and is enforced as a GRANT control in Conditional Access. Conditions = when, Grant = what's required, Session = during.*

---

### 11. Certificate-Based Authentication (CBA) — First Step
1. **Exam Scenario:** Enable Entra CBA for users with certificates issued by an on-premises CA (CA1).
2. **Key Decision Point:** Establish trust with the issuing CA before anything else.
3. **Decision Path:**
   - FIRST step -> **Upload CA1's public certificate** to the Entra tenant (Reason: Entra must trust the CA to validate certificate chains and revocation).
   - REJECT auto-enrollment as the first step (Reason: that's later-stage certificate *distribution* to users).
   - REJECT Azure Key Vault (Reason: secrets/keys management, not part of the CBA trust workflow).
   - REJECT Windows Hello for Business (Reason: a separate passwordless method, not a CBA prerequisite).
4. **SC-300 Golden Rule:** *Trust the CA first (upload its public cert), then everything else follows. WHfB ≠ CBA.*

---

### 12. Phishing-Resistant MFA — Authentication Strengths
1. **Exam Scenario:** Enforce phishing-resistant MFA for administrators via Conditional Access.
2. **Key Decision Point:** Which methods satisfy public-key-cryptography-based, phishing-resistant multi-factor authentication?
3. **Decision Path:**
   - SELECT the built-in **"Phishing-resistant MFA"** authentication strength, which includes:
     - **Windows Hello for Business**
     - **FIDO2 security keys**
     - **Certificate-based authentication (MFA)**
   - REJECT **SMS / voice call** (Reason: SIM-swap and social-engineering vulnerable).
   - REJECT **email OTP** (Reason: trivially phishable).
   - REJECT **single-factor certificate-based authentication** (Reason: doesn't satisfy the MFA requirement).
   - CAUTION with **Microsoft Authenticator** (Reason: passwordless phone sign-in is phishing-resistant, but the app also supports OTPs — less precise answer).
4. **SC-300 Golden Rule:** *Phishing-resistant = WHfB + FIDO2 + MFA-grade CBA. Anything OTP/SMS/voice-based is phishable by design.*

---

### 13. PIM State Logic — Eligible vs. Active + Protected Password Resets
1. **Exam Scenario:** Predict whether password resets and role usage succeed given PIM assignments (via groups) and dates.
2. **Key Decision Point:** Assignment state (Active vs. Eligible) at the relevant date + password-reset protection tier of the target.
3. **Decision Path:**
   - IF the target user is *eligible* (not active) for an admin role -> they CANNOT use the role's permissions until they **activate it in PIM** (possibly with MFA/justification).
   - IF a **User Administrator** tries to reset the password of someone who is (even just *eligible* for) a privileged role (Exchange Admin, Global Admin, Security Admin) -> BLOCKED (Reason: User Admins cannot reset passwords of protected-role holders, even eligible ones).
   - IF a **User Administrator** resets the password of another **User Administrator** or a standard user -> ALLOWED (Reason: explicitly permitted by the role's permission set, provided the target holds no higher protected role).
4. **SC-300 Golden Rule:** *Eligible ≠ Active — you must activate. User Admins can't reset privileged-role accounts (even eligible ones), but CAN reset other User Admins and users.*

---

### 14. Entra Connect — OU + Group-Based (Pilot) Filtering
1. **Exam Scenario:** Group-based pilot filtering combined with OU filtering; predict which objects sync.
2. **Key Decision Point:** Is the object a **direct member** of the pilot group AND inside an in-scope OU (including the OU holding the pilot group itself)?
3. **Decision Path:**
   - IF object is in an in-scope OU AND a direct member of the pilot group -> SYNC.
   - IF object is in an in-scope OU but NOT in the pilot group -> FILTERED OUT (Reason: group filtering is a strict whitelist).
   - IF a group is a **direct member** of the pilot group (nested group) -> the **group object itself syncs**, but its members do NOT (Reason: the sync engine does not resolve nested memberships).
   - Ensure the OU containing the pilot group is ALSO selected in OU filtering (Reason: both the group's OU and members' OUs must be in scope).
4. **SC-300 Golden Rule:** *Group filtering = whitelist of DIRECT members. Nested groups sync as objects; their members never do.*

---

### 15. Cross-Tenant Synchronization — Membership & UserType Rules
1. **Exam Scenario:** Predict which users sync to a target tenant via cross-tenant synchronization.
2. **Key Decision Point:** Direct group membership + UserType (Member vs. Guest).
3. **Decision Path:**
   - IF the user is a **direct member** of the in-scope group AND `UserType = Member` -> SYNC succeeds.
   - IF the user is only nested inside a group that belongs to the in-scope group -> NO SYNC (Reason: provisioning engine evaluates direct membership only, no nesting).
   - IF the user is explicitly in scope but `UserType = Guest` -> NO SYNC (Reason: cross-tenant sync supports internal Members only; B2B guests are blocked).
4. **SC-300 Golden Rule:** *Cross-tenant sync = Direct members + Member type only. No nesting, no guests — ever.*

---

### 16. Access Reviews — Scope & Nested Group Limitations
1. **Exam Scenario:** Determine which users fall within an access review scoped to **Guest users only** on Group1, reviewed by the group Owner.
2. **Key Decision Point:** UserType (Guest vs. Member) + direct membership in the reviewed group.
3. **Decision Path:**
   - IF `UserType = Member` -> EXCLUDED (Reason: review scope is Guest users only), regardless of membership.
   - IF the user is in a **nested group** (member of Group2, which is a member of Group1) -> EXCLUDED (Reason: access reviews evaluate DIRECT members only; no nested expansion).
   - IF the user is a Guest but only in Group2, not Group1 -> EXCLUDED (Reason: wrong group scope).
   - The reviewer = the user holding the **Owner** role on the group.
4. **SC-300 Golden Rule:** *Access reviews see DIRECT members only, filtered by the configured scope (e.g., Guests only). Nested = invisible. Member when scope = Guest = invisible.*

---

### 17. Administrative Units — No Transitive Permissions Through Groups
1. **Exam Scenario:** User5 is User Administrator scoped to AU1; Group1 is a member of AU1. Whose passwords can User5 reset?
2. **Key Decision Point:** Is the target **directly placed** in the AU, or only reachable via a group inside the AU?
3. **Decision Path:**
   - IF the user is a **direct member** of AU1 -> WITHIN scope (manageable).
   - IF the user is a member of Group1, where Group1 itself is in AU1 -> OUT of scope (Reason: AU role permissions apply to the group *object* only — e.g., editing group properties — never transitively to its members).
   - IF the user is neither directly in the AU nor in any AU member group -> completely outside scope.
4. **SC-300 Golden Rule:** *AU scopes bind to objects placed IN the unit, not to group membership chains. Group in AU ≠ members in AU.*

---

### 18. Delegating Scoped User Administration — Container Selection
1. **Exam Scenario:** Delegate administrative rights over only the marketing department's users to User1.
2. **Key Decision Point:** Which container supports scoped Entra role assignment?
3. **Decision Path:**
   - IF delegating admin over a subset of Entra users/groups -> CREATE an **Administrative Unit**, add the marketing users, assign the role (e.g., User Administrator) scoped to it.
   - REJECT **Microsoft 365 group** (Reason: collaboration container — mailbox/site — not an admin scope).
   - REJECT **management group** (Reason: Azure subscription governance, not Entra users).
   - REJECT **resource group** (Reason: container for Azure resources, not directory objects).
4. **SC-300 Golden Rule:** *Scoped Entra administration = Administrative Unit. M365 group = collaboration. Management/resource group = Azure resources.*

---

### 19. Conditional Access — App Control (Session Policies) & CAE Configuration
1. **Exam Scenario:** (a) Enforce a Defender for Cloud Apps session policy; (b) enable/disable Continuous Access Evaluation for a specific admin role.
2. **Key Decision Point:** Both live inside a Conditional Access policy — which control section?
3. **Decision Path:**
   - IF deploying Defender for Cloud Apps **session policies** -> FIRST create a CA policy with the Session control set to **"Use Conditional Access App Control"** (Reason: this proxies the session; the MDCA session policy then applies in real time).
     - REJECT Defender-portal-only actions (user monitoring, app onboarding, continuous reports — discovery/investigation, not policy triggers).
   - IF configuring **CAE** (including disabling it) for users holding a role like Application Administrator -> create a CA policy targeting that role and customize CAE under the **Session** controls (Reason: CAE is on by default; only CA policies manage it).
     - REJECT admin consent settings, sign-in risk policies (risk-reactive only), and access reviews (governance, not real-time session control).
4. **SC-300 Golden Rule:** *Session controls in CA policies do double duty: "Use CA App Control" → MDCA session policies; CAE toggle → real-time evaluation. The CA policy is always the trigger.*

---

### 20. Risk-Based Blocking — Fraud Reports & Conditional Access
1. **Exam Scenario:** Users report fraudulent MFA prompts via Authenticator ("Report suspicious activity"); will this automatically block them?
2. **Key Decision Point:** Does the reporting mechanism itself enforce the block, or is a second policy required?
3. **Decision Path:**
   - IF only the MFA notification/"report suspicious activity" setting is configured -> the user's **risk level rises to High**, but **NO automatic block** occurs.
   - To auto-block -> ALSO configure a **Conditional Access policy**: scope = users with **High user risk** + Grant control = **Block access**.
   - REJECT "Yes" answers (Reason: reporting is a prerequisite signal, not the enforcement).
4. **SC-300 Golden Rule:** *Fraud report → risk = High. Risk = High → nothing happens UNLESS a risk-based CA policy blocks it. Signal ≠ enforcement.*

---

### 21. Azure ABAC — Where Role Assignment Conditions Are Supported
1. **Exam Scenario:** Apply attribute-based conditions (e.g., blob index tags) on top of RBAC role assignments for a resource.
2. **Key Decision Point:** Does the service support ABAC conditions on **data actions**?
3. **Decision Path:**
   - IF the target is **Azure Storage** (Blob, Queue, Data Lake Gen2) or **Azure Container Registry** -> ABAC conditions ARE supported.
   - REJECT **Key Vault** (Reason: RBAC/access policies only — no ABAC conditions).
   - REJECT **Virtual Machines** (Reason: no ABAC for compute resources).
   - REJECT **App Services** (Reason: management-plane RBAC and App Service auth only).
4. **SC-300 Golden Rule:** *ABAC conditions = Storage (Blob/Queue/ADLS Gen2) + Container Registry data actions. Nothing else — yet.*

---

### 22. Custom Security Attributes — Who Defines & What Supports Them
1. **Exam Scenario:** Determine who can create custom security attribute definitions and which objects can receive them.
2. **Key Decision Point:** Explicit role assignment + supported object types.
3. **Decision Path:**
   - IF a user needs to create/manage attribute definitions -> they MUST hold **Attribute Definition Administrator** (Reason: even Global Admins lack this permission by default).
   - IF assigning attributes to **users, service principals/enterprise apps, or managed identities** -> SUPPORTED.
   - IF assigning attributes to **security groups or Microsoft 365 groups** -> NOT SUPPORTED (Reason: groups are not valid targets for custom security attributes).
4. **SC-300 Golden Rule:** *Custom security attributes: Global Admin can't define them — Attribute Definition Administrator can. Target users, service principals, and managed identities — never groups.*

---

### 23. Identity Protection — Report Retention
1. **Exam Scenario:** Determine how long Identity Protection (risky users/sign-ins/detections) report data is retained.
2. **Key Decision Point:** Retention window and the escalation path for longer needs.
3. **Decision Path:**
   - Identity Protection reports -> retained **90 days maximum**.
   - IF longer retention is required -> EXPORT to external storage or a **SIEM (e.g., Microsoft Sentinel)**.
   - DO NOT confuse with: **30 days** = Entra audit/sign-in logs retention for P1/P2 tenants.
   - REJECT 60 and 180 days (Reason: not standard Identity Protection retention periods).
4. **SC-300 Golden Rule:** *ID Protection reports = 90 days. Audit/sign-in logs = 30 days (P1/P2). Need more? Export to Sentinel.*

---

### 24. Entra Permissions Management — Onboarding & RBAC Role Selection
1. **Exam Scenario:** Onboard an Azure subscription to Permissions Management, and select the least-privileged role enabling monitoring + right-sized role creation.
2. **Key Decision Point:** Read access for discovery + write access to **role assignments** (not resources).
3. **Decision Path:**
   - Onboarding two steps: (1) Assign **Reader** to the Permissions Management service principal at the subscription scope; (2) Create a **data collector** in the Permissions Management portal.
     - REJECT Application Proxy (remote access to on-prem web apps), Diagnostic settings (log streaming), manual app registration (automated by the wizard), Data Collection Rules (Azure Monitor for VMs).
   - For the operator role -> SELECT **User Access Administrator** (Reason: it can assign roles — `Microsoft.Authorization/*/Write` — enabling right-sized roles, without resource-management rights).
   - REJECT **Reader** (Reason: monitor-only; cannot create role assignments).
   - REJECT **Contributor** (Reason: manages resources but CANNOT manage access/role assignments).
   - REJECT **Owner** (Reason: works but grossly violates least privilege).
4. **SC-300 Golden Rule:** *Onboard = Reader assignment + data collector. Operate = User Access Administrator (role-assignment rights, nothing more). Contributor can't grant; Owner is overkill.*

---

### 25. Defender for Cloud Apps — Cloud App Catalog
1. **Exam Scenario:** Identify cloud apps lacking/with weak authentication standards before they're used in the org.
2. **Key Decision Point:** Researching *inherent* app security attributes vs. discovering *actual usage*.
3. **Decision Path:**
   - IF researching the security characteristics of apps in general (90+ risk factors: SAML support, encryption, etc.) -> USE the **Cloud App Catalog** (31,000+ apps, filterable).
   - IF analyzing apps already used in your org from traffic logs -> that's a **discovered app query / Cloud Discovery** (snapshot = point-in-time from uploaded logs).
   - IF managing permissions consented by users to OAuth apps already connected -> that's an **OAuth app policy**.
4. **SC-300 Golden Rule:** *Catalog = what an app IS. Discovery = what your org USES. OAuth policies = what users CONNECTED.*

---

### 26. Entra ID Logs → Log Analytics — First Step
1. **Exam Scenario:** Stream Entra audit/sign-in logs to a pre-created Log Analytics workspace for querying.
2. **Key Decision Point:** Data path configuration before visualization/querying.
3. **Decision Path:**
   - FIRST step -> Create **Diagnostic settings** for the Entra tenant; select log categories (AuditLogs, SignInLogs) and destination = the Log Analytics workspace.
   - REJECT creating a workbook first (Reason: workbooks visualize data that must already be flowing — a later step).
   - REJECT `Update-MgDomain` / `Update-MgOrganization` cmdlets (Reason: domain/tenant property updates, unrelated to log streaming).
4. **SC-300 Golden Rule:** *No diagnostic settings = no data = nothing to query. Diagnostics first, workbooks later.*

---

### 27. Entra Connect Sync Latency — What Does NOT Fix It
1. **Exam Scenario:** Disabled on-premises accounts still authenticate for up to 30 minutes; evaluate whether Entra Password Protection fixes this.
2. **Key Decision Point:** Password *strength* policy vs. account *state* synchronization schedule.
3. **Decision Path:**
   - IF the problem is the ~30-minute sync delay of the enabled/disabled attribute -> **Entra Password Protection does NOT help** (Reason: it enforces banned/weak password lists, not sync frequency or account state propagation).
   - REJECT "Yes" answers claiming Password Protection accelerates sync (Reason: category error — password hygiene ≠ attribute sync).
4. **SC-300 Golden Rule:** *Password Protection stops weak passwords; it does NOT speed up sync. A disabled on-prem account takes ~30 minutes to block in Entra ID.*

---

### 28. PIM + Key Vault — Role Selection Under PIM Governance
1. **Exam Scenario:** Manage Key Vault access via PIM; User1 must read AND update secrets; User2 must only read secrets.
2. **Key Decision Point:** PIM manages Entra roles + Azure RBAC — NOT Key Vault access policies; then pick the least-privileged Key Vault RBAC role.
3. **Decision Path:**
   - IF the proposal uses Key Vault **access policies** with PIM -> REJECT (Reason: PIM does not natively govern access policies).
   - IF a user needs read + write on secret contents -> assign **Key Vault Secrets Officer**.
   - IF a user needs read-only on secret contents -> assign **Key Vault Secrets User** (least privilege; no modification rights).
4. **SC-300 Golden Rule:** *PIM = Entra roles + Azure RBAC only, never access policies. Secrets User = read. Secrets Officer = read + manage.*

---

### 29. Managed Identities on Azure Services — Valid Combinations
1. **Exam Scenario:** Determine valid identity configurations for VM1 and App1 given dropdown options including a human user account (User1).
2. **Key Decision Point:** Can the identity type legally attach to the resource?
3. **Decision Path:**
   - IF an option includes a **standard Entra user account** (User1) as a resource identity -> ELIMINATE immediately (Reason: user accounts cannot attach to Azure infrastructure resources).
   - IF the resource is a **VM** -> supports one system-assigned + multiple user-assigned MIs → "Managed1 and Managed2 only" style answers are valid.
   - IF the resource is an **App Service app** -> supports one system-assigned MI **plus** one or more user-assigned MIs concurrently → pick the most complete remaining valid option (e.g., "A system-assigned managed identity and Managed2 only").
   - Azure **Storage accounts** also support user-assigned MIs (e.g., for customer-managed keys).
4. **SC-300 Golden Rule:** *Humans never become resource identities. VMs: system + many user-assigned. App Service: system + user-assigned together. Storage: user-assigned works too.*

---

## Quick-Reference Master Rules (Part 8)

| Signal in Question | Correct Answer Instinct |
|---|---|
| Multi-cloud permissions (Az/AWS/GCP) | Entra Permissions Management (CIEM) |
| Auto-provision users to SaaS | SCIM 2.0 |
| Guest invitation domain restriction | External collaboration → Collaboration restrictions |
| MAU billing for External IDs | Linked subscriptions |
| Enforce ToU at sign-in | CA Grant control (document uploaded as **PDF**) |
| Phishing-resistant MFA | WHfB + FIDO2 + MFA CBA (authentication strengths) |
| CBA first step | Upload CA public cert to the tenant |
| PIM manages... | Entra roles + Azure RBAC (never Key Vault access policies) |
| ABAC conditions | Azure Storage + Container Registry data actions only |
| ID Protection retention | 90 days |
| AU-scopable roles | User/Helpdesk/Password/Groups admins — not Security/Global |
| AU + nested group | Group object managed; its members NOT |
| Nested groups (sync/access reviews/CTS) | Direct members only; nested objects sync but members don't |
| Cross-tenant sync | Members only, direct membership only |
| User-assigned MI region | Global usability; multi-attach allowed |
| First troubleshooting for VM Entra logon | `enterpriseregistration.windows.net` over TCP 443 |# SC-300 Decision Trees - Part 9

# SC-300 Decision Trees & Logic Rules — Part 9 (Questions 289–324)

---

### 1. Enforcing Terms of Use (ToU) for Resource Access
1. **Exam Scenario:** The organization must deny access to any user who has not accepted the corporate Terms of Use before reaching a cloud application.
2. **Key Decision Point:** Which enforcement mechanism binds a user *consent action* to *resource access*.
3. **Decision Path:**
   - IF the requirement is to block access until a document is accepted -> SELECT **Conditional Access policy with a Terms of Use Grant control** (Reason: ToU lives in Entra ID and is enforced exclusively as a CA Grant control targeting users + cloud apps).
   - IF the option is a **Defender for Cloud Apps access policy** -> REJECT (Reason: Used for real-time session monitoring/control, not initial sign-in conditions).
   - IF the option is an **Intune compliance policy** -> REJECT (Reason: Evaluates device health — OS version, encryption — not user acceptance of terms).
   - IF the option is **Intune Terms and Conditions** -> REJECT (Reason: Applies only to users *enrolling devices* into Intune, not general tenant resource access).
4. **SC-300 Golden Rule:** *"ToU = Conditional Access Grant control. Intune T&C = enrollment only."*

---

### 2. Onboarding Azure Subscriptions to Entra Permissions Management
1. **Exam Scenario:** You must onboard an Azure subscription into Microsoft Entra Permissions Management with the least privilege possible.
2. **Key Decision Point:** Which RBAC role and cmdlet the first-party CIEM app (App ID `b46c3ac5-9da6-418f-a849-0a07a10b3c6c`) requires.
3. **Decision Path:**
   - IF assigning an *existing* RBAC role to a principal at a scope -> SELECT **`New-AzRoleAssignment`** (Reason: Assigns existing roles; does not create them).
   - IF the goal is to *create a brand-new custom role* -> SELECT `New-AzRoleDefinition` (Distractor trap: only correct when authoring a custom role definition).
   - IF the CIEM app needs to *discover/collect inventory data* -> SELECT the **Reader role at the subscription scope** (Reason: Least privilege — read-only discovery).
   - IF the option is `New-AzTag` -> REJECT (Reason: Applies metadata tags to resources, unrelated to RBAC).
4. **SC-300 Golden Rule:** *"Permissions Management onboarding = Reader role at subscription scope via New-AzRoleAssignment."*

---

### 3. Approval Workflow for New Enterprise Applications
1. **Exam Scenario:** Users must submit a request that designated reviewers approve before new third-party applications are added to the tenant.
2. **Key Decision Point:** Proactive *initial approval* vs. periodic *recertification* of existing access.
3. **Decision Path:**
   - IF users must request approval **before** an app is consented/added -> SELECT the **Admin consent workflow** in the Entra admin center (Reason: Routes user consent requests to designated reviewer admins).
   - IF the option is an **Access Review** -> REJECT (Reason: Access reviews recertify *existing* access periodically; they do not approve *new* app additions).
   - IF the option is a **Cloud Discovery anomaly detection policy** -> REJECT (Reason: Detects unusual app usage patterns reactively, not an approval workflow).
   - IF the option is a **Defender for Cloud Apps app connector** -> REJECT (Reason: Connects to already-sanctioned apps for visibility, not initial approval).
4. **SC-300 Golden Rule:** *"Admin consent workflow = gate for NEW apps. Access review = audit for EXISTING access."*

---

### 4. Acquiring Tokens for Managed Identities (IMDS)
1. **Exam Scenario:** An application on an Azure VM with a managed identity must obtain an access token without stored credentials.
2. **Key Decision Point:** The correct, non-routable endpoint for token requests from inside the VM.
3. **Decision Path:**
   - IF the app runs *inside* an Azure VM and needs a token -> SELECT **Azure Instance Metadata Service (IMDS) at `169.254.169.254`** (Reason: Link-local, non-routable address; returns a JWT for the requested resource).
   - IF the option is `127.0.0.1` -> REJECT (Reason: Loopback/localhost — for services on the VM itself, not IMDS).
   - IF the option is the VM's **public IP** -> REJECT (Reason: Internet-facing communication, not the internal metadata endpoint).
   - IF the option is the VM's **private VNet IP** -> REJECT (Reason: For intra-VNet communication only).
4. **SC-300 Golden Rule:** *"Managed identity tokens always come from 169.254.169.254 — memorize the magic IP."*

---

### 5. Service Principal Credential Types (App-to-App Authentication)
1. **Exam Scenario:** App1 (a service principal) must authenticate non-interactively to App2's API.
2. **Key Decision Point:** Valid credential types for an application identity vs. user/delegated identities.
3. **Decision Path:**
   - IF the app must authenticate *as itself* with no user present -> SELECT **Client secret** OR **Certificate** (Reason: The only two credential types configurable on an app registration; certificate signs a JWT assertion and is more secure).
   - IF the option is **Managed identity** -> REJECT (Reason: An identity *for an Azure resource*, not a credential type on a generic app registration).
   - IF the option is a **User account** -> REJECT (Reason: Delegated flow — app acts on behalf of a signed-in user, not app-only auth).
   - IF the option is a **One-time password** -> REJECT (Reason: An MFA factor for users, never for service principals).
4. **SC-300 Golden Rule:** *"App credentials = Secret or Certificate. Everything else belongs to users or Azure resources."*

---

### 6. Hybrid Authentication: PTA vs. PHS vs. Federation
1. **Exam Scenario:** Password validation must occur **live against on-premises AD DS** with minimal administrative effort, and cloud password changes must write back on-premises.
2. **Key Decision Point:** Where password validation physically occurs + which tool configures the agents.
3. **Decision Path:**
   - IF passwords must be validated **directly against on-prem AD DS** during sign-in -> SELECT **Pass-through Authentication (PTA)** (Reason: Lightweight agent validates against the DC in real time).
   - IF the option is **Password Hash Synchronization (PHS)** -> REJECT (Reason: Validation happens *in the cloud* against synced hashes — fails the "on-prem validation" requirement).
   - IF the option is **AD FS Federation** -> REJECT when "minimize administrative effort" is stated (Reason: Valid against on-prem DCs, but requires heavy infrastructure/cert management; PTA is lighter).
   - IF configuring/deploying PTA agents -> USE **Microsoft Entra Connect** (Reason: The on-prem wizard is the only place to enable PTA; the Entra admin center only *views* status).
   - IF on-prem users must use cloud-initiated **SSPR** -> ENABLE **Password writeback** in Entra Connect (Reason: Bidirectional sync of password changes back to AD DS).
   - IF the option is **Entra Password Protection** -> REJECT (Reason: Blocks weak passwords; not an authentication mechanism).
   - IF the option is **Cross-tenant synchronization** -> REJECT (Reason: Automates B2B user lifecycle across tenants; unrelated to on-prem auth).
4. **SC-300 Golden Rule:** *"On-prem validation + low effort = PTA via Entra Connect. Cloud SSPR for AD users = Password writeback."*

---

### 7. Cross-Tenant Synchronization Object Scope
1. **Exam Scenario:** Multi-tenant org configures cross-tenant sync between contoso.com and fabrikam.com; which objects replicate?
2. **Key Decision Point:** Object types supported by the cross-tenant sync engine.
3. **Decision Path:**
   - IF the object is a **user** -> SELECT **Yes, it syncs** (Reason: The engine natively supports user objects only — created as B2B collaboration users in the target).
   - IF the object is a **Security group** or **Microsoft 365 group** -> SELECT **No, it does not sync** (Reason: Groups can *scope* which users sync, but group objects themselves are never replicated to the target tenant).
4. **SC-300 Golden Rule:** *"Cross-tenant sync = USERS ONLY. Groups scope the sync; they never sync themselves."*

---

### 8. B2B Guest Invitations Failing for One Domain
1. **Exam Scenario:** Guest invitations succeed for fabrikam.com but fail for adatum.com.
2. **Key Decision Point:** Domain-based allow/deny control in External Identities.
3. **Decision Path:**
   - IF invitations fail for a *specific domain* while others succeed -> SELECT **Collaboration restrictions (allowlist/denylist)** in Entra External Identities (Reason: Classic symptom of a domain denylist or a missing domain on the allowlist).
   - IF the option is **Verifiable credentials** -> REJECT (Reason: Decentralized identity for issuing/verifying claims, not domain restrictions).
   - IF the option is **Named locations** -> REJECT (Reason: Trusted IP/country ranges for Conditional Access — network-based, not domain-based).
   - IF the option is **Guest invite settings** -> REJECT (Reason: Controls *who can invite*, not *which domains* are allowed).
4. **SC-300 Golden Rule:** *"One domain blocked, others fine? Check B2B collaboration restrictions (allow/deny list)."*

---

### 9. Inviting an External Contractor (B2B via PowerShell)
1. **Exam Scenario:** A contractor with an outlook.com account must access App1 using their **existing credentials**.
2. **Key Decision Point:** Creating a guest identity vs. creating a member account.
3. **Decision Path:**
   - IF the external user must keep their existing identity -> SELECT **`New-MgInvitation`** (Reason: Creates a guest user object linked to the external identity; B2B collaboration).
   - IF the option is `New-MgUser` -> REJECT (Reason: Creates a native *member* account requiring a new password — violates "existing credentials").
   - IF the option is **External collaboration settings** -> REJECT (Reason: Defines *policy* for who can invite; does not invite a specific user).
   - IF the option is **Entra Connect sync** -> REJECT (Reason: Syncs on-prem AD identities; irrelevant for external users).
4. **SC-300 Golden Rule:** *"Existing external credentials = B2B guest = New-MgInvitation."*

---

### 10. Restoring Soft-Deleted Users
1. **Exam Scenario:** A deleted user account must be restored with all properties, licenses, and group memberships intact.
2. **Key Decision Point:** Retention window + least-privileged role with `microsoft.directory/deletedItems/restore`.
3. **Decision Path:**
   - IF asked how long a deleted user is recoverable -> SELECT **30 days maximum** (Reason: Soft-delete window; after 30 days deletion is permanent and irreversible).
   - IF asked for the least-privileged role that can restore -> SELECT **User Administrator** (Reason: Holds `microsoft.directory/deletedItems/restore`).
   - IF the option is **Helpdesk Administrator** -> REJECT (Reason: Can only reset passwords/force sign-out for non-admins; cannot restore deleted objects).
4. **SC-300 Golden Rule:** *"Deleted users: 30-day soft delete. Restore permission = User Administrator (least privilege)."*

---

### 11. Global Secure Access — Web Filtering & Private Access Enablement
1. **Exam Scenario:** (a) Block a specific URL via the GSA client; (b) enable Entra Private Access traffic forwarding.
2. **Key Decision Point:** Correct configuration sequence and correct blade.
3. **Decision Path:**
   - IF blocking a URL/FQDN via GSA -> FOLLOW the chain: **Web content filtering policy → Security profile (link the policy) → Conditional Access policy with session control selecting the security profile** (Reason: Filtering policies cannot be assigned to users directly — they must ride on a security profile).
   - IF enabling **Private Access** traffic forwarding -> SELECT the **Traffic forwarding blade → enable "Private access profile"** (Reason: This is where forwarding profiles are toggled).
   - IF the option is the **Remote networks** blade -> REJECT for profile enablement (Reason: Defines on-prem sites/connectivity, not traffic profiles).
   - IF the option is **Connectors** -> REJECT (Reason: Manages private network connector agents, not the profile toggle).
   - IF the option is **Security profiles** blade for enabling the profile -> REJECT (Reason: Profiles attach *policies*; they don't enable traffic forwarding).
4. **SC-300 Golden Rule:** *"GSA web filter: Policy → Profile → CA session control. Private Access toggle lives in Traffic forwarding."*

---

### 12. Access Reviews — Scope, Reviewers, and Nested Groups
1. **Exam Scenario:** Access reviews configured on Group1 with various scopes/reviewers; determine who can review whom.
2. **Key Decision Point:** Reviewer assignment mode + scope object types + nested group traversal.
3. **Decision Path:**
   - IF reviewers = **"Users review their own access"** -> SELECT **each member reviews only themselves** (Reason: Self-review is per-user; owners review nothing unless explicitly set; members cannot review guests or others).
   - IF reviewers = **"Group owner(s)"** -> SELECT **only the group owner(s) review all in-scope members** (including reviewing their own access if they are also members).
   - IF the scope is **"All users"** and the object is a **managed identity / service principal** -> SELECT **No — excluded** (Reason: "All users" reviews user objects only; service principals need a dedicated access review type).
   - IF the target user is in a **nested group** (member of Group2, which is nested in Group1) -> SELECT **No — excluded** (Reason: Access reviews evaluate *direct members only* and never traverse nesting).
4. **SC-300 Golden Rule:** *"Access reviews: self-review = review yourself only; nested groups are invisible; service principals need their own review."*

---

### 13. Azure RBAC — Control Plane vs. Data Plane (Storage)
1. **Exam Scenario:** Users hold different roles (Reader, Storage Contributor, Storage Blob Data Reader); who can read blob data and file share data?
2. **Key Decision Point:** Management-plane roles vs. data-plane roles + the `listkeys` escape hatch.
3. **Decision Path:**
   - IF the user holds **Storage Blob Data Reader** -> SELECT **Yes for blob data** (Reason: Explicit data-plane read role), but **No for file share data via portal** (Reason: No key-listing permission and no file data-plane role).
   - IF the user holds **Storage Contributor** -> SELECT **Yes for both** (Reason: Management role, but contains `Microsoft.Storage/storageAccounts/listkeys/action` — can retrieve account keys and use them for data access).
   - IF the user holds only **Reader** -> SELECT **No for both** (Reason: Can view the resource in the portal but grants no data access and no key retrieval).
4. **SC-300 Golden Rule:** *"Reader = see, not touch. Data roles = data access. Contributor = keys = everything."*

---

### 14. Self-Service Application Access & Approvals
1. **Exam Scenario:** App1 has self-service access enabled with "Require approval = Yes"; User1 is already in Group1, User2 is the approver.
2. **Key Decision Point:** Group-based assignment inheritance + where approvals happen.
3. **Decision Path:**
   - IF the user is **already a member** of a group assigned to the app -> SELECT **No request needed** (Reason: Group-based assignment already grants access; self-service flow is bypassed).
   - IF "Require approval = Yes" -> SELECT **No one is added automatically** — even the approver's own request stays pending (Reason: Approval is mandatory before grant).
   - IF asked *where* approvers act -> SELECT **My Apps portal → "Approvals" tab** (Reason: Business approvers use the end-user portal, NOT the Entra admin center).
4. **SC-300 Golden Rule:** *"Already in the group = already has access. Approvals happen in My Apps, never in the admin center."*

---

### 15. Conditional Access Template Safety — Creator Auto-Exclusion
1. **Exam Scenario:** Admin4 creates a CA policy from the "Require MFA for Azure management" template targeting all users. Who is excluded?
2. **Key Decision Point:** Built-in template safeguard behavior.
3. **Decision Path:**
   - IF a CA policy is created **from a template** -> SELECT **the creating admin is automatically excluded** (Reason: Built-in lockout safeguard — Admin4 is excluded).
   - IF evaluating other admins (Admin1/2/3) -> SELECT **they ARE targeted and must perform MFA** (Reason: The template targets *all users*, not just specific directory roles).
   - IF an answer includes the creator in enforcement -> REJECT (Reason: Violates the creator-exclusion safeguard).
   - IF an answer omits non-creator admins -> REJECT (Reason: Template scope is all users, not role-restricted).
4. **SC-300 Golden Rule:** *"CA templates protect their creator: whoever builds the policy is auto-excluded."*

---

### 16. SSPR Verification Methods
1. **Exam Scenario:** Users must verify identity to reset passwords via SSPR; which methods are valid?
2. **Key Decision Point:** SSPR-supported methods vs. sign-in-only methods.
3. **Decision Path:**
   - IF the method is **mobile app notification, mobile app code, email, phone call, SMS, or security questions** -> SELECT **Supported for SSPR**.
   - IF the method is a **Smartcard** -> REJECT (Reason: Certificate-based sign-in only, not SSPR verification).
   - IF the method is a **FIDO2 security key** -> REJECT (Reason: Passwordless *sign-in* method; not available in SSPR flow).
   - IF the method is a **Windows Hello PIN** -> REJECT (Reason: Device-specific credential for Windows sign-in, not SSPR).
4. **SC-300 Golden Rule:** *"SSPR menu: Authenticator (push/code), email, phone, SMS, security questions. FIDO2/Hello/smartcard = sign-in only."*

---

### 17. Key Vault RBAC — Least Privilege Selection
1. **Exam Scenario:** A user must only *view metadata* of keys, secrets, and certificates — never their values.
2. **Key Decision Point:** Read-only metadata vs. secret content vs. crypto operations.
3. **Decision Path:**
   - IF only metadata/properties are needed -> SELECT **Key Vault Reader** (Reason: `Microsoft.KeyVault/vaults/read` — lists objects and properties, no secret values).
   - IF the option is **Key Vault Secrets User** -> REJECT (Reason: Reads actual secret *content* — exceeds metadata-only requirement).
   - IF the option is **Key Vault Crypto User** -> REJECT (Reason: Performs encrypt/decrypt/sign operations — beyond read-only).
   - IF the option is **Key Vault Crypto Officer** -> REJECT (Reason: High-privilege management of crypto objects — far exceeds need).
4. **SC-300 Golden Rule:** *"Metadata = Reader. Secret values = Secrets User. Crypto ops = Crypto User. Always pick the narrowest."*

---

### 18. Global Secure Access — Client vs. Remote Network Coverage
1. **Exam Scenario:** Route internet traffic from ALL on-prem devices (Windows **and** Linux) through Global Secure Access.
2. **Key Decision Point:** Per-device client vs. network-edge tunnel.
3. **Decision Path:**
   - IF devices include **non-Windows OSes** -> SELECT **Create a remote network (IPsec tunnel from the edge router)** (Reason: OS-agnostic — captures traffic from every device with no client install).
   - IF the option is **Deploy the GSA client** -> REJECT (Reason: Client is Windows-only; fails Linux coverage).
   - IF the option is **Create a named location** -> REJECT (Reason: Defines trusted IPs for Conditional Access, not traffic routing).
   - IF the option is **Create an access package** -> REJECT (Reason: Entitlement Management feature for app/group access, unrelated to networking).
4. **SC-300 Golden Rule:** *"Mixed OS fleet = remote network tunnel. GSA client = Windows only."*

---

### 19. Defender for Cloud Apps Log Ingestion from Third-Party Gateways
1. **Exam Scenario:** Gateway1 (third-party web gateway) must feed logs into Defender for Cloud Apps **automatically and continuously** with minimal effort.
2. **Key Decision Point:** Automated continuous ingestion vs. manual snapshot.
3. **Decision Path:**
   - IF logs must flow automatically and continuously -> SELECT **Deploy a log collector** (Reason: Docker-based agent receiving Syslog/FTP from the appliance and auto-uploading to MDCA).
   - IF the option is **Create a snapshot report** -> REJECT (Reason: Manual, one-time log upload — contradicts "automatic").
   - IF the option is **Add a data source** -> REJECT as the *solution* (Reason: A prerequisite step, also used for manual snapshots; the log collector is the automation component).
   - IF the option is **Create an app registration** -> REJECT (Reason: Grants API access to apps, not a log ingestion mechanism).
4. **SC-300 Golden Rule:** *"Continuous gateway logs = Log collector (Docker). Snapshot = manual one-shot."*

---

### 20. Administrative Units — Switching to Dynamic User Membership
1. **Exam Scenario:** AU1 changes from Assigned to "Dynamic User" with rule `(user.department -eq "hr")`. Who remains/is added?
2. **Key Decision Point:** Dynamic engine ownership of membership.
3. **Decision Path:**
   - IF the AU is switched to **Dynamic User** -> SELECT **all manual/static assignments are stripped** (Reason: The dynamic engine takes sole ownership of membership).
   - IF a user's attribute **matches the rule** (Department = HR) -> SELECT **dynamically added**, regardless of prior status.
   - IF a user's attribute **does not match** (Department = IT) -> SELECT **removed**, even if previously assigned.
   - IF the question involves **groups as AU members** under Dynamic User -> SELECT **Not supported** (Reason: Dynamic User AUs cannot contain group members).
4. **SC-300 Golden Rule:** *"Dynamic AU = rule is king. Static assignments wiped; only matching attributes survive."*

---

### 21. Managed Identity Strategy — System-Assigned vs. User-Assigned
1. **Exam Scenario:** Multiple VMs need secure storage of third-party API secrets with minimal administrative effort.
2. **Key Decision Point:** Identity lifecycle binding + secret storage service.
3. **Decision Path:**
   - IF **multiple VMs** share one identity -> SELECT **User-assigned managed identity + Azure Key Vault** (Reason: Standalone resource assignable to all VMs; one permission set to manage; Key Vault is the designated secret store).
   - IF the option is **System-assigned identities** -> REJECT for multi-VM scenarios (Reason: Tied to a single VM's lifecycle — one identity per VM = per-VM permission management).
   - IF the option is **User accounts** (with Key Vault or Storage Encryption) -> REJECT (Reason: Manual credential management contradicts "minimize effort").
   - IF the option is **Storage Service Encryption** -> REJECT (Reason: Protects data at rest; irrelevant to app authentication/secret storage).
4. **SC-300 Golden Rule:** *"Many VMs, one identity = user-assigned MI. Secrets live in Key Vault, never in code."*

---

### 22. Entra Connect Filtering — OU + Group-Based Rules
1. **Exam Scenario:** OU filtering selects OU1 and OU2; group-based filtering uses Group1. Which objects sync?
2. **Key Decision Point:** OU prerequisites evaluated *before* group membership; direct vs. nested membership.
3. **Decision Path:**
   - IF the object is a **direct member of the filtering group** AND resides in a **selected OU** -> SELECT **Syncs** (applies to users and group objects alike).
   - IF the object is in a **synced OU but NOT in the filtering group** -> SELECT **Does not sync** (Reason: Group-based filtering limits sync to direct members only).
   - IF the object is a **nested group inside the filtering group** -> SELECT **The group object itself syncs** (Reason: The engine ignores the nested group's *members*, not the group object — the wizard warning refers to membership, not the object).
4. **SC-300 Golden Rule:** *"OU filter first, then group filter. Nested group object syncs; its members don't."*

---

### 23. Blocking Self-Service Sign-Up (Graph PowerShell)
1. **Exam Scenario:** Prevent users from creating accounts in the tenant via self-service sign-up (email-verified users).
2. **Key Decision Point:** The tenant-wide authorization policy property.
3. **Decision Path:**
   - IF blocking self-service sign-up via Graph PowerShell -> SELECT **`Update-MgPolicyAuthorizationPolicy`** with `allowEmailVerifiedUsersToJoinOrganization = $false` (Reason: Controls the tenant-wide authorization policy).
   - IF the option is `Update-MgDomain` -> REJECT (Reason: No self-service sign-up parameter; that existed only in deprecated `Set-MsolDomain`).
   - IF the option is `Update-MgPolicyPermissionGrantPolicyExclude` -> REJECT (Reason: Governs app *consent* policies, not sign-up).
   - IF the option is `Update-MgDomainFederationConfiguration` -> REJECT (Reason: Configures SSO federation with external IdPs).
4. **SC-300 Golden Rule:** *"Self-service sign-up = Authorization Policy = Update-MgPolicyAuthorizationPolicy."*

---

### 24. Passwordless MFA for Entra ID Linux VM Sign-In
1. **Exam Scenario:** Users sign in to Linux VMs via the Entra ID extension using passwordless, MFA-compliant methods.
2. **Key Decision Point:** Portable, phishing-resistant methods compatible with the device code flow.
3. **Decision Path:**
   - IF the method is **Microsoft Authenticator passwordless phone sign-in** or a **FIDO2 security key / Passkey** -> SELECT **Supported** (Reason: Passwordless + inherently MFA; works with the interactive device code flow from another device).
   - IF the method is **Windows Hello for Business** -> REJECT (Reason: Bound to a specific trusted Windows device — not portable).
   - IF the method is a **Temporary Access Pass** -> REJECT (Reason: Time-limited onboarding/recovery credential, not for ongoing sign-in).
   - IF the method is **SMS** -> REJECT (Reason: Weak second factor paired with a password; fails the "no passwords" requirement).
4. **SC-300 Golden Rule:** *"Passwordless + MFA + portable = Authenticator phone sign-in or FIDO2 passkey."*

---

### 25. Smart Lockout Configuration Location
1. **Exam Scenario:** Customize the lockout threshold and duration for brute-force protection.
2. **Key Decision Point:** Where smart lockout settings live in the Entra admin center.
3. **Decision Path:**
   - IF configuring **lockout threshold/duration** -> SELECT **Password protection settings** (Reason: Smart lockout parameters are managed there).
   - IF the option is **User risk policy** or **Sign-in risk policy** -> REJECT (Reason: Identity Protection features responding to risk events, not base lockout configuration).
   - IF the option is **Authentication strengths** -> REJECT (Reason: Defines permitted MFA method combinations in CA, not lockouts).
4. **SC-300 Golden Rule:** *"Smart lockout lives under Password protection — not under Identity Protection."*

---

### 26. Conditional Access — Exclusion Precedence & Block Overrides Grant
1. **Exam Scenario:** Users in overlapping include/exclude groups across multiple CA policies; who needs MFA, who is blocked?
2. **Key Decision Point:** Two precedence rules: (1) Exclusion beats inclusion; (2) Block beats Grant.
3. **Decision Path:**
   - IF a user is in an **included group only** -> SELECT **Policy applies** (enforce its Grant control, e.g., MFA).
   - IF a user is in **both an included and an excluded group** -> SELECT **Policy does NOT apply** (Reason: Exclusions always override inclusions — no MFA required).
   - IF a user matches **one policy requiring MFA** and another policy that **excludes them** -> SELECT **exclusion wins; no MFA**.
   - IF a user matches a **Block policy** and a **Grant policy** simultaneously -> SELECT **Blocked** (Reason: A single Block control strictly overrides any Grant controls across all applicable policies).
   - IF the only applicable control is a **session control** (e.g., sign-in frequency) -> SELECT **No MFA enforced** (Reason: Session controls do not grant MFA).
4. **SC-300 Golden Rule:** *"Exclusion > Inclusion. Block > Grant. Session control ≠ MFA."*

---

### 27. Azure VM Sign-In Roles — Data Plane Login
1. **Exam Scenario:** Users hold various roles at different scopes; who can sign in to VM1/VM2?
2. **Key Decision Point:** Login roles vs. management roles + scope inheritance.
3. **Decision Path:**
   - IF the user holds **Virtual Machine User Login** or **Virtual Machine Administrator Login** at the **Subscription** scope -> SELECT **Can sign in to ALL VMs** in the subscription (Reason: Role assignments inherit downward through resource groups).
   - IF the login role is scoped to a **single VM** -> SELECT **Can sign in to that VM only**.
   - IF the user holds **Virtual Machine Contributor** -> SELECT **Cannot sign in to any VM** (Reason: Management-plane role — start/stop/delete the VM, but no data-plane OS login).
4. **SC-300 Golden Rule:** *"'Login' roles = sign in. Contributor = manage, not log in. Scope inherits downward."*

---

### 28. Global Secure Access Traffic Profiles + Linked CA Policies
1. **Exam Scenario:** CAPolicy1 requires MFA for GSA-forwarded traffic; the Microsoft traffic profile is linked to RemoteNetwork1 and Group1. Who gets prompted for MFA?
2. **Key Decision Point:** Traffic must actually *reach* GSA (via client OR assigned remote network) AND the user must be in the assigned group.
3. **Decision Path:**
   - IF the user is in the **assigned group** AND traffic reaches GSA (via **GSA client** or via a **remote network assigned to the profile**) -> SELECT **MFA prompted** (Reason: Both client-based and network-based forwarding trigger the linked CA policy).
   - IF the user is in a **non-assigned group** AND on a **non-assigned remote network** -> SELECT **No MFA** (Reason: Traffic bypasses GSA entirely; the CA policy never evaluates it).
   - IF the user is in the assigned group but on an **unassigned remote network** without the client -> SELECT **No MFA** (Reason: No forwarding path to GSA).
4. **SC-300 Golden Rule:** *"GSA CA policies only fire on traffic that actually reaches GSA — group assignment + forwarding path, both required."*

---

### 29. User Consent Restrictions for Applications
1. **Exam Scenario:** Users may consent only to a specific low-impact permission set (e.g., `User.Read`, `profile`).
2. **Key Decision Point:** The consent policy configuration vs. auxiliary features.
3. **Decision Path:**
   - IF restricting which permissions users can consent to -> SELECT **User consent settings** under Enterprise applications → Consent and permissions (Reason: Configure user consent to a defined low-impact permission set — the fundamental first step).
   - IF the option is **Security defaults** -> REJECT (Reason: Disables user consent entirely — contradicts the requirement to *allow* limited consent).
   - IF the option is **Permission classifications** -> REJECT as the primary step (Reason: Optional grouping of permissions; the consent *policy* must be configured first).
   - IF the option is **Identity Protection settings** -> REJECT (Reason: Risk detection/remediation, unrelated to consent).
4. **SC-300 Golden Rule:** *"User consent policy first; classifications are just labels. Security defaults = consent off."*

---

### 30. Group Expiration Policies
1. **Exam Scenario:** Configure an expiration policy for groups and determine the shortest allowed lifetime.
2. **Key Decision Point:** Group type eligibility + minimum lifetime value.
3. **Decision Path:**
   - IF applying an **expiration policy** -> SELECT **Microsoft 365 groups ONLY** (Reason: Expiration is exclusively supported for M365 groups — not Security groups, mail-enabled security groups, or distribution lists).
   - IF asked for the **shortest custom lifetime** -> SELECT **30 days minimum** (Reason: Official specification floor for custom expiration values).
4. **SC-300 Golden Rule:** *"Expiration = M365 groups only. Custom lifetime floor = 30 days."*

---

### 31. Defender for Cloud Apps Session Control — Deployment Order
1. **Exam Scenario:** Enable real-time session-level monitoring for App1 via Conditional Access App Control (CAAC).
2. **Key Decision Point:** Correct sequence — identity integration before session enforcement.
3. **Decision Path:**
   - IF deploying CAAC session control -> FOLLOW the order: **(1) Register App1 in Entra ID → (2) Create a CA policy with session controls → (3) Finalize Connected apps settings in Defender for Cloud Apps → (4) Create the session policy** (Reason: SSO establishes Entra as IdP; the CA session control routes traffic through the MDCA reverse proxy; only then can session policies enforce).
   - IF the option is **Cloud Discovery** -> REJECT (Reason: Identifies shadow IT / unmanaged apps, not controls for a known app).
   - IF the option is a **Traffic forwarding profile** -> REJECT (Reason: A Global Secure Access component for endpoint traffic routing — different mechanism from the CAAC reverse proxy).
   - IF the option is an **OAuth app policy** -> REJECT (Reason: Governs third-party app permissions already granted, not real-time user sessions).
4. **SC-300 Golden Rule:** *"Session control chain: SSO first → CA session control → Connected apps → Session policy. No SSO, no proxy."*