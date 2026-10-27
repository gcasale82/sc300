# SC-300 Decision Trees - Part 1

# SC-300 Decision Trees & Logic Rules — Part 1

---

### 1. Leaked Credentials Detection for Hybrid Identities
1. **Exam Scenario:** Detect compromised passwords for users synchronized from on-premises AD and force remediation.
2. **Key Decision Point:** Does Microsoft Entra ID need to *see* the password hash?
3. **Decision Path:**
   - IF hybrid identities + leaked credential detection required → **Enable Password Hash Synchronization (PHS)** (Identity Protection matches hashes against dark-web/breach data; only credentials leaked *after* PHS is enabled are matched).
   - IF federation (PingFederate/AD FS) → **REJECT** (Entra ID never validates the password, so no hash to compare).
   - IF "prevent weak passwords" → **Password Protection** (proactive banned list, not leak detection).
   - IF "which MFA methods users can use" → **Authentication method policy** (unrelated to credential compromise).
4. **SC-300 Golden Rule:** *No hash, no leak detection — PHS is the price of admission for the "Leaked credentials" risk detection.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-protection/id-protection-faq

---

### 2. Location-Based MFA (Conditional Access)
1. **Exam Scenario:** Require/skip MFA for users connecting from a specific office (e.g., Boston).
2. **Key Decision Point:** Public IP visibility from the cloud.
3. **Decision Path:**
   - IF location-based MFA needed → **Named Locations (public IP ranges) + Conditional Access location condition**.
   - IF private IP ranges (10.x, 192.168.x) → **REJECT** (Entra ID cannot see internal IPs; it only sees the source public IP).
   - IF "Trusted IPs" offered → **REJECT as distractor** (legacy per-user MFA setting; modern best practice is Named Locations + CA).
4. **SC-300 Golden Rule:** *Cloud sees only public IPs — Named Locations + Conditional Access is the modern location control.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/locations

---

### 3. Dynamic Membership Rules (Members vs. Guests)
1. **Exam Scenario:** Group must include all internal users but exclude guests.
2. **Key Decision Point:** `user.userType` attribute.
3. **Decision Path:**
   - IF "all internal users only" → `(user.objectId -ne null) and (user.userType -eq "Member")`.
   - IF guests must be included → `user.userType -eq "Guest"`.
   - IF company/department scoping → add `user.companyName -eq "..."` style clauses.
4. **SC-300 Golden Rule:** *Member = internal, Guest = external — filter on userType, never on group nesting.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/users/groups-dynamic-membership

---

### 4. Password Protection Deployment (Proxy + DC Agent)
1. **Exam Scenario:** Deploy custom banned password list for on-premises AD.
2. **Key Decision Point:** Where the proxy service is installed.
3. **Decision Path:**
   - IF proxy installation target → **domain-joined member server with outbound internet** (e.g., SERVER2), NOT a domain controller.
   - IF custom banned list location → **configured in Microsoft Entra admin center (cloud)**; proxy downloads and distributes it to DC agents.
4. **SC-300 Golden Rule:** *Proxy on a member server, list authored in the cloud, agents on the DCs.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-password-ban-bad-on-premises

---

### 5. Syncing Custom Attributes + License Automation
1. **Exam Scenario:** Sync custom attribute (LWLicenses) from AD and auto-assign licenses based on it.
2. **Key Decision Point:** Schema extension vs. license assignment mechanism.
3. **Decision Path:**
   - IF custom on-prem attribute needed in Entra ID → **Azure AD Connect → Directory Extensions**.
   - IF automatic license assignment by attribute → **Dynamic User security group + group-based licensing** (e.g., `user.extensionAttribute1 -eq "E5"`).
   - IF nested groups offered for licensing → **REJECT** (nested groups are NOT supported for group-based licensing).
4. **SC-300 Golden Rule:** *Directory Extensions brings the attribute; a Dynamic User group spends the license.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-sync-feature-directory-extensions

---

### 6. Who Can Assign Directory Roles vs. Azure RBAC Roles
1. **Exam Scenario:** Delegate management of Entra roles vs. Azure resource roles.
2. **Key Decision Point:** Plane of the role (directory vs. Azure resources).
3. **Decision Path:**
   - IF directory roles (Global Admin, Helpdesk Admin, PIM) → **Privileged Role Administrator**.
   - IF Azure RBAC roles (Owner/Contributor/Reader on subscriptions/RGs) → **User Access Administrator**.
4. **SC-300 Golden Rule:** *Privileged Role Admin = Entra roles; User Access Admin = Azure RBAC.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/permissions-reference

---

### 7. On-Prem App Publishing vs. In-Session Control
1. **Exam Scenario:** Publish on-prem web apps with Entra sign-in; control sessions inside SharePoint Online.
2. **Key Decision Point:** Where the control must apply (edge vs. inside the session).
3. **Decision Path:**
   - IF on-premises app + remote access/CA → **Azure AD Application Proxy** (no VPN needed).
   - IF session controls/DLP/monitoring in a cloud app → **Microsoft Defender for Cloud Apps + Conditional Access App Control**.
4. **SC-300 Golden Rule:** *App Proxy gets users in; Defender for Cloud Apps controls what they do inside.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/app-proxy/application-proxy

---

### 8. Microsoft Sentinel Multi-Stage Attack Detection
1. **Exam Scenario:** Correlate low-fidelity events across sources into one incident.
2. **Key Decision Point:** Detection logic vs. response/visualization.
3. **Decision Path:**
   - IF detection of multi-stage attacks → **Create/customize analytics rules (KQL)**.
   - IF visualization → workbook; IF log ingestion → data connectors; IF automated response → playbook (Logic Apps). All **REJECT** for detection logic.
4. **SC-300 Golden Rule:** *Analytics rules detect, playbooks respond, workbooks visualize.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/azure/sentinel/detect-threats-custom

---

### 9. Restricting App Registration + Delegating to Developers
1. **Exam Scenario:** Block all users from registering apps, but allow specific users to.
2. **Key Decision Point:** Tenant-wide switch + least-privilege role.
3. **Decision Path:**
   - IF tenant-wide restriction → **User settings → "Users can register applications" = No**.
   - IF selective allowance → **Assign the Application Developer role** (`microsoft.directory/applications/create`).
4. **SC-300 Golden Rule:** *Turn it off tenant-wide, then hand the Application Developer role to the few who need it.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/permissions-reference

---

### 10. Delegating Access Package Management (Entitlement Management)
1. **Exam Scenario:** Let non-admins manage application access assignments.
2. **Key Decision Point:** Delegation container in Entitlement Management.
3. **Decision Path:**
   - IF delegating access management → **Create a catalog + assign catalog owners** (first step; then access package managers).
   - IF user consent settings / admin consent requests → **REJECT** (govern app permissions, not access assignment).
   - IF "programs" → **REJECT** (legacy grouping of access reviews only).
4. **SC-300 Golden Rule:** *Catalog first, owner second, access package third — delegation starts at the catalog.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/entitlement-management-delegate-managers

---

### 11. Scoped Helpdesk Administration by Office
1. **Exam Scenario:** Helpdesk admins manage licenses only for users in their branch office.
2. **Key Decision Point:** Administrative scope of a role.
3. **Decision Path:**
   - IF role must apply to a subset of users → **Administrative Units** (one per office) + scoped role assignment (e.g., License Administrator).
   - IF license assignment to users at scale → dynamic group (see #12), NOT an AU.
4. **SC-300 Golden Rule:** *AUs scope admin permissions; groups assign licenses — never confuse the two.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/administrative-units

---

### 12/19. Group-Based Licensing at Scale
1. **Exam Scenario:** Auto-license all users of a company (current and future).
2. **Key Decision Point:** Object type that supports license assignment.
3. **Decision Path:**
   - IF automated, attribute-driven licensing → **Dynamic User security group** (e.g., `user.companyName -eq "ADatum"`) + license on the group.
   - IF distribution group → **REJECT** (email only, no licensing).
   - IF administrative unit → **REJECT** (scopes admin rights, cannot hold licenses).
   - IF OU → **REJECT** (on-prem AD DS construct only).
4. **SC-300 Golden Rule:** *Licensing rides on security groups; dynamic rules make it self-maintaining.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/users/licensing-groups-assign

---

### 13/23. User Risk vs. Sign-in Risk
1. **Exam Scenario:** Self-remediation of compromised identity vs. risky sign-in challenge.
2. **Key Decision Point:** Is the *identity* compromised or the *authentication attempt* suspicious?
3. **Decision Path:**
   - IF leaked credentials / probability identity is compromised → **User risk policy** (remediation = secure password change; requires **SSPR registration** for self-remediation).
   - IF anonymous IP (Tor), unfamiliar sign-in properties, atypical travel → **Sign-in risk policy** (remediation = MFA challenge).
4. **SC-300 Golden Rule:** *User risk = who you are is compromised → password reset; Sign-in risk = this login looks bad → MFA.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-protection/howto-identity-protection-configure-risk-policies

---

### 14/28. PIM Role Settings (JIT Governance)
1. **Exam Scenario:** Implement planned changes / secure a privileged role like User Administrator.
2. **Key Decision Point:** Change the role's *governance settings* vs. its *assignments*.
3. **Decision Path:**
   - IF changing role behavior → **PIM → Role settings** (MFA on activation, max duration, approval).
   - IF JIT + least privilege → **Set assignments to Eligible** + **expire eligible assignments after N** (time-bound).
   - IF "set all assignments to Active" → **REJECT** (standing privilege, opposite of JIT).
   - IF justification/ticket on activation → valid hardening, but not the *core* JIT/time-bound answer.
   - IF access review / administrative unit offered → **REJECT** (reviews validate assignments; AUs scope them — neither changes role settings).
4. **SC-300 Golden Rule:** *Eligible + expiring = just-in-time; role settings govern behavior, assignments govern people.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-configure

---

### 15. Azure AD Connect Sync Scoping
1. **Exam Scenario:** Sync only users from a specific domain/OU.
2. **Key Decision Point:** Scope of sync vs. schedule vs. sign-in method.
3. **Decision Path:**
   - IF selecting which objects sync → **Connect wizard → "Customize synchronization options"** (domain/OU/attribute filtering).
   - IF `Set-ADSyncScheduler` → REJECT (changes schedule only).
   - IF `Start-ADSyncSyncCycle` → REJECT (manual run only).
   - IF "Change user sign-in" → REJECT (changes auth method: PHS/PTA/federation).
4. **SC-300 Golden Rule:** *Filtering = Customize sync options; scheduler = Set-ADSyncScheduler; auth = Change user sign-in.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-install-custom

---

### 16. Publishing On-Prem Web Apps Externally
1. **Exam Scenario:** Remote access to internal app without VPN.
2. **Key Decision Point:** Reverse proxy capability.
3. **Decision Path:**
   - IF external access to on-prem web app → **Azure AD Application Proxy** (pre-auth + SSO).
   - IF Endpoint Manager policy / app config policy → **REJECT** (device/app management, not publishing).
   - IF app registration alone → **REJECT** (prerequisite, not the publishing solution).
4. **SC-300 Golden Rule:** *App Proxy = the VPN-free front door for on-prem web apps.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/app-proxy/application-proxy

---

### 17. Streaming Entra Audit Logs to Log Analytics
1. **Exam Scenario:** Retain/analyze audit and sign-in logs with KQL.
2. **Key Decision Point:** Azure platform log routing mechanism.
3. **Decision Path:**
   - IF exporting Entra logs → **Diagnostic settings** → select AuditLogs/SignInLogs → destination Log Analytics workspace.
   - IF company branding / External Identities / App registrations → **REJECT** (unrelated to log export).
4. **SC-300 Golden Rule:** *Diagnostic settings are the only pipe from Entra logs to Log Analytics.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/monitoring-health/howto-access-activity-logs

---

### 18. External Collaboration via Entitlement Management
1. **Exam Scenario:** Marketing access lifecycle + let fabrikam.com users request access.
2. **Key Decision Point:** Internal packaging vs. external partnership definition.
3. **Decision Path:**
   - IF bundling resources with request/approval/expiration → **Access package**.
   - IF external domain users must request packages → **Connected organization** (fabrikam.com).
4. **SC-300 Golden Rule:** *Access package = what they get; connected organization = who from outside can ask.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/entitlement-management-organization

---

### 20. M&A Forest Sync
1. **Exam Scenario:** Sync a newly acquired forest (litware.com) into an existing tenant.
2. **Key Decision Point:** Parallel, independent sync vs. modifying existing server.
3. **Decision Path:**
   - IF new/disconnected forest, minimal disruption → **Azure AD Connect cloud sync** (lightweight agent, parallel forests).
   - IF adding domain to existing AAD Connect server → valid but heavier/less flexible distractor.
   - IF staging mode → **REJECT** (reads but never writes to Entra ID).
4. **SC-300 Golden Rule:** *Mergers & acquisitions → cloud sync; staging mode is a backup, not a sync engine.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/cloud-sync/what-is-cloud-sync

---

### 21/32. Consent Policies & Permission Classification
1. **Exam Scenario:** Control how apps gain access to org data; allow consent only for low-impact permissions.
2. **Key Decision Point:** Consent settings + permission classification.
3. **Decision Path:**
   - IF governing app data access → **User consent settings** (e.g., "Allow user consent for apps from verified publishers, for selected permissions").
   - IF granular allow-list → **Classify delegated permissions (e.g., User.Read) as low impact**.
   - IF auth methods / access packages / app proxy → **REJECT** (identity proofing, resource access lifecycle, publishing — not app consent).
4. **SC-300 Golden Rule:** *Consent settings set the gate; permission classification defines what passes through.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/configure-user-consent

---

### 22. SSPR for Administrators (Two-Gate Policy)
1. **Exam Scenario:** Admin attempts SSPR with 4 methods configured tenant-wide.
2. **Key Decision Point:** Non-configurable admin SSPR policy.
3. **Decision Path:**
   - IF admin SSPR → **Two gates (2 methods) required, security questions FORBIDDEN** — only Email, Phone, Authenticator usable.
   - IF expecting tenant settings to apply to admins → **REJECT** (admin policy overrides tenant config and cannot be changed).
4. **SC-300 Golden Rule:** *Admins always reset with two gates and never with security questions.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-sspr-policy

---

### 24. Device Limit per User
1. **Exam Scenario:** Sales users hit device-join limit; increase to 10.
2. **Key Decision Point:** Tenant-wide device setting location.
3. **Decision Path:**
   - IF device join/registration limit → **Device settings → "Maximum number of devices per user"**.
   - IF User settings / Access reviews / Security defaults → **REJECT** (none contain device limits).
4. **SC-300 Golden Rule:** *Device limits live in Device settings, nowhere else.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/devices/device-management-portal

---

### 25. Role-Assignable Groups
1. **Exam Scenario:** Group doesn't appear when assigning an Entra role.
2. **Key Decision Point:** `isAssignableToRole` property.
3. **Decision Path:**
   - IF group missing from role assignment list → **Delete and recreate with "Entra roles can be assigned to the group" = Yes** (property is immutable).
   - IF changing membership type / adding owners → **REJECT** (does not affect role assignability).
4. **SC-300 Golden Rule:** *Role-assignable is set at birth and never changed — recreate the group.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/roles/groups-concept

---

### 26. Who Can Create/Manage Access Reviews
1. **Exam Scenario:** Determine which users can administer an access review of an access package.
2. **Key Decision Point:** Required directory roles.
3. **Decision Path:**
   - IF creating/managing access reviews → allowed only for **Global Administrator, User Administrator, or Identity Governance Administrator**.
   - IF user lacks all three → **REJECT**.
4. **SC-300 Golden Rule:** *Global, User, or Identity Governance admin — the only three keys to access reviews.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/deploy-access-reviews

---

### 27. Guest Invitation Failures
1. **Exam Scenario:** Guest invitations blocked/failing.
2. **Key Decision Point:** Invitation process vs. session/sign-in controls.
3. **Decision Path:**
   - IF invitation issues → **External collaboration settings** (who can invite, self-service sign-up, allow/deny domain lists).
   - IF CAE / access reviews / Conditional Access → **REJECT** (govern sessions or existing access, not invitations).
4. **SC-300 Golden Rule:** *Invitations break? Check External collaboration settings first.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/external-collaboration-settings-configure

---

### 29. Per-User MFA (When Conditional Access Is Forbidden)
1. **Exam Scenario:** Require MFA for a group, exempt one user, CA explicitly disallowed.
2. **Key Decision Point:** Legacy per-user MFA portal states.
3. **Decision Path:**
   - IF CA is forbidden → **Per-user MFA portal**: set group members to **Enforced**, exempt user to **Disabled**.
   - IF Conditional Access allowed → always prefer CA (group targeting + exclusions) over per-user MFA.
4. **SC-300 Golden Rule:** *No CA? Per-user MFA: Enforced for the many, Disabled for the exempted few.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/howto-mfa-userstates

---

### 30. Access Review Recipe (Guests on an App)
1. **Exam Scenario:** Monthly review of guest access to Salesforce with auto-removal.
2. **Key Decision Point:** Scope, reviewers, recurrence, automation.
3. **Decision Path:**
   - Target → **Salesforce app, Guest users only**.
   - Reviewers → **Manager** with named **fallback reviewer**.
   - Schedule → **Monthly, 5-day duration**.
   - Automation → **Auto-apply results + "Remove access" on no response**.
4. **SC-300 Golden Rule:** *Scope it, name the reviewer (with fallback), set recurrence, and let auto-apply do the firing.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/deploy-access-reviews

---

### 31. Adding a Resource to an Existing Access Package
1. **Exam Scenario:** Add an app (LinkedIn) to an access package without disturbing others.
2. **Key Decision Point:** Modify vs. recreate.
3. **Decision Path:**
   - IF adding a resource → **Access package → Resource roles → Add resource roles** (additive only).
4. **SC-300 Golden Rule:** *Resource roles is the additive path — never rebuild a package to add one app.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/entitlement-management-access-package-resources

---

### 34. Grant Controls: OR vs. AND
1. **Exam Scenario:** Require compliant device **or** app protection policy for executives.
2. **Key Decision Point:** Grant control combination logic.
3. **Decision Path:**
   - IF "either/or" requirement → select **both controls + "Require one of the selected controls"** (OR).
   - IF "and" requirement → **"Require all the selected controls"** (AND).
4. **SC-300 Golden Rule:** *One of = OR; all of = AND — the radio button is the exam answer.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-grant

---

### 35. Smart Lockout Configuration
1. **Exam Scenario:** Lock after 10 failed attempts for 5 minutes.
2. **Key Decision Point:** Threshold + duration math.
3. **Decision Path:**
   - IF N failed attempts → **Lockout threshold = N**.
   - IF M minutes → **Lockout duration = M × 60 seconds** (5 min = 300 s).
4. **SC-300 Golden Rule:** *Threshold counts failures; duration is always in seconds.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/howto-password-smart-lockout

---

### 36. Blocking Legacy Authentication
1. **Exam Scenario:** Block legacy auth protocols tenant-wide.
2. **Key Decision Point:** Client apps condition + block grant.
3. **Decision Path:**
   - IF blocking legacy auth → **CA policy: All users / All cloud apps → Client apps = Legacy authentication clients → Grant = Block access**.
4. **SC-300 Golden Rule:** *Legacy auth can't do MFA — so don't grant it, block it.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/howto-conditional-access-block-legacy-authentication# SC-300 Decision Trees - Part 2

# SC-300 Decision Trees & Logic Rules — Part 2 (Questions 37–72)

Below is the structured decision-tree extraction from Part 2, validated against current Microsoft Learn documentation. All terminology reflects current Microsoft Entra standards.

---

### 1. My Apps Portal: Collections & Application Visibility (Q37, Q48, Q50, Q52)
1. **Exam Scenario:** Customize the app launcher for a specific team (e.g., "Operations" tab with LinkedIn + Box); troubleshoot why assigned users cannot see an app in My Apps; predict visibility when `User assignment required` and `Visible to users` toggles change.
2. **Key Decision Point:** The three-way interaction of **collection assignment**, **User assignment required?**, and **Visible to users?** — collections organize, they never authorize.
3. **Decision Path:**
   - IF the goal is a *separate tab* of grouped apps for a specific group → SELECT **Create a collection** (requires Entra ID P1/P2), add apps, assign the group under *Users and groups* (Reason: collections render as dedicated tabs for assigned members).
   - IF a collection is assigned to a group → REJECT the assumption that apps inside are auto-assigned (Reason: collection assignment ≠ application assignment; apps must be assigned separately).
   - IF `Visible to users? = No` → the app is HIDDEN from My Apps and the Microsoft 365 app launcher **for everyone**, including directly assigned users.
   - IF `User assignment required? = No` + `Visible to users? = Yes` → ALL tenant users can sign in AND see the app tile.
   - IF `User assignment required? = Yes` and a group cannot see the app → ADD the group directly under **Users and groups** (Reason: only explicit assignments surface the app).
   - IF least privilege is required → REJECT "set assignment required to No" (Reason: it exposes the app tenant-wide).
4. **SC-300 Golden Rule:** *Visibility = (Direct assignment) AND (Visible to users = Yes). Collections curate the launcher; they never grant access.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/application-properties • https://learn.microsoft.com/entra/identity/enterprise-apps/access-panel-collections

---

### 2. Enterprise App Access: Self-Service Requests & Group Types (Q48, Q51, Q53)
1. **Exam Scenario:** Company policy requires users to request access to applications with an approval step; determine which group types are valid for app assignment; verify who owns a group after an approver is designated.
2. **Key Decision Point:** **Self-service settings** vs. other app blades; the `securityEnabled` attribute of the group being assigned.
3. **Decision Path:**
   - IF users must *request* access → SELECT **Self-service settings** on the enterprise app (Reason: it creates the My Apps request workflow with optional approver).
   - IF a user is set as the self-service **approver** → REJECT the claim this changes group ownership (Reason: approver ≠ group owner; ownership is unchanged).
   - IF assigning a group to an enterprise app → SELECT **Security groups only** (native security principals).
     - Distribution groups → REJECT (`securityEnabled = false`).
     - Microsoft 365 groups → REJECT (not security-enabled by default).
     - Mail-enabled security groups → REJECT (Exchange-managed, read-only via Graph, not the supported native path).
   - IF a group is **nested** inside the assigned group → its members do NOT inherit the app; they must request access via self-service.
4. **SC-300 Golden Rule:** *App assignment speaks only "security group" — one level deep. Requests flow through Self-service settings, not provisioning or app roles.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/assign-user-or-group-access-portal • https://learn.microsoft.com/entra/fundamentals/how-to-manage-groups

---

### 3. Role-Assignable Groups + PIM Eligible Assignments (Q38)
1. **Exam Scenario:** Create a group that can receive Microsoft Entra role assignments, where members must explicitly **activate** the role before using its permissions.
2. **Key Decision Point:** The group-creation property **"Microsoft Entra roles can be assigned to the group"** + the PIM **assignment type**.
3. **Decision Path:**
   - IF the group must be assignable to Entra roles → SET **"Azure AD roles can be assigned to the group" = Yes** at creation (role-assignable group).
   - IF members must *activate* the role → assign the group to the role **through PIM** with assignment type **Eligible** (Reason: eligible = candidate status requiring activation; active = standing access).
   - IF the role is assigned directly (IAM/roles blade) → REJECT (Reason: members get permanent standing access — fails the activation requirement).
   - IF a regular (non-role-assignable) security group is used → REJECT (Reason: Entra roles cannot be assigned to it).
4. **SC-300 Golden Rule:** *Role-assignable group = the container; PIM "Eligible" = the activation gate. Direct assignment = standing access = wrong answer when "activate" appears.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/groups-concept • https://learn.microsoft.com/entra/id-governance/privileged-identity-management/groups-assign-member-owner

---

### 4. PIM Activation Mechanics: Approval, Duration, MFA & Identity Types (Q42, Q44, Q45)
1. **Exam Scenario:** Interpret PIM exhibits — eligible assignments, approver groups, activation maximum duration, eligibility end dates, "Require MFA on activation," and which identity types (guests, managed identities) support eligible assignments.
2. **Key Decision Point:** Eligible vs. Active semantics; role-setting boundaries; interactive vs. non-interactive identities.
3. **Decision Path:**
   - IF assignment type = **Eligible** → the user has NO standing permissions; activation workflow is mandatory.
   - IF the approver group contains the requestor → self-approval is blocked; only the **other** members of the approver group can approve.
   - IF activation is approved *before* the eligibility window expires → the active assignment runs its **full approved duration** (e.g., 5 hours) and is NOT truncated by eligibility expiry.
   - IF Activation maximum duration = 8 hours → any requested duration ≤ 8h succeeds (2h is valid).
   - IF the user already satisfied MFA at sign-in → PIM's "Require MFA on activation" is satisfied by the **existing MFA claim** in the session token (no second prompt).
   - IF identity = B2B **guest user** → eligible assignments ARE supported (interactive sign-in, MFA, justification all possible).
   - IF identity = **managed identity or service principal** → ONLY **active** assignments are possible (Reason: non-interactive; cannot complete activation steps).
4. **SC-300 Golden Rule:** *Eligible = candidate, not permission. Approvers can't approve themselves. An activated role runs its full clock. Only interactive identities (users, guests) can activate — service principals and managed identities are Active-only.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/groups-assign-member-owner • https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-configure

---

### 5. Entitlement Management: Connected Organizations & Domain Blocking (Q39, Q46)
1. **Exam Scenario:** Allow external users from two partners to request an access package — one partner is an Entra tenant with two verified domains, the other is a third-party IdP with two domains; simultaneously block one specific domain of an allowed tenant.
2. **Key Decision Point:** Connected-organization granularity = **per logical partner entity/directory**, not per domain; domain-level exclusion = B2B deny list.
3. **Decision Path:**
   - IF partner = an Entra tenant → ONE connected organization automatically covers **all verified domains** (and subdomains) of that tenant.
   - IF partner = third-party IdP with multiple domains → add **multiple domains into one** connected organization.
   - Minimum count = number of logical partner entities (scenario answer: **2**).
     - REJECT 1 (Reason: distinct entities with different identity models cannot merge).
     - REJECT 3/4 (Reason: one connected org per domain is unnecessary — domains consolidate).
   - IF one domain of an allowed tenant must be blocked → the connected org **cannot selectively exclude** a verified domain → use **External collaboration settings → B2B collaboration allow/deny list** (deny the specific domain).
4. **SC-300 Golden Rule:** *Connected org = per partner entity, all domains included. Domain-level surgery = B2B deny list, never the connected org.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/entitlement-management-organization • https://learn.microsoft.com/entra/external-id/allow-deny-list • https://learn.microsoft.com/entra/id-governance/entitlement-management-access-package-create

---

### 6. Access Reviews Scope (Q40)
1. **Exam Scenario:** Determine which resources support access reviews: security group membership, enterprise app assignments, Microsoft Entra roles, and Azure resource roles.
2. **Key Decision Point:** Resource type → which review engine covers it (Identity Governance vs. PIM).
3. **Decision Path:**
   - IF target = group memberships or enterprise app assignments → SELECT **Access reviews (groups & applications)** in Identity Governance.
   - IF target = Microsoft Entra roles or Azure resource roles (e.g., Contributor on a subscription) → SELECT **PIM access reviews** (Reason: PIM extends reviews to privileged roles in both planes).
   - IF target = access package assignments → SELECT **Entitlement management access reviews**.
   - IF an answer option excludes apps, Entra roles, or Azure roles → REJECT it (Reason: all four resource classes are reviewable — the correct answer includes everything).
4. **SC-300 Golden Rule:** *Access reviews span groups, apps, access packages, and PIM roles (Entra + Azure). If the exam lists all four — pick all four.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/create-access-review • https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-create-roles-and-resource-roles-review

---

### 7. Activity Logs: Source, Format, Retention & Streaming to Azure Monitor (Q41, Q43, Q47)
1. **Exam Scenario:** Analyze Conditional Access policy usage in a SIEM; answer a sign-in log retention question; make Entra logs visible in Azure Monitor.
2. **Key Decision Point:** Sign-in vs. audit logs; JSON vs. CSV; license tier; diagnostic settings as the ingestion prerequisite.
3. **Decision Path:**
   - IF analyzing which CA policies were *evaluated/applied* during authentication → SELECT **Sign-in logs** (Reason: they capture per-event CA policy results).
   - IF the target is a SIEM needing nested CA detail → SELECT **JSON** (Reason: CSV flattens the hierarchical policy structure and loses fidelity).
   - IF the log source is "audit logs" for policy usage → REJECT (Reason: audit logs record *administrative changes* to policies, not their application at sign-in).
   - IF retention question → Free = **7 days**; P1/P2 = **30 days** (audit & sign-ins); longer retention = archive via **Azure Monitor/Storage** (not a default).
   - IF logs must appear in Azure Monitor → configure **Diagnostic settings** → *Send to Log Analytics workspace* FIRST.
     - REJECT `Get-AzureADAuditDirectoryLogs` (one-time pull, no streaming).
     - REJECT creating a workbook first (Reason: workbooks visualize data already in the workspace).
     - REJECT `Set-AzureADTenantDetail` (tenant properties, unrelated).
4. **SC-300 Golden Rule:** *Sign-in logs show what policies fired; audit logs show who changed them. JSON preserves the nesting; Diagnostic settings feed Azure Monitor; P1/P2 = 30 days.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/monitoring-health/reference-reports-data-retention • https://learn.microsoft.com/entra/identity/monitoring-health/howto-integrate-activity-logs-with-azure-monitor-logs

---

### 8. Conditional Access Anatomy: Grant vs. Session Controls (Q56, Q60)
1. **Exam Scenario:** Configure MFA enforcement, sign-in frequency limits, and country/IP restrictions within a Conditional Access policy.
2. **Key Decision Point:** Which policy bucket hosts each control — **Conditions**, **Grant**, or **Session**.
3. **Decision Path:**
   - IF enforcing MFA, device compliance, authentication strength, terms of use, or password change → SELECT **Access controls → Grant**.
   - IF controlling re-authentication cadence (e.g., every 8 hours) → SELECT **Session → Sign-in frequency**.
   - IF restricting by IP ranges or countries/regions (e.g., HighRiskCountries) → SELECT **Conditions → Locations** (named locations).
   - IF placing sign-in frequency under Grant, or named locations under Access controls → REJECT (Reason: classic bucket-confusion distractors).
4. **SC-300 Golden Rule:** *Grant = "what must be true to get in." Session = "what happens during the session." Locations = a Condition, never a control.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-grant • https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-session

---

### 9. Conditional Access Prerequisites & Legacy Authentication Blocking (Q57, Q62)
1. **Exam Scenario:** Conditional Access options are grayed out in a new tenant; force Modern authentication for Exchange Online by blocking basic-auth clients (POP3/IMAP4/legacy Outlook).
2. **Key Decision Point:** Security defaults state; the **Client apps** condition.
3. **Decision Path:**
   - IF CA policy creation/management is unavailable → **Disable Security defaults first** (Reason: they are mutually exclusive; new tenants have them enabled).
   - IF legacy/basic auth must be blocked → CA policy: Target Exchange Online (or all resources) → **Conditions → Client apps** = *Exchange ActiveSync* + *Other clients* → **Grant = Block access**.
   - IF the proposed fix is an Intune compliance policy, an app control profile, or a Defender for Cloud Apps OAuth policy → REJECT (Reason: they govern device posture, app execution, or third-party OAuth consents — not authentication protocols).
4. **SC-300 Golden Rule:** *Security defaults OFF before CA goes ON. Legacy authentication dies by the Client apps condition + Block grant — never by Intune or MCAS.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/security-defaults • https://learn.microsoft.com/entra/identity/conditional-access/policy-block-legacy-authentication

---

### 10. Identity Protection: Risk Classification & Remediation (Q58, Q64)
1. **Exam Scenario:** Remediate risky sign-ins in real time without blocking access; remediate leaked credentials while keeping the user productive.
2. **Key Decision Point:** **Sign-in risk** vs. **user risk**; the matching remediation control and its prerequisites.
3. **Decision Path:**
   - IF sign-in risk policy must remediate without blocking → SELECT **Require MFA** as the grant control (prerequisite: users registered for MFA).
   - IF the detection is **leaked credentials** → classify as **User risk** (Reason: the identity itself is compromised, independent of any session).
   - IF user risk is high but access must continue → CA policy with **user risk condition → Require password change** (secure SSPR flow remediates the risk).
     - Constraint: the policy must target **all resources** and cannot combine with other grant controls.
   - IF the proposed fix is access reviews, password protection, or SSPR alone for sign-in risk → REJECT (Reason: reviews are periodic governance; password protection is preventative; SSPR is the *user-risk* prerequisite, not the sign-in-risk remediation).
4. **SC-300 Golden Rule:** *Sign-in risk → MFA challenge. User risk (leaked creds) → password change via SSPR. Both remediate without blocking.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-protection/concept-identity-protection-policies • https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-grant

---

### 11. Per-User MFA States & MFA Account Lockout (Q63, Q65)
1. **Exam Scenario:** Predict per-user MFA states after sign-in events; interpret the MFA account lockout screen (which denials count, when the block clears).
2. **Key Decision Point:** The **Disabled → Enabled → Enforced** state machine; lockout scope = secondary-factor denials only.
3. **Decision Path:**
   - IF state = **Disabled** → sign-ins trigger NO registration; the state remains Disabled.
   - IF state = **Enabled** → the next modern-auth sign-in prompts registration; completing it **auto-transitions to Enforced**.
   - IF state = **Enforced** → it stays Enforced; "Remember MFA on trusted devices (X days)" changes prompt *frequency*, never the directory state.
   - IF the screen is the MFA **account lockout** page → it counts only **secondary authentication denials** (e.g., wrong Authenticator code); wrong passwords fall under Entra ID **Smart Lockout** instead.
   - IF "Minutes until account is automatically unblocked" = 30 → the block self-clears after 30 minutes.
4. **SC-300 Golden Rule:** *Enabled + successful registration = automatic Enforced. MFA lockout counts only second-factor failures and expires on its own timer.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/howto-mfa-userstates

---

### 12. SSPR Authentication Methods (Q61)
1. **Exam Scenario:** Users must supply one authentication method to reset their password; identify which configured methods are valid.
2. **Key Decision Point:** The valid SSPR method list + the one-method-vs-two-methods constraint.
3. **Decision Path:**
   - IF only **one** method is required → verification code is the only option; the Authenticator app **cannot** be the sole method.
   - IF **two** methods are required → notification OR code may be used alongside other enabled methods.
   - IF the candidate method is **Mobile app notification** or **Mobile app code** → SELECT both (both are distinct, valid SSPR methods).
   - IF the candidate is "email to an address in your organization" → REJECT (Reason: SSPR email must be a pre-registered *external* contact address).
   - IF the candidate is "home phone" → REJECT (Reason: the listed methods are *Mobile phone* and *Office phone*).
4. **SC-300 Golden Rule:** *SSR methods = Mobile app (notification/code), Email (external), Mobile/Office phone, Security questions — and Authenticator can never stand alone.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-sspr-howitworks • https://learn.microsoft.com/entra/identity/authentication/tutorial-enable-sspr

---

### 13. Hybrid Identity: PTA High Availability & Failover Behavior (Q59, Q67)
1. **Exam Scenario:** Install an additional Pass-through Authentication agent for HA when Domain Controllers are forbidden internet access; predict who can sign in when on-premises internet connectivity is lost.
2. **Key Decision Point:** Agent placement constraints (outbound 443, no DCs) + the **absence of automatic PTA→PHS failover**.
3. **Decision Path:**
   - IF HA is required → install extra PTA agent(s) on a **separate member server** (Microsoft recommends 2 extra agents beyond the Entra Connect server).
   - IF the candidate host is a Domain Controller → REJECT (Reason: security policy forbids DC internet access; the agent requires outbound 443).
   - IF the candidate already runs Entra Connect + the primary agent → REJECT (Reason: no redundancy against a single server failure).
   - IF on-prem connectivity is lost → synced users CANNOT sign in; PTA does **NOT** automatically fail over to PHS — an admin must **manually switch** the sign-in method in Entra Connect.
   - IF the user is cloud-only (not directory-synced) → authentication is unaffected by the outage.
4. **SC-300 Golden Rule:** *PTA agents need internet and never live on DCs. PHS is a manual backup, not an automatic failover — cloud-only users never notice the outage.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-pta-faq • https://learn.microsoft.com/entra/identity/hybrid/connect/choose-ad-authn

---

### 14. Microsoft Entra Connect Staging Mode (Q70)
1. **Exam Scenario:** Deploy a high-availability standby sync server that minimizes failover time.
2. **Key Decision Point:** **Staging mode** + pre-processing the full sync pipeline on the standby.
3. **Decision Path:**
   - IF a standby server is required → install Entra Connect on Server2 with **staging mode enabled** (Reason: it imports and processes but never exports).
   - IF preparing for rapid failover → run **full import** for all connectors, then **delta synchronization** for all connectors (Reason: pre-populates the metaverse and pending exports).
   - IF Server1 fails → disable staging mode on Server2; only a final **export** remains — minimal downtime.
   - IF the standby is installed *without* staging mode → REJECT (Reason: two active exporters cause synchronization disasters).
4. **SC-300 Golden Rule:** *Staging server = warm standby: import always, sync always, export never — until you flip the switch.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-sync-staging-server

---

### 15. External Identities: B2B Collaboration & MAU Billing (Q66, Q68)
1. **Exam Scenario:** Grant users from another division/tenant access to SharePoint Online without extra licensing costs; enable Monthly Active Users (MAU) billing for External Identities.
2. **Key Decision Point:** B2B guest invitations vs. sync/proxy alternatives; the billing activation mechanism.
3. **Decision Path:**
   - IF external users need tenant resource access without extra licenses → SELECT **B2B collaboration guest invitations** (Reason: first **50,000 MAU/month are free**; SharePoint natively supports external sharing).
   - IF the proposal is one Entra Connect server syncing to multiple tenants → REJECT (Reason: 1:1 server-to-tenant mapping only).
   - IF the proposal is Application Proxy → REJECT (Reason: it publishes *on-premises* web apps, not cross-tenant cloud services).
   - IF the proposal is syncing one forest to multiple tenants via separate servers → REJECT (Reason: duplicate cloud identities, management overhead, no native external-access model).
   - IF MAU billing must be enabled → **link the tenant to an Azure subscription** (Reason: linkage auto-enables MAU-based billing for External Identities).
   - IF the proposal is an access review, terms of use, or a user flow → REJECT (Reason: none establish a billing relationship).
4. **SC-300 Golden Rule:** *B2B guests ride the free 50k MAU tier; billing switches on the moment you link an Azure subscription.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/external-identities-pricing • https://learn.microsoft.com/entra/external-id/customers/faq-customers

---

### 16. Group-Based Licensing & Nested Groups (Q69)
1. **Exam Scenario:** An E5 license is assigned to Group1, which directly contains User1, User3, and two nested groups (Group2, Group4) — count the licenses consumed.
2. **Key Decision Point:** The licensing engine evaluates **direct members only** — nesting is invisible.
3. **Decision Path:**
   - IF a license is assigned to a group → only **direct user members** receive it.
   - IF nested groups exist under the licensed group → their members are **ignored** (Reason: applying licenses to nested security groups is unsupported).
   - IF an answer counts nested users → REJECT it (Reason: no inheritance in group-based licensing).
4. **SC-300 Golden Rule:** *Licensing sees exactly one level deep: direct members only. Nested groups are invisible to the license engine — same rule as app assignment.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/how-to-manage-groups

---

### 17. App Registrations: Least Privilege Role & App-Only (Daemon) Access (Q49, Q54)
1. **Exam Scenario:** The tenant setting "Users can register applications" = No; grant exactly one admin the ability to register apps; enable an autonomous web service (App1) to read directory data from Microsoft Graph without a signed-in user.
2. **Key Decision Point:** Built-in role scope (least privilege); **Application** vs. **Delegated** permissions; admin consent.
3. **Decision Path:**
   - IF one admin must register apps under a locked-down tenant setting → SELECT the **Application developer** role (Reason: can create registrations, owns own apps, cannot manage others' apps).
   - IF the proposal is **Cloud application administrator** → REJECT (Reason: over-privileged — manages ALL registrations and enterprise apps).
   - IF the proposal is an Azure RBAC role (App Configuration Data Owner, Managed Application Contributor) → REJECT (Reason: wrong control plane — Azure resources, not Entra app registrations).
   - IF a service needs Graph data with **no user present** → client credentials flow: 1) **app registration**, 2) **Application permissions** (e.g., `Directory.Read.All`), 3) **admin consent** (mandatory for application permissions).
   - IF the proposal uses Delegated permissions → REJECT (Reason: delegated requires a signed-in user context).
4. **SC-300 Golden Rule:** *Least privilege for registration = Application developer. App-only access = Application permissions + admin consent — delegated permissions need a human.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/permissions-reference

---

### 18. Conditional Access App Control (Defender for Cloud Apps) (Q55)
1. **Exam Scenario:** Enable real-time session monitoring for App1 by routing traffic through the Defender for Cloud Apps reverse proxy.
2. **Key Decision Point:** The mandatory **onboarding sequence** — CA routes the traffic, MDCA polices the session.
3. **Decision Path:**
   - IF real-time session control is required → execute in order:
     1. Publish App1 in Entra ID (establish SSO/identity provider).
     2. Create a CA policy targeting App1 with the **"Use Conditional Access App Control"** session control.
     3. Onboard the app under **Connected apps** in Defender for Cloud Apps.
     4. Create the **session policy** in MDCA (monitor/block activities, e.g., sensitive downloads).
   - IF a session policy is created before the app is onboarded/traffic routed → REJECT (Reason: MDCA cannot enforce what it never sees).
4. **SC-300 Golden Rule:** *Conditional Access routes the traffic; Defender for Cloud Apps watches the session. Onboard before you police.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-session

---

### 19. Tenant Settings: Self-Service Sign-Up & Attribute Source of Authority (Q71, Q72)
1. **Exam Scenario:** Block users from creating accounts via self-service sign-up; determine which users' **Job title** and **Usage location** can be edited directly in Microsoft Entra ID.
2. **Key Decision Point:** Tenant-wide cmdlet parameter; **source of authority** for synced vs. cloud-only attributes.
3. **Decision Path:**
   - IF self-service sign-up must be blocked → `Set-MsolCompanySettings -AllowAdHocSubscriptions $false` (tenant-level setting).
   - IF the proposal is `Set-MsolDomainFederationSettings`, `Update-MsolFederatedDomain`, or `Set-MsolDomain` → REJECT (Reason: federation/domain configuration, not sign-up policy).
   - IF the user is **directory-synced** → on-premises AD is the source of authority for Job title/Department/Manager → these are **read-only** in the cloud.
   - IF the user is **cloud-only** (member or guest, not synced) → Job title is editable in Entra ID.
   - IF the attribute is **Usage location** → editable for **ALL** users (Reason: it is an Entra licensing-compliance attribute, not mapped from on-prem AD by default).
4. **SC-300 Golden Rule:** *Synced user = on-prem owns the attribute (Usage location excepted — always cloud-editable). Cloud-only = Entra owns it. Ad-hoc sign-ups die with `-AllowAdHocSubscriptions $false`.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/reference-connect-sync-attributes-synchronized • https://learn.microsoft.com/powershell/module/msonline/set-msolcompanysettings

---

## Quick-Reference Matrix (Question → Decision Tree)

| Questions | Decision Tree | Core Discriminator |
|---|---|---|
| Q37, Q48, Q50, Q52 | 1. My Apps Collections & Visibility | Assignment + Visible toggle |
| Q48, Q51, Q53 | 2. Self-Service & Group Types | Security groups only; no nesting |
| Q38 | 3. Role-Assignable Groups | PIM Eligible = activation |
| Q42, Q44, Q45 | 4. PIM Activation Mechanics | Eligible vs Active; interactive identities |
| Q39, Q46 | 5. Connected Organizations | Per entity; B2B deny list for domains |
| Q40 | 6. Access Reviews Scope | Groups + Apps + PIM roles = all |
| Q41, Q43, Q47 | 7. Logs: Source/Format/Retention | Sign-in logs, JSON, Diagnostic settings |
| Q56, Q60 | 8. Grant vs Session Controls | MFA=Grant; Sign-in frequency=Session |
| Q57, Q62 | 9. CA Prerequisites & Legacy Auth | Disable security defaults; Client apps |
| Q58, Q64 | 10. Risk Classification | Sign-in risk→MFA; User risk→password change |
| Q63, Q65 | 11. Per-User MFA States | Enabled→Enforced auto-transition |
| Q61 | 12. SSPR Methods | Authenticator can't stand alone |
| Q59, Q67 | 13. PTA HA & Failover | No auto PHS failover; no DC agents |
| Q70 | 14. Staging Mode | Import+sync always, export never |
| Q66, Q68 | 15. B2B & MAU Billing | 50k free MAU; link subscription |
| Q69 | 16. Group-Based Licensing | Direct members only |
| Q49, Q54 | 17. App Registration & App-Only | Application developer; app permissions + consent |
| Q55 | 18. CA App Control | Onboard in MDCA before session policy |
| Q71, Q72 | 19. Tenant Settings & Source of Authority | Synced = read-only; usage location = editable |# SC-300 Decision Trees - Part 3

# SC-300 Decision Trees & Logic Rules — Part 3 (Questions 73–108)

All terminology and references validated against official Microsoft Learn documentation.

---

### 1. Group Nesting Rules by Group Type (Q73, Q87)

1. **Exam Scenario:** You must determine which users/groups can be added as members of a Security group, a Microsoft 365 group, or a mail-enabled security group.
2. **Key Decision Point:** The **type of the parent group** — nesting capability is dictated by the parent, not the child.
3. **Decision Path:**
   - IF parent is a **Security Group** (Assigned or Dynamic) → CAN contain: users, devices, **other security groups** (nested), and Microsoft 365 groups as members (Reason: security groups support full nesting).
   - IF parent is a **Microsoft 365 Group** → CAN contain **only users and guests** → REJECT any attempt to nest a security group or another Microsoft 365 group inside it (Reason: Microsoft 365 groups support **no nesting whatsoever**).
   - IF parent is a **Mail-enabled Security Group** → members must be **mail-ready and licensed** → REJECT unlicensed users (Reason: mail-enabled groups resolve members through Exchange/Microsoft 365 workflows requiring licenses).
   - IF the question asks about **license inheritance** → only **direct (first-level) user members** inherit; nested group members do NOT inherit licenses.
4. **SC-300 Golden Rule:** *"Security groups nest; Microsoft 365 groups never nest — they hold only users and guests."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/faq-groups

---

### 2. Seamless SSO Browser Configuration (Q74)

1. **Exam Scenario:** Users on domain-joined corporate devices must be signed in automatically to cloud resources without credential prompts.
2. **Key Decision Point:** The Kerberos endpoint must be trusted by the browser — the **Local Intranet zone**.
3. **Decision Path:**
   - IF Seamless SSO is deployed → ADD `https://autologon.microsoftazuread-sso.com` to the **Local Intranet security zone** via Group Policy (Reason: browsers only forward Kerberos tickets to URLs explicitly placed in the intranet zone).
   - IF the option is **Enterprise State Roaming** → REJECT (Reason: it syncs user settings/app data across devices, unrelated to SSO authentication).
   - IF the option is **Sign-in options (Windows Hello/PIN)** → REJECT (Reason: local device authentication, not cloud SSO).
   - IF the option is **Azure AD Connect Authentication Agent** → REJECT (Reason: that agent belongs to **Pass-through Authentication (PTA)**, a different sign-in method).
4. **SC-300 Golden Rule:** *"Seamless SSO = Kerberos ticket + intranet zone entry for autologon.microsoftazuread-sso.com."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-sso-quick-start

---

### 3. Custom Domain Validation Sequence (Q75)

1. **Exam Scenario:** A custom domain (contoso.com) must become the default UPN suffix for all newly created users.
2. **Key Decision Point:** Strict **order of operations** for domain validation.
3. **Decision Path:**
   - Step 1 → **Add/Register** the domain in the Microsoft 365 admin center.
   - Step 2 → Create a **TXT record** in the DNS hosting provider's zone (proof of ownership).
   - Step 3 → **Verify** the domain in the admin center after DNS propagation.
   - Step 4 → Set the domain as **Primary (default)** so new users automatically receive it as UPN suffix.
   - IF an option says "delete the .onmicrosoft.com domain" → REJECT (Reason: the initial `.onmicrosoft.com` domain is **permanently required** for tenant routing and backend management).
4. **SC-300 Golden Rule:** *"Add → TXT → Verify → Set Primary. Never delete .onmicrosoft.com."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/add-custom-domain

---

### 4. Azure Monitor Alert Recipients: Action Groups vs. Roles vs. DCRs (Q76–Q78)

1. **Exam Scenario:** Alerts for failed sign-ins must be redirected from one administrator to a new security administrator.
2. **Key Decision Point:** Where are alert **notification recipients** defined? → **Action Groups**, nothing else.
3. **Decision Path:**
   - IF the goal is to change who **receives alert emails** → MODIFY the **Action Group** attached to the alert rule (remove old email, add new email) → ✅ YES.
   - IF the solution is "assign the Insights administrator role" → ❌ NO (Reason: role assignments grant **view/manage** permissions; they do not alter existing alert rules or notification recipients).
   - IF the solution is "create a Data Collection Rule (DCR)" → ❌ NO (Reason: a DCR governs **telemetry collection/transformation/destinations** (e.g., Log Analytics), not alerting logic or recipients).
   - IF the solution is "modify the alert rule's action group" → ✅ YES (Reason: this is the standard, direct method).
4. **SC-300 Golden Rule:** *"Roles grant visibility, DCRs move data — only Action Groups decide who gets the alert."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/azure/azure-monitor/alerts/action-groups

---

### 5. MFA Fraud Reporting vs. Block/Unblock vs. Account Lockout (Q79–Q81)

1. **Exam Scenario:** Users must be **automatically blocked** when they report an MFA prompt as fraudulent via Microsoft Authenticator.
2. **Key Decision Point:** Which MFA service setting reacts to a **user-initiated fraud report**?
3. **Decision Path:**
   - IF the requirement is "automatically block users who report fraud" → ENABLE **Report suspicious activity** (formerly the "Fraud alert" feature) in Entra MFA service settings → ✅ YES (Reason: users reporting a suspicious prompt are set to **High User Risk**, integrated with Identity Protection for risk-based remediation).
   - IF the solution is "Block/unblock users" → ❌ NO (Reason: this is a **manual administrative action** — an admin must explicitly block each user; no automation).
   - IF the solution is "MFA Account Lockout" → ❌ NO (Reason: lockout triggers after a **number of denied MFA attempts** in a timeframe — anti-brute-force, not fraud-report-driven).
   - ⚠️ **Current-state note (Microsoft Learn):** As of March 1, 2025, the legacy Block/unblock users, Fraud alert, and Notifications features were **removed** and replaced by **Report suspicious activity** (Entra ID → Authentication methods → Settings). Exam answers still map "Fraud alert" to this feature.
4. **SC-300 Golden Rule:** *"Fraud report = automatic block; Block/unblock = manual; Account Lockout = brute-force protection."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/howto-mfa-mfasettings

---

### 6. Group-Based Licensing: Supported Group Types (Q82)

1. **Exam Scenario:** Determine which groups can receive Microsoft 365 E5 licenses via group-based licensing.
2. **Key Decision Point:** Membership type must target **users** — group type may be Security or Microsoft 365.
3. **Decision Path:**
   - IF group is **Security, Assigned** → ✅ Supported.
   - IF group is **Security, Dynamic User** → ✅ Supported (Reason: dynamic **user** membership is valid for licensing; requires Entra ID P1 for members).
   - IF group is **Microsoft 365, Assigned** → ✅ Supported.
   - IF group is **Microsoft 365, Dynamic User** → ✅ Supported.
   - IF group is **Dynamic Device** (or any device-targeted membership) → ❌ REJECT (Reason: group-based licensing is **user-based**; device groups cannot receive user licenses).
   - IF the parent group contains nested groups → ✅ Still supported for **direct assignment** to the parent, but only **immediate first-level user members** inherit the license.
4. **SC-300 Golden Rule:** *"Licensing follows users: Security or M365 groups, Assigned or Dynamic-User — never Dynamic-Device."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/users/licensing-groups-assign

---

### 7. B2B Guest Authentication: Email One-Time Passcode (Q83)

1. **Exam Scenario:** Determine which guest users authenticate with an email OTP when accessing a shared SharePoint library.
2. **Key Decision Point:** Is the guest **new (unredeemed invitation)** and lacking any existing account/federation?
3. **Decision Path:**
   - IF guest is **new**, has no Microsoft account, no Entra account, no federated IdP → ✅ Receives **Email OTP** at redemption.
   - IF guest is an **existing/redeemed guest** in the tenant → ❌ No OTP (Reason: OTP applies only to **new invitations**; established guests use their existing credentials).
   - IF user is an **internal member** of the tenant's native domain → ❌ No OTP (Reason: internal users authenticate natively with company credentials).
4. **SC-300 Golden Rule:** *"Email OTP = fallback for NEW guests only; redeemed guests and internal users never see it."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/one-time-passcode

---

### 8. Bulk License Removal — Least Administrative Effort (Q84)

1. **Exam Scenario:** Remove Office 365 E3 licenses from 2,500 users with the least effort.
2. **Key Decision Point:** GUI **bulk operation** vs. per-object scripting.
3. **Decision Path:**
   - IF a bulk GUI path exists → SELECT **Licenses blade** → select the E3 product → select all users → **Remove license** (Reason: single bulk action, least effort).
   - IF the option is the **Identity Governance blade** → REJECT (Reason: it manages access lifecycles, reviews, entitlement — not one-time bulk license removal).
   - IF the option is `Set-AzureAdUser` → REJECT (Reason: modifies **one user at a time**; 2,500 users would require a script = more effort).
   - IF the option is `Set-WindowsProductKey` → REJECT (Reason: Windows OS product keys, completely unrelated).
4. **SC-300 Golden Rule:** *"For bulk license changes, the Licenses blade beats any script."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/users/licensing-groups-assign

---

### 9. External Collaboration Domain Restrictions (Q85)

1. **Exam Scenario:** B2B policy allows invitations **only to specified domains** (e.g., outlook.com). Determine who can accept invitations.
2. **Key Decision Point:** Domain restrictions are evaluated **at invitation send and redemption time** — not retroactively.
3. **Decision Path:**
   - IF the invited user's domain is **on the allowlist** and invitation is pending → ✅ Can accept and redeem.
   - IF the user **already redeemed** the invitation before a restriction was added → ✅ Retains access (Reason: restrictions do **not** retroactively block redeemed guests).
   - IF the invited user's domain is **not on the allowlist** → ❌ The invitation is **blocked at send time** — the user never receives it and cannot accept.
4. **SC-300 Golden Rule:** *"Allow/block lists gate the invitation, not the already-admitted guest."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/external-collaboration-settings-configure

---

### 10. Bulk B2B Invitation CSV Required Fields (Q86)

1. **Exam Scenario:** Invite many B2B guests in one operation via CSV upload.
2. **Key Decision Point:** The CSV template's **two mandatory fields**.
3. **Decision Path:**
   - REQUIRED field 1 → **Email address to invite** (InvitedUserEmailAddress).
   - REQUIRED field 2 → **Redirection URL** (where the user lands after accepting, e.g., `https://myapps.microsoft.com`).
   - IF the option is **username/UPN** → REJECT (Reason: guest UPN is auto-generated from their email).
   - IF the option is **shared key** → REJECT (Reason: service principal credentials, irrelevant to guest invitations).
   - IF the option is **password** → REJECT (Reason: guests authenticate with their **home IdP** credentials; the inviting tenant never sets passwords).
4. **SC-300 Golden Rule:** *"Bulk invite CSV = Email + Redirect URL. Nothing else is mandatory."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/tutorial-bulk-invite

---

### 11. Internal Admin Takeover of an Unmanaged Tenant (Q88)

1. **Exam Scenario:** contoso.com exists as an unmanaged ("self-service") tenant; you must become its Global Administrator.
2. **Key Decision Point:** Prove **domain ownership** via DNS TXT record.
3. **Decision Path:**
   - Step 1 → **Self-service sign-up** with an email matching the unmanaged domain (creates a user context).
   - Step 2 → Sign in to the Microsoft 365 admin center → the portal prompts **"Become the admin."**
   - Step 3 → Create the specified **TXT record** in the contoso.com DNS zone.
   - Step 4 → DNS verification succeeds → you are elevated to **Global Administrator** (internal admin takeover).
4. **SC-300 Golden Rule:** *"Unmanaged tenant takeover = self-sign-up + 'Become admin' + TXT record proof."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/users/domains-admin-takeover

---

### 12. Entra Connect Sync Delay vs. Pass-through Authentication (Q89–Q91)

1. **Exam Scenario:** A user disabled in on-premises AD can still sign in to cloud resources for a period of time.
2. **Key Decision Point:** Default Entra Connect sync interval = **30 minutes**; only PTA validates credentials **in real time**.
3. **Decision Path:**
   - IF the solution is **password writeback** → ❌ NO (Reason: writeback flows passwords **cloud → on-premises** for SSPR; it does not change the sync schedule or the account-enabled attribute direction).
   - IF the solution is **Pass-through Authentication (PTA)** → ✅ YES (Reason: PTA validates credentials **live against on-premises AD** at every sign-in; a disabled account fails instantly, eliminating sync latency).
   - IF the solution is **Conditional Access policies** → ❌ NO (Reason: CA is evaluated **after** first-factor authentication succeeds against Entra ID, using the state Entra ID already knows — it cannot force a real-time on-premises check).
4. **SC-300 Golden Rule:** *"Sync delay = 30 min. PTA checks live; writeback and Conditional Access cannot."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-pta

---

### 13. Administrative Units: Group Object vs. Group Members (Q92)

1. **Exam Scenario:** A User Administrator is scoped to an Administrative Unit (AU) that contains a group. Can they manage the group's members?
2. **Key Decision Point:** Adding a **group** to an AU scopes the **group object only** — NOT its members.
3. **Decision Path:**
   - IF the target object is the **group itself** (rename, membership changes) → ✅ AU-scoped admin CAN manage it (Reason: the group object is within AU scope).
   - IF the target is a **user who is merely a member of that group** (password reset, attribute/auth-method changes) → ❌ CANNOT (Reason: per Microsoft Learn, AU-scoped User Administrator can manage group name/membership but **cannot** reset passwords or manage properties of individual group members unless those users are **directly added** to the AU).
   - IF the admin's role is scoped to the **entire directory** → ✅ Can manage all standard non-admin objects tenant-wide (e.g., reset any regular user's password).
   - **License note:** AU-scoped role assignments require **Entra ID P1** for each AU administrator.
4. **SC-300 Golden Rule:** *"AU scope covers the group object, not its members — add users directly to the AU to manage them."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/administrative-units

---

### 14. Password Reset Role Selection — Least Privilege (Q93)

1. **Exam Scenario:** An administrator must reset passwords for non-admin users only.
2. **Key Decision Point:** Match the role's privilege level to the exact requirement.
3. **Decision Path:**
   - IF the target is **non-administrative users only** → SELECT **Helpdesk administrator** (Reason: resets passwords, invalidates refresh tokens/sessions — exactly the requirement, nothing more).
   - IF the option is **Authentication administrator** → REJECT (Reason: can also reset credentials for select admin roles — exceeds least privilege).
   - IF the option is **Privileged authentication administrator** → REJECT (Reason: can reset credentials for **anyone, including Global Administrators** — far beyond scope).
   - IF the option is **Security operator** → REJECT (Reason: read-only monitoring of security alerts; no password reset permission).
   - IF the current role is **Security administrator** → ❌ Insufficient (Reason: manages security features/Identity Protection but **cannot** reset user passwords).
4. **SC-300 Golden Rule:** *"Helpdesk = user passwords; Authentication admin = select admins; Privileged Authentication admin = everyone."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/permissions-reference

---

### 15. Entra Password Protection — Fuzzy Matching Normalization (Q94)

1. **Exam Scenario:** Determine which passwords are blocked by a custom banned password list.
2. **Key Decision Point:** Passwords are **normalized** (tokenization + fuzzy matching) before comparison.
3. **Decision Path:**
   - Normalization rules → lowercase conversion + leet-speak substitution (`0`→`o`, `1`→`i`/`l`, `@`→`a`, `$`→`s`).
   - IF the normalized password **contains** a banned word as a substring → ❌ BLOCKED.
     - `Pr0jectlitw@re` → "projectlitware" → contains **project** + **litware** → blocked.
     - `T@ilw1nd` → "tailwind" → blocked.
     - `C0nt0s0` → "contoso" → blocked.
   - IF any answer option excludes a password that normalizes to a banned word → REJECT that option.
4. **SC-300 Golden Rule:** *"De-leet, lowercase, substring-match — if the banned word hides inside, the password dies."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-password-ban-bad

---

### 16. MFA Method Selection Under Constraints (Q95, Q97)

1. **Exam Scenario A:** User's phone has **no connectivity**; laptop has internet. Which MFA method works?
2. **Key Decision Point:** Does the method require the phone to be reachable?
3. **Decision Path (Scenario A):**
   - IF method = **push notification** → ❌ REJECT (Reason: requires internet on the phone).
   - IF method = **voice call** → ❌ REJECT (Reason: requires cellular signal).
   - IF method = **security questions** → ❌ REJECT (Reason: an **SSPR** method, not a sign-in MFA second factor).
   - IF method = **app password** → ✅ SELECT (Reason: pre-configured app passwords serve legacy/non-modern-auth apps from the internet-connected laptop, bypassing the interactive MFA challenge).
4. **Decision Path (Scenario B — call center: shared PCs, no phones, no biometrics):**
   - IF the requirement is phishing-resistant auth tied to the **user, not the device** → SELECT **FIDO2 security keys** (Reason: portable hardware keys work on any shared/"hot-desk" computer).
   - IF the option is **named network location** → REJECT (Reason: a CA *condition*, not an authentication method).
   - IF the option is **Microsoft Authenticator app** → REJECT (Reason: requires a phone — prohibited).
   - IF the option is **Windows Hello for Business** → REJECT (Reason: device-specific credential; impractical when users switch computers).
5. **SC-300 Golden Rule:** *"No phone signal → app password. Shared PCs, no phones → FIDO2 keys."*
6. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-authentication-passwordless

---

### 17. Security Defaults vs. Conditional Access (Q96)

1. **Exam Scenario:** You need granular, custom access control via Conditional Access in a new tenant.
2. **Key Decision Point:** Security defaults and Conditional Access are **mutually exclusive**.
3. **Decision Path:**
   - IF Security defaults are enabled → ❌ You **cannot** create or use any Conditional Access policies.
   - FIRST STEP → **Disable Security defaults** (tenant-wide) → then build CA policies (e.g., MFA registration policy).
   - IF the option is "disable user consent settings" → REJECT (Reason: app consent governance, unrelated prerequisite).
   - IF the option is "create an MFA registration policy" → REJECT (Reason: that policy is built **inside** the CA framework, which must first be unlocked).
   - IF the option is "AD password protection" → REJECT (Reason: hybrid/on-premises feature, not a CA prerequisite).
4. **SC-300 Golden Rule:** *"Security defaults OFF before Conditional Access ON — they cannot coexist."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/security-defaults

---

### 18. Conditional Access Conditions: Blocking Legacy Authentication (Q98)

1. **Exam Scenario:** Block sign-ins from clients that don't support modern authentication (POP, IMAP, SMTP).
2. **Key Decision Point:** Which CA **condition** filters by client/protocol type?
3. **Decision Path:**
   - IF the goal is to block legacy protocols → SELECT the **Client apps** condition → target **"Legacy authentication clients"** → Grant: **Block**.
   - IF the option is **Cloud apps or actions** → REJECT (Reason: targets applications, not authentication protocols).
   - IF the option is **User risk** → REJECT (Reason: Identity Protection compromised-user state, not protocol).
   - IF the option is **Sign-in risk** → REJECT (Reason: real-time risk of the attempt, not authentication type).
4. **SC-300 Golden Rule:** *"Legacy auth dies under the Client apps condition + Block control."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/howto-conditional-access-policy-block-legacy

---

### 19. Identity Protection: User Risk vs. Sign-In Risk (Q99)

1. **Exam Scenario:** Classify risk detections (leaked credentials, impossible travel, anonymous IP, atypical travel).
2. **Key Decision Point:** Is the detection computed **offline about the identity** (user risk) or **in real time about the attempt** (sign-in risk)?
3. **Decision Path:**
   - IF detection = **Leaked credentials** → **USER risk** (Reason: threat intelligence finds valid credentials in a breach — the identity is compromised independent of any sign-in).
   - IF detection = **Impossible travel / Atypical travel / Anonymous IP address / Unfamiliar sign-in properties / Malware-linked IP** → **SIGN-IN risk** (Reason: evaluated in real time during the authentication request).
4. **SC-300 Golden Rule:** *"Leaked credentials = user risk (offline). Everything location/IP-based = sign-in risk (real time)."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-protection/concept-identity-protection-risks

---

### 20. Conditional Access Session Controls with Defender for Cloud Apps (Q100)

1. **Exam Scenario:** Users on unmanaged (Entra-registered) devices may **view** SharePoint files in the browser but must be **blocked from downloading/syncing** in real time.
2. **Key Decision Point:** In-session, device-state-aware control → CA **Session** control + **Conditional Access App Control**.
3. **Decision Path:**
   - IF real-time in-session restriction is required → CA policy → Session controls → **Use Conditional Access App Control** (leveraging Microsoft Defender for Cloud Apps) → monitor-only mode blocks downloads while allowing browser viewing.
   - IF the option is a **Defender for Cloud Apps activity policy** → REJECT (Reason: **reactive** — alerts/governance actions occur *after* the download, not proactive prevention).
   - IF the option is the **Client apps condition** → REJECT (Reason: distinguishes browser vs. desktop client, **not** device management state — the core discriminator here).
   - IF the option is an **app discovery policy** → REJECT (Reason: for shadow IT discovery, not session control on sanctioned apps).
4. **SC-300 Golden Rule:** *"Real-time download blocking = Session control + App Control; activity policies only react after the fact."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/howto-conditional-access-session-controls

---

### 21. Extending MFA to RADIUS Infrastructure (VPN) (Q101)

1. **Exam Scenario:** An on-premises VPN must enforce cloud-based MFA without replacing existing infrastructure.
2. **Key Decision Point:** VPN = RADIUS client → needs a **RADIUS server with the Entra MFA NPS extension**.
3. **Decision Path:**
   - IF the VPN authenticates via RADIUS → DEPLOY **Network Policy Server (NPS) with the Microsoft Entra MFA extension** (Reason: VPN sends RADIUS request → NPS → extension calls Entra ID to enforce MFA as second factor; primary auth stays on-premises AD).
   - IF the option is **Application Proxy** → REJECT (Reason: publishes on-premises **web apps** for remote access, not RADIUS/MFA).
   - IF the option is **Password Protection proxy** → REJECT (Reason: extends banned password lists/smart lockout on-premises; provides no MFA).
   - IF the option is **Pass-through Authentication proxy** → REJECT (Reason: an Entra Connect agent for sign-in password validation, not a RADIUS MFA proxy).
4. **SC-300 Golden Rule:** *"VPN + MFA = NPS extension. App Proxy is for web apps, not RADIUS."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/howto-mfa-nps-extension

---

### 22. On-Premises Password Protection Proxy Redundancy (Q102)

1. **Exam Scenario:** Domain controllers have no internet access; ensure password protection survives a proxy server failure.
2. **Key Decision Point:** Architecture = **DC agent** (on DCs) + **Proxy service** (on any domain-joined, internet-connected server).
3. **Decision Path:**
   - IF DCs are internet-blocked → they rely on the **Proxy service** to forward password validation requests to the cloud.
   - IF fault tolerance is required → DEPLOY the **proxy service on an additional server** (Reason: the proxy is **stateless** — no load balancer needed; multiple instances across servers provide redundancy automatically).
   - IF the option is **Entra Connect** → REJECT (Reason: directory synchronization only; does not host proxy traffic).
   - IF the option is **Application Proxy** → REJECT (Reason: remote access to web apps; unrelated to password validation).
   - IF the option is **PCNS** → REJECT (Reason: legacy MIM component for password sync to alternate directories).
4. **SC-300 Golden Rule:** *"DC agent + stateless proxy = just install more proxies for redundancy."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/howto-password-ban-bad-on-premises-deploy

---

### 23. Defender for Cloud Apps: OAuth App Policies & Shadow IT Discovery (Q103, Q104)

1. **Exam Scenario A:** Alert when third-party apps request excessive permissions (e.g., Mail.ReadWrite).
2. **Decision Path (A):**
   - IF app-to-app OAuth permission governance is required → CREATE an **OAuth app policy** in Microsoft Defender for Cloud Apps (Reason: continuously scans connected apps and alerts on excessive/granted permissions).
   - IF the option is **Identity Protection** → REJECT (Reason: user-level and sign-in-level risk only).
   - IF the option is **Identity Governance** → REJECT (Reason: entitlement management, access reviews, PIM workflows).
   - IF the option is **Intune/Endpoint Manager** → REJECT (Reason: device compliance and MAM, not cloud API permissions).
3. **Exam Scenario B:** Use **firewall logs** to build a list of unmanaged external cloud apps and their users.
   - IF the source is network device logs (firewalls/proxies) → USE **Cloud App Discovery** in Defender for Cloud Apps (Reason: analyzes uploaded firewall traffic logs to report shadow IT apps, users, and risk levels).
   - IF the option is **Application Insights** → REJECT (Reason: APM for developer-built apps).
   - IF the option is **Access reviews** → REJECT (Reason: reviews access to *known* resources, not discovery of unknown apps).
   - IF the option is **Enterprise applications blade** → REJECT (Reason: manages already-registered apps, not unmanaged discovery).
4. **SC-300 Golden Rule:** *"OAuth permissions → app policy; firewall logs → Cloud App Discovery."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-cloud-apps/app-permission-policy

---

### 24. Application Proxy Connector — Outbound-Only Architecture (Q105)

1. **Exam Scenario:** Publish an on-premises ASP.NET app (App1) to Entra ID users through a hardware firewall.
2. **Key Decision Point:** Connector communication direction.
3. **Decision Path:**
   - IF publishing an on-premises web app → INSTALL the **Microsoft Entra Application Proxy connector** on a server that can reach the app (Server4).
   - IF the question asks about firewall rules → ✅ **NO inbound ports required** (Reason: the connector makes **outbound-only** connections — HTTPS 443 / HTTP 80 — to the Entra service; no inbound firewall openings).
4. **SC-300 Golden Rule:** *"App Proxy connector = outbound 443 only. Never open inbound firewall ports."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/app-proxy/application-proxy-add-on-premises-application

---

### 25. App Assignment & Registration Permissions (Q106)

1. **Exam Scenario:** Determine who can (a) assign users to an enterprise app registered by someone else, and (b) register a new app under **default** tenant settings.
2. **Key Decision Point:** Directory-wide role vs. object ownership vs. default user permissions.
3. **Decision Path:**
   - **Assigning users to an enterprise app:**
     - IF the admin holds **Application administrator** or **Cloud application administrator** → ✅ CAN assign users/groups to any enterprise app (directory-wide rights).
     - IF the admin is an **Application developer** → ❌ CANNOT (Reason: manages only app registrations they **created or own**; no directory-wide assignment rights).
     - IF the account is a **standard user** → ❌ CANNOT.
   - **Registering a new app (default settings):**
     - IF the tenant switch **"Users can register applications" = Yes** (the default) → ✅ **ALL member users** (Admin1, Admin2, Admin3, User1) can register apps.
4. **SC-300 Golden Rule:** *"App/Cloud App admins assign to any app; developers only to their own; by default every member user can register apps."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/users-default-permissions

---

### 26. Emergency Access (Break-Glass) Accounts (Q107)

1. **Exam Scenario:** Configure an emergency Global Administrator account to remain usable during outages (federation failure, MFA service failure, PIM unavailability).
2. **Key Decision Point:** **Minimize dependencies** — every added control is a potential point of failure.
3. **Decision Path:**
   - IF the action **adds a dependency** (PIM activation, corporate-network CA restriction, MFA requirement) → ❌ REJECT:
     - PIM activation → fails if the PIM service is down.
     - CA restricting to corporate network → fails during on-premises outages or off-network emergencies.
     - CA requiring MFA → fails if the MFA service is unavailable.
   - IF the action is **monitoring/alerting on sign-ins and configuration changes** (Azure Monitor/Sentinel) → ✅ SELECT (Reason: adds security without creating a sign-in dependency; aligns with Microsoft's guidance to monitor break-glass usage).
   - **Microsoft's full guidance:** at least **two cloud-only** Global Admin accounts, credentials never expire, **exclude from all CA policies**, monitor all usage, test quarterly.
4. **SC-300 Golden Rule:** *"Break-glass accounts must work when everything else is broken — monitor them, never gate them."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/security-emergency-access

---

### 27. Enforcing Terms of Use (Q108)

1. **Exam Scenario:** All users must accept a Terms of Use (ToU) document before accessing tenant resources.
2. **Key Decision Point:** ToU is enforced **exclusively through Conditional Access grant controls**.
3. **Decision Path:**
   - IF a ToU must be presented at sign-in → Create the ToU in Entra ID → Create a **Conditional Access policy** → Grant control: **"Require acceptance of Terms of Use"** → users are blocked until they accept.
   - IF the option is a **Cloud App Security access policy** → REJECT (Reason: session control/threat detection, not ToU enforcement at sign-in).
   - IF the option is **Intune Terms and conditions** → REJECT (Reason: presented only during **device enrollment** into Intune, not general resource access).
   - IF the option is an **Intune compliance policy** → REJECT (Reason: defines device compliance rules; compliance can be a CA *condition*, but the policy itself does not enforce ToU).
4. **SC-300 Golden Rule:** *"ToU lives in Conditional Access grant controls — nowhere else."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/terms-of-use

---

## Quick-Reference Cheat Sheet (Part 3)

| Trigger in Question | Correct Answer Pattern |
|---|---|
| Nest groups | Security groups nest; M365 groups never nest |
| Seamless SSO browser config | Intranet zone: `autologon.microsoftazuread-sso.com` |
| Change alert recipient | Modify the **Action Group** (never roles/DCRs) |
| Auto-block on fraud report | **Report suspicious activity** (Fraud alert) — not Block/unblock, not lockout |
| Licensing via groups | Security/M365 × Assigned/Dynamic-User; never Dynamic-Device |
| New guest, no account | Email OTP |
| Bulk invite CSV | Email address + Redirection URL |
| Disabled user signs in late | PTA fixes it; writeback & CA don't |
| AU contains a group | Manage the group ✅ / manage its members ❌ |
| Reset non-admin passwords | Helpdesk administrator |
| Banned password check | Normalize (leet→letters), substring match |
| No phone signal | App password; shared PCs no phones → FIDO2 |
| Enable Conditional Access | Disable Security defaults first |
| Block legacy auth | Client apps condition → Legacy authentication clients |
| Leaked credentials | **User** risk (all others = sign-in risk) |
| Block download in session | CA Session control + Defender for Cloud Apps |
| VPN + MFA | NPS extension |
| Password protection redundancy | Add another stateless proxy |
| Excessive OAuth permissions | Defender for Cloud Apps app policy |
| Publish on-prem web app | App Proxy connector, outbound 443 only |
| Break-glass account | Monitor only; exclude from CA/MFA/PIM |
| Enforce ToU | Conditional Access grant control |# SC-300 Decision Trees - Part 4

All key facts are now validated against Microsoft Learn. Here is the structured set of Decision Trees and logic rules derived from Part 4 (Questions 109–144).

---

# SC-300 Decision Trees & Logic Rules — Part 4 (Q109–Q144)

---

### 1. Access Reviews — Eligible Review Targets (Group Types)
1. **Exam Scenario:** You must determine which groups (Assigned, Dynamic User, Dynamic Device, Security, Microsoft 365) can be targeted by a Microsoft Entra access review. *(Q109)*
2. **Key Decision Point:** Does the group contain **user/service principal identities** or **device objects**?
3. **Decision Path:**
   - IF group is a **Security group** (Assigned or Dynamic User) → SELECT it as reviewable (Reason: access reviews support security groups regardless of membership type).
   - IF group is a **Microsoft 365 group** (Assigned or Dynamic User) → SELECT it as reviewable (Reason: M365 groups are fully supported review targets).
   - IF group is a **Dynamic Device group** → REJECT it as a review target (Reason: reviews evaluate *user/service principal* access; device objects cannot attest or be attested).
   - IF the group is **Dynamic User** → SELECT it, but remember: decisions will **not auto-modify membership** (attribute rules control membership); the review is still fully supported.
4. **SC-300 Golden Rule:** *Access reviews review PEOPLE (users, guests, service principals) — never DEVICES. Dynamic user groups can be reviewed, but the review never rewrites the dynamic rule.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/access-reviews-overview

---

### 2. Access Reviews — Reviewer Configuration (Self, Manager, Fallback)
1. **Exam Scenario:** A review is configured with reviewer type **Members (self)** or **(Preview) Manager** with a fallback reviewer; you must identify who reviews whom and why reviews fall back. *(Q110, Q115, Q116, Q117, Q125)*
2. **Key Decision Point:** The **reviewer type** setting + whether the **manager attribute** is populated on user objects.
3. **Decision Path:**
   - IF reviewer = **Members (self)** and scope = Everyone → SELECT: *every scoped user reviews their own access* (members AND guests alike; owners are NOT reviewers).
   - IF reviewer = **Group owners** → SELECT: only owners review; members do not.
   - IF reviewer = **(Preview) Manager** and the user's **manager attribute is populated** → SELECT: the manager receives the review.
   - IF reviewer = **(Preview) Manager** and the **manager attribute is empty/undefined** → SELECT: the review routes to the **fallback reviewer** (root cause = missing attribute, not review design).
   - IF the goal is to stop fallback routing → SELECT: **populate the manager property on each user object**; REJECT "add managers to the fallback list" (Reason: fallback is a single catch-all, it cannot map users to department heads) and REJECT "create one review per role" (Reason: does not fix missing manager relationships).
   - IF the proposed fix is **Member (self)** to satisfy "managers must review" → REJECT (Reason: self-review bypasses management oversight entirely).
4. **SC-300 Golden Rule:** *Fallback reviewers fire only when the manager attribute is blank. Fix the ATTRIBUTE, not the review — self-review is never the answer when managers must attest.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/access-reviews-overview

---

### 3. Access Reviews — Review Lifecycle & Timing
1. **Exam Scenario:** A review starts Jan 15 with a 14-day duration and monthly recurrence; you must decide who can respond and when. *(Q131)*
2. **Key Decision Point:** Is the date **inside the active review window**, and is the user a **designated reviewer**?
3. **Decision Path:**
   - IF date is within [start date … start + duration] → SELECT: reviewers may submit **and change** their answers until the review ends.
   - IF date falls between occurrences (after close, before next recurrence) → REJECT: no responses can be submitted or modified.
   - IF reviewer = Members (self) and the user is an **owner but not a member** → REJECT: owners are not reviewers under "Members (self)".
4. **SC-300 Golden Rule:** *Reviews are answerable only during their active window, and only by the configured reviewer type — membership in the scope defines the reviewer under self-review.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/access-reviews-overview

---

### 4. Access Reviews vs. Entitlement Management — Choosing the Governance Tool
1. **Exam Scenario:** Periodically review and revoke **guest access to a specific application**; configure what happens to guest accounts after their **last access package assignment** expires. *(Q114, Q123)*
2. **Key Decision Point:** Is the task **recertifying existing access** (review) or **provisioning/lifecycle of new access** (entitlement)?
3. **Decision Path:**
   - IF requirement = *periodically review existing app access for guests and auto-remove on deny* → SELECT: an **access review of the enterprise application**, scoped to **Guest users only**, recurring monthly (Reason: reviews the app's assignment list directly, covers all assignment methods).
   - IF requirement = *bundle resources + request/approval workflow for new access* → SELECT: an **access package**; REJECT access packages for recurring recertification of a single app (Reason: they manage initial grants, not periodic attestation).
   - IF requirement = *block external user from sign-in / remove external user / days before removal after last assignment* → SELECT: **Entitlement Management global (external user lifecycle) settings**; REJECT access packages (policy-level only), Terms of use (consent documents), and Access reviews settings (periodic attestation, not account lifecycle).
4. **SC-300 Golden Rule:** *Reviews = "should they STILL have it?" Entitlement Management = "how do they GET it and what happens when they lose it?" Guest account deletion lives in Entitlement Management settings, not in the access package.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/access-reviews-overview

---

### 5. PIM — Activation Settings: MFA Frequency & Default Approvers
1. **Exam Scenario:** A PIM role requires MFA on activation with an 8-hour maximum activation duration; approval is required but **no approvers are listed**. *(Q111)*
2. **Key Decision Point:** Activation **maximum duration** + whether **approvers are explicitly configured**.
3. **Decision Path:**
   - IF "On activation, require MFA = Yes" and max activation duration = 8 hours → SELECT: continuous access requires **MFA every 8 hours** (each re-activation re-triggers MFA).
   - IF "Require approval = Yes" and **Approvers = None** → SELECT: **active Global Administrators and Privileged Role Administrators become the default approvers** (validated Microsoft Learn behavior).
   - IF all PRA/GA assignments are eligible-but-inactive, approval is required, and no approvers exist → REJECT that configuration (Reason: tenant lockout risk — always configure emergency access accounts + explicit approvers).
4. **SC-300 Golden Rule:** *MFA is per-activation, so MFA frequency = activation duration. No approver selected? Active Global Admins + Privileged Role Admins inherit the approver duty.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-how-to-change-default-settings

---

### 6. PIM — Eligible vs. Active (Converting to Just-in-Time)
1. **Exam Scenario:** Users hold **permanent (standing) role assignments**; the business requires access **only when needed**. *(Q113)*
2. **Key Decision Point:** The **assignment type** (Eligible vs. Active) — not expiration timers.
3. **Decision Path:**
   - IF requirement = JIT / "access only when required" → SELECT: change **Assignment type to Eligible** (Reason: no standing permissions; user must activate with justification/MFA/approval).
   - IF proposed fix = "Expire eligible assignments after" → REJECT (Reason: caps how long eligibility lasts, does not create JIT).
   - IF proposed fix = "Expire active assignments after" → REJECT (Reason: time-limits standing access but user still holds permissions without activation).
   - IF proposed fix = "Assignment type = Active" → REJECT (Reason: that IS the current permanent-access problem).
4. **SC-300 Golden Rule:** *Standing access = Active. Just-in-time = Eligible. Expiration timers limit duration; only Eligible creates activation.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-how-to-change-default-settings

---

### 7. Terms of Use — Per-Device Consent & Expiration Schedules
1. **Exam Scenario:** "Require users to consent on every device" is On; consent given Nov 15 on Device3; expiration starts Dec 10; user signs in from registered Device1 (Nov 20), unregistered Device2 (Dec 11), and Device3 (Dec 7). *(Q112)*
2. **Key Decision Point:** Is the device **registered in Entra ID**, and is the date **before the expiration start date**?
3. **Decision Path:**
   - IF per-device consent = On AND device is **registered** AND never consented on it → SELECT: user is **prompted on that device** (device ID tracks per-device consent).
   - IF device is **NOT registered** → SELECT: user is prompted **every time** on that device (no persistent device record can be stored).
   - IF consent exists on the device AND current date is **before "Expire starting on"** → SELECT: existing consent remains valid, **no prompt**.
   - IF date is on/after the expiration start (or frequency interval elapsed) → SELECT: consent expires, user must re-accept (at next session).
4. **SC-300 Golden Rule:** *Per-device ToU = registered device + per-device consent record. Unregistered device = prompt every time. Expiration date governs re-acceptance, not the original accept date.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/conditional-access/terms-of-use

---

### 8. B2B Collaboration — Inviting External Contractors (MSA Guests)
1. **Exam Scenario:** Grant an external contractor using **user1@outlook.com (MSA)** access to an enterprise application with least administrative effort. *(Q118, Q137)*
2. **Key Decision Point:** The contractor's identity lives **outside the tenant** — which mechanism creates a linked guest object?
3. **Decision Path:**
   - IF external user has their own identity (MSA/any IdP) → SELECT: **B2B invitation** (portal "Invite external user" or `New-AzureADMSInvitation`) → creates a **guest object** linked to their external identity; they keep using their own credentials.
   - IF proposed = `New-AzADUser` → REJECT (Reason: creates an internal **member** account with tenant-managed credentials).
   - IF proposed = configure **External collaboration settings** → REJECT (Reason: defines *policy* for who may invite/guest access levels; does not invite anyone).
   - IF proposed = add a **WS-Fed/SAML identity provider** → REJECT (Reason: direct federation is for a partner *organization's* IdP, not an individual MSA).
   - IF proposed = **Microsoft Entra Connect** → REJECT (Reason: syncs on-premises AD objects; unrelated to guests).
4. **SC-300 Golden Rule:** *External identity + internal resource = B2B invite. The invitation creates the guest object; everything else (settings, federation, sync) is the wrong tool.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/b2b-quickstart-add-guest-users-portal

---

### 9. Microsoft Entra Connect — Sync Filtering (Inbound Rules & Group-Based Pilot)
1. **Exam Scenario:** Prevent users with `extensionAttribute15 = NoSync` from syncing; and determine which objects sync when **Domain/OU filtering + Group-based filtering** are both active. *(Q119, Q126)*
2. **Key Decision Point:** Data **flow direction** (inbound from AD) and whether the object satisfies **ALL** active filters.
3. **Decision Path:**
   - IF filtering objects sourced from on-premises AD → SELECT: **inbound synchronization rule on the Active Directory (AD DS) connector** with high precedence + scoping filter (or set `cloudFiltered` via transformation).
   - IF proposed = inbound rule on the **Windows Azure Active Directory connector** → REJECT (Reason: that connector's inbound direction flows *from* Entra ID, not from AD).
   - IF proposed = Full Import / Export run profile → REJECT (Reason: run profiles *execute* sync steps; they don't define filtering logic).
   - IF **both** OU filtering AND group-based filtering are enabled → SELECT: object syncs only if it satisfies **both** (in filtered OU **AND** direct member of the pilot group).
   - IF object is in the filtered OU but **not a direct member** of the sync group → REJECT: not synchronized.
   - IF object is a **nested group** inside the sync group → REJECT: **group-based filtering does not support nested groups** (nested groups are ignored).
4. **SC-300 Golden Rule:** *Filter at the source: inbound rule on the AD connector. With group-based filtering, only DIRECT members sync — nesting is ignored — and multiple filters are ANDed, not ORed.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-fix-default-rules

---

### 10. Hybrid Sign-In Resilience — PTA vs. PHS During an On-Premises Outage
1. **Exam Scenario:** On-premises domain loses internet connectivity. User1 = cloud-only, User2 = synced (tenant primary method = **Pass-through Authentication**, PHS also enabled), User3 = guest. Who can still sign in? *(Q120, Q121)*
2. **Key Decision Point:** Where does **credential validation** physically occur for each identity type?
3. **Decision Path:**
   - IF user is **cloud-only** or **guest (B2B)** → SELECT: sign-in succeeds (Reason: authentication is handled entirely in the cloud / home IdP; no on-prem dependency).
   - IF user is **synced** and tenant primary method = **PTA** → SELECT: sign-in **fails** during outage (Reason: on-prem PTA agents can't reach Entra ID to validate credentials).
   - IF PHS is *also* enabled → REJECT "PHS auto-failover" (Reason: PHS is a standalone method or a **manual** disaster-recovery fallback — it does not automatically take over mid-outage).
   - IF the problem is a **sync delay** (disabled account takes ~30 min via delta sync) → REJECT "Azure AD Password Protection" as the fix (Reason: it enforces password strength/banned passwords; it has zero effect on sync schedule or AccountEnabled replication).
4. **SC-300 Golden Rule:** *PTA = on-premises dependency (dies with the outage). PHS = cloud validation (survives it, but never auto-fails-over). Password Protection fixes passwords, never sync latency.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-pta

---

### 11. SSPR — Authentication Methods & Password Writeback
1. **Exam Scenario:** Users must reset with a **mobile app notification or security questions**; cloud-initiated resets must also update on-premises AD. *(Q122)*
2. **Key Decision Point:** Which SSPR blade section controls **methods**, and which Entra Connect feature controls **on-prem sync of resets**?
3. **Decision Path:**
   - IF requirement specifies *which* methods and *how many* → SELECT: **Authentication methods** section of the Password reset blade (choose methods + number required to reset).
   - IF requirement = cloud SSPR resets must flow back to on-premises AD → SELECT: enable **Password writeback** in Microsoft Entra Connect (Express settings already give you PHS; writeback is the separate toggle).
   - IF proposed = any other Entra Connect option → REJECT (Reason: only writeback propagates cloud resets to AD DS).
4. **SC-300 Golden Rule:** *Methods live in the SSPR blade; cross-premises propagation lives in Entra Connect writeback. Security questions & email = SSPR-only methods, never MFA methods.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-sspr-howitworks

---

### 12. Log Retention by License Tier & the Monitoring Pipeline
1. **Exam Scenario:** How long are sign-in logs retained natively, and where do you change alert **recipients** or stream logs for analysis? *(Q124, Q135, Q141)*
2. **Key Decision Point:** **License tier** (Free vs P1/P2) and the distinction between **log routing** vs. **alert notification**.
3. **Decision Path:**
   - IF tenant = **Entra ID Free** → SELECT: sign-ins & audit logs retained **7 days**.
   - IF tenant = **P1 or P2** → SELECT: sign-ins & audit logs retained **30 days** (REJECT 14 days — not a native period; REJECT 90 days — that's risky sign-ins on P2, not sign-in logs; REJECT 365 days — only achievable by archiving to storage/Log Analytics).
   - IF goal = analyze Entra logs in Azure Monitor → SELECT: create a **diagnostic setting** in Entra ID → route to Log Analytics workspace (REJECT workbooks/cmdlets — they consume data already in the workspace).
   - IF goal = change **who receives an alert** → SELECT: modify the **Action Group** on the Azure Monitor alert rule; REJECT editing Entra diagnostic settings (Reason: diagnostics control *where logs go*, not alert recipients).
4. **SC-300 Golden Rule:** *Free = 7, Premium = 30 (sign-ins & audit). Diagnostic settings move LOGS; Action Groups move ALERTS. Longer retention = archive, don't upgrade and expect retroactive data.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/monitoring-health/reference-reports-data-retention

---

### 13. MFA Method Selection Under Connectivity Constraints
1. **Exam Scenario:** Laptop has wired internet; phone has **no Wi-Fi and no cellular**. Which MFA method works? *(Q127, Q134)*
2. **Key Decision Point:** Does the method require the **phone to be online** at verification time?
3. **Decision Path:**
   - IF phone is fully offline → SELECT: **Microsoft Authenticator verification code (TOTP/OATH)** — generated offline on the phone, typed into the online laptop; or **Windows Hello for Business** (PIN/biometric + device-bound key = MFA entirely on the laptop).
   - IF proposed = Authenticator **push notification** → REJECT (Reason: push requires phone internet/data).
   - IF proposed = **SMS** or **voice call** → REJECT (Reason: requires cellular connectivity).
   - IF proposed = **app password** → REJECT (Reason: legacy workaround for non-modern-auth apps, not an interactive MFA method).
   - IF proposed = **email** or **security questions** → REJECT (Reason: SSPR methods only — not valid MFA verification methods at sign-in).
4. **SC-300 Golden Rule:** *Offline phone? Think TOTP code or Windows Hello for Business. Anything that "pushes" or "texts" needs the phone online; email/security questions are SSPR, not MFA.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-sspr-howitworks

---

### 14. Identity Protection — Role Permissions, Risk Policies & Sentinel
1. **Exam Scenario:** Who may configure the **user risk policy**, who may **view risky-users reports**, does "report suspicious MFA activity" auto-block users, and how do you stream risk alerts into **Sentinel**? *(Q128, Q129, Q132)*
2. **Key Decision Point:** Least-privileged **role** per task + the separation between **risk detection** and **risk response**.
3. **Decision Path:**
   - IF task = configure/enable **user risk or sign-in risk policy** → SELECT: **Security Administrator** (REJECT Conditional Access Administrator — can build CA policies *using* risk as a condition, but cannot configure native Identity Protection risk policies; REJECT Security Operator — remediation only).
   - IF task = **dismiss risk / remediate** → SELECT: **Security Operator**.
   - IF task = **view** risk reports → SELECT: **Security Administrator or Security Operator** (Security Reader reads all; CA/Authentication admins have no default read on these reports).
   - IF user enables **"Report suspicious activity"** MFA notifications → SELECT: reported users get risk level **High**, but REJECT "this alone blocks the user" (Reason: a separate **user risk policy** with a Block/Require-remediation grant control must also exist).
   - IF goal = Sentinel incidents from Identity Protection → SELECT: enable the **Azure AD Identity Protection data connector** (populates `SecurityAlert` for built-in analytics rules); REJECT Notify settings (email only), playbooks (response *after* alerts), and Entra diagnostic settings (different tables, e.g., `AADRiskyUsers`).
4. **SC-300 Golden Rule:** *Security Admin configures, Security Operator remediates, Security Reader reads. Detection ≠ enforcement: reporting fraud raises risk; only a risk POLICY blocks. Sentinel needs the dedicated Identity Protection connector.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/delegate-by-task

---

### 15. Enterprise Applications & Conditional Access Device Filters
1. **Exam Scenario:** Integrate a third-party SaaS app from the gallery with SSO, and restrict access to **Entra-joined devices only**. *(Q130)*
2. **Key Decision Point:** App **onboarding method** + access restriction **mechanism**.
3. **Decision Path:**
   - IF app exists in the Entra gallery → SELECT: add as an **enterprise application** from the gallery (Reason: template-driven federation/SSO config = least effort, no credential prompts).
   - IF restriction is based on **device state/trust type** → SELECT: **Conditional Access policy** with the **"Filter for devices"** condition (e.g., `device.trustType` equals AzureADjoined/AzureADregistered).
   - IF restriction were purely group/user-based → SELECT: assignment of users/groups to the enterprise app instead.
4. **SC-300 Golden Rule:** *Gallery app = enterprise application. Device-state gating = Conditional Access "Filter for devices" — never app assignments.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/access-reviews-overview *(app governance context; CA device filters: concept-filters-for-devices)*

---

### 16. Device Registration — Limits & Device Admin Roles
1. **Exam Scenario:** Tenant max devices per user = 5; user already has 4 devices; Cloud Device Administrator must toggle MFA at registration; Device Administrator's local rights on an **Entra registered** device. *(Q133)*
2. **Key Decision Point:** Count **existing devices against the quota**; match role to **permission scope**; distinguish **joined vs. registered**.
3. **Decision Path:**
   - IF max devices = 5 and user already has 4 → SELECT: user can add only **1 more** device (count existing devices first).
   - IF task = modify **device registration policy** (e.g., require MFA on register/join) → SELECT: **Cloud Device Administrator** (holds `microsoft.directory/deviceRegistrationPolicy/basic/update`).
   - IF device is **Entra joined** → SELECT: Device Administrator role members are added to the **local Administrators group**.
   - IF device is merely **Entra registered** (BYOD) → REJECT: Device Administrator gets **no local admin rights** there.
4. **SC-300 Golden Rule:** *Cloud Device Admin = registration POLICY. Device Admin = local admin on JOINED Windows devices only — registered/BYOD devices get nothing.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/delegate-by-task

---

### 17. Least-Privilege Role & Admin Center Mapping
1. **Exam Scenario:** Map tasks to the correct admin center and least-privileged role: access review creation, review history reports, MFA/auth-method management scoped to executives, restoring deleted users. *(Q136, Q142, Q143, Q144)*
2. **Key Decision Point:** Task **domain** (Entra vs. Purview vs. Intune) + **least-privileged built-in role**.
3. **Decision Path:**
   - IF task = create/manage **access reviews** → SELECT: **User Administrator** (REJECT Global Administrator — violates least privilege).
   - IF task = **view access review history reports** → SELECT: **Security Reader** (REJECT Reports Reader — lacks identity-governance read; REJECT Global Reader — over-privileged).
   - IF task = manage **MFA/authentication methods + password resets** for a subset of users → SELECT: **Authentication Administrator** role, scoped via an **administrative unit** containing those users (REJECT Helpdesk/Password Administrator — can reset passwords but cannot manage auth methods/MFA registration).
   - IF delegation must be **scoped to a subset of users** → SELECT: **Administrative unit** (container for role scope); REJECT groups/dynamic groups (Reason: groups don't scope admin roles).
   - IF task = Records Management / compliance → SELECT: assign in **Microsoft Purview (compliance center)**.
   - IF task = Endpoint Security Manager / Intune Role Administrator → SELECT: **Microsoft Intune admin center** (RBAC roles).
   - IF task = **restore a deleted user** → SELECT: **User Administrator**; recovery window = **30 days** soft-delete, after which the object is unrecoverable.
4. **SC-300 Golden Rule:** *Least privilege first: User Admin = reviews & restores (30-day window), Security Reader = review reports, Authentication Admin + admin unit = scoped MFA management. Admin units scope ROLES; groups scope ACCESS.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/delegate-by-task

---

### 18. Smart Lockout — Bad-Password Hash Buffer & SSPR Reset
1. **Exam Scenario:** A user types a sequence of wrong passwords (with repeats); determine how many attempts count toward lockout, and how to clear a lockout immediately. *(Q139)*
2. **Key Decision Point:** Does the attempted password match one of the **last three distinct bad-password hashes**?
3. **Decision Path:**
   - IF attempted password matches a hash in the rolling **last-3 bad-password buffer** → SELECT: attempt is **ignored** — lockout counter does NOT increment.
   - IF attempted password is a **new** bad hash → SELECT: counter increments; oldest hash drops out of the 3-slot buffer.
   - IF account is locked and user needs immediate access → SELECT: start **SSPR and choose "I forgot my password"** → lockout timer resets to **0 seconds** (REJECT "I know my password" — timer keeps running).
   - IF customizing thresholds/duration → note: requires **Entra ID P1+**; default threshold 10 (public cloud), duration 60 s, escalating on repeats.
4. **SC-300 Golden Rule:** *Smart Lockout remembers the last THREE distinct bad passwords — repeats are free. "I forgot my password" zeroes the lockout; "I know my password" waits it out.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/howto-password-smart-lockout

---

### 19. My Apps Collections — Organizing the Access Panel
1. **Exam Scenario:** Group applications into logical categories for users in the My Apps portal. *(Q140)*
2. **Key Decision Point:** Which feature is **purpose-built** for user-facing app organization?
3. **Decision Path:**
   - IF goal = categorize apps in My Apps for specific user sets → SELECT: **Collections** (assign apps + users/groups to a collection; requires **Entra ID P1 or P2**).
   - IF proposed = tags → REJECT (Reason: Azure resource management/billing metadata, not user-facing).
   - IF proposed = naming policies → REJECT (Reason: M365 group naming conventions only).
   - IF proposed = dynamic groups → REJECT (Reason: membership automation; creates no portal categories).
4. **SC-300 Golden Rule:** *Collections = My Apps folders (P1/P2). Tags, naming policies, and dynamic groups never touch the user's app portal view.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/access-reviews-overview *(app-access governance context; feature doc: access-panel-collections)*

---

### 20. Bulk License Operations — Cmdlet Selection
1. **Exam Scenario:** 2,500 users hold **directly assigned** E3 licenses; new licenses were assigned via group; remove the direct assignments with least effort. *(Q138)*
2. **Key Decision Point:** Which cmdlet **manages license assignment state** vs. object properties?
3. **Decision Path:**
   - IF task = remove/add licenses in bulk via script → SELECT: `Set-MsolUserLicense` with `-RemoveLicenses` (MSOnline) — or its Graph equivalent `Set-MgUserLicense`.
   - IF proposed = `Set-MsolProductKey` → REJECT (Reason: subscription product keys, not per-user licenses).
   - IF proposed = `Update-MgGroup` / `Update-MgUser` → REJECT (Reason: they modify group/user **properties**; license state needs the dedicated license cmdlet).
4. **SC-300 Golden Rule:** *License state = Set-MsolUserLicense / Set-MgUserLicense. Group-based licensing replaces direct assignment — but you must still strip the direct licenses explicitly.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/tutorial-bulk-invite *(bulk-operations pattern; licensing cmdlets per MSOnline/Graph SDK docs)*

---

## Cross-Cutting Exam Heuristics (Part 4)

| Signal in the question | Correct instinct | Trap to reject |
|---|---|---|
| "Dynamic **device** group" in an access review | Not reviewable | Assuming all dynamic groups qualify |
| "Manager as reviewer + fallback reviewer" | Fix the **manager attribute** | Adding managers to fallback list / per-role reviews |
| "Access only when required" (PIM) | **Eligible** assignment | Expiration timers or "Active" type |
| "Approval required, no approvers listed" | Active **GA + PRA** become approvers | Assuming activation is blocked forever |
| "Consent on every device" | Requires **device registration** | Assuming consent carries across devices |
| "outlook.com contractor" | **B2B invite** (guest object) | New-AzADUser / federation / External collaboration settings |
| "NoSync attribute" filtering | **Inbound rule on AD connector** | Rule on Azure AD connector / run profiles |
| "PTA + on-prem outage" | Synced user **cannot** sign in | Assuming PHS auto-failover |
| "Phone offline" MFA | **TOTP code** or **Windows Hello for Business** | Push, SMS, email, security questions |
| "Report suspicious MFA activity" | Risk → **High**, needs separate **user risk policy** to block | Assuming the toggle blocks access |
| "Change alert recipients" | **Action Group** in Azure Monitor | Entra diagnostic settings |
| "Restore deleted user" | **User Administrator**, **30-day** window | Helpdesk Administrator |
| "Scoped admin delegation" | **Administrative unit** + least-privilege role | Security groups as role scope |# SC-300 Decision Trees - Part 5

# SC-300 Decision Trees & Logic Rules — Part 5 (Questions 145–180)

*Validated against Microsoft Learn (Microsoft Entra ID Governance, ID Protection, Conditional Access, and Defender for Cloud Apps documentation).*

---

### 1. Conditional Access App Control & Defender for Cloud Apps Session Policies (Q145, Q158)
1. **Exam Scenario:** You must block/monitor real-time user actions (e.g., prevent printing PDFs from SharePoint Online) or deploy session policies in Defender for Cloud Apps.
2. **Key Decision Point:** Where is the control enforced — at sign-in (Entra CA) or inside the session (reverse proxy)?
3. **Decision Path:**
   - IF the task is to *enable/trigger* session control for an app → SELECT **Create a Conditional Access policy with the Session control "Use Conditional Access App Control"** (Reason: This routes the session through the Defender for Cloud Apps reverse proxy — the mandatory first step; requires Entra ID P1 + Defender for Cloud Apps license).
   - IF the task is to *block an in-session action* (print, download, copy/paste) → SELECT **A session policy in Defender for Cloud Apps** (Reason: Only session policies inspect and govern activities in real time within the proxied session).
   - IF the option is an **activity policy** → REJECT (Reason: Activity policies alert/act on events that *already occurred*, not real-time blocking).
   - IF the option is a **file policy** → REJECT (Reason: File policies scan data *at rest*, not live user actions).
   - IF the option is an **access policy** → REJECT (Reason: Access policies decide *whether* the user can reach the app at all — no granular in-session control).
   - IF the option is "connect/onboard the app" or "create a continuous report" → REJECT (Reason: Onboarding is a prerequisite, not the trigger; continuous reports are for log export/analysis).
4. **SC-300 Golden Rule:** **CA opens the door, App Control walks you through it** — Conditional Access *proxies* the session; only a Defender for Cloud Apps *session policy* controls what happens inside it.
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-xdr/pilot-deploy-defender-cloud-apps#step-6-configure-conditional-access-app-control and https://learn.microsoft.com/defender-cloud-apps/apps-manual-onboarding-with-microsoft-entra-id#deploy-an-app-with-conditional-access-app-control

---

### 2. Access Reviews — Scope, Nested Groups & On-Premises Synced Groups (Q146, Q177)
1. **Exam Scenario:** An access review targets Group2/Group3; users are nested or synced from on-prem AD; or you must auto-remove inactive guests from an app.
2. **Key Decision Point:** Is Microsoft Entra ID the **source of authority** for the group, and is the user a **direct member**?
3. **Decision Path:**
   - IF the group is **cloud-sourced (Microsoft Entra ID)** and the user is a **direct member** who fails to respond → SELECT **Auto-removal succeeds** ("Remove access" fallback applies) (Reason: Entra ID owns the membership and can write the change).
   - IF the group is **synced from on-premises Active Directory** → REJECT **automatic removal** (Reason: Entra ID is not the source of authority; membership cannot be written back — download results and fix on-prem).
   - IF the user is a member **via a nested group** → REJECT **removal** (Reason: Reviews evaluate *direct members only*; Entra ID will not drill into the nested group).
   - IF the requirement is "remove guests inactive for 30 days from App1" → SELECT **Access review of the application, scoped to Guest users only, with inactivity-based recommendations + Auto-apply results** (Reason: Inactivity uses last sign-in date; auto-apply revokes without admin effort).
   - IF the option is a Conditional Access policy or compliance policy → REJECT (Reason: CA evaluates at sign-in; it cannot recertify historical access).
4. **SC-300 Golden Rule:** **Access reviews only cut what Entra ID owns** — cloud groups, direct members. Synced groups and nested memberships are untouchable.
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/access-reviews-faqs and https://learn.microsoft.com/entra/id-governance/complete-access-review#apply-the-changes

---

### 3. Identity Protection — User Risk vs. Sign-in Risk (Q147, Q161, Q163, Q166, Q173, Q174)
1. **Exam Scenario:** Leaked credentials, anonymous IP sign-ins, "confirm user compromised" actions, and risk-based CA outcomes.
2. **Key Decision Point:** Is the **identity itself** compromised (User risk) or is this **one authentication attempt** suspicious (Sign-in risk)?
3. **Decision Path:**
   - IF the detection is **leaked credentials / Azure AD threat intelligence / confirmed compromise** → SELECT **User risk** (Reason: Offline detection — the account is compromised regardless of sign-in context; leaked credentials always arrive as High).
   - IF the detection is **anonymous IP, unfamiliar sign-in properties, atypical travel, Tor** → SELECT **Sign-in risk** (Reason: Real-time evaluation of a single authentication attempt).
   - IF the policy must remediate a compromised credential while keeping the user productive → SELECT **Grant access but require password change** (secure self-remediation via SSPR) or **Require risk remediation (MFA)**.
   - IF the admin performs **"Confirm user compromised"** → SELECT **Risk state = Confirmed compromised, risk level = High (permanent override)** → User risk policy applies (Block = blocked; no MFA escape).
   - IF the admin **dismisses** the risk → SELECT **User risk resets to None**, but a *new* sign-in risk (e.g., anonymous IP) can still trigger the sign-in risk policy.
   - IF the user must self-remediate via MFA but **MFA registration = Disabled** → SELECT **Sign-in blocked** (Reason: Risk remediation requires registered MFA methods).
   - IF the goal is to force *one specific user* to reset their password with least effort → SELECT **"Confirm user compromised"** (Reason: Leverages the existing High-risk user policy; do NOT reconfigure the global policy or reset MFA registration).
4. **SC-300 Golden Rule:** **User risk = who is bad; Sign-in risk = where/when is bad.** Leaked credentials → User risk → password change. Anonymous IP → Sign-in risk → MFA.
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-protection/concept-identity-protection-policies#user-risk-based-conditional-access-policy

---

### 4. Entitlement Management — Catalogs, Catalog Creators & Access Package Lifecycle (Q148, Q151, Q180)
1. **Exam Scenario:** Bundle a SharePoint site + Team into a requestable, time-limited package; or delegate catalog creation to a non-admin.
2. **Key Decision Point:** What is the **container hierarchy** and who holds the **delegated permission**?
3. **Decision Path:**
   - IF you must create an access package for Site1/Team1 → SELECT **Create a catalog first** (Reason: *All access packages must live in a catalog*; the catalog is the prerequisite container for resources).
   - IF a non-admin must create new catalogs and add their own resources → SELECT **Identity Governance → Entitlement Management settings → add user to "Catalog creators"** (Reason: Catalog creators automatically become catalog owners of what they create; NOT a directory role).
   - IF the option is an administrative unit → REJECT (Reason: AUs delegate admin permissions over users/groups, not entitlement resources).
   - IF the option is an app registration → REJECT (Reason: App registrations integrate apps; irrelevant to internal resource governance).
   - IF access must expire after 30 days with auto-approval for a group → SELECT **Access package policy: auto-approve for group members + assignment expiration of 30 days** (Reason: Lifecycle settings govern time-bound access).
   - IF the option is PIM, Conditional Access, or Defender for Cloud Apps access policy → REJECT (Reason: PIM = privileged roles; CA = sign-in conditions; MCAS access policy = session control — none manage request/approval/expiration lifecycle).
4. **SC-300 Golden Rule:** **Catalog → Access Package → Policy.** No catalog, no package. Catalog creators are made in *Entitlement Management settings*, not in Roles and administrators.
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/entitlement-management-access-package-create

---

### 5. Azure RBAC vs. Microsoft Entra ID Roles — Two Control Planes (Q149, Q155)
1. **Exam Scenario:** Clone a role for a subscription custom role; or let users sign in to / manage Azure VMs with Entra credentials.
2. **Key Decision Point:** Which **authorization plane** does the target resource live in — Azure resources (Azure RBAC) or the directory (Entra roles)?
3. **Decision Path:**
   - IF creating a custom **Azure subscription** role via portal cloning → SELECT **Built-in Azure subscription roles + existing custom Azure subscription roles (Role2)** (Reason: Same plane — cloneable).
   - IF the option includes cloning **Entra ID roles (Role1 or built-in Entra roles)** → REJECT (Reason: Entra roles operate on a separate identity control plane; they cannot seed Azure RBAC roles).
   - IF users must **sign in to a VM** with Entra credentials → SELECT **Azure RBAC "Virtual Machine Administrator Login" / "Virtual Machine User Login"** at the resource level.
   - IF users must **create/manage VMs** → SELECT **Azure RBAC "Virtual Machine Contributor"**.
   - IF the option is an Entra directory role (e.g., Global Administrator) or managed identity → REJECT (Reason: Directory roles govern tenant objects; managed identities are service-to-service, not user logins).
4. **SC-300 Golden Rule:** **Entra roles govern the directory; Azure RBAC governs the cloud.** Never cross the planes — and VM access is *always* Azure RBAC.
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/permissions-reference (role plane separation) and https://learn.microsoft.com/azure/role-based-access-control/custom-roles-portal

---

### 6. Defender for Cloud Apps — Policy Types & Shadow IT Discovery (Q150, Q156)
1. **Exam Scenario:** Alert when a high-permission OAuth app is consented by >20 users; or discover unsanctioned cloud apps from firewall logs.
2. **Key Decision Point:** Is the signal an **app attribute**, a **user activity**, or **network traffic**?
3. **Decision Path:**
   - IF the alert is based on **app permission level + number of consenting users** → SELECT **OAuth app policy** (Reason: Purpose-built for third-party OAuth consent governance).
   - IF the trigger is a **user behavior** (mass download, failed sign-ins) → SELECT **Activity policy**.
   - IF the requirement is **discovering shadow IT from firewall/proxy logs** → SELECT **Cloud App Discovery** (upload traffic logs → reports of detected apps, users, volume).
   - IF the option is "enterprise applications in Entra ID" → REJECT (Reason: Manages *already integrated* apps, not discovery).
   - IF the option is Application Insights → REJECT (Reason: APM for developers' custom apps, not shadow IT).
   - IF the option is access reviews → REJECT (Reason: Recertifies *known* access; discovers nothing).
4. **SC-300 Golden Rule:** **OAuth policy = app consent; Activity policy = user deeds; Cloud App Discovery = firewall logs.**
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-cloud-apps/tutorial-shadow-it (Cloud App Discovery / shadow IT)

---

### 7. Microsoft Graph Permissions — Delegated vs. Application (Q157)
1. **Exam Scenario:** Which user can use an app registration to create calendar appointments for *every* user in the tenant?
2. **Key Decision Point:** Does the app hold the permission as **Delegated** (signed-in user context) or **Application** (tenant-wide, no user)?
3. **Decision Path:**
   - IF the app has **Calendars.ReadWrite (Application)** → SELECT **That app's user can write to ALL calendars tenant-wide** (Reason: Application permissions run without a signed-in user).
   - IF the app has only **Calendars.Read (Delegated)** → REJECT (Reason: Read-only, limited to the logged-in user's own context).
   - IF the app has only **Calendars.Read (Application)** → REJECT (Reason: Tenant-wide but read-only — cannot create/modify).
4. **SC-300 Golden Rule:** **Delegated = "as me"; Application = "as the app, everywhere."** ReadWrite writes; Read only reads.
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity-platform/permissions-consent-overview

---

### 8. Hybrid Authentication — PTA, Password Writeback & Seamless SSO (Q159, Q171)
1. **Exam Scenario:** Users authenticate against on-prem DCs; SSPR must work for synced users; browsers must sign in silently.
2. **Key Decision Point:** Where does **password validation** happen, and can Entra ID **write back** changes?
3. **Decision Path:**
   - IF credentials must be validated **by the on-prem domain controller** → SELECT **Pass-through Authentication (PTA)** (Reason: Lightweight agent validates against the DC; PHS validates in-cloud; federation uses AD FS).
   - IF cloud **SSPR** must update on-prem passwords → SELECT **Enable Password writeback in Entra Connect** (Reason: Without writeback, cloud resets never reach AD).
   - IF browsers on domain-joined machines must SSO silently → SELECT **Add https://autologon.microsoftazuread-sso.com to the Local intranet zone** (via GPO) (Reason: Kerberos ticket forwarding requires the trusted zone).
   - IF the option is the Authentication Agent on clients → REJECT (Reason: PTA agents install on *servers*, not client machines).
   - IF the option is Enterprise State Roaming or Settings > Sign-in options → REJECT (Reason: Settings sync / local device auth — not cloud SSO).
4. **SC-300 Golden Rule:** **PTA validates on-prem, writeback writes on-prem, intranet zone unlocks Seamless SSO.**
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-pta and https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-sso

---

### 9. Access Review Licensing — P2 Members vs. MAU Guests (Q160)
1. **Exam Scenario:** Count licenses for access reviews involving 500 members + 25 guests, or 95 guests reviewed by one owner.
2. **Key Decision Point:** Is the governed identity a **member user** (license-based) or a **guest user** (MAU billing)?
3. **Decision Path:**
   - IF **member users** are in scope of (or performing) the review → SELECT **One Microsoft Entra ID P2 (or ID Governance) license per member user** (Reason: Every employee reviewing or being reviewed needs a license).
   - IF **guest users** are being reviewed → SELECT **No P2 license for guests** (Reason: Guests are billed via the Monthly Active User (MAU) model, not per-user licenses).
   - IF only the **reviewer** is a member (e.g., one Group Owner reviewing 95 guests) → SELECT **Exactly 1 P2 license** (for the owner).
4. **SC-300 Golden Rule:** **Members count licenses; guests count MAU.** License the reviewer *and* the reviewed members — never the guests.
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/licensing-fundamentals

---

### 10. Conditional Access Testing — What If Tool (Q162, Q175)
1. **Exam Scenario:** Simulate a sign-in from another country with a specific risk level before enabling a policy.
2. **Key Decision Point:** Is the requirement **proactive simulation** or **post-event analysis**?
3. **Decision Path:**
   - IF you must simulate conditions (user, IP, risk level, platform) → SELECT **Conditional Access What If tool** (Reason: Reports which policies would apply and their grant/session outcomes without affecting users).
   - IF the option is sign-in logs → REJECT (Reason: Records *actual past* sign-ins — reactive, not simulated).
   - IF the option is access reviews or Identity Secure Score → REJECT (Reason: Governance recertification / posture metric — not policy simulation).
   - IF the option is the Microsoft 365 network connectivity test → REJECT (Reason: Measures network performance, not identity policy logic).
4. **SC-300 Golden Rule:** **What If simulates the future; sign-in logs autopsy the past.**
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/whatif

---

### 11. MFA Methods — Number Matching & Registration Grace Period (Q152, Q165)
1. **Exam Scenario:** Which users see a number-matching prompt? How long can users postpone MFA registration?
2. **Key Decision Point:** Which **authentication method** is assigned, and which policy governs **enrollment**?
3. **Decision Path:**
   - IF the user's method is **Microsoft Authenticator push** → SELECT **Number matching applies** (Reason: Now standard for all Authenticator push notifications to defeat MFA fatigue attacks).
   - IF the method is **FIDO2** → REJECT (Reason: Physical key + PIN/biometric — no push/number match).
   - IF the method is **SMS** → REJECT (Reason: One-time passcode via text — no interactive prompt).
   - IF users must be forced to register for MFA → SELECT **ID Protection MFA registration policy** (Reason: CA enforces MFA at sign-in but does *not* manage registration grace).
   - IF asked about the bypass window → SELECT **Default 14-day grace period**, after which sign-in is blocked until registration completes.
4. **SC-300 Golden Rule:** **Number matching rides only on Authenticator push; the 14-day grace lives in the MFA registration policy, not Conditional Access.**
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/how-to-mfa-number-match

---

### 12. Application Roles & Admin Consent Workflow (Q167, Q168)
1. **Exam Scenario:** Manage App Proxy settings without owning apps or managing credentials; approve admin consent requests.
2. **Key Decision Point:** Which role grants the needed rights at **least privilege**?
3. **Decision Path:**
   - IF the task is managing **Application Proxy settings** without owning apps → SELECT **Cloud Application Administrator** (Reason: Can manage proxy/enterprise apps but *cannot* manage app credentials — least privilege vs. Application Administrator).
   - IF the user must *register* apps (becoming owner) → REJECT **Application developer** for User1 (Reason: Registration auto-makes the registrant an owner — violates the requirement).
   - IF approving **admin consent requests** → SELECT **Global Administrator, Cloud Application Administrator, or Application Administrator** (Reason: The reviewer must natively hold consent-granting privileges; being named a "reviewer" in the workflow grants nothing).
   - IF the reviewer has no directory role (User1/User2) or holds Authentication administrator (Admin2) → REJECT (Reason: No consent-grant privileges).
4. **SC-300 Golden Rule:** **Cloud App Admin = proxy without credentials; consent approval follows the role, not the reviewer list.**
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/permissions-reference and https://learn.microsoft.com/entra/identity/enterprise-apps/configure-admin-consent-workflow

---

### 13. B2B Guest Authentication — Email OTP & User Flows (Q169, Q172)
1. **Exam Scenario:** Which auth method does each guest use? Collect first name/city/email at sign-up.
2. **Key Decision Point:** What identity does the guest **arrive with**, and is a **user flow** configured?
3. **Decision Path:**
   - IF the guest has an **Entra ID account or MSA (outlook.com)** → SELECT **Home-tenant/MSA credentials** (Reason: Existing identity providers take precedence).
   - IF the guest has **gmail.com (no Google federation configured)** → SELECT **Email one-time passcode (OTP) fallback** — valid for exactly **30 minutes**.
   - IF you must collect user attributes at sign-up → SELECT **Configure a user flow → User attributes section** (Reason: Attributes are selected in the user flow, not the identity provider settings).
   - IF enabling OTP itself → SELECT **External Identities → All identity providers → Email one-time passcode**.
4. **SC-300 Golden Rule:** **Real identity first, OTP last — and OTP lives 30 minutes.** Attributes are a *user flow* setting.
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/one-time-passcode

---

### 14. Security Defaults & Break-Glass Accounts (Q170, Q176)
1. **Exam Scenario:** Allow alternate MFA methods despite security defaults; design emergency access accounts.
2. **Key Decision Point:** Is the tenant under **security defaults**, and is the account **cloud-only and permanently privileged**?
3. **Decision Path:**
   - IF security defaults are enabled → SELECT **All users are forced to Microsoft Authenticator only**; to permit other methods → **Disable security defaults** (requires Global Admin, Security Admin, or Conditional Access Admin).
   - IF creating a break-glass account → SELECT **Cloud-only account in Entra ID (*.onmicrosoft.com) + permanent Global Administrator** (Reason: Must survive on-prem outage/federation failure/sync failure).
   - IF the option places the account in an on-prem OU or a synced location → REJECT (Reason: Break-glass accounts must never depend on on-prem infrastructure).
   - IF the option assigns a lower role or excludes Global Admin → REJECT (Reason: Break-glass exists to recover the *entire* tenant).
4. **SC-300 Golden Rule:** **Security defaults = Authenticator-only; break-glass = cloud-only + Global Admin, forever.**
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/security-defaults and https://learn.microsoft.com/entra/identity/role-based-access-control/security-emergency-access

---

### 15. Logging & Reporting — Sign-in vs. Audit Logs, ToU, Secure Score (Q153, Q178)
1. **Exam Scenario:** Find who accepted/declined a Terms of Use; identify IP/location of sign-ins; quantify security posture.
2. **Key Decision Point:** Is the event an **authentication attempt**, an **administrative/consent action**, or a **posture metric**?
3. **Decision Path:**
   - IF you need **IP address / geographic location** of authentication attempts → SELECT **Sign-in logs**.
   - IF you need **who accepted or declined a Terms of Use** → SELECT **Audit logs** (filter "Accept/Decline terms of use policy") (Reason: ToU acceptance is a recorded *activity event*, not the sign-in itself).
   - IF you need **tenant-wide admin changes** (users, groups, service principals) → SELECT **Audit logs**.
   - IF you need **quantified improvement recommendations** → SELECT **Identity Secure Score**.
   - IF the option is provisioning logs or Usage & Insights → REJECT (Reason: Provisioning = automated object sync; Usage & Insights = aggregated trends, not event-level ToU status).
4. **SC-300 Golden Rule:** **Sign-in logs = the attempt; Audit logs = the action; Secure Score = the grade.**
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/terms-of-use

---

### 16. App Registration Lifecycle — Soft Delete (Q179)
1. **Exam Scenario:** How long after deleting an app registration can it be restored?
2. **Key Decision Point:** Is the object within the **soft-delete retention window**?
3. **Decision Path:**
   - IF an app registration is deleted → SELECT **Soft-deleted for 30 days** (Reason: Registration + service principal + permissions are fully restorable via Entra admin center → App registrations → Deleted applications, or Graph `deletedItems`).
   - IF the answer is 14, 60, or 180 days → REJECT (Reason: Official retention is exactly 30 days; after that, permanent deletion).
4. **SC-300 Golden Rule:** **Deleted apps rest in the recycle bin for 30 days — restore the registration, restore the service principal.**
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/architecture/recover-from-deletions and https://learn.microsoft.com/entra/identity/enterprise-apps/restore-application

---

### 17. Group Types for Role Assignment (Q154)
1. **Exam Scenario:** Create a group that can receive Entra roles AND have users added manually.
2. **Key Decision Point:** Is the group **role-assignable**, and is membership **Assigned** (manual) vs. **Dynamic** (rule-based)?
3. **Decision Path:**
   - IF roles must be assigned to the group → SELECT **"Entra roles can be assigned to the group" = Yes** (available for both Security and Microsoft 365 groups).
   - IF specific users must be added manually → SELECT **Assigned membership** (Dynamic User/Dynamic Device are rule-based and reject manual adds; Dynamic Device targets *devices*, not users).
   - IF the option is Dynamic User or Dynamic Device → REJECT (Reason: Contradicts manual membership control).
4. **SC-300 Golden Rule:** **Role-assignable + Assigned = roles via group, members by hand.**
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/roles/groups-concept

---

### 18. Tenant Restrictions Prerequisites (Q164)
1. **Exam Scenario:** Enforce tenant restrictions for users on Outlook 2013 and Outlook 2016 clients.
2. **Key Decision Point:** Does the client support **modern authentication (OAuth 2.0)**?
3. **Decision Path:**
   - IF clients include Outlook 2013 (legacy auth, out of support) → SELECT **Upgrade non-compliant clients to Outlook 2016+ first** (Reason: Tenant restrictions intercept modern auth flows only; 2013 needs registry hacks and is unsupported).
   - IF the option upgrades *all* clients including compliant 2016 → REJECT (Reason: Unnecessary effort — violates the minimal-change requirement).
   - IF the option is Organization Sharing → REJECT (Reason: Exchange calendar-sharing feature, unrelated to tenant restrictions).
4. **SC-300 Golden Rule:** **Tenant restrictions speak modern auth — upgrade the legacy client before touching policy.**
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/tenant-restrictions

---

## Quick-Reference Master Table

| Signal in Question | Correct Answer Pattern | Kill Shot (Reject) |
|---|---|---|
| Block printing/downloads in-session | MCAS **session policy** + CA App Control | Activity/File/Access policy |
| Leaked credentials | **User risk** → require password change | Sign-in risk policy |
| Anonymous IP / Tor | **Sign-in risk** → require MFA | User risk policy |
| Access package for Site1 | **Catalog first**, then package + policy | PIM / CA / admin unit |
| Group synced from on-prem AD in review | Removal **fails** (no source of authority) | Auto-apply succeeds |
| Nested group member in review | **Retains access** (direct members only) | Removal succeeds |
| Guest in access review | **MAU billing**, no P2 | P2 per guest |
| VM sign-in / VM management | **Azure RBAC** (VM Login / VM Contributor) | Entra directory role |
| Clone custom Azure role | Built-in **subscription** roles + custom subscription roles | Cloning Entra ID roles |
| OAuth app consent >20 users | **OAuth app policy** | Anomaly/activity policy |
| Shadow IT from firewall logs | **Cloud App Discovery** | Enterprise apps / App Insights |
| Write calendars for all users | **Application** permission (ReadWrite) | Delegated Read |
| Simulate CA policy | **What If tool** | Sign-in logs / Secure Score |
| ToU accept/decline tracking | **Audit logs** | Sign-in logs / Usage & Insights |
| Deleted app registration | **30-day** soft delete | 14/60/180 days |
| Break-glass account | **Cloud-only + Global Admin** | On-prem OU / synced account |
| OTP validity for B2B guests | **30 minutes** | Other durations |
| MFA registration grace | **14 days** (ID Protection policy) | Conditional Access |# SC-300 Decision Trees - Part 6

All technical points are now validated against Microsoft Learn. Below is the structured decision-tree analysis for Part 6 (Questions 181–216).

# SC-300 Decision Trees & Logic Rules — Part 6 (Q181–216)

---

## Section 1 — Privileged Identity Management (PIM)

### 1.1 PIM Role Activation & Portal Navigation
1. **Exam Scenario:** A user is *eligible* for a privileged role (e.g., Application administrator) and must perform a task (e.g., configure an App Proxy connector group). Where/how does activation happen?
2. **Key Decision Point:** Assignment type (Eligible vs. Active) and the correct portal for the role's domain.
3. **Decision Path:**
   - IF the phrase *"eligible for the role"* appears -> SELECT **PIM activation in the Microsoft Entra admin center** (ID Governance > PIM > My roles > Activate) (Reason: "Eligible" = PIM just-in-time; permissions exist only after activation).
   - IF the role is an **Entra ID directory role** -> USE the **Microsoft Entra roles** blade in PIM (NOT Azure resources) (Reason: PIM has separate panes for Entra roles, Azure resource roles, and privileged access groups).
   - IF the portal offered is **M365 admin center / Defender portals** -> REJECT them (Reason: M365 admin center manages licenses/services; Defender portals are threat management — neither activates directory roles).
4. **SC-300 Golden Rule:** *"Eligible" means PIM — activation always happens in the Microsoft Entra admin center, never in a workload portal.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-how-to-activate-role

### 1.2 PIM Activation Settings: MFA, Approval, Justification
1. **Exam Scenario:** A PIM exhibit shows per-role activation settings ("Require MFA on activation", "Require approval", "Approvers", "Require justification"). Evaluate Yes/No statements about who is prompted or must act.
2. **Key Decision Point:** The **per-role** PIM configuration exhibit (settings are scoped per role, not tenant-wide).
3. **Decision Path:**
   - IF *"On activation, require Azure MFA" = Yes* -> the eligible user **IS prompted for MFA** at activation.
   - IF *"Require approval to activate" = No* (or Approvers = None) -> **NO approval event is generated** for any approver.
   - IF the requester IS the designated approver -> request **CANNOT be approved** (Reason: PIM explicitly blocks self-approval; service principals also can't approve).
   - IF justification appears "not required" for the requester -> don't assume the approver is exempt: the **approver must always enter a justification** when approving/denying.
   - IF asked who can EDIT PIM assignments -> require **Privileged Role Administrator** or **Global Administrator** (Reason: Privileged Authentication Administrator manages auth methods/passwords only — NOT PIM assignments).
4. **SC-300 Golden Rule:** *MFA/justification gate the requester; approval gates activate only if configured; self-approval is always impossible; approvers always justify.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-approval-workflow

### 1.3 Password Administrator Scoping & Administrative Units (AU)
1. **Exam Scenario:** Password Administrators with different scopes (Organization vs. AU-scoped) attempt password resets on mixed users (non-admins, Global Reader, other Password Admins).
2. **Key Decision Point:** Two independent filters — **role scope (AU boundary)** AND **target user's role tier**.
3. **Decision Path:**
   - IF target is a **non-administrator** AND inside the admin's scope -> RESET allowed.
   - IF target is a **Password Administrator** (org-scoped admin) -> RESET allowed by an org-scoped Password Administrator (Password Admins are in the self-service reset subset).
   - IF target is a **Global Reader / higher-tier admin** -> REJECT (Reason: Global Reader requires Global Administrator or Privileged Authentication Administrator to reset).
   - IF the resetter is **AU-scoped** AND target holds any **org-wide admin role** -> REJECT (Reason: AU-scoped admins can never reset org-wide admins — anti-escalation guardrail).
   - IF target is outside the AU boundary -> REJECT regardless of role.
4. **SC-300 Golden Rule:** *AU scope is a hard boundary; org-wide admins are untouchable by AU-scoped admins; Global Readers outrank Password Admins.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/permissions-reference

---

## Section 2 — Identity Governance: Entitlement, Consent & Reviews

### 2.1 Least-Privilege Role for Access Reviews & Access Packages
1. **Exam Scenario:** A user must create/manage **access reviews** (including for Entra roles) or **access package settings** (e.g., review frequency) with minimum privilege.
2. **Key Decision Point:** Match the governance surface to the narrowest built-in role.
3. **Decision Path:**
   - IF task = create/manage **access reviews** or **access packages** -> SELECT **Identity Governance Administrator** (Reason: purpose-built for governance surfaces; least privilege).
   - IF option offered is **Privileged Role Administrator** -> REJECT as a distractor (Reason: overly broad — manages PIM role assignments and role-assignable groups; violates least privilege even though it *can* create reviews of Entra roles).
   - IF option is **User administrator** -> REJECT for access reviews (Reason: manages user/group lifecycle only, not governance features).
   - IF option is **User Access Administrator** -> REJECT (Reason: that is an **Azure RBAC** role for Azure resources — wrong permission plane for Entra ID role reviews).
   - IF the sole requirement is viewing Secure Score (read) -> **Security Reader**.
4. **SC-300 Golden Rule:** *Governance work → Identity Governance Administrator; Azure-resource permissions → User Access Administrator; never confuse the two permission planes.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/delegate-by-task

### 2.2 Application Consent Governance (User Consent & Admin Consent Workflow)
1. **Exam Scenario:** (a) Restrict users to consent only to low-impact permission apps; (b) designate consent-request reviewers and determine who can actually approve tenant-wide consent.
2. **Key Decision Point:** Distinguish **classifying permissions** (prerequisite), **consent policy**, and the **role required to grant tenant-wide consent**.
3. **Decision Path:**
   - IF goal = users consent only to low-impact apps -> STEP 1: configure **permission classifications** (mark permissions as "low impact") -> STEP 2: set user consent to *"consent only for low-impact classified permissions"*.
     - REJECT **access package** (bundles resources, not permission risk tiers).
     - REJECT **app collection** (My Apps organization only — no consent effect).
     - REJECT **access review** (recertifies existing access; doesn't govern initial consent).
   - IF goal = consent request reviewers -> any designated user can **view** requests, but only a reviewer holding a role with tenant-wide consent authority can **approve**:
     - **CAN approve:** Cloud Application Administrator, Application Administrator, (and higher: Privileged Role Administrator, Global Administrator).
     - **CANNOT approve:** Security Administrator, User Administrator, or a plain user/owner — designation alone is insufficient.
4. **SC-300 Golden Rule:** *Classify first, then restrict consent; a reviewer without a consent-granting admin role is a spectator, not an approver.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/configure-admin-consent-workflow

### 2.3 Entitlement Management: Custom Extensions & Access Package Distribution
1. **Exam Scenario:** Extend access package lifecycles with custom automation and distribute catalog resources to users.
2. **Key Decision Point:** Mapping lifecycle events to automation primitives.
3. **Decision Path:**
   - IF custom automation (e.g., notifications, provisioning) must run at lifecycle events -> CREATE/USE an **Azure Logic App** wired as a **custom extension** (Reason: custom extensions in Entitlement Management are powered by Logic Apps).
   - IF Catalog1's resources must reach users -> CREATE an **access package** in the catalog (Reason: catalogs are containers; only access packages + their policies make resources requestable — policies also attach the custom extension to lifecycle stages).
   - IF the option is a "catalog" as the distribution mechanism -> REJECT (Reason: a catalog alone grants nothing).
4. **SC-300 Golden Rule:** *Catalog = container, Access package = product, Policy = rules, Logic App = automation.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/entitlement-management-access-package-create

### 2.4 External Collaboration Settings (B2B Guest Controls)
1. **Exam Scenario:** Guests must not read directory data (e.g., staff emails); only a specific admin (User1, a User Administrator) may invite; no self-service entry.
2. **Key Decision Point:** Choose the **most restrictive** matching option in each of the three External collaboration settings groups.
3. **Decision Path:**
   - IF requirement = guests must NOT query directory properties -> SELECT **"Guest users have restricted access... access properties and memberships of their own directory objects only"** (Reason: the "limited access" option still allows querying some properties → fails the requirement).
   - IF only User1 (User Administrator) may invite -> SELECT **"Only users assigned to specific admin roles can invite guest users"** (Reason: User Administrator/Guest Inviter qualify; members and guests are blocked).
   - IF guests must enter ONLY via invitation -> set **guest self-service sign-up via user flows = No** (Reason: "Yes" creates an invitation bypass).
4. **SC-300 Golden Rule:** *When a scenario says "completely prevent," always pick the most restrictive guest setting — intermediate options leak directory data.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/external-collaboration-settings-configure

---

## Section 3 — Conditional Access & Microsoft Defender for Cloud Apps (MDCA)

### 3.1 Security Defaults vs. Conditional Access — Mutual Exclusivity
1. **Exam Scenario:** Enabling security defaults fails because Conditional Access policies exist in the tenant. What must be removed first?
2. **Key Decision Point:** Tenant-level exclusivity between baseline security defaults and CA policies.
3. **Decision Path:**
   - IF any Conditional Access policy exists (active or configured) -> security defaults CANNOT be enabled until **all CA policies are deleted/disabled**.
   - IF the distractor is an Identity Governance object (access package, access reviews config) -> REJECT (Reason: no interaction with security defaults).
   - IF the actor only holds **Authentication administrator scoped to an AU** -> REJECT (Reason: lacks tenant-wide scope; security defaults are tenant-wide authentication policy).
4. **SC-300 Golden Rule:** *Security defaults and Conditional Access are either/or — CA policies must go before security defaults come on.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/security-defaults

### 3.2 Named Locations (CA) vs. Per-User MFA Trusted IPs
1. **Exam Scenario:** Users connect from private IPs (10.x/192.168.x) NAT-ed to public IPs. A CA policy applies to a group at "All trusted locations"; legacy per-user MFA has its own trusted IP range. Who gets prompted for MFA?
2. **Key Decision Point:** Entra ID evaluates the **public NAT IP**, never the private IP. Two independent mechanisms: CA named locations vs. legacy per-user MFA trusted IPs.
3. **Decision Path:**
   - IF named location is defined with a **private subnet** but the observed IP is **public NAT** -> location **does not match**; a CA policy scoped to "All trusted locations" **does not apply** → fall through to per-user MFA state.
   - IF CA policy doesn't apply AND per-user MFA = **Disabled** -> **no MFA prompt**.
   - IF CA policy doesn't apply AND per-user MFA = **Enforced** -> MFA applies UNLESS the public IP matches the **legacy MFA Trusted IP range**.
     - IF public NAT IP ∈ trusted range -> **no MFA prompt** (bypass honored).
     - IF public NAT IP ∉ trusted range -> **MFA prompted**.
   - IF a distractor assumes the private IP is evaluated -> REJECT (Reason: only the internet-facing public IP reaches Entra ID).
4. **SC-300 Golden Rule:** *Policies match public NAT IPs; trusted-location CA and legacy per-user MFA trusted IPs are separate systems — check group membership, MFA state, then the IP plane.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/location-condition

### 3.3 MDCA: Onboarding an App for Session Control
1. **Exam Scenario:** Real-time access/session policies must be enforced on App1. What is the FIRST configuration step?
2. **Key Decision Point:** MDCA session enforcement requires routing the session through the MDCA reverse proxy via a Conditional Access session control.
3. **Decision Path:**
   - IF real-time access/session control on an Entra SSO app is required -> CREATE a **Conditional Access policy** targeting the app with **Session control = "Use Conditional Access App Control"** (Reason: this onboards/proxies the app to MDCA; only then can MDCA policies be authored).
   - IF the option is **token configuration / API permissions** -> REJECT (Reason: token claims and Graph permissions never route traffic through MDCA).
   - IF the option is **"Use app-enforced restrictions"** -> REJECT (Reason: the app itself enforces limits, e.g., SharePoint — not the MDCA proxy).
   - **Licensing threshold:** CAAC deployment requires **Microsoft Entra ID P1 (or higher) + a Defender for Cloud Apps license**.
4. **SC-300 Golden Rule:** *No CA session control → no MDCA proxy → no real-time session policy; onboarding starts in Conditional Access, not in MDCA.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-cloud-apps/apps-manual-onboarding-with-microsoft-entra-id

### 3.4 MDCA: Choosing the Correct Policy Type
1. **Exam Scenario:** Four requirements — (a) block sign-ins to Yammer from high-risk locations; (b) alert on >50 downloads/minute on Site1; (c) identify (monitor-only) who accesses Facebook; (d) control user sessions in real time.
2. **Key Decision Point:** Intent taxonomy: **prevent sign-in → access policy; control session actions → session policy; alert on repeated activity thresholds → activity policy; behavioral anomalies → anomaly detection; content-at-rest → file policy.**
3. **Decision Path:**
   - IF block access based on **location/IP risk** -> **Access policy** with Location filter (Anonymous proxy, Satellite, custom tags) + Action = **Block**.
   - IF alerting on a **repeatable activity threshold** (e.g., >50 downloads/minute) on a specific app/site -> **Activity policy** with the **"Repeated activity"** trigger + app/site filters.
   - IF monitoring-only identification of who uses an app -> **Access policy** in **monitor-only** mode (minimal effort; better than tagging the app unsanctioned, which only feeds block scripts to network appliances).
   - IF enforcing real-time restrictions **within** a session (e.g., block downloads) -> **Session policy** (requires CAAC onboarding per 3.3).
   - IF the trigger is user-behavior ML baselines (impossible travel) -> **Anomaly detection policy** (REJECT for fixed thresholds like "50 files" — anomaly detection doesn't enforce user-defined counts).
   - IF scanning stored content/sharing -> **File policy** (REJECT for user actions like downloads).
4. **SC-300 Golden Rule:** *Access = who may get in; Session = what they may do inside; Activity = how often they do it; Anomaly = how unusual it is; File = what's stored.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-cloud-apps/troubleshooting-proxy

### 3.5 App Governance Enablement & Portal Consolidation
1. **Exam Scenario:** After purchasing the app governance add-on license, where is the feature enabled?
2. **Key Decision Point:** Feature home portal (Microsoft is consolidating Defender portals into Microsoft Defender XDR).
3. **Decision Path:**
   - IF enabling/configuring app governance (OAuth app policy management) -> USE the **Microsoft Defender XDR / Microsoft 365 Defender portal** (Settings > Cloud Apps) — NOT the standalone MDCA portal.
   - IF options are M365 admin center / Entra admin center / Purview -> REJECT (Reason: licensing, identity, and compliance portals do not host app governance configuration).
4. **SC-300 Golden Rule:** *App governance lives under Defender for Cloud Apps, managed centrally in Microsoft Defender XDR — after buying the add-on license.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-cloud-apps/apps-manual-onboarding-with-microsoft-entra-id

---

## Section 4 — Passwords, Authentication & App Registration

### 4.1 SSPR Administrator Two-Gate Policy (Security Questions Ban)
1. **Exam Scenario:** SSPR is configured with security questions; determine which accounts may actually use them.
2. **Key Decision Point:** Microsoft's **unmodifiable two-gate admin SSPR policy** overrides tenant settings for any account holding an admin role.
3. **Decision Path:**
   - IF user holds ANY admin role from the restricted list (User Administrator, Password Administrator, Global Administrator, Security Administrator, etc.) -> two-gate policy applies: **two different auth data pieces required; security questions PROHIBITED** (cannot be changed).
   - IF user holds a **read-only role (e.g., Security Reader)** or **no admin role** -> tenant SSPR policy applies → **security questions permitted**.
   - IF the distractor assumes the tenant-level policy applies to everyone -> REJECT (Reason: admin policy is separate and enforced by Microsoft, independent of the Authentication methods policy).
4. **SC-300 Golden Rule:** *Any admin = forced two-gate SSPR; security questions are for members only — never for admins.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-sspr-policy

### 4.2 SSPR Writeback, Pass-Through Authentication & OU Filtering
1. **Exam Scenario:** Microsoft Entra Connect shows OU-filtered sync scopes, "Enable password writeback" checked, "Enable pass-through authentication" selected. Evaluate per-user capabilities.
2. **Key Decision Point:** An object must be **synced** to benefit from cloud features; auth path determines where credentials are validated.
3. **Decision Path:**
   - IF user's OU IS synced AND **password writeback enabled** -> cloud SSPR resets **write back** to on-prem AD → statement TRUE.
   - IF **PTA selected** -> cloud sign-ins (e.g., Exchange Online) validate credentials **on-premises via the PTA agent against a domain controller** → statement TRUE.
   - IF user's OU is **excluded from sync** -> identity never exists in Entra ID → **cannot be added** to cloud resources (e.g., SharePoint Online) → statement FALSE.
   - IF the distractor claims filtered-out users exist in the cloud -> REJECT (Reason: OU filtering prevents object creation in Entra ID entirely).
4. **SC-300 Golden Rule:** *No sync = no cloud object; writeback = cloud SSPR reaches AD; PTA = password check happens at your DCs.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-sync-configure-filtering

### 4.3 Identity Secure Score — Role Permission Matrix (Q197–Q200)
1. **Exam Scenario:** Four Yes/No drills test whether SharePoint Admin, Exchange Admin, User Admin, or Security Operator can **update the status** of Secure Score improvement actions.
2. **Key Decision Point:** Service-scoped admin roles (SharePoint/Exchange) do **not** inherit security-plane write rights; distinguish **read** vs. **update**.
3. **Decision Path:**
   - IF role = **SharePoint Administrator** or **Exchange Administrator** -> REJECT (Reason: scoped exclusively to their workload; no `secureScore/update` permission).
   - IF role = **User Administrator** -> REJECT (Reason: user/group lifecycle only — no security write).
   - IF role = **Security Operator** -> REJECT for updates (Reason: **read-only** on Secure Score; can manage alerts but cannot change improvement-action status).
   - IF the goal is to UPDATE improvement actions -> SELECT **Security Administrator** (least privilege) or Global Administrator.
   - IF the goal is merely to READ the score -> Security Reader suffices.
4. **SC-300 Golden Rule:** *Workload admins manage their workload, not security; for Secure Score writes it's Security Administrator — Security Operator can look but not touch.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/delegate-by-task

### 4.4 App Registration: Redirect URI by Platform
1. **Exam Scenario:** Register a .NET web application for authentication — what must be supplied?
2. **Key Decision Point:** Platform-specific registration identifiers.
3. **Decision Path:**
   - IF platform = **web app** -> REQUIRE a **Redirect URI** (token delivery endpoint; Entra ID only sends tokens to pre-registered URIs — anti-token-theft control).
   - IF platform = **iOS/macOS** -> Bundle ID; IF **Android** -> Package name; IF **desktop/native** -> executable/custom scheme — all REJECT for web apps.
4. **SC-300 Golden Rule:** *Web apps are identified by Redirect URI; native platforms by bundle/package identifiers.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity-platform/quickstart-register-app

---

## Section 5 — Azure Resources & Key Vault

### 5.1 Managed Identity Selection & the Region Rule (Q181, Q195)
1. **Exam Scenario:** (a) App1 needs Azure SQL access with zero credential maintenance and exclusive-to-App1 credentials; (b) attach user-assigned identities to VM1 across regions; (c) grant an identity Azure RBAC roles.
2. **Key Decision Point:** Identity type (system vs. user-assigned) and the **region co-location requirement** for user-assigned identities.
3. **Decision Path:**
   - IF credentials must be **exclusive to one resource** AND lifecycle-tied to it AND zero admin overhead -> SELECT **system-assigned managed identity** (Reason: auto-created/deleted with the resource; Azure rotates credentials).
   - IF identity must be **shared across multiple resources** or outlive them -> SELECT **user-assigned managed identity** (REJECT system-assigned then).
   - IF attaching a user-assigned identity to a resource -> identity and resource must be in the **SAME region** (cross-region attachment = invalid → statement FALSE).
   - IF multiple user-assigned identities on one VM -> **VALID** (no simultaneity limit; only the region rule applies).
   - IF granting **Azure RBAC** (e.g., Owner on RG1) to a VM's identity -> **VALID regardless of region** (Reason: RBAC is control-plane; region co-location applies only to attaching user-assigned identities, not to role assignments).
   - IF options are AD user accounts / SQL logins -> REJECT (Reason: manual password/secret management = increased admin effort).
4. **SC-300 Golden Rule:** *System-assigned = 1:1 lifetime + zero secrets; user-assigned = shareable but must live in the resource's region; RBAC role assignments ignore geography.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/managed-identity-best-practice-recommendations

### 5.2 Valid Security Principals for Azure RBAC
1. **Exam Scenario:** Assign Contributor on RG1 to: User1 (user), Group1 (**dynamic** security group), VM1 (**system-assigned managed identity**), App1 (**enterprise application**).
2. **Key Decision Point:** Anything resolvable to a **security principal** in Entra ID can hold an Azure RBAC role.
3. **Decision Path:**
   - IF principal = user, security group (dynamic or assigned), managed identity (which IS a service principal), or enterprise application (service principal) -> **ALL valid** role-assignment targets.
   - IF the distractor claims dynamic groups can't receive RBAC -> REJECT (Reason: dynamic membership governs how members join, not the group's role-assignability).
   - IF the distractor omits managed identities or service principals -> REJECT (Reason: both are service principals natively).
4. **SC-300 Golden Rule:** *Users, groups (even dynamic), managed identities, and service principals — all four eat Azure RBAC roles.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/managed-identities-faq

### 5.3 Key Vault Soft Delete & Purge Protection
1. **Exam Scenario:** Objects deleted on June 1; Vault A: 10-day retention, no purge protection; Vault B: 15-day retention **with purge protection**. Can an admin recover/purge on given dates?
2. **Key Decision Point:** Distinguish **recover** (anyone with data-plane recover permission, within retention) vs. **purge** (blocked by purge protection until retention elapses, then automatic).
3. **Decision Path:**
   - IF target date < deletion date + retention AND purge protection **off** AND admin holds **Key Vault Administrator** (data-plane role incl. `secrets/recover/action`) -> **recover SUCCEEDS**.
   - IF vault has **purge protection enabled** -> NO role — regardless of privilege — can purge before retention expiry (Reason: purge protection cannot be overridden by anyone, including Microsoft; recovery is the only path; after retention, purge is **automatic**).
   - IF the statement asserts a role can override purge protection -> REJECT (Reason: no administrator role or permission can override, disable, or circumvent it).
4. **SC-300 Golden Rule:** *Soft delete = recoverable recycle bin; purge protection = time-locked bin nobody can force open; retention math decides the date.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/azure/key-vault/general/key-vault-recovery

### 5.4 Key Vault Data-Plane RBAC Roles
1. **Exam Scenario:** User1 must create/manage **keys**; User2 must access **certificates** in Vault1.
2. **Key Decision Point:** Match object type to its dedicated data-plane role.
3. **Decision Path:**
   - IF task = manage/create **keys** -> SELECT **Key Vault Crypto Officer**.
   - IF task = access/manage **certificates** -> SELECT **Key Vault Certificates Officer**; IF **secrets** -> Key Vault Secrets Officer.
   - IF the option mixes object types (e.g., Crypto Officer for certificates) -> REJECT (Reason: each Officer role is object-scoped).
4. **SC-300 Golden Rule:** *Crypto Officer = keys, Certificates Officer = certificates, Secrets Officer = secrets — pick by object type, not by vault.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/azure/key-vault/general/rbac-guide

### 5.5 Application Proxy & App Registration — Least-Privilege Roles
1. **Exam Scenario:** Who may install the Application Proxy connector, and who may create app registrations?
2. **Key Decision Point:** The Cloud Application Administrator role **excludes** App Proxy permissions — role boundaries matter.
3. **Decision Path:**
   - IF task = install **Application Proxy connector** -> require **Application Administrator** or **Global Administrator** (REJECT Cloud Application Administrator — explicitly lacks proxy rights; prefer Application Administrator over Global Admin for least privilege).
   - IF task = **register an application** -> SELECT **Application Developer** (Reason: can create registrations without broader app-management powers).
4. **SC-300 Golden Rule:** *Cloud App Admin ≠ Application Admin for proxy work; registering apps needs only Application Developer.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/app-proxy/application-proxy-add-on-premises-application

---

## Section 6 — Directory Operations & Monitoring

### 6.1 Dynamic Membership Rules: Precedence & Null Checks (Q193, Q210)
1. **Exam Scenario:** Rule `-and`/`-or` chains determine membership; find users with no department set.
2. **Key Decision Point:** Operator precedence (-and binds tighter than -or) and null-literal syntax.
3. **Decision Path:**
   - IF the rule mixes `-and` and `-or` -> evaluate as `(all -and conditions) OR (each -or condition)` — a single `-or` clause (e.g., Title -eq "SalesRep") independently qualifies a user who fails the `-and` block.
   - IF testing "attribute is not set" -> use `user.department -eq null` — the **unquoted keyword `null`**.
     - REJECT `$null` (PowerShell syntax — invalid in Entra rules).
     - REJECT `"null"` or `""` (literal strings, not the null state).
4. **SC-300 Golden Rule:** *`-and` beats `-or`; null is a bare keyword — no dollar sign, no quotes.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/users/groups-dynamic-membership

### 6.2 Bulk User Creation — Mandatory CSV Properties
1. **Exam Scenario:** Choose the property set that must be present in the bulk-create CSV template.
2. **Key Decision Point:** The minimal identity-creation contract.
3. **Decision Path:**
   - IF performing **bulk create** -> REQUIRED: **displayName (Name), userPrincipalName (User name), passwordProfile (Initial password)** — all other columns (givenName, surname, usageLocation) are optional.
   - IF an option omits userPrincipalName or passwordProfile -> REJECT (Reason: no login ID / no initial credential = import failure).
   - IF an option presents optional attributes (givenName/surname/usageLocation) as mandatory -> REJECT.
4. **SC-300 Golden Rule:** *Name, User name, Initial password — the only mandatory trio in the bulk CSV.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/users/users-bulk-add

### 6.3 Troubleshooting Sign-in Failures: Log Selection & KQL
1. **Exam Scenario:** (a) User1 can't access the MDCA portal — find the cause with minimal effort; (b) graphically display failed sign-in counts per user.
2. **Key Decision Point:** Match log type to event class, then the minimal-effort tool.
3. **Decision Path:**
   - IF diagnosing a **user sign-in failure** (error code, failure reason, app, user) -> SELECT **Sign-in logs** in the Entra admin center (Reason: direct filter-by-user/app; no pre-configuration needed).
   - IF the option is **audit logs** -> REJECT (administrative/config change events, not sign-ins); IF **provisioning logs** -> REJECT (automated account lifecycle to target apps).
   - IF the option is **Log Analytics** as the FIRST step -> REJECT for minimal effort (Reason: requires prior diagnostic-settings configuration + KQL authoring).
   - IF the requirement is a **chart** of counts grouped by identity in Log Analytics -> KQL pattern: `| summarize count() by <Identity> | render columnchart` (Reason: `summarize` aggregates; `render` produces the visualization).
4. **SC-300 Golden Rule:** *Sign-in failures live in sign-in logs; audit = admin actions; provisioning = object sync. Chart it with summarize + render.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/monitoring-health/concept-sign-ins

---

## Quick-Reference Cheat Table (Part 6)

| Trigger phrase in question | Correct answer instinct | Trap to reject |
|---|---|---|
| "Eligible for role" | PIM activation (Entra admin center) | M365/Defender portals |
| "Approver is the requester" | Self-approval blocked | Assuming approval succeeds |
| "Guests must not read directory" | Most restrictive guest access setting | "Limited access" middle option |
| "Security defaults" + existing CA policies | Delete/disable CA policies first | Identity Governance objects |
| Real-time MDCA session control | CA policy + "Use Conditional Access App Control" | App-enforced restrictions |
| Block sign-in by location (MDCA) | Access policy | Activity/anomaly policies |
| Alert on repeated activity threshold | Activity policy ("Repeated activity") | Anomaly detection (ML baseline only) |
| Update Secure Score status | Security Administrator | Security Operator (read-only), workload admins |
| Access reviews / packages, least privilege | Identity Governance Administrator | Privileged Role Administrator |
| User-assigned identity + different region | Attach INVALID | Assuming RBAC grants need region match |
| Purge protection enabled | Nobody purges before retention ends | "Admin role can override" |
| Admin + security questions SSPR | Two-gate policy — questions banned | Tenant policy applies to admins |
| Dynamic rule + `$null` | Use bare `null` keyword | PowerShell `$null` syntax |
| Dynamic rule `-and` + `-or` | `-and` evaluated first | Flat left-to-right reading |
| Bulk CSV required fields | displayName, userPrincipalName, passwordProfile | givenName/surname/usageLocation as required |

---

**Licensing quick-checks validated against Microsoft Learn:** PIM requires **Microsoft Entra ID P2**; Conditional Access App Control requires **Entra ID P1 + MDCA license**; App governance requires its **add-on license**; SSPR admin two-gate policy applies at all tiers and is non-modifiable.# SC-300 Decision Trees - Part 7

All key facts are now validated against official Microsoft Learn documentation. Below is the structured Decision Tree and logic-rule set derived from Part 7 (Questions 217–252), with every rule grounded in current Microsoft Learn terminology (Microsoft Entra ID, PIM, Entitlement Management, etc.).

---

# SC-300 Decision Trees & Logic Rules — Part 7 (Q217–Q252)

> **Legend:** ✅ = Correct selection | ❌ = Common distractor to reject | 🔑 = Licensing/scope threshold

---

## SECTION A — Built-in Directory Roles & Least Privilege

### D1. Delegating User Management Tasks (Q217)
1. **Exam Scenario:** A helpdesk-style task requires *both* resetting user passwords *and* managing license assignments with a single, least-privileged role.
2. **Key Decision Point:** Which built-in role's permission set is the **superset** of both required tasks?
3. **Decision Path:**
   - IF tasks = [reset passwords + manage licenses] -> SELECT ✅ **User Administrator** (Reason: manages all aspects of users and groups, resets passwords for non-admins and limited admin roles, and can add/remove/update license assignments)
   - IF task = license assignments only -> ❌ REJECT **License Administrator** (Reason: cannot reset passwords)
   - IF task = password reset only -> ❌ REJECT **Helpdesk Administrator** (Reason: cannot manage license assignments)
   - IF task = purchasing/subscriptions -> ❌ REJECT **Billing Administrator** (Reason: manages purchases, invoices, subscriptions — no password or license-per-user management)
4. **SC-300 Golden Rule:** *User Administrator = Users + Groups + Passwords + Licenses; Helpdesk = Passwords only; License = Licenses only; Billing = Subscriptions only. Always pick the smallest role whose permission set is a superset of the task list.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/permissions-reference

### D2. Comparing Built-in Role Permissions (Q222)
1. **Exam Scenario:** Need a side-by-side comparison of up to three Entra ID built-in roles with minimal administrative effort.
2. **Key Decision Point:** Which portal hosts a **native multi-role comparison tool**?
3. **Decision Path:**
   - IF requirement = compare ≤3 built-in roles in a tabular grid -> SELECT ✅ **Microsoft 365 admin center → Role assignments → Compare roles** (Reason: built-in interactive side-by-side comparison)
   - IF offered "Microsoft Entra admin center" as the comparator -> ❌ REJECT for *this specific task* (Reason: core directory portal, but no native multi-role side-by-side comparison feature)
   - IF offered Defender portal or Purview portal -> ❌ REJECT (Reason: security operations / compliance governance scope, not directory role permission comparison)
4. **SC-300 Golden Rule:** *Role *lookup* lives in Entra admin center; role *comparison* lives in the Microsoft 365 admin center.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/permissions-reference

---

## SECTION B — Authentication Registration, MFA Methods & Security Defaults

### D3. Combined Registration for New Users (MFA + SSPR) (Q218)
1. **Exam Scenario:** A brand-new user hits the combined registration portal in interrupt mode and must register methods for both MFA and SSPR.
2. **Key Decision Point:** Which method can be registered **first**, with no prior authentication method in place?
3. **Decision Path:**
   - IF user has **zero methods registered** + interrupt mode -> SELECT ✅ **Microsoft Authenticator app** first (Reason: satisfies MFA-compliant requirement; additional SSPR-only methods can follow in the same session)
   - IF SSPR-only method needed (e.g., email one-time passcode) -> ✅ ALLOW **after** the first MFA-compliant method is registered (Reason: email OTP is valid for SSPR but never for MFA)
   - IF offered FIDO2 key or hardware OATH token as the *first* registration -> ❌ REJECT (Reason: registering FIDO2/hardware tokens requires authenticating with an existing MFA method — impossible for a new user)
   - IF offered Windows Hello for Business via the web portal -> ❌ REJECT (Reason: WHfB enrollment happens during Windows sign-in on a device, not in the web-based security info portal)
4. **SC-300 Golden Rule:** *Bootstrap order = Authenticator first; FIDO2, hardware tokens, and WHfB always come after the first credential exists.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-registration-mfa-sspr-combined

### D4. Security Defaults — Enforced MFA Registration Method (Q221)
1. **Exam Scenario:** New tenant created after October 2019; Security Defaults enabled; which method is users required to register?
2. **Key Decision Point:** 🔑 **Tenant age + Security Defaults posture** determine the enforced registration method.
3. **Decision Path:**
   - IF tenant created after Oct 2019 + Security Defaults ON -> SELECT ✅ **Microsoft Authenticator app (notifications)** (Reason: the only enforced registration method under Security Defaults; users may also use OATH TOTP apps for codes)
   - IF offered SMS or voice call as the *default* requirement -> ❌ REJECT (Reason: configurable later via Conditional Access after disabling Security Defaults, but not the out-of-box mandated method)
   - IF offered email OTP as an MFA method -> ❌ REJECT (Reason: email OTP is for B2B/SSPR scenarios, never a sign-in MFA factor)
   - IF sync account in Directory Synchronization Accounts role -> note: **excluded** from Security Defaults MFA prompts
4. **SC-300 Golden Rule:** *Security Defaults = Authenticator (notification) registration or nothing. To use SMS/voice/FIDO2, graduate to Conditional Access policies.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/security-defaults

---

## SECTION C — Conditional Access: Templates, Device Filters, Risk Logic

### D5. Conditional Access Template Auto-Exclusion (Q219)
1. **Exam Scenario:** An admin deploys a template such as *"Require password change for high-risk users."* Who ends up excluded from the policy assignment?
2. **Key Decision Point:** **Who is deploying the template** (anti-lockout safety mechanism).
3. **Decision Path:**
   - IF template targets user identities -> SELECT ✅ **"All users" included + the creating admin (Admin1) auto-excluded** (Reason: built-in guard against tenant lockout during deployment)
   - IF break-glass/emergency accounts also need exclusion -> ✅ ADD them **manually after** initial deployment (Reason: only the creator is auto-excluded; nothing else is)
   - IF an answer claims other admins/groups are auto-excluded -> ❌ REJECT (Reason: no such default behavior exists)
4. **SC-300 Golden Rule:** *Templates include everyone but their creator. Break-glass exclusions are always a manual, post-deployment step.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-policy-templates

### D6. Filter for Devices — Null Property Logic (Q223)
1. **Exam Scenario:** CA policies use device filters with `device.displayName -startsWith "Device"` in **exclude** mode; devices range from unregistered to Entra-registered. Determine who is blocked.
2. **Key Decision Point:** 🔑 **Device registration state** × **operator polarity (positive vs negative)**.
3. **Decision Path:**
   - IF device is **unregistered** (join type = None) -> ALL device properties are treated as **null** (Reason: device object does not exist in the directory)
     - IF operator is **positive** (`-startsWith`, `-eq`, `-contains`) -> filter expression = **False → filter NOT applied** -> exclusion ignored -> **policy (Block) applies** → user blocked ✅
     - IF operator is **negative** (`-notStartsWith`, `-ne`) -> filter **DOES apply** to unregistered devices
   - IF device is **registered** + positive operator matches (e.g., displayName starts with "Device") -> filter = True → device **excluded** from the block policy → access decided by other policies (e.g., CAPolicy2 requiring MFA, which users pass → access granted)
   - IF user is only in the second policy's group (no filter logic involved) -> ✅ access granted when MFA succeeds
4. **SC-300 Golden Rule:** *Unregistered device = all-null attributes. Positive operators can't match null → the filter silently fails and the policy hits the user. Negative operators are the only safe way to target unregistered devices.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-condition-filters-for-devices

### D7. Risk Policies — Exclusion Beats Inclusion (Q246)
1. **Exam Scenario:** A user risk policy (e.g., require password change) has Group1 included, Group2 excluded, condition = "Medium and above." Users overlap groups with varying risk levels.
2. **Key Decision Point:** **Group membership inclusion/exclusion** is evaluated **before** risk-level conditions.
3. **Decision Path:**
   - IF user included + risk level ≥ threshold -> ✅ policy applies (password change enforced)
   - IF user **excluded**, regardless of risk level ("Medium" or even "High") -> ❌ policy bypassed entirely
   - IF user in BOTH included and excluded groups -> ❌ **Exclusion always overrides inclusion** — policy never applies
4. **SC-300 Golden Rule:** *Exclusions are absolute in Conditional Access / risk policies. One excluded group membership defeats any inclusion — even High risk.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-users-exclude

---

## SECTION D — Defender for Cloud Apps: Policy Types & App Connectors

### D8. Blocking Initial Access for High-Risk Users (Q220)
1. **Exam Scenario:** Block a user's *initial access* to a cloud app based on their high-risk state at sign-in time.
2. **Key Decision Point:** **Timing of enforcement** — sign-in/entry vs. in-session activity vs. detection only.
3. **Decision Path:**
   - IF control must fire **at sign-in / before entry** -> SELECT ✅ **Access policy** (evaluated in real time; a CA policy routes high-risk sign-ins to Defender for Cloud Apps where the access policy blocks entry)
   - IF offered **Activity policy** -> ❌ REJECT (Reason: acts on actions *after* access is granted, e.g., mass download)
   - IF offered **Anomaly detection policy** -> ❌ REJECT (Reason: detection/alerting engine only — no pre-defined access enforcement)
   - IF offered **OAuth app policy** -> ❌ REJECT (Reason: governs third-party apps consented via OAuth, not user sign-in access)
   - IF control must fire **mid-session** -> SELECT ✅ **Session policy** instead (not the distractors)
4. **SC-300 Golden Rule:** *Access policy = the gate (block entry); Session policy = the chaperone (in-session); Activity policy = the recorder of deeds; Anomaly detection = the alarm bell.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-cloud-apps/access-policy-aad

### D9. Monitoring OAuth Apps via App Connectors (Q224, Q225, Q226, Q231)
1. **Exam Scenario:** Monitor OAuth app authentication requests/permissions across Google Workspace, AWS, and GitHub. Solutions propose adding connectors — one per platform, or only the Azure connector.
2. **Key Decision Point:** 🔑 **Connector-to-platform mapping** — visibility is strictly per connected app.
3. **Decision Path:**
   - IF target platform = Google Workspace / AWS / GitHub -> SELECT ✅ **That platform's own App Connector** (Reason: connectors pull API activity logs — e.g., AWS CloudTrail — enabling OAuth app discovery and permission monitoring)
   - IF the "solution" adds only the **Azure** connector and claims multicloud coverage (AWS + Google Workspace + GitHub) -> ❌ REJECT / Answer = **No** (Reason: Azure connector sees Azure only; zero visibility into other clouds)
   - IF answer asks "Does connecting [Google Workspace | AWS | GitHub] enable OAuth monitoring?" -> ✅ **Yes** (Reason: app connectors are the standard, required mechanism for this visibility)
4. **SC-300 Golden Rule:** *One cloud, one connector: OAuth visibility is exactly as wide as the connectors you deploy. Azure's connector never sees AWS, GCP, or GitHub.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-cloud-apps/connect-apps

---

## SECTION E — Consent & Application Governance

### D10. Enabling Admin Consent Request Workflow (Q227)
1. **Exam Scenario:** Users cannot consent to an app needing high-privilege permissions; you must let them *request* admin consent (least privilege).
2. **Key Decision Point:** **Sequencing** — the workflow must be switched on before any reviewer can be designated.
3. **Decision Path:**
   - IF step 1 required -> SELECT ✅ **Enable the admin consent request feature at tenant level** (Reason: no requests can be submitted or reviewed until the feature is ON)
   - THEN (step 2) -> designate reviewers for the tenant
   - IF offered "grant tenant-wide admin consent for App1 directly" -> ❌ REJECT (Reason: bypasses the request workflow; violates least privilege and the requirement that *users* initiate)
   - IF offered "create a Conditional Access policy for App1" -> ❌ REJECT (Reason: CA governs user sign-in access to apps — unrelated to app permission consent)
4. **SC-300 Golden Rule:** *Consent workflow order: (1) Enable feature → (2) Assign reviewers → (3) Users request → (4) Admins approve. Never shortcut with blanket tenant-wide consent.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/configure-admin-consent-workflow

### D11. Soft-Deleted App Registrations — 30-Day Window (Q229)
1. **Exam Scenario:** Four app registrations (with service principals, secrets, app roles, user/group assignments, self-service settings) deleted April 5; restore attempted April 16.
2. **Key Decision Point:** 🔑 **Days since deletion** vs. the **30-day retention window** (restoration dates for objects are irrelevant).
3. **Decision Path:**
   - IF deletion < 30 days ago (11 days here) -> ✅ **ALL four apps restorable** — including app object properties (app roles, client secrets) and service-principal/Enterprise App tenant properties (users & groups assignments, self-service settings)
   - IF deletion > 30 days ago -> ❌ permanent deletion; only restoration from backups is possible
   - IF an answer hinges on when objects were last *updated* in March -> ❌ REJECT (Reason: the clock starts at deletion, not last modification)
4. **SC-300 Golden Rule:** *Deleted app = 30-day soft-delete countdown from deletion date. Restoration brings back the app object AND its service principal's tenant-specific settings.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/restore-deleted-app-portal

### D12. Application Permissions vs Delegated Permissions (Q237)
1. **Exam Scenario:** An app runs as a **daemon/service with no user interaction** and must call Microsoft Graph.
2. **Key Decision Point:** **Is a signed-in user present?**
3. **Decision Path:**
   - IF **no user context** (background/daemon) -> SELECT ✅ **Application permissions** (admin-granted to the service principal; app authenticates with its own credential — client secret/certificate — receiving a user-less token)
   - IF a **signed-in user** is present -> ✅ **Delegated permissions** (app acts *on behalf of* the user)
   - IF offered custom/built-in **RBAC roles** as the Graph authorization mechanism -> ❌ REJECT (Reason: RBAC roles govern Entra/Azure resource management; API call authorization is governed by granted API permissions)
4. **SC-300 Golden Rule:** *No user, no delegated: daemon = application permissions + own credentials = user-less token.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity-platform/scenario-daemon-overview

---

## SECTION F — Privileged Identity Management (PIM): Scopes & Coverage

### D13. Which Roles Can PIM Manage? (Q228)
1. **Exam Scenario:** Admin1 = Account Administrator, Admin2 = Service Administrator, Admin3 = SharePoint Administrator. Who can activate roles via PIM?
2. **Key Decision Point:** 🔑 **Role family** — Entra/M365 roles vs. **classic subscription administrator roles**.
3. **Decision Path:**
   - IF role is an **Entra ID directory role or Microsoft 365 admin role** (e.g., SharePoint Administrator) -> ✅ **PIM-supported** (just-in-time activation available)
   - IF role = **Account Administrator / Service Administrator / Co-Administrator** -> ❌ **NOT PIM-supported** (classic subscription administrator roles are explicitly excluded from PIM management)
   - Therefore: Admin3 only ✅; every option containing Admin1 and/or Admin2 -> ❌ REJECT
4. **SC-300 Golden Rule:** *PIM governs Entra roles, Azure RBAC roles, and PIM-for-Groups — but NEVER the three classic subscription admins (Account / Service / Co-Administrator).*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-roles

### D14. Selecting PIM as the Governance Solution (Q242)
1. **Exam Scenario:** Evaluate and remediate risk for privileged accounts: JIT access, time-bound activation, approval on activation, MFA on activation, automated reviews — with minimal admin effort.
2. **Key Decision Point:** Match requirements to the **identity governance service**, not security/network/CIEM services.
3. **Decision Path:**
   - IF requirements = JIT + time-bound + approval + MFA at activation + recurring access reviews -> SELECT ✅ **Microsoft Entra PIM** (policy-based automation covers all criteria)
   - IF offered **Microsoft Entra Permissions Management** -> ❌ REJECT (Reason: CIEM for multicloud permission *rightsizing* — Azure/AWS/GCP — not Entra role lifecycle governance)
   - IF offered **Verified ID** -> ❌ REJECT (Reason: verifiable credentials, not privileged role governance)
   - IF offered **Global Secure Access** -> ❌ REJECT (Reason: SSE/network security, unrelated to role elevation)
4. **SC-300 Golden Rule:** *JIT + time-bound + approval + MFA + reviews = PIM. CIEM rightsizes; PIM activates.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-configure

### D15. PIM for Groups — Membership Type Threshold (Q247)
1. **Exam Scenario:** Which groups can be brought under PIM for Groups for JIT membership/ownership?
2. **Key Decision Point:** 🔑 **Membership type = Assigned** (and group must be cloud-managed).
3. **Decision Path:**
   - IF group = **Security group** + Assigned membership -> ✅ supported
   - IF group = **Microsoft 365 group** + Assigned membership -> ✅ supported
   - IF group = **Dynamic membership** -> ❌ NOT supported (cannot be governed by PIM for Groups)
   - IF group is **synced from on-premises AD** -> ❌ NOT supported
4. **SC-300 Golden Rule:** *PIM for Groups = Assigned only. Dynamic or synced groups are ineligible; both Security and M365 group types qualify when membership is Assigned.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/concept-pim-for-groups

---

## SECTION G — Entitlement Management & Access Reviews

### D16. External Guest Lifecycle After Access Package Expiry (Q230)
1. **Exam Scenario:** Three guests: one with an **active directory role**, one with only an expiring access package, one added **manually** (no access package). Will each still exist after expiry + 30 days?
2. **Key Decision Point:** 🔑 **Was the guest governed by entitlement management**, and does any **directory role** anchor them?
3. **Decision Path:**
   - IF guest was **invited via an access package** AND the package expires AND guest holds **no directory role** -> ✅ lifecycle kicks in: **blocked from sign-in, then removed after 30 days** → gone by day 34+ → **NOT in directory**
   - IF guest holds an **active directory role** when the last access package expires -> ✅ **stays in directory** (lifecycle removal does not strip role-holding guests)
   - IF guest was invited **manually / added to groups directly** (never governed by an access package, never "converted to governed") -> ✅ **stays in directory** (30-day policy applies only to entitlement-managed guests)
   - IF a guest added via access package *also* received non-package resources (OneDrive/SharePoint site) -> still removed — extra resource access does not prevent deletion
4. **SC-300 Golden Rule:** *Entitlement Management only reaps what it sowed: package-invited guests lose their last package → block → 30 days → delete. A directory role anchors them; manual invites are invisible to the reaper.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/entitlement-management-external-users

### D17. Time-Bound Automatic Access at Scale (Q232)
1. **Exam Scenario:** Automatically grant Group1 members access to a SharePoint site for exactly 90 days, then expire — with minimal ongoing administration.
2. **Key Decision Point:** **Grant + expire automatically in one artifact** vs. periodic recertification.
3. **Decision Path:**
   - IF need **automatic assignment + automatic expiry** -> SELECT ✅ **Access package** (policy auto-assigns the group; duration of 90 days expires access automatically)
   - IF offered **Access review** -> ❌ REJECT (Reason: reviews *recertify existing* access periodically; they don't grant or time-box initial access)
   - IF offered **Lifecycle workflow** -> ❌ REJECT (Reason: driven by joiner/mover/leaver lifecycle events, not resource-specific time-limited access)
   - IF offered **Conditional Access policy** -> ❌ REJECT (Reason: CA enforces sign-in conditions; it grants nothing and manages no assignment duration)
4. **SC-300 Golden Rule:** *Grant-and-expire in one shot = Access Package. Review-what-exists = Access Review. React-to-lifecycle-events = Lifecycle Workflow. Enforce-at-sign-in = CA.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/entitlement-management-overview

---

## SECTION H — Managed Identities & Least-Privilege Azure RBAC

### D18. Choosing Managed Identity Type (Q233, Q236, Q245, Q239)
1. **Exam Scenario:** Workloads (App Service apps, VMs, Automation) need access to Azure resources (storage) without credential management.
2. **Key Decision Point:** **Identity sharing** — one resource vs. many resources vs. lifecycle independence.
3. **Decision Path:**
   - IF **multiple resources/apps** must share ONE identity (e.g., App2 on VM1+VM2, or several App Service apps) -> SELECT ✅ **User-assigned managed identity** (Reason: standalone resource; permissions configured once; independent lifecycle)
   - IF **a single resource** needs its own identity (e.g., VM1) -> SELECT ✅ **System-assigned managed identity** (Reason: tied 1:1 to the resource lifecycle — simplest for one VM; deleted with the resource)
   - IF offered an **Entra user account** for the VM -> ❌ REJECT (Reason: that's interactive sign-in / Entra login for VMs, not the VM's workload identity)
   - IF offered **Azure Automation account** or **Run As account** -> ❌ REJECT (Reason: Run As = legacy credential method; Automation account is not an attachable resource identity)
   - **Enablement pattern (Q239):** `Get-AzVM` → enable system-assigned → `Update-AzVM`; the auto-created **service principal shares the VM's display name** → retrieve with `Get-AzADServicePrincipal -DisplayName` → add to group via `Add-AzureADGroupMember`
4. **SC-300 Golden Rule:** *One-to-one = system-assigned; one-to-many or independent lifecycle = user-assigned. Never credentials, never SAS, never Run As.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview

### D19. RBAC Scope & Data-Plane Role Precision (Q234, Q248, Q252)
1. **Exam Scenario:** Least-privilege access to a *specific* Key Vault secret, a *specific* Cosmos DB account, or secrets in a specific resource group.
2. **Key Decision Point:** 🔑 **Scope granularity** (individual secret < vault < RG < subscription) × **management plane vs. data plane** role.
3. **Decision Path:**
   - IF access to **one secret only** (Secret1 in Vault1) -> SELECT ✅ **Access control (IAM) on the secret object itself** + role **Key Vault Secrets User** (Reason: vault-level IAM would leak access to all secrets — violates least privilege)
   - IF AKS needs to read **one Cosmos DB account's** keys/metadata -> SELECT ✅ **Cosmos DB Account Reader Role scoped to DB1** (Reason: includes `listKeys` to reach the data plane; RG-level = too broad; subscription Owner = wildly excessive; generic Reader lacks `listKeys`)
   - IF user in group with **Reader** on Sub1 only -> ❌ cannot read secret *contents* (Reader = management plane, no data-plane access)
   - IF group with **Key Vault Secrets User** scoped to RG2 + vault in RG2 -> ✅ can read secrets; same role scoped to RG1 with vault in RG1 -> ❌ scope mismatch = denied
   - IF group with **Owner** on RG1 + VM1 in RG1 -> ✅ full management-plane control of VM1
   - IF offered configuring permissions from the resource's own **Identity blade** -> ❌ REJECT (Reason: that blade toggles the identity itself; permissions are assigned on the *target* resource's IAM)
4. **SC-300 Golden Rule:** *Scope tight, role right: assign the data-plane role (Key Vault Secrets User / Cosmos DB Account Reader) at the narrowest possible scope — the secret or account itself — never at vault/RG/subscription "just in case."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/azure/role-based-access-control/conditions-overview

### D20. Azure ABAC Conditions — Role Eligibility (Q235)
1. **Exam Scenario:** Which role assignments can have ABAC attribute conditions attached (e.g., on blob storage)?
2. **Key Decision Point:** 🔑 Does the role contain **storage data actions** (Blob/Queue data plane)?
3. **Decision Path:**
   - IF role = **Storage Blob Data Reader** (data actions on Blob/Queue) -> ✅ ABAC conditions supported
   - IF roles = Reader / Contributor / Virtual Machine Contributor -> ❌ REJECT (Reason: ARM **management-plane** roles — no storage data actions, conditions not supported)
4. **SC-300 Golden Rule:** *ABAC conditions ride on data actions: Blob and Queue data-plane roles only. Management-plane roles can't take conditions.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/azure/role-based-access-control/conditions-overview

---

## SECTION I — Administrative Units & Naming Policies

### D21. Administrative Unit Role Scoping (Q240)
1. **Exam Scenario:** Admin1 = User Administrator scoped to AU *Department1*; Admin3 = User Administrator with directory (tenant) scope. Reset passwords / edit groups?
2. **Key Decision Point:** 🔑 **AU scope grants authority only over objects directly IN the AU** — adding a group to an AU does **not** pull its members in.
3. **Decision Path:**
   - IF target user is NOT directly in the scoped AU (only a member of a group placed in the AU) -> ❌ scoped admin **cannot** manage/reset that user (Reason: group membership in an AU-assigned group ≠ AU membership)
   - IF target group is NOT in the scoped AU -> ❌ scoped admin **cannot** modify the group (can't add User1 to Group3)
   - IF admin holds role with **directory (tenant-wide) scope** -> ✅ can manage/reset passwords for users in ANY AU (AU membership is irrelevant)
4. **SC-300 Golden Rule:** *AU role scope = direct objects only. Groups don't drag their members into the AU; directory scope outranks every AU.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/administrative-units

### D22. Group Naming Policy Exemptions (Q238)
1. **Exam Scenario:** Naming policy (prefix/suffix + blocked words) enforced for Microsoft 365 groups across Teams/Outlook/SharePoint. Which users are affected?
2. **Key Decision Point:** 🔑 **Exempted directory roles**: only **Global Administrator** and **User Administrator** bypass the policy on all group endpoints.
3. **Decision Path:**
   - IF user = Global Administrator or User Administrator -> ✅ **EXEMPT** (can bypass prefix/suffix and blocked words)
   - IF user = Groups Administrator or a standard user -> ❌ **fully subject to** the naming policy
4. **SC-300 Golden Rule:** *Only two exemptions exist — Global Admin and User Admin. Groups Administrator is NOT exempt, despite the misleading name.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/groups-naming-policy

---

## SECTION J — Microsoft Entra Permissions Management (CIEM)

### D23. Permissions Management: Purchase, Onboard, Onboard-Clouds, Operate (Q241, Q243, Q250, Q251)
1. **Exam Scenario:** Four sub-decisions: who can *buy* the license; which clouds can be *onboarded*; how to grant a user *initial access*; how to *remediate* a user to read-only.
2. **Key Decision Point:** Distinguish **tenant purchase rights** vs. **service coverage** vs. **app-role assignment** vs. **remediation quick actions**.
3. **Decision Path:**
   - IF task = purchase the license with least privilege -> SELECT ✅ **Billing Administrator** (Reason: purchases/subscriptions only; ❌ Global Admin violates least privilege; ❌ User Access Administrator = Azure resource-access role; ❌ Permissions Management Administrator = in-service role, post-onboarding)
   - IF task = additional onboarding clouds (Azure already connected) -> SELECT ✅ **AWS and GCP** (Reason: CIEM supports exactly Azure + AWS + GCP; ❌ Alibaba Cloud unsupported)
   - IF task = give User1 initial access -> SELECT ✅ **Assign a Permissions Management app role** (Administrator / User / Reader) to the user in the Entra admin center (Reason: access is app-role based; ❌ creating a group alone grants nothing; ❌ Role/Policy templates target *resources*, not the service; ❌ My Requests requires existing access)
   - IF task = replace User1's permissions with read-only -> SELECT ✅ **Remediation tab → Permissions subtab → "Replace with Read-Only" quick action** (Reason: one action revokes existing grants and assigns a read-only policy; ❌ creating a role/template doesn't apply or revoke anything; ❌ My Requests is self-service for identities *with* access)
4. **SC-300 Golden Rule:** *Buy with Billing, onboard AWS+GCP only, grant access via app roles, remediate via Replace-with-Read-Only quick action. Permissions Management = CIEM for Azure/AWS/GCP — nothing else.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/permissions-management/understand-assignments

---

## SECTION K — Logging, Reporting & Monitoring

### D24. Entra ID Log Retention by License Tier (Q244)
1. **Exam Scenario:** How long are sign-in and audit logs available natively in the Entra admin center?
2. **Key Decision Point:** 🔑 **License tier** drives native retention; anything longer requires export.
3. **Decision Path:**
   - IF tenant = **Microsoft Entra ID Free** -> **7 days** (audit + sign-ins)
   - IF tenant = **Entra ID P1 or P2** -> ✅ **30 days** (audit + sign-ins) — the exam answer
   - IF asked for 90/365-day retention -> ❌ REJECT as "default" (Reason: requires routing to Azure Storage / Log Analytics via Azure Monitor)
   - **Bonus signal table (remember!):** Risky sign-ins = 7d (Free) / 30d (P1) / **90d (P2)**; Risky users = no limit
   - IF offered **Usage & insights report** for per-app sign-in success/failure rates (Q249) -> ✅ SELECT it (Reason: pre-calculated success/failure/success-rate per app; ❌ Sign-in logs = raw data needing manual aggregation; ❌ Audit logs = admin/config changes; ❌ Access reviews = recertification, not analytics)
4. **SC-300 Golden Rule:** *Free = 7, P1/P2 = 30 for audit+sign-ins; risky sign-ins stretch to 90 only on P2. Beyond that — export to Azure Monitor. Fastest per-app sign-in math = Usage & insights report.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/monitoring-health/reference-reports-data-retention

---

## Master Exam Cheat-Sheet (Part 7)

| Trigger in question | Instant answer |
|---|---|
| Passwords + licenses, one role | **User Administrator** |
| New user, first security info registration | **Authenticator app** (FIDO2/token/WHfB impossible first) |
| Tenant created after Oct 2019, Security Defaults | **Authenticator (notifications)** |
| CA template deployment | All users **except the deploying admin** |
| Unregistered device + positive filter operator | Filter **fails (null)** → policy applies |
| Included vs excluded group overlap | **Exclusion wins — always** |
| Block sign-in entry by risk | Defender for Cloud Apps **Access policy** |
| OAuth app monitoring on SaaS | **That platform's App Connector** (Azure connector ≠ multicloud) |
| Admin consent requests | **Enable feature first**, then reviewers |
| Deleted app registration | **30 days** soft-delete from deletion date |
| Daemon app → Graph | **Application permissions** |
| Account/Service/Co-Administrator | ❌ **PIM cannot manage these** |
| PIM for Groups | **Assigned membership only** (Security + M365 OK) |
| Guest lost last package, no role | Block → **30 days** → removed |
| Time-boxed auto access | **Access package** (not review/lifecycle/CA) |
| Multiple apps, one identity | **User-assigned** managed identity |
| One VM, one identity | **System-assigned** managed identity |
| One secret / one Cosmos account | IAM at **that object's scope** + Secrets User / Account Reader |
| ABAC conditions | Only roles with **storage data actions** |
| Naming policy exemptions | **Global Admin + User Admin only** |
| AU-scoped admin | Manages **direct AU objects only** |
| Permissions Management clouds | **Azure + AWS + GCP** (never Alibaba) |
| Log retention | **7 (Free) / 30 (P1/P2)**; risky sign-ins 90 on P2 |# SC-300 Decision Trees - Part 8

# SC-300 Decision Trees & Logic Rules — Part 8 (Questions 253–288)

---

### 1. Managed Identity Assignment & Regional Scope (Q253, Q271, Q278)
1. **Exam Scenario:** Determine which identities can receive Azure RBAC roles, and which resources (VMs, storage accounts, App Service apps) across different regions can use a given user-assigned managed identity.
2. **Key Decision Point:** Identity *type* (system-assigned vs. user-assigned vs. user account) and whether the underlying **service principal** exists in Microsoft Entra ID.
3. **Decision Path:**
   - IF the object has its own service principal (user-assigned MI, or VM with system-assigned MI enabled) -> **CAN** be assigned Azure RBAC roles (e.g., Owner).
   - IF a VM only *uses* a user-assigned identity (no system-assigned identity enabled) -> **REJECT** assigning a role to the VM itself (Reason: no service principal of its own exists).
   - IF a standard Entra user account (User1) appears as an option to attach to a VM/App -> **REJECT** (Reason: user accounts cannot be attached as resource identities — only managed identities can).
   - IF the user-assigned MI was created in Region A but the target resource is in Region B -> **ALLOW** (Reason: the MI is a regional resource, but its service principal is **global** in Microsoft Entra ID — validated in [Managed identities FAQ](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/managed-identities-faq#limitations)).
   - IF the question asks whether multiple user-assigned MIs can coexist on one VM -> **YES** (VMs support multiple user-assigned MIs; App Service supports one system-assigned MI *plus* one or more user-assigned MIs).
4. **SC-300 Golden Rule:** *"The service principal is global, the identity resource is regional. Only objects with a service principal can hold a role — and a user account is never a resource identity."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/managed-identities-faq#limitations

---

### 2. Granting Managed Identity Access to Azure Resources (Q259, Q264, Q273)
1. **Exam Scenario:** WebApp1 must access storage1; VM1 must read secrets from Vault1; Permissions Management must monitor permissions and implement right-sized roles — all with least privilege.
2. **Key Decision Point:** Authorization mechanism (Azure RBAC vs. keys/SAS/access policies) and the *minimum* built-in role that satisfies the action.
3. **Decision Path:**
   - IF a managed identity needs access to a storage account -> assign an RBAC role (e.g., **Storage Blob Data Contributor**) via **Access control (IAM)** on the storage account.
   - IF a VM must *read* secrets from Key Vault -> assign **Key Vault Secrets User** (least privilege; read-only on secret contents).
   - IF a user must *read AND update* secrets -> assign **Key Vault Secrets Officer** (full secret management).
   - IF a tool must *assign roles / implement right-sized roles* -> assign **User Access Administrator** (Reason: only this role has `Microsoft.Authorization/*/Write`; Reader can't write, Contributor can't assign roles, Owner violates least privilege).
   - IF options include **access keys** or **SAS tokens** -> **REJECT** (Reason: the entire purpose of managed identities is eliminating credential management).
   - IF options include adding a user-assigned MI when a system-assigned MI already exists -> **REJECT** (Reason: redundant, increases administrative effort).
4. **SC-300 Golden Rule:** *"Secrets User reads, Secrets Officer writes, User Access Administrator assigns — and never touch keys or SAS when a managed identity exists."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/how-managed-identities-work-vm#user-assigned-managed-identity

---

### 3. Entra ID Authentication for Windows VMs — Troubleshooting (Q254)
1. **Exam Scenario:** Users cannot sign in to Windows Server VMs with Microsoft Entra credentials; the first troubleshooting step must be identified.
2. **Key Decision Point:** Whether the VM can complete **device registration** (outbound TCP 443 to `https://enterpriseregistration.windows.net`).
3. **Decision Path:**
   - IF sign-in fails across many VMs and registration was never possible -> **VERIFY network connectivity** to `enterpriseregistration.windows.net` over port 443 (NSG/Firewall blocking = root cause).
   - IF the proposed fix is "revoke the PRT" -> **REJECT** (Reason: PRT revocation is user-specific SSO remediation, not a systemic registration fix).
   - IF the proposed fix is "delete the device registrations" -> **REJECT as first step** (Reason: re-registration will fail again if the network block persists).
   - IF the proposed fix is "enable SSH/OpenSSH" -> **REJECT** (Reason: SSH is for Linux VMs; Windows Server uses RDP).
4. **SC-300 Golden Rule:** *"No port 443 to enterpriseregistration.windows.net = no device registration = no Entra sign-in. Check the network first."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/devices/howto-vm-sign-in-azure-ad-windows

---

### 4. proxyAddresses Uniqueness & Sync Errors (Q255)
1. **Exam Scenario:** Multiple on-premises users are updated/provisioned with the same `sales@contoso.com` proxy address; predict which sync successfully.
2. **Key Decision Point:** Whether `proxyAddresses` values are **unique across all objects** in the tenant.
3. **Decision Path:**
   - IF the proxy address is unique in the tenant (e.g., changed to `admin@contoso.com`) -> sync **succeeds**.
   - IF two or more objects claim the *same* proxy address simultaneously -> Entra Connect flags **AttributeValueMustBeUnique** errors for *all* conflicting objects.
   - IF an option suggests Entra Connect "resolves" duplicates automatically -> **REJECT** (Reason: uniqueness is strictly enforced; conflicts block sync).
4. **SC-300 Golden Rule:** *"One proxy address, one object. Duplicate proxyAddresses = AttributeValueMustBeUnique error for every conflicting object."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/troubleshoot-object-does-not-sync

---

### 5. Administrative Units — Role Scope & Membership Boundaries (Q256, Q280, Q282)
1. **Exam Scenario:** Delegate administration (e.g., password resets) over a department's users only; determine which roles can be AU-scoped and who falls inside an admin's scope.
2. **Key Decision Point:** (a) Is the role in the **supported subset** for AU scope? (b) Is the target object a **direct member** of the AU?
3. **Decision Path:**
   - IF the role is **User Administrator** (or Helpdesk Administrator, Password Administrator, etc.) -> **CAN** be assigned at tenant scope AND AU scope.
   - IF the role is **Security Administrator** (or Global Administrator, Exchange Administrator, etc.) -> **CANNOT** be AU-scoped; tenant scope only.
   - IF a user is a **direct member** of the AU -> the AU-scoped admin can manage them (reset passwords, etc.).
   - IF a user is only a member of a *group* that is in the AU -> **REJECT** management claims (Reason: adding a group brings the **group object** into scope, **not its members** — confirmed in [Administrative units docs](https://learn.microsoft.com/entra/identity/role-based-access-control/administrative-units#groups)).
   - IF the requirement is "delegate admin over a department's users" -> **CREATE an administrative unit** and scope the role to it.
   - IF options offer Microsoft 365 group / management group / resource group -> **REJECT** (Reason: none of these scope *directory user administration*).
4. **SC-300 Golden Rule:** *"AU scope = direct members only. Group in AU ≠ members in AU. And only a subset of roles (like User Administrator) can be AU-scoped."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/administrative-units#groups

---

### 6. Custom Security Attributes — Who Can Create & What Can Hold Them (Q287)
1. **Exam Scenario:** Determine which admin can define custom security attributes and which directory objects can receive them.
2. **Key Decision Point:** Role assignment (**Attribute Definition Administrator**) and supported object types.
3. **Decision Path:**
   - IF the user lacks the **Attribute Definition Administrator** role -> **CANNOT** create/define custom security attributes (even Global Admins don't get this by default).
   - IF the target object is a **user, service principal/enterprise app, or managed identity** -> attributes **CAN** be assigned.
   - IF the target object is a **security group or Microsoft 365 group** -> **REJECT** (Reason: groups are not currently supported for custom security attributes).
4. **SC-300 Golden Rule:** *"Attribute Definition Administrator defines; users, service principals, and managed identities hold — groups never."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/custom-security-attributes-overview

---

### 7. Microsoft Entra Permissions Management (CIEM) (Q257, Q273, Q285)
1. **Exam Scenario:** Assess privilege assignment risk across **Azure, AWS, and GCP** with a single tool; onboard an Azure subscription.
2. **Key Decision Point:** Tool purpose (CIEM vs. SIEM vs. CASB vs. ID Protection) and onboarding steps.
3. **Decision Path:**
   - IF the requirement is multi-cloud (Azure + AWS + GCP) **permission/entitlement** visibility -> SELECT **Microsoft Entra Permissions Management** (CIEM).
   - IF the requirement is threat detection/log analysis -> **REJECT** Microsoft Sentinel (SIEM/SOAR).
   - IF the requirement is SaaS app security/data flow control -> **REJECT** Defender for Cloud Apps (CASB).
   - IF the requirement is risky sign-ins/leaked credentials *within Entra ID only* -> **REJECT** Entra ID Protection (no AWS/GCP coverage).
   - IF onboarding an Azure subscription -> (1) assign **Reader** role to the Permissions Management service principal at the subscription scope, (2) configure a **data collector** for Azure in the Permissions Management portal.
   - IF options include App Proxy, diagnostic settings, or manual app registration -> **REJECT** (Reason: unrelated to onboarding; app registration is automated).
4. **SC-300 Golden Rule:** *"Permissions Management = CIEM for Azure + AWS + GCP. Onboard = Reader role assignment + data collector."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/permissions-management/onboard-azure

---

### 8. SCIM 2.0 Provisioning (Q258)
1. **Exam Scenario:** Automate user account lifecycle (create/update/delete) in a SaaS application.
2. **Key Decision Point:** Protocol purpose — **provisioning** vs. **authentication/authorization**.
3. **Decision Path:**
   - IF the requirement is automated user provisioning/de-provisioning -> SELECT **SCIM 2.0** (Entra ID = SCIM client; SaaS app = SCIM server).
   - IF options offer WS-Federation -> **REJECT** (Reason: SSO protocol, not provisioning).
   - IF options offer LDAP -> **REJECT** (Reason: on-prem directory protocol, not SaaS provisioning).
   - IF options offer OAuth 2.0 -> **REJECT** (Reason: delegated authorization framework, not identity lifecycle).
4. **SC-300 Golden Rule:** *"SCIM provisions, SSO authenticates, OAuth authorizes. Lifecycle = SCIM 2.0."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/app-provisioning/use-scim-to-provision-users-and-groups

---

### 9. External Collaboration Settings & B2B Domain Restrictions (Q260, Q284)
1. **Exam Scenario:** Restrict guest invitations to specific external domains; link billing for External Identities MAU pricing.
2. **Key Decision Point:** Where the control lives — **External collaboration settings → Collaboration restrictions**.
3. **Decision Path:**
   - IF the requirement is an **allow-list of any external domains** (Entra or non-Entra) -> SELECT **"Allow invitations only to the specified domains"** under Collaboration restrictions.
   - IF the requirement is per-tenant B2B policy with another Entra tenant -> use **Cross-tenant access settings** (not a general domain allow-list).
   - IF the requirement is federating external IdPs (Google, Facebook, SAML/WS-Fed) -> use **All identity providers** (controls *authentication*, not *invitation domains*).
   - IF the requirement is MAU-based External Identities billing -> use **Linked subscriptions** (link tenant to an Azure subscription).
4. **SC-300 Golden Rule:** *"Domain allow/deny lists live in External collaboration settings; billing linkage lives in Linked subscriptions."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/allow-deny-list

---

### 10. Azure RBAC Least Privilege — Counting Role Assignments (Q261)
1. **Exam Scenario:** User needs read access to an entire subscription but create/restart rights only for VMs and storage accounts in one resource group.
2. **Key Decision Point:** Match **permission level** to **scope** with the minimum number of assignments.
3. **Decision Path:**
   - IF read access is needed tenant/subscription-wide -> **Reader** at subscription scope (1 assignment).
   - IF VM create/restart is needed only in RG1 -> **Virtual Machine Contributor** at RG1 scope (1 assignment).
   - IF storage account creation is needed only in RG1 -> **Storage Account Contributor** at RG1 scope (1 assignment).
   - IF a proposed answer uses broad **Contributor** at RG1 -> **REJECT** (Reason: grants excessive permissions, violating least privilege).
   - IF a proposed answer uses fewer assignments than the distinct permission/scope combinations -> **REJECT** (Reason: one assignment cannot express different permissions at different scopes).
4. **SC-300 Golden Rule:** *"One assignment per (role, scope) pair. Reader wide, Contributor narrow — never broad Contributor to 'save' assignments."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/azure/role-based-access-control/overview

---

### 11. Conditional Access — Grant Controls & Terms of Use (Q262, Q270, Q274)
1. **Exam Scenario:** Force users to accept a Terms of Use (ToU) agreement before access; identify the required file format and configuration location.
2. **Key Decision Point:** CA policy section (**Grant controls**) and ToU document format (**PDF**).
3. **Decision Path:**
   - IF the requirement is "user must accept ToU" -> upload the ToU in **Identity Governance**, then in the CA policy select **Terms of Use** as a **Grant control**.
   - IF options point to **Conditions** -> **REJECT** (Reason: Conditions define *signals* — location, device, risk — not access requirements).
   - IF options point to **Session** -> **REJECT** (Reason: Session controls = sign-in frequency, App Control — not ToU).
   - IF options point to **Target resources / Cloud apps** -> **REJECT** (Reason: defines *what* is protected, not the *control*).
   - IF asked for the ToU file format -> **PDF only** (HTML, RTF, DOCX are all rejected by the upload interface).
   - IF options offer access packages or lifecycle workflows for ToU enforcement -> **REJECT** (Reason: access packages bundle requestable resources; lifecycle workflows automate joiner/mover/leaver tasks — neither presents agreements at sign-in).
4. **SC-300 Golden Rule:** *"ToU = PDF uploaded in Identity Governance + Grant control in Conditional Access. Conditions signal, Session tunes, Grant enforces."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/terms-of-use

---

### 12. Certificate-Based Authentication (CBA) — First Step (Q263)
1. **Exam Scenario:** Enable Microsoft Entra CBA for users with certificates issued by an on-premises CA.
2. **Key Decision Point:** Establishing **trust with the issuing CA** before anything else.
3. **Decision Path:**
   - IF enabling CBA -> **FIRST** upload the CA's **public certificate** to the Entra tenant (registers it as a trusted authority for validation/revocation checks).
   - IF the proposed step is "enable auto-enrollment" -> **REJECT as first step** (Reason: certificate distribution comes *after* trust is established).
   - IF the proposed step is "deploy Azure Key Vault" -> **REJECT** (Reason: not part of the CBA trust workflow).
   - IF the proposed step is "deploy Windows Hello for Business" -> **REJECT** (Reason: a distinct passwordless method, not a CBA prerequisite).
4. **SC-300 Golden Rule:** *"No trusted CA = no CBA. Upload the CA public certificate first; everything else follows."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/certificate-based-authentication-get-started

---

### 13. Authentication Strengths — Phishing-Resistant MFA (Q272)
1. **Exam Scenario:** Enforce phishing-resistant MFA for administrators via Conditional Access.
2. **Key Decision Point:** Which methods satisfy the built-in **Phishing-resistant MFA strength**.
3. **Decision Path:**
   - IF the requirement is phishing-resistant MFA -> use the **Require authentication strength** grant control with the built-in **Phishing-resistant MFA strength**, which allows: **Windows Hello for Business**, **FIDO2 security keys**, and **certificate-based authentication (multifactor)** — validated in [Authentication strengths docs](https://learn.microsoft.com/entra/identity/authentication/concept-authentication-strengths#built-in-and-custom-authentication-strengths).
   - IF options include **voice call, SMS, or email OTP** -> **REJECT** (Reason: vulnerable to SIM-swap, interception, and phishing — not phishing-resistant).
   - IF options include **single-factor certificate-based authentication** -> **REJECT** (Reason: fails the *multi-factor* requirement).
   - IF options include generic "Microsoft Authenticator" -> **REJECT** (Reason: the app also supports OTP passcodes; the term is less precise than WHfB/FIDO2/MFA-CBA).
4. **SC-300 Golden Rule:** *"Phishing-resistant = WHfB + FIDO2 + MFA-CBA. Public-key crypto where the private key never leaves the device."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-authentication-strengths#built-in-and-custom-authentication-strengths

---

### 14. Defender for Cloud Apps — App Catalog & Session Policies (Q265, Q275)
1. **Exam Scenario:** Research apps' authentication capabilities; enforce real-time session monitoring via session policies.
2. **Key Decision Point:** Discovery/research tool vs. enforcement trigger (Conditional Access App Control).
3. **Decision Path:**
   - IF the requirement is to research the *inherent security features* of apps (e.g., weak/no authentication) -> use the **Cloud App Catalog** (31,000+ apps scored on 90+ risk factors, filterable by security attributes).
   - IF the requirement is to find apps *in use* in your org -> discovered app queries / snapshot Cloud Discovery reports (usage analysis, not security research).
   - IF the requirement is to control OAuth-granted permissions of connected apps -> OAuth policies (not catalog research).
   - IF the requirement is to enforce a **session policy** -> **FIRST** create a Conditional Access policy with the **Session** control set to **"Use Conditional Access App Control"** (this proxies the session to Defender for Cloud Apps).
   - IF options offer User monitoring, App onboarding, or continuous reports as the first step -> **REJECT** (Reason: none create the CA policy framework that triggers session control).
4. **SC-300 Golden Rule:** *"Catalog researches, Discovery discovers, CA App Control proxies. Session policy always starts with a Conditional Access policy."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-cloud-apps/proxy-deployment-aad

---

### 15. PIM — Eligible vs. Active & Password Reset Restrictions (Q266, Q277)
1. **Exam Scenario:** Predict whether password resets succeed between admins holding PIM assignments; choose Key Vault roles manageable via PIM.
2. **Key Decision Point:** Assignment state (**Active** vs. **Eligible**) and role-privilege protection boundaries.
3. **Decision Path:**
   - IF the target user is **eligible** (not active) for a privileged role (Exchange Admin, Global Admin, Security Admin) -> a **User Administrator CANNOT reset their password** (Reason: restricted roles protect against privilege escalation).
   - IF the target user holds only an *active* User Administrator assignment (no higher-privileged role) -> another **User Administrator CAN reset their password**.
   - IF the user holds an **Eligible** assignment -> they have **NO permissions** until they activate the role in PIM (with MFA/justification as configured).
   - IF asked to manage Key Vault access via PIM -> **REJECT** Key Vault *access policies* (Reason: PIM manages only Entra roles and Azure RBAC roles, not Key Vault access policies — use **Key Vault Secrets Officer** for read/update, **Key Vault Secrets User** for read-only).
4. **SC-300 Golden Rule:** *"Eligible = dormant until activated. User Admins can't reset privileged-role holders. PIM covers Entra + Azure RBAC roles — never Key Vault access policies."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-configure

---

### 16. Microsoft Entra Connect — OU & Group-Based Filtering (Q267)
1. **Exam Scenario:** Pilot synchronization using a group; predict which objects sync given OU and group filters.
2. **Key Decision Point:** Group-based filtering acts as a **strict whitelist**; the pilot group's OU and members' OUs must both be in scope.
3. **Decision Path:**
   - IF the user is in an in-scope OU **AND** is a **direct member** of the sync group -> **SYNCS**.
   - IF the user is in an in-scope OU but **NOT** a direct member of the sync group -> **DOES NOT SYNC** (whitelist filter excludes them).
   - IF a *group* is a direct member of the sync group -> the **group object itself syncs**, but its nested members are **ignored** (sync engine does not resolve nested memberships).
   - IF the pilot group's own OU is excluded from OU filtering -> **REJECT** any sync claims (both the group's OU and members' OUs must be selected).
4. **SC-300 Golden Rule:** *"Group filtering = whitelist. Direct membership only; nested groups sync as objects, their members never do."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-sync-configure-filtering

---

### 17. Cross-Tenant Synchronization — Scope Rules (Q276)
1. **Exam Scenario:** Predict which users sync between tenants via cross-tenant synchronization.
2. **Key Decision Point:** **Direct membership** in the scoped group + **UserType = Member**.
3. **Decision Path:**
   - IF the user is a **direct member** of the scoped group AND `UserType = Member` -> **SYNCS** to the target tenant.
   - IF the user is only a member of a group *nested inside* the scoped group -> **DOES NOT SYNC** (Reason: nested groups are not supported — confirmed in [cross-tenant sync FAQ](https://learn.microsoft.com/entra/identity/multi-tenant-organizations/cross-tenant-synchronization-overview#frequently-asked-questions)).
   - IF the user's UserType is **Guest** (B2B) -> **DOES NOT SYNC** (Reason: only internal member users can be provisioned; guest users are blocked).
4. **SC-300 Golden Rule:** *"Cross-tenant sync = direct members + Member type only. No nesting, no guests."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/multi-tenant-organizations/cross-tenant-synchronization-configure#step-7-define-who-is-in-scope-for-provisioning

---

### 18. Access Reviews — Scope & Nested Group Limitations (Q283)
1. **Exam Scenario:** An access review targets Guest users of Group1, reviewed by the group Owner; determine who is actually reviewed.
2. **Key Decision Point:** Review scope (Guests only) + **direct membership only** (no nested expansion).
3. **Decision Path:**
   - IF the reviewer is the group's **Owner** -> valid reviewer selection.
   - IF the user is a **Member** (not Guest) -> **EXCLUDED** even if directly in the group (scope = Guests only).
   - IF the user is a member of a group *nested inside* the reviewed group -> **EXCLUDED** (Reason: access reviews evaluate **direct members only**, no nested expansion).
   - IF the user is a Guest but only in the *nested* group -> **EXCLUDED**.
4. **SC-300 Golden Rule:** *"Access reviews see direct members only — nesting is invisible, and the scope filter (Guests/Members) applies on top."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/access-reviews-overview

---

### 19. Identity Protection — Data Retention (Q268)
1. **Exam Scenario:** Determine how long Identity Protection report data (risky users, risky sign-ins, risk detections) is retained.
2. **Key Decision Point:** Retention period = **90 days maximum**.
3. **Decision Path:**
   - IF the question asks about Identity Protection report retention -> **90 days**.
   - IF options offer 30 days -> **REJECT** (Reason: that's Entra audit/sign-in log retention for P1/P2 tenants, not ID Protection reports).
   - IF longer retention is required -> **EXPORT** to external storage or a SIEM (e.g., Microsoft Sentinel); the portal does not retain beyond 90 days.
4. **SC-300 Golden Rule:** *"ID Protection remembers for 90 days — after that, export or lose it."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-protection/howto-identity-protection-investigate-risk

---

### 20. Continuous Access Evaluation (CAE) — Configuration Location (Q269)
1. **Exam Scenario:** Configure/customize CAE for users holding a specific directory role (Application Administrator).
2. **Key Decision Point:** CAE is configured via **Conditional Access policy → Session controls**.
3. **Decision Path:**
   - IF the requirement is to enable/disable/customize CAE for a role-holding user set -> create a CA policy targeting the role, then adjust CAE under **Session** controls (enabled by default).
   - IF options offer "admin consent settings" -> **REJECT** (Reason: governs app permission grants, unrelated to session evaluation).
   - IF options offer "a sign-in risk policy" -> **REJECT** (Reason: reacts to calculated risk levels; it is not the CAE configuration tool).
   - IF options offer "an access review" -> **REJECT** (Reason: periodic access certification, not real-time session control).
4. **SC-300 Golden Rule:** *"CAE lives in the Session section of a Conditional Access policy — target the role, tune the session."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-continuous-access-evaluation

---

### 21. Risk-Based Conditional Access — Fraud Reports & Auto-Blocking (Q279)
1. **Exam Scenario:** Users report fraudulent MFA requests; the goal is to *automatically block* them.
2. **Key Decision Point:** Reporting fraud raises **user risk to High** — but blocking requires a **separate CA policy**.
3. **Decision Path:**
   - IF only the MFA notification setting ("Report suspicious activity") is configured -> **GOAL NOT MET** (Reason: it raises user risk to High but does not block anyone by itself).
   - IF a CA policy scoped to **High user risk** with **Block access** is also configured -> **GOAL MET**.
   - IF an answer claims the notification setting alone blocks users -> **REJECT** (Reason: detection ≠ enforcement; enforcement lives in Conditional Access).
4. **SC-300 Golden Rule:** *"Fraud report raises risk; Conditional Access pulls the trigger. Risk signal + Block access policy = automatic block."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/howto-conditional-access-policy-risk

---

### 22. Azure ABAC — Supported Resources (Q281)
1. **Exam Scenario:** Apply attribute-based role assignment conditions to a resource's data plane.
2. **Key Decision Point:** ABAC conditions are supported **only** for Azure Storage (Blob/Queue/Data Lake Gen2) and Azure Container Registry **data actions**.
3. **Decision Path:**
   - IF the resource is a **storage account** -> **ABAC SUPPORTED** (conditions on blob index tags, container names, etc.).
   - IF the resource is **Key Vault** -> **REJECT** (Reason: data actions handled strictly by standard RBAC or access policies — no ABAC).
   - IF the resource is a **VM** or **App Service** -> **REJECT** (Reason: no ABAC support for compute resources or App Service).
4. **SC-300 Golden Rule:** *"ABAC = Storage + Container Registry data actions only. Key Vault, VMs, and App Service stay plain RBAC."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/azure/role-based-access-control/conditions-overview

---

### 23. Entra ID Logs to Log Analytics — First Step (Q288)
1. **Exam Scenario:** Send Entra ID audit/sign-in logs to a pre-created Log Analytics workspace.
2. **Key Decision Point:** **Diagnostic settings** on the Entra tenant define log categories and destination.
3. **Decision Path:**
   - IF logs must flow to Log Analytics -> **FIRST** create **Diagnostic settings** selecting AuditLogs/SignInLogs with the workspace as destination.
   - IF the proposed step is "create a workbook" -> **REJECT as first step** (Reason: workbooks *visualize* data already in the workspace — a later step).
   - IF the proposed step is `Update-MgDomain` or `Update-MgOrganization` -> **REJECT** (Reason: these Graph cmdlets update domain/tenant properties, not log streaming).
4. **SC-300 Golden Rule:** *"No Diagnostic settings = no logs. Workbooks visualize; diagnostics stream."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/monitoring-access/overview-monitoring-access

---

### 24. On-Prem Account Disable Sync Delay (Q286)
1. **Exam Scenario:** Disabled on-premises accounts can still authenticate for up to 30 minutes; a proposed fix must eliminate the delay.
2. **Key Decision Point:** The delay is inherent to the **Entra Connect sync cycle** (~30 min default).
3. **Decision Path:**
   - IF the proposed solution is **Microsoft Entra Password Protection** -> **REJECT / GOAL NOT MET** (Reason: it enforces password strength/banned passwords; it does not accelerate attribute sync like account enable/disable state).
   - IF the requirement is truly immediate revocation -> look to token revocation / CAE mechanisms, not password protection.
4. **SC-300 Golden Rule:** *"Password Protection fixes weak passwords, not sync latency. A disabled account takes a sync cycle to stop authenticating."*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-password-hash-synchronization

---

## Master Cheat Sheet — Part 8

| Signal in Question | Correct Answer Pattern |
|---|---|
| User-assigned MI across regions | ✅ Works — SP is global in Entra ID |
| Attach identity to VM/App | Managed identities only — never a user account |
| Read secrets (least privilege) | Key Vault **Secrets User** |
| Read + update secrets | Key Vault **Secrets Officer** |
| Assign roles / right-size roles | **User Access Administrator** |
| AU-scoped role | User Administrator ✅ / Security Administrator ❌ |
| Group inside AU | Group object in scope, **members NOT** |
| Nested group (sync, access review, cross-tenant) | Direct members only — nesting ignored |
| B2B Guest in cross-tenant sync | ❌ Never syncs |
| ToU enforcement | PDF + Grant control in CA policy |
| Phishing-resistant MFA | WHfB + FIDO2 + MFA-CBA |
| First step for CBA | Upload CA public certificate |
| First step for VM Entra sign-in failure | Verify port 443 to enterpriseregistration.windows.net |
| ID Protection retention | 90 days |
| CAE configuration | CA policy → Session controls |
| ABAC conditions | Storage + ACR data actions only |
| Logs to Log Analytics | Diagnostic settings first |
| PIM scope | Entra roles + Azure RBAC — never Key Vault access policies |# SC-300 Decision Trees - Part 9

# SC-300 Decision Trees & Logic Rules — Part 9 (Questions 289–324)

Validated against Microsoft Learn (Conditional Access precedence, SSPR methods, Terms of Use, Access Reviews, Cross-tenant sync known issues, Group lifecycle, Deleted-user restore, Global Secure Access). All terminology follows current Microsoft Entra naming standards.

---

## A. Conditional Access & Access Enforcement

### 1. Enforcing Terms of Use (ToU)
1. **Exam Scenario:** Deny access to any user who has not accepted the organization's Terms of Use before accessing cloud applications.
2. **Key Decision Point:** Which control enforces *user acceptance at sign-in* — a Conditional Access Grant control vs. device/session tooling.
3. **Decision Path:**
   - IF the requirement is "block access until the user accepts a document during sign-in" -> SELECT **Microsoft Entra Terms of Use + a Conditional Access policy (Grant control: require the ToU)** (Reason: ToU is a Grant control evaluated at sign-in; access is granted only after acceptance; requires Microsoft Entra ID P1; max 40 terms per tenant).
   - IF the option is a **Defender for Cloud Apps access policy** -> REJECT (Reason: real-time session monitoring/control, not initial sign-in conditions).
   - IF the option is an **Intune compliance policy** -> REJECT (Reason: evaluates device health/settings, not user acceptance of terms).
   - IF the option is **Intune Terms and Conditions** -> REJECT (Reason: applies only to users *enrolling devices* into Intune, not general tenant resource access).
4. **SC-300 Golden Rule:** "ToU lives in Conditional Access — upload the PDF, then build a CA policy whose Grant control requires the terms."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/terms-of-use

### 2. Conditional Access Precedence — Exclusions & Block
1. **Exam Scenario:** Users overlap across include/exclude groups across multiple policies (MFA, Block, compliant device); determine who is blocked, who must do MFA.
2. **Key Decision Point:** Evaluation order: exclusion vs. inclusion, and Block vs. Grant across multiple applicable policies.
3. **Decision Path:**
   - IF a user is in BOTH an included group AND an excluded group of the same policy -> the policy does **NOT** apply to them (Reason: "the exclude action overrides the include action in a policy" — validated on Learn).
   - IF multiple policies apply and one grants access while another blocks -> **Block wins** (Reason: a Block control strictly overrides any Grant control).
   - IF a user is only in included assignments -> the policy applies and its Grant/session controls are enforced.
   - IF the only applicable control is a session control (e.g., sign-in frequency) -> REJECT assuming MFA is enforced (Reason: session controls do not equal Grant controls).
4. **SC-300 Golden Rule:** "Exclude beats include; Block beats Grant; across policies, the most restrictive outcome wins."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-users-groups and https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-grant

### 3. Conditional Access Templates — Creator Auto-Exclusion
1. **Exam Scenario:** An admin creates the "Require multifactor authentication for Azure management" template policy; determine which admins are forced into MFA.
2. **Key Decision Point:** Who is targeted by the template vs. who is protected by the built-in lockout safeguard.
3. **Decision Path:**
   - IF the policy is created from the template targeting **all users** for Azure management -> SELECT **all admins except the creator** are enforced (Reason: the template's safety mechanism automatically excludes the creating admin to prevent tenant lockout).
   - IF an option includes the creating admin (Admin4) -> REJECT (Reason: creator is auto-excluded at creation time).
   - IF an option omits other Global Admins or ordinary admins -> REJECT (Reason: the template targets all directory users, not just specific directory roles).
4. **SC-300 Golden Rule:** "The template protects its creator — everyone else gets MFA on Azure management."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-policy-templates

### 4. SSPR Verification Methods
1. **Exam Scenario:** Identify which registered method a user can use to verify identity during self-service password reset.
2. **Key Decision Point:** Is the method an *SSPR verification method* or merely a *sign-in authentication method*?
3. **Decision Path:**
   - IF the method is **mobile app notification, mobile app code (Authenticator OATH code), email, phone call, SMS, or security questions** -> SELECT it as valid for SSPR.
   - IF the method is a **smart-club card / certificate-based auth** -> REJECT (Reason: sign-in only, not an SSPR verification method).
   - IF the method is a **FIDO2 security key** -> REJECT (Reason: passwordless *sign-in* method, unavailable for SSPR).
   - IF the method is a **Windows Hello PIN** -> REJECT (Reason: device-specific sign-in credential, not SSPR).
4. **SC-300 Golden Rule:** "SSPR accepts app codes, email, phone, and questions — never smartcard, FIDO2, or a Hello PIN." (Requires Microsoft Entra ID P1 for end users.)
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/concept-sspr-howitworks

### 5. Smart Lockout Configuration Location
1. **Exam Scenario:** Configure the lockout threshold and lockout duration for brute-force protection.
2. **Key Decision Point:** Which settings blade owns lockout parameters.
3. **Decision Path:**
   - IF configuring lockout threshold/duration -> SELECT **Password protection settings** in the Microsoft Entra admin center (Reason: smart lockout values are managed there).
   - IF the option is a **user risk policy** or **sign-in risk policy** -> REJECT (Reason: Identity Protection policies automate *risk responses*, not base lockout tuning).
   - IF the option is **authentication strengths** -> REJECT (Reason: defines permitted MFA method combinations in CA, not lockouts).
4. **SC-300 Golden Rule:** "Lockout numbers live in Password protection; risk policies react, they don't configure lockouts."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/howto-password-smart-lockout

### 6. Passwordless MFA for Microsoft Entra VM Sign-In
1. **Exam Scenario:** Sign in to a Linux VM with the Microsoft Entra extension using a passwordless method that satisfies MFA (device code flow).
2. **Key Decision Point:** Is the method passwordless, phishing-resistant, portable, and supported in the Entra device-code flow?
3. **Decision Path:**
   - IF the method is **Microsoft Authenticator passwordless phone sign-in** or a **FIDO2 security key** -> SELECT it (Reason: both are passwordless MFA methods fully supported in the Entra authentication flow).
   - IF the method is **Windows Hello for Business** -> REJECT (Reason: bound to a specific trusted Windows device, not portable).
   - IF the method is a **Temporary Access Pass** -> REJECT (Reason: onboarding/recovery only, not ongoing sign-in).
   - IF the method is **SMS** -> REJECT (Reason: a weak second factor used *with* a password; fails the "no passwords" requirement).
4. **SC-300 Golden Rule:** "Portable passwordless = Authenticator app or FIDO2; device-bound or recovery credentials never satisfy ongoing passwordless sign-in."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/authentication/howto-authentication-passwordless-phone

---

## B. Identity Governance (Access Reviews, Lifecycle, Consent)

### 7. Access Reviews — Reviewers, Scope & Membership Mechanics
1. **Exam Scenario:** Determine who can review whom in a group access review, and which objects fall in scope.
2. **Key Decision Point:** The **Reviewers** setting + the **Scope** setting + membership type (direct vs. nested, user vs. workload identity).
3. **Decision Path:**
   - IF Reviewers = **"Users review their own access"** -> each in-scope user reviews **only their own** membership (Reason: self-attestation is per-user; members cannot review guests or others).
   - IF Reviewers = **"Group owner(s)"** -> owners review all in-scope members, **including themselves** if they are also members (Reason: owner-reviewer covers every in-scope object; an owner-member can attest their own access).
   - IF Scope = **"All users"** and the object is a **managed identity or service principal** -> NOT in scope (Reason: "All users" evaluates user objects only; workload identities require a separate review type — PIM access reviews of service principals, additionally requiring Microsoft Entra Workload ID Premium).
   - IF the user is a member only via a **nested group** -> per the exam key: excluded from the review (Reason: reviews evaluate direct group membership). ⚠️ *Learn watch-out:* current Microsoft Learn documentation states that in a group review "nested groups are automatically flattened, so users from nested groups appear as individual users" — know both, but follow the exam key for this item.
   - **Licensing threshold:** group/application access reviews require **Microsoft Entra ID Governance or Microsoft Entra Suite** (some capabilities available with Microsoft Entra ID P2).
4. **SC-300 Golden Rule:** "Self-review = review yourself only; 'All users' = humans only; owners review everyone in scope — and service principals need their own review."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/create-access-review and https://learn.microsoft.com/entra/id-governance/access-reviews-overview

### 8. Group Expiration Policies
1. **Exam Scenario:** Configure lifecycle expiration for groups and determine the shortest allowed lifetime.
2. **Key Decision Point:** Group type eligibility and minimum lifetime value.
3. **Decision Path:**
   - IF the group is a **Microsoft 365 group** -> SELECT it for expiration (Reason: expiration policies apply **only** to Microsoft 365 groups).
   - IF the group is a **security group, mail-enabled security group, or distribution list** -> REJECT (Reason: expiration is not supported for these types).
   - IF a custom lifetime is required -> set **30 days or more** (Reason: custom values below 30 days are invalid; presets also exist, e.g., 180/365).
4. **SC-300 Golden Rule:** "Expiration is Microsoft-365-groups-only and never shorter than 30 days; unrenewed groups are deleted and restorable for 30 days."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/users/groups-lifecycle

### 9. Deleted User Recovery
1. **Exam Scenario:** Restore a recently deleted user; identify the window and the least-privileged role.
2. **Key Decision Point:** Soft-delete window + role holding `microsoft.directory/deletedItems/restore`.
3. **Decision Path:**
   - IF the user was deleted -> the account is restorable for **30 days** with properties, licenses, and group memberships intact; after 30 days permanent deletion starts automatically and cannot be stopped.
   - IF choosing the least-privileged role to view/restore -> SELECT **User Administrator** (Reason: least-privileged built-in role with the restore permission).
   - IF the option is **Helpdesk Administrator** -> REJECT (Reason: can reset passwords/sign out non-admin users only; cannot restore deleted directory objects).
4. **SC-300 Golden Rule:** "30 days to bring them back; User Administrator is the least-privileged restorer."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/fundamentals/users-restore

### 10. Self-Service Application Access & Approvals
1. **Exam Scenario:** App1 allows self-service access via Group1 with approval required; predict request/approval behavior.
2. **Key Decision Point:** Existing group membership, the approval flag, and *where* approvers act.
3. **Decision Path:**
   - IF the user is **already a member** of the group assigned to the app -> they already have access; no self-service request occurs (Reason: group-based assignment already grants the entitlement).
   - IF "Require approval before granting access" = **Yes** -> nobody is added automatically — **not even the designated approver** (Reason: requests stay pending until an explicit approval action).
   - IF asked where the approver approves -> SELECT the **My Apps portal (Approvals tab)**, REJECT the Microsoft Entra admin center (Reason: the admin center is for tenant administration; end-user requests are managed in My Apps).
4. **SC-300 Golden Rule:** "Group membership = instant access; approval = pending until acted on; approvers live in My Apps."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/self-service-application-assignment

### 11. Consent Governance — Admin Consent Workflow vs. User Consent Policy
1. **Exam Scenario:** (a) Require approval before users can add new enterprise applications; (b) restrict which permissions users may consent to.
2. **Key Decision Point:** Approval of *new app access* vs. restriction of *consentable permissions*.
3. **Decision Path:**
   - IF the requirement is an **approval process for new applications** -> SELECT the **admin consent workflow** (Reason: users submit requests; designated reviewers grant/deny before consent).
   - IF the requirement is to **limit which permissions users can consent to** -> SELECT the **user consent settings / user consent policy** (e.g., allow consent only to low-impact classified permissions such as `User.Read`).
   - IF the option is **security defaults** -> REJECT (Reason: security defaults disable user consent entirely, contradicting a "controlled but allowed" requirement).
   - IF the option is **permission classifications** alone -> REJECT as the primary step (Reason: classifications label permissions; the consent *policy* must be configured to use them).
   - IF the option is an **access review** -> REJECT (Reason: access reviews recertify *existing* access; they do not approve *initial* app addition).
   - IF the option is a **Cloud Discovery anomaly policy** or an **app connector** -> REJECT (Reason: anomaly detection alerts on usage patterns; connectors attach to already-sanctioned apps).
4. **SC-300 Golden Rule:** "Admin consent workflow = who approves new app requests; consent policy = what users may approve."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/configure-admin-consent-workflow and https://learn.microsoft.com/entra/identity/enterprise-apps/configure-user-consent

---

## C. External Identities (B2B & Multitenant)

### 12. Inviting External Users (B2B Collaboration)
1. **Exam Scenario:** Grant an external contractor (user1@outlook.com) access to an enterprise app while they keep using their existing credentials.
2. **Key Decision Point:** Create a *new internal identity* vs. *invite the external identity*.
3. **Decision Path:**
   - IF the external user must authenticate with their **existing external credentials** -> SELECT **B2B collaboration invitation via `New-MgInvitation`** (Reason: creates a guest user object linked to the external identity).
   - IF the option is `New-MgUser` -> REJECT (Reason: creates a native **member** account requiring a new password).
   - IF the option is **external collaboration settings** -> REJECT (Reason: defines *policy* — who may invite, guest permissions — but invites nobody).
   - IF the option is **Microsoft Entra Connect sync** -> REJECT (Reason: synchronizes on-premises AD to Entra ID; irrelevant for external identities).
4. **SC-300 Golden Rule:** "Invite, don't create — `New-MgInvitation` keeps the contractor's own identity."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/what-is-b2b

### 13. B2B Collaboration Restrictions (Allow/Deny Domains)
1. **Exam Scenario:** Guest invitations succeed for fabrikam.com but fail for adatum.com.
2. **Key Decision Point:** Domain-specific invitation control.
3. **Decision Path:**
   - IF invitations fail for one specific partner domain only -> SELECT **collaboration restrictions (allowlist/denylist of domains)** in External Identities (Reason: per-domain allow/deny is exactly this feature).
   - IF the option is **verifiable credentials** -> REJECT (Reason: decentralized identity claims, not domain invite control).
   - IF the option is **named locations** -> REJECT (Reason: trusted IP/country ranges for Conditional Access — network-based, not domain-based).
   - IF the option is **guest invite settings** -> REJECT (Reason: controls *who can invite*, not *which domains* are permitted).
4. **SC-300 Golden Rule:** "One domain works, another doesn't = collaboration restrictions allow/block list."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/external-id/what-is-b2b

### 14. Cross-Tenant Synchronization Scope
1. **Exam Scenario:** Predict which objects (users vs. security groups vs. Microsoft 365 groups) are provisioned to the target tenant.
2. **Key Decision Point:** Object types supported by the cross-tenant synchronization engine.
3. **Decision Path:**
   - IF the object is a **user** in the scoped group -> SELECT it as synchronized (Reason: cross-tenant sync automates B2B collaboration **user** lifecycle across tenants).
   - IF the object is a **security group or Microsoft 365 group** -> REJECT (Reason: groups may be *assigned to the configuration to scope which users sync*, but group objects themselves are **not provisioned** to the target tenant — validated: "Synchronizing groups, devices, and contacts into another tenant" is an unsupported scenario).
4. **SC-300 Golden Rule:** "Cross-tenant sync moves people, not groups — groups only define the scope."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/app-provisioning/known-issues?pivots=cross-tenant-synchronization and https://learn.microsoft.com/entra/identity/multi-tenant-organizations/cross-tenant-synchronization-overview

---

## D. Hybrid Identity

### 15. Hybrid Authentication Method Selection (+ SSPR Writeback)
1. **Exam Scenario:** Passwords must be validated against on-premises AD DS at sign-in with minimal administrative effort; on-prem users must also use cloud SSPR.
2. **Key Decision Point:** Where password validation occurs + bidirectional password sync needs.
3. **Decision Path:**
   - IF validation must occur **against on-prem AD DS** with **minimal admin effort** -> SELECT **Pass-through Authentication (PTA)** (Reason: lightweight agents validate against the on-prem DC; deployed via **Microsoft Entra Connect**).
   - IF the org already runs federation or needs advanced claims/infrastructure -> SELECT **AD FS** (Reason: also validates against on-prem DCs, but requires heavy infrastructure — fails "minimize effort").
   - IF cloud-side validation is acceptable -> SELECT **Password hash synchronization** (Reason: validation happens in Entra ID — fails any "on-prem validation" requirement, but offers leak-credential detection and resiliency).
   - IF synced users must perform **SSPR** -> also enable **Password writeback** (Reason: cloud password changes must flow back to on-prem AD).
   - IF the option is **Microsoft Entra Password protection** -> REJECT as an auth method (Reason: blocks weak passwords; not an authentication mechanism).
   - IF the option is **Microsoft Identity Manager** or the **Entra admin center** for PTA enablement -> REJECT (Reason: PTA agents are enabled/configured via the Entra Connect wizard on-premises).
4. **SC-300 Golden Rule:** "PTA = on-prem validation with an agent; PHS = cloud validation; writeback = SSPR's return ticket to AD."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-pta and https://learn.microsoft.com/entra/identity/authentication/concept-sspr-writeback

### 16. Microsoft Entra Connect Filtering Order
1. **Exam Scenario:** OU filtering and group-based filtering are both configured; predict which objects sync.
2. **Key Decision Point:** Evaluation order (OU filter → group filter) and direct-membership semantics.
3. **Decision Path:**
   - IF the object is a **direct member of the filtering group** AND resides in a **selected OU** -> SELECT it as synchronized (applies to users *and* to a nested group **object** that is itself a direct member of the filter group).
   - IF the user is only a member via a **nested group** -> REJECT as synchronized (Reason: group-based filtering limits sync to **direct members**; nested members are ignored).
   - IF the object sits in an **unselected OU** -> REJECT (Reason: OU filtering is evaluated first and excludes the object regardless of group membership).
4. **SC-300 Golden Rule:** "OU first, then group; group filtering counts direct members only — a nested group's *members* are ignored, but the nested group *object* can still sync."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-sync-configure-filtering

---

## E. Workload Identities & Azure RBAC

### 17. Managed Identity Token Acquisition (IMDS)
1. **Exam Scenario:** An application on an Azure VM must acquire an access token for its managed identity.
2. **Key Decision Point:** The endpoint used to request the token.
3. **Decision Path:**
   - IF requesting a managed identity token from inside a VM -> SELECT **Azure Instance Metadata Service (IMDS) at 169.254.169.254** (Reason: non-routable link-local address; returns a JWT for the specified resource).
   - IF the option is the VM's **public IP** -> REJECT (Reason: internet communication, not IMDS).
   - IF the option is **127.0.0.1** -> REJECT (Reason: localhost loopback for VM-local services, not the Azure-provided IMDS).
   - IF the option is the VM's **private VNet IP** -> REJECT (Reason: intra-VNet traffic, not IMDS).
4. **SC-300 Golden Rule:** "Managed identity tokens come from the magic link-local: 169.254.169.254."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/how-to-use-vm-token

### 18. Managed Identity Type Selection + Secret Storage
1. **Exam Scenario:** Multiple VMs need to authenticate to a third-party API using secrets; minimize administrative effort; keep secrets out of code.
2. **Key Decision Point:** Identity lifecycle coupling + designated secret store.
3. **Decision Path:**
   - IF **one identity must be shared across many VMs** -> SELECT a **user-assigned managed identity** (Reason: standalone Azure resource created once and assigned to all VMs — one permission set to manage).
   - IF the identity must be tied to a **single resource's lifecycle** -> SELECT **system-assigned** (Reason: per-VM identity means per-VM permission management — fails "minimize effort" at scale).
   - IF storing third-party API credentials -> SELECT **Azure Key Vault** (Reason: the designated Azure service for secrets).
   - IF the option pairs **user accounts** with anything -> REJECT (Reason: manual credential management contradicts minimal effort).
   - IF the option is **Storage Service Encryption** -> REJECT (Reason: protects data at rest in a storage account; unrelated to app authentication or secret storage).
4. **SC-300 Golden Rule:** "Many VMs, one identity = user-assigned; secrets belong in Key Vault, never in code."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview

### 19. Service Principal Credentials
1. **Exam Scenario:** App1 (service principal) must authenticate as itself to call App2's API with no user interaction.
2. **Key Decision Point:** Credential types valid for *application-only* (app-only) authentication.
3. **Decision Path:**
   - IF the app must prove its own identity -> SELECT **client secret** OR **certificate** (Reason: a certificate signs a JWT assertion with the private key — the more secure option; both enable non-interactive app-only auth).
   - IF the option is a **managed identity** -> REJECT (Reason: an identity *for an Azure resource*, not a credential configured on an app registration).
   - IF the option is a **user account** -> REJECT (Reason: delegated auth — the app acts on behalf of a signed-in user).
   - IF the option is a **one-time password** -> REJECT (Reason: a user MFA factor, inapplicable to service principals).
4. **SC-300 Golden Rule:** "App-only auth = secret or certificate; everything else belongs to users or Azure resources."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity-platform/howto-create-service-principal-portal

### 20. Azure RBAC — Control Plane vs. Data Plane
1. **Exam Scenario:** Predict who can read blob data, read a file share, log into a VM, or view Key Vault metadata, given various role assignments and scopes.
2. **Key Decision Point:** Does the role grant **data-plane** access, only **management-plane** access, or the `listkeys` escape hatch?
3. **Decision Path:**
   - **Storage blobs:** IF assigned **Storage Blob Data Reader** -> can read blob *data* (data plane). IF assigned **Storage Contributor** -> can also read blob data **via `listkeys`** (Reason: contains `Microsoft.Storage/storageAccounts/listkeys/action`). IF assigned only **Reader** -> REJECT both data access and key retrieval (Reason: management-plane read only).
   - **File shares:** IF reading files via management interfaces (portal) requires **storage account keys** -> only the role that can list keys (Storage Contributor) succeeds; Storage Blob Data Reader and Reader cannot.
   - **VM sign-in:** IF the requirement is logging into the VM OS -> SELECT **Virtual Machine Administrator Login / Virtual Machine User Login** (data-plane login roles; assignments **inherit down** from subscription scope). IF assigned **Virtual Machine Contributor** -> REJECT OS login (Reason: manages the VM resource — start/stop/delete — but grants no data-plane login).
   - **Key Vault:** IF only viewing **metadata** of vaults/keys/secrets/certs -> SELECT **Key Vault Reader** (least privilege). IF the option is **Key Vault Secrets User** -> REJECT (Reason: reads actual secret *values* — exceeds requirement). IF **Key Vault Crypto User/Officer** -> REJECT (Reason: cryptographic operations/management — excessive).
   - **Permissions Management onboarding:** IF assigning an existing RBAC role via PowerShell -> SELECT `New-AzRoleAssignment` with **Reader** at subscription scope (least privilege for the CIEM first-party app). IF the cmdlet is `New-AzRoleDefinition` -> REJECT (Reason: creates a *new custom* role). IF `New-AzTag` -> REJECT (Reason: applies metadata tags).
4. **SC-300 Golden Rule:** "Management roles ≠ data access — unless the role can list keys; login roles are data-plane roles; when only metadata is needed, Reader is the answer."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/azure/role-based-access-control/built-in-roles and https://learn.microsoft.com/azure/key-vault/general/rbac-guide

---

## F. Global Secure Access & Defender for Cloud Apps

### 21. Global Secure Access — Profiles, Filtering & Remote Networks
1. **Exam Scenario:** Block a URL via the GSA client; enable Private Access; route traffic for mixed Windows/Linux sites; predict when a GSA-scoped CA policy prompts MFA.
2. **Key Decision Point:** Traffic must actually **traverse Global Secure Access** (via client or remote network) and policies attach through **security profiles**.
3. **Decision Path:**
   - IF blocking a specific FQDN for client users -> sequence: **enable Internet Access forwarding → create web content filtering policy (Block) → create security profile and link the policy → CA policy whose session control selects the security profile** (Reason: filtering policies cannot be assigned directly to users).
   - IF enabling Private Access -> SELECT the **Traffic forwarding blade → "Private access profile" checkbox** (Reason: profiles are enabled there; REJECT *Remote networks* = site connectivity, *Connectors* = on-prem proxy agents, *Security profiles* = not where profiles are enabled).
   - IF traffic must be captured from **all OSes (Windows + Linux)** at a site -> SELECT a **remote network** (IPsec tunnel from the edge router; OS-agnostic). IF the option is the **GSA client** -> REJECT (Reason: Windows-only client; fails Linux coverage). IF **named location** -> REJECT (Reason: trusted IP ranges for CA, not traffic routing). IF **access package** -> REJECT (Reason: entitlement management, unrelated).
   - IF a CA policy is scoped to GSA traffic -> MFA fires **only when traffic traverses GSA**: user in the profile-assigned group **AND** (GSA client installed **OR** connected to an assigned remote network). IF either the group assignment or the network path is missing -> traffic bypasses GSA and the CA policy never evaluates (no MFA).
   - **Licensing threshold:** the Microsoft traffic profile requires Microsoft Entra ID P1/P2; Internet/Private Access require Microsoft Entra Suite / SSE licensing.
4. **SC-300 Golden Rule:** "No GSA traversal = no GSA-scoped CA policy; enable profiles on the Traffic forwarding blade; filters attach to security profiles, never directly to users."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/global-secure-access/concept-traffic-forwarding and https://learn.microsoft.com/entra/global-secure-access/how-to-configure-web-content-filtering

### 22. Defender for Cloud Apps — Session Control & Log Ingestion
1. **Exam Scenario:** (a) Deploy real-time session monitoring for App1; (b) ingest logs from a third-party web gateway automatically.
2. **Key Decision Point:** CAAC deployment order; automated vs. manual log ingestion.
3. **Decision Path:**
   - IF enabling session-level controls -> ordered sequence: **register App1 in Microsoft Entra ID → configure SSO → create a CA policy with session controls → finalize Connected apps settings in Defender for Cloud Apps → create a session policy**. IF asked for the *first* action -> SELECT **configure SSO** (Reason: Entra ID must be the identity provider before sessions can be proxied).
   - IF the option is **Cloud Discovery** -> REJECT (Reason: discovers shadow IT; doesn't configure controls for a known app). IF an **OAuth app policy** -> REJECT (Reason: governs third-party app permissions, not live sessions). IF a **traffic forwarding profile** -> REJECT (Reason: Global Secure Access mechanism, not the CAAC reverse proxy).
   - IF third-party gateway logs must flow **automatically and continuously** -> SELECT a **log collector** (Docker-based agent receiving Syslog/FTP and uploading to the service). IF the option is a **snapshot report** -> REJECT (Reason: manual, one-time upload). IF **add a data source** alone -> REJECT (Reason: also required for manual snapshots; not sufficient for automation). IF **app registration** -> REJECT (Reason: API access, not log ingestion).
4. **SC-300 Golden Rule:** "SSO before session control; log collectors for automation, snapshots for one-offs."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-cloud-apps/proxy-introduction and https://learn.microsoft.com/defender-cloud-apps/discovery-docker

---

## G. Directory Administration

### 23. Administrative Units — Dynamic Membership
1. **Exam Scenario:** AU1 is switched from *Assigned* to *Dynamic User* with rule `(user.department -eq "hr")`; predict membership outcomes.
2. **Key Decision Point:** Dynamic membership takes **sole ownership** of AU membership.
3. **Decision Path:**
   - IF the AU membership type changes to **Dynamic User** -> all **manually assigned members are removed**, including groups (Reason: groups cannot be members of a Dynamic-User AU).
   - IF a user's attribute **matches the rule** -> dynamically added, regardless of prior assignment status.
   - IF a user's attribute **does not match** (e.g., Department = IT) -> removed, even if previously statically assigned.
4. **SC-300 Golden Rule:** "Dynamic AUs own membership — attribute in, stay; attribute out, removed; manual assignments erased."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/roles/administrative-units

### 24. Blocking Self-Service Sign-Up via Microsoft Graph PowerShell
1. **Exam Scenario:** Prevent users from creating accounts in the tenant via self-service sign-up (email-verified flow).
2. **Key Decision Point:** The tenant-wide **authorization policy** property.
3. **Decision Path:**
   - IF blocking email-verified self-service sign-up -> SELECT `Update-MgPolicyAuthorizationPolicy` with **`allowEmailVerifiedUsersToJoinOrganization` = `$false`**.
   - IF the cmdlet is `Update-MgDomain` -> REJECT (Reason: no self-service sign-up parameter; that existed only in the deprecated MSOnline module).
   - IF the cmdlet is `Update-MgPolicyPermissionGrantPolicyExclude` -> REJECT (Reason: governs app *consent* policies, not sign-up).
   - IF the cmdlet is `Update-MgDomainFederationConfiguration` -> REJECT (Reason: configures SSO federation with external IdPs).
4. **SC-300 Golden Rule:** "Authorization policy governs sign-up; permission grant policies govern consent."
5. **Microsoft Learn Reference:** https://learn.microsoft.com/graph/api/resources/authorizationpolicy

---

## ⚡ Master Decision Rules (Cross-Cutting Quick Reference)

| Rule | Logic |
|---|---|
| **CA Exclusion Precedence** | Exclude always overrides include within the same policy |
| **CA Block Precedence** | Block overrides any Grant control across all applicable policies |
| **CA Template Safety** | All-user templates auto-exclude the creating admin (lockout safeguard) |
| **Access Review Scope** | "All users" = user objects only; service principals/managed identities need separate (PIM/Workload ID Premium) reviews; self-review = review yourself only |
| **Access Review Licensing** | Group/app reviews: Entra ID Governance or Entra Suite (some capabilities with P2) |
| **Least Privilege** | Metadata-only → Reader (Key Vault Reader, subscription Reader); data access → data-plane roles or `listkeys` capability |
| **Identity Type Selection** | External user keeps own credentials → B2B invite (`New-MgInvitation`); many VMs, one identity → user-assigned managed identity; app-only auth → secret/certificate |
| **Hybrid Validation** | On-prem DC validation + low effort → PTA; SSPR for synced users → password writeback |
| **Cross-Tenant Sync** | Users only; groups scope the sync but are never provisioned |
| **Lifecycle Numbers** | Deleted user: 30-day restore (User Administrator); M365 group expiration: ≥ 30 days, M365 groups only; deleted M365 group: 30-day restore |
| **GSA Enforcement** | CA policies scoped to GSA fire only when traffic traverses GSA (client or assigned remote network); filtering policies attach via security profiles |# SC-300 Decision Trees - Part 10

# SC-300 Decision Trees & Logic Rules — Part 10 (Questions 325–358)

Below is a structured set of decision trees distilled from the explanations, validated against official Microsoft Learn documentation.

---

### 1. Conditional Access: Exclusions Override Inclusions
1. **Exam Scenario:** Multiple CA policies target overlapping groups with includes and excludes; you must determine whether each user is prompted for MFA or blocked (Q325).
2. **Key Decision Point:** Whether the user appears in the **exclusion** list of any policy that also includes them — exclusions always win over inclusions *within the same policy*.
3. **Decision Path:**
   - IF user is in an included group AND explicitly excluded (by user, group, or directory role) in the same policy → **Policy does NOT apply** (Reason: "The exclude action overrides the include action in a policy" — Microsoft Learn).
   - IF user is in an included group and NOT excluded → **Policy applies** (MFA prompt / block enforced).
   - IF user is not in any included group of a policy → **Policy does not apply**, regardless of exclusions.
   - REJECT the assumption that "being in an excluded group elsewhere" matters — evaluation is per-policy, not across policies.
4. **SC-300 Golden Rule:** *Evaluate each CA policy independently: Include first, then Exclude — exclusion always wins inside the policy.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-users-groups#exclude-users

---

### 2. Workload Identity Conditional Access + On-Premises Server Authentication
1. **Exam Scenario:** Restrict an application registration (App1) so a scheduled task on an on-premises Windows server can call Microsoft Graph only from the corporate network (Q326).
2. **Key Decision Point:** Identity type (workload identity vs. user) and where the workload runs (Azure vs. on-premises).
3. **Decision Path:**
   - IF the target is an app registration/service principal → SELECT **Conditional Access for workload identities** with a **location condition** (corporate IP ranges) (Reason: CA policies scoped to users do not block service principal calls; workload identity policies do).
   - IF the workload runs on-premises (non-interactive scheduled task) → SELECT **certificate-based client credentials flow** (Reason: secure, uses local Windows Certificate Store, no plaintext secret).
   - IF the workload runs on an Azure resource → SELECT **managed identity** (Reason: natively designed for Azure resources; no credential management).
   - REJECT **client secret** for on-premises scheduled tasks (Reason: plaintext credential storage risk).
   - REJECT **managed identity** for on-premises servers (Reason: managed identities are tied to Azure resources only).
4. **SC-300 Golden Rule:** *Azure resource → managed identity; on-premises workload → certificate; restrict a service principal → CA for workload identities with location condition.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-users-groups (workload identities section)

---

### 3. Azure RBAC: Scope Inheritance & Data Plane vs. Control Plane
1. **Exam Scenario:** Determine whether users can read storage data, create resources, or assign roles based on role assignments at subscription, resource group, and resource scopes (Q327, Q342).
2. **Key Decision Point:** (a) Assignment scope (does it inherit down to the target resource?) and (b) whether the role grants **data actions** or only **control-plane actions**.
3. **Decision Path:**
   - IF role is scoped at subscription/RG level → permissions **inherit down** to all child resource groups and resources.
   - IF the task requires reading *data* (e.g., blob contents, secret values) → the role must include **data actions**:
     - `Reader` → view resources only, **NO data access** → REJECT for reading data.
     - `Reader and Data Access` → data plane read, but check its **scope** (e.g., scoped to RG1 does not cover storage1 in RG2).
     - `Key Vault Reader` → metadata only, **cannot read secret values**.
     - `Key Vault Secrets User` → **can read secret contents** at its assigned scope.
     - `Key Vault Administrator` → full data plane access, inherits down from subscription scope.
   - IF the task is creating resources (e.g., a virtual network) → requires `Contributor` (or higher) **at that scope**; `Reader` inherited from a parent scope is insufficient.
   - IF the task is assigning roles → requires `Microsoft.Authorization/roleAssignments/write` → `User Access Administrator` or `Owner` at the target scope.
4. **SC-300 Golden Rule:** *Scope determines WHERE, data actions determine WHAT — a Reader at the subscription can never read data; a data role scoped to RG1 never reaches RG2.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/azure/role-based-access-control/built-in-roles

---

### 4. Blocking Legacy/Basic Authentication
1. **Exam Scenario:** Prevent clients using IMAP, POP3, and other Basic authentication protocols from connecting to Exchange Online (Q328).
2. **Key Decision Point:** Which control mechanism governs the *authentication protocol* itself.
3. **Decision Path:**
   - IF the goal is to block Basic auth protocols → SELECT a **Conditional Access policy** with the **Client apps** condition set to **Legacy authentication clients**, targeting Exchange Online, with **Block access** grant control.
   - REJECT **Defender for Cloud Apps OAuth app policy** (Reason: governs third-party OAuth app permissions, not authentication protocols).
   - REJECT **Intune app protection policy** (Reason: controls data handling inside managed apps, e.g., copy/paste, not the auth protocol).
   - REJECT **Intune compliance policy** (Reason: defines device health rules; it feeds CA but does not block auth methods directly).
4. **SC-300 Golden Rule:** *Legacy authentication is killed with a CA policy: Client apps → Legacy authentication clients → Block.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-policies

---

### 5. Global Secure Access Client: Supported Platforms & Device States
1. **Exam Scenario:** Determine which user groups/devices can use the Global Secure Access (GSA) client, given Entra join/registration status and Windows multi-session environments (Q329, Q339).
2. **Key Decision Point:** Client platform availability (Windows, macOS, iOS, Android) and device registration state; AVD multi-session support.
3. **Decision Path:**
   - IF device is Windows 10/11, macOS, iOS, or Android → **GSA client supported**.
   - IF device is Microsoft Entra joined, hybrid joined, or Microsoft Entra registered → **supported** (registered/BYOD devices supported since client v2.26.108; on registered devices only Private Access traffic is supported).
   - IF environment is **Azure Virtual Desktop multi-session** (e.g., Windows 11 Enterprise multi-session for frontline workers) → **NOT supported** by the GSA client.
   - IF environment is AVD single-session or Windows 365 → **supported**.
   - IF device has no client (e.g., Linux, printers, cameras) → use a **remote network** (IPsec tunnel) instead of the client.
4. **SC-300 Golden Rule:** *GSA client = Windows/macOS/iOS/Android, joined or registered — but AVD multi-session is the deal-breaker; Linux gets a remote network, not a client.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/global-secure-access/how-to-install-windows-client (prerequisites) and https://learn.microsoft.com/entra/global-secure-access/concept-clients

---

### 6. Global Secure Access: Remote Networks for Clientless Devices
1. **Exam Scenario:** Connect on-premises Linux devices to Global Secure Access for Internet Access without installing client software (Q330).
2. **Key Decision Point:** Whether a client exists for the platform; if not, connectivity must be network-level.
3. **Decision Path:**
   - IF devices have no GSA client (Linux, mainframes, IoT) → SELECT **create a remote network** (IPsec tunnel from the branch edge router/firewall to GSA) (Reason: routes all outbound traffic from that location through the SSE, no endpoint software needed).
   - REJECT **private network connector** (Reason: an Entra Private Access component for publishing on-premises *apps*, not for forwarding internet traffic).
   - REJECT **Adaptive Access settings** (Reason: applies risk-based CA to traffic *after* GSA connectivity exists).
   - REJECT **Azure Connected Machine agent** (Reason: Azure Arc management agent, not a network client).
4. **SC-300 Golden Rule:** *No client for the OS? Build a remote network — an IPsec tunnel from the edge, not a connector, not Arc.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/global-secure-access/concept-remote-network-connectivity

---

### 7. Global Secure Access + Conditional Access: Compliant Network Locations & Security Profiles
1. **Exam Scenario:** (a) Require users to access resources only via the GSA client; (b) apply a GSA security profile to a CA policy (Q343, Q344).
2. **Key Decision Point:** Tenant-level GSA signaling and the correct CA assignments (target resources + session control).
3. **Decision Path:**
   - IF requiring access only via GSA client → FIRST **enable Global Secure Access signaling** at the tenant level (Reason: this makes the built-in **"All compliant network locations"** location available in CA) → then use it as the location condition in a CA policy.
   - REJECT **creating a standard named location** (Reason: signaling creates the built-in compliant location automatically).
   - REJECT **tenant restrictions tagging** (Reason: controls access to *external* orgs' tenants, not your own resources).
   - REJECT **remote network** (Reason: site-to-site connectivity for branch offices, not per-device compliance).
   - IF applying a GSA security profile in CA → configure BOTH: **Target resources** = *All internet resources with Global Secure Access* AND **Session** control = *Use Global Secure Access security profile* (e.g., SecurityProfile1).
4. **SC-300 Golden Rule:** *Enable signaling first → "All compliant network locations" appears; security profiles ride on Target resources + Session controls.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/global-secure-access/concept-conditional-access (integrated with https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-policies)

---

### 8. PIM: Access Reviews for Microsoft Entra Roles
1. **Exam Scenario:** A user must create access reviews of Microsoft Entra roles in PIM using least privilege (Q347).
2. **Key Decision Point:** Which directory role can create PIM role access reviews.
3. **Decision Path:**
   - IF creating access reviews for **Microsoft Entra roles** in PIM → SELECT **Privileged Role Administrator** (least privilege) or Global Administrator (Reason: "To create access reviews for Microsoft Entra roles, you must be assigned at least the Privileged Role Administrator role").
   - IF creating access reviews for **Azure resources** → requires **Owner** or **User Access Administrator** on the Azure resource.
   - REJECT **User Administrator** (Reason: can create group access reviews, not privileged role reviews).
   - REJECT **Identity Governance Administrator** (Reason: manages group/app reviews, lacks PIM Entra-role review permissions).
   - REJECT **User Access Administrator** (Reason: Azure RBAC role for Azure resources, not Entra directory roles).
   - Licensing note: PIM requires **Microsoft Entra ID P2** (or ID Governance/Suite); reviewing service principals additionally requires **Workload Identities Premium**.
4. **SC-300 Golden Rule:** *Entra role reviews in PIM = Privileged Role Administrator; Azure resource reviews = Owner/User Access Administrator — all on P2.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-create-roles-and-resource-roles-review

---

### 9. PIM: Just-in-Time Eligibility & Approval Configuration
1. **Exam Scenario:** Make User1/User2 eligible for User Administrator, require approval for User2's activation, and designate User3 as approver (Q357).
2. **Key Decision Point:** The prerequisite step before any activation/approval settings.
3. **Decision Path:**
   - IF configuring JIT access in PIM → FIRST **create the role assignment (make users eligible)** (Reason: eligibility is the foundation; activation settings, approval, and approvers are configured on the role *after* the assignment exists).
   - REJECT **Conditional Access policies** as the first step (Reason: they enforce MFA *during* activation, not eligibility).
   - REJECT **security groups** as the primary step (Reason: groups can hold role assignments, but the core action is the assignment itself).
   - REJECT **administrative units** (Reason: they *scope* a role, not grant eligibility).
4. **SC-300 Golden Rule:** *PIM order of operations: Assign eligibility → configure activation → set approval and approvers.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-configure

---

### 10. Access Reviews: "Upon Completion" Settings for Non-Responsive Reviewers
1. **Exam Scenario:** Automatically remove guest users from a group if the group owner (reviewer) fails to respond (Q333).
2. **Key Decision Point:** Which access review configuration section controls automated outcomes.
3. **Decision Path:**
   - IF reviewers don't respond and access must be auto-revoked → configure **"If reviewers don't respond"** = **Remove access** in the **Upon completion settings** section.
   - REJECT **Reviewers** section (Reason: selects *who* reviews, not the outcome of non-response).
   - REJECT **Advanced settings** (Reason: recommendations, justification prompts, reminders — not auto-removal logic).
   - REJECT **General** section (Reason: name, description, start date only).
4. **SC-300 Golden Rule:** *Non-responsive reviewer + auto-remove = "Upon completion" → "If reviewers don't respond" → Remove access.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-governance/create-access-review

---

### 11. Risk Policies: User Risk vs. Sign-in Risk
1. **Exam Scenario:** Determine whether users are prompted for password change/MFA based on user risk and sign-in risk policies and MFA registration status (Q358).
2. **Key Decision Point:** Which policy targets the user's group, and whether the user is **registered for MFA**.
3. **Decision Path:**
   - IF user is in the **user risk policy** group AND registered for MFA → password change required when user risk threshold is met → remediation succeeds (Yes).
   - IF user is only in the **sign-in risk policy** group → a *high user risk* status does NOT trigger a sign-in-risk policy (Reason: user risk = long-term compromise signals; sign-in risk = real-time transaction telemetry) → No.
   - IF user matches a sign-in risk policy condition but is **NOT registered for MFA** → the policy requiring MFA **blocks the sign-in** (Reason: no registered MFA methods to satisfy the challenge) → No.
4. **SC-300 Golden Rule:** *User risk ≠ Sign-in risk; and an MFA requirement on an unregistered user = blocked, not challenged.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/id-protection/howto-identity-protection-configure-risk-policies

---

### 12. Authentication Strengths & Protected Actions
1. **Exam Scenario:** Require phishing-resistant MFA for protected administrative actions (Q348).
2. **Key Decision Point:** Order of configuration — the strength must exist before the CA policy references it.
3. **Decision Path:**
   - IF requiring phishing-resistant MFA for protected actions → FIRST **configure the authentication strength** (built-in *Phishing-resistant MFA* — Windows Hello for Business, FIDO2, certificate-based auth — or a custom strength) → THEN create the **Conditional Access policy** targeting the protected action with **Require authentication strength**.
   - REJECT **access package** (Reason: Entitlement Management governs app/group access, not admin action security).
   - REJECT **authentication context** (Reason: step-up auth *within an app's* sensitive operations, not tenant-level protected actions).
   - Note: You cannot combine **Require MFA** and **Require authentication strength** grant controls in the same policy.
4. **SC-300 Golden Rule:** *Strength first, policy second — protected actions are enforced at action time, not sign-in time.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/protected-actions-overview and https://learn.microsoft.com/entra/identity/authentication/concept-authentication-strengths

---

### 13. Administrative Units: Role Scoping & Restricted Management
1. **Exam Scenario:** Password Administrators scoped to different AUs; determine who can reset whose passwords, with dynamic membership and restricted management AU settings (Q337).
2. **Key Decision Point:** Role scope (AU-scoped vs. tenant-wide) × user's AU membership × AU "restricted management" state.
3. **Decision Path:**
   - IF admin's role is scoped to AU-X AND user is a member of AU-X → admin can manage the user (e.g., reset password).
   - IF admin's role is scoped to AU-Y but user is in AU-X → **No** (scope mismatch).
   - IF admin has a **tenant-wide (unscoped)** role assignment → admin can manage all standard users, INCLUDING users in AUs where **restricted management = Disabled**.
   - IF an AU has **restricted management = Enabled** → tenant-level admins are *blocked* from managing users in that AU unless explicitly scoped to it.
4. **SC-300 Golden Rule:** *AU-scoped roles only reach their AU; tenant-wide roles reach everywhere — except into restricted-management AUs.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/administrative-units

---

### 14. Admin Consent Requests: Designated Reviewers Override Roles
1. **Exam Scenario:** Determine who can approve pending admin consent requests when specific users are configured as reviewers (Q336).
2. **Key Decision Point:** Explicit reviewer designation in the admin consent workflow vs. directory roles.
3. **Decision Path:**
   - IF a user is explicitly selected as a reviewer in **Admin consent settings** → they CAN review/approve requests, **regardless of directory roles** (even a user with no admin roles).
   - IF a user is an admin but NOT selected as a reviewer → they cannot act in this workflow via this designation.
   - Therefore: count ALL configured reviewers — the answer is the full reviewer list, not a subset filtered by role.
4. **SC-300 Golden Rule:** *Admin consent workflow reviewers are designated by name, not by role — every listed reviewer can approve.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/configure-admin-consent-workflow

---

### 15. User Consent & Admin Consent Workflow Configuration
1. **Exam Scenario:** Disable user consent to apps and enable the admin consent request workflow (Q350).
2. **Key Decision Point:** Navigation path in the Microsoft Entra admin center.
3. **Decision Path:**
   - IF disabling user consent → **Identity → Applications → Enterprise applications → Consent and permissions → User consent settings**.
   - IF enabling the admin consent workflow → **Identity → Applications → Enterprise applications → Admin consent settings** (users can then request approval from the sign-in screen).
4. **SC-300 Golden Rule:** *User consent lives under "Consent and permissions"; the admin consent request workflow lives under "Admin consent settings" — both under Enterprise applications.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/configure-user-consent

---

### 16. Defender for Cloud Apps: OAuth App Policies vs. Session Policies
1. **Exam Scenario:** (a) Monitor third-party OAuth apps' permissions; (b) a Security Administrator (read-only in MDCA) must create session policies (Q334, Q335).
2. **Key Decision Point:** Policy type selection and the Entra role required for MDCA administration.
3. **Decision Path:**
   - IF monitoring/governing third-party OAuth apps → SELECT an **OAuth app policy** in Defender for Cloud Apps (alerts based on permission level, community trust, user count).
   - REJECT **endpoint protection policy** (device security via Intune/Defender for Endpoint), **app permission policy** (Teams client app control), **access policy** (monitors/controls user *sessions*, not app permissions).
   - IF a user must *create* session policies in MDCA → assign the **Cloud App Security Administrator** role (Reason: Security Administrator gets only **read-only** MDCA access; Cloud App Security Administrator has full control).
   - REJECT **Cloud Application Administrator** (Reason: manages Entra app registrations/enterprise apps, not MDCA policies).
   - REJECT **creating a CA policy first** (Reason: needed for enforcement later, but doesn't grant MDCA permissions).
4. **SC-300 Golden Rule:** *OAuth apps → OAuth app policy; MDCA write access → Cloud App Security Administrator (Security Administrator is read-only there).*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/defender-cloud-apps/app-permission-policy and https://learn.microsoft.com/entra/identity/role-based-access-control/permissions-reference

---

### 17. Microsoft Entra Permissions Management: Onboarding Roles
1. **Exam Scenario:** Onboard an Azure subscription to Permissions Management / allow a user to onboard data sources, with least privilege (Q332, Q338).
2. **Key Decision Point:** The dual-role requirement (Entra role + Azure RBAC role) and least privilege.
3. **Decision Path:**
   - IF a user must administer Permissions Management (onboard data systems) → assign **Permissions Management Administrator** (Entra built-in role) at the tenant level.
   - IF onboarding an Azure subscription → ALSO assign **User Access Administrator** at the subscription (Sub1) scope (Reason: needed to manage role assignments for the data collector).
   - REJECT **Security Administrator** and **Application Administrator** (Reason: lack Permissions Management service permissions).
   - REJECT **Global Administrator** and **Owner** (Reason: technically sufficient but violate least privilege).
4. **SC-300 Golden Rule:** *Permissions Management onboarding = Permissions Management Administrator (tenant) + User Access Administrator (subscription) — never Global Admin when least privilege is graded.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/permissions-management/onboard-azure

---

### 18. Entra Insights & Permission Creep Index (PCI)
1. **Exam Scenario:** Review global administrators and inspect a specific user's PCI score (Q331).
2. **Key Decision Point:** Which tab of the Entra admin center provides which view.
3. **Decision Path:**
   - IF inventorying identities permanently assigned to Global Administrator → use the **Insights** tab → "Review global administrators."
   - IF inspecting a specific user's PCI score → use the **Analytics** tab (Reason: the Dashboard shows only an org-wide PCI heat map; per-user granular PCI is in Analytics).
4. **SC-300 Golden Rule:** *Insights = privileged role oversight; Analytics = per-user PCI deep dive.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/role-based-access-control/security-emergency-access (related governance docs: https://learn.microsoft.com/entra/id-governance/permissions-management)

---

### 19. Entra Diagnostic Settings: Log Destinations
1. **Exam Scenario:** Analyze Microsoft Entra sign-in logs with KQL queries (Q341).
2. **Key Decision Point:** Which diagnostic destination supports interactive analysis.
3. **Decision Path:**
   - IF the requirement is to *analyze/query* logs (KQL, workbooks, alerts) → SELECT **a Log Analytics workspace**.
   - REJECT **Azure Storage account** (Reason: archival/audit only, not interactive analysis).
   - REJECT **Azure SQL database** (Reason: not a supported Entra diagnostic destination at all).
   - REJECT **Azure Event Hub** (Reason: streaming to external SIEMs/real-time pipelines, not interactive analysis).
4. **SC-300 Golden Rule:** *Analyze = Log Analytics; archive = Storage; stream to SIEM = Event Hub; SQL = not an option.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/monitoring-access/overview-identity-monitoring

---

### 20. Microsoft Entra ID Login for Azure VMs
1. **Exam Scenario:** Determine whether users can RDP to Windows VMs using Entra credentials, given roles and device states (Q345).
2. **Key Decision Point:** Correct Azure RBAC login role + client device state/OS.
3. **Decision Path:**
   - IF user needs Entra login to a VM → must hold **Virtual Machine User Login** or **Virtual Machine Administrator Login**; any other/incorrect role name (e.g., "Virtual Machine Local User Login") → **No**.
   - IF the client device is **not** Entra joined or Entra registered to the same tenant → **No** (native Entra RDP requires joined/registered client).
   - IF the client device is not Windows 10/11 (e.g., Android) → **No** (unsupported platform for Entra-based Windows RDP credential exchange).
4. **SC-300 Golden Rule:** *Entra VM login = correct login RBAC role + Entra joined/registered Windows 10/11 client — all three or it fails.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/devices/howto-vm-sign-in-azure-ad-windows

---

### 21. Terms of Use: Per-Device Consent & Expiration
1. **Exam Scenario:** Determine when users are prompted to accept terms, given "consent on every device" and "expire consents" settings (Q346).
2. **Key Decision Point:** Per-device enforcement requires device registration; expiration date governs re-prompting.
3. **Decision Path:**
   - IF **"Require users to consent on every device" = On** → user is prompted on each new device they use (per-device terms of use).
   - IF the device is **not registered** in the tenant → per-device terms of use enforcement is constrained; however, standard terms-of-use enforcement via CA does not require device registration (user can still accept).
   - IF consent date is **before** the "Expire starting on" date → consent is still valid → **no re-prompt** (user cannot re-accept early).
   - IF the expiration date has passed → user must re-accept.
   - Note: B2B users are not supported for per-device terms of use.
4. **SC-300 Golden Rule:** *Per-device consent = prompt on every new device; expired consent = re-prompt; unexpired consent = silence.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/conditional-access/terms-of-use

---

### 22. Managed Identities: Choosing the Right Type
1. **Exam Scenario:** (a) VMs accessing Azure SQL MI with minimal admin effort; (b) Automation account whose permissions must vanish when the account is deleted (Q351, Q353).
2. **Key Decision Point:** Lifecycle coupling between the identity and the Azure resource.
3. **Decision Path:**
   - IF one identity must be shared across multiple resources (VM1 + VM2) with minimal effort → SELECT **user-assigned managed identity** (Reason: standalone identity, assignable to many resources, no credential management).
   - IF permissions must be **automatically removed when the resource is deleted** → SELECT **system-assigned managed identity** (Reason: lifecycle-tied to the resource; Azure cleans up identity and role assignments on deletion).
   - REJECT **client secret / certificate** (Reason: manual lifecycle management, not auto-removed).
   - REJECT **user-assigned identity** when auto-cleanup is required (Reason: it persists independently of the resource).
   - REJECT **SAS token** for Azure SQL MI (Reason: not a supported auth mechanism for SQL MI).
   - REJECT **Entra user account** on VMs (Reason: credential storage/rotation burden).
4. **SC-300 Golden Rule:** *Shared across resources = user-assigned; die with the resource = system-assigned; never store secrets when a managed identity exists.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview

---

### 23. Group Membership Types: What Can Be Added Manually
1. **Exam Scenario:** Determine which groups can accept a managed identity (service principal) or a manual cloud-user addition, given membership type and source (Q352).
2. **Key Decision Point:** Group type (Security vs. Microsoft 365), membership type (Assigned vs. Dynamic), and source (Cloud vs. Windows Server AD sync).
3. **Decision Path:**
   - IF adding a **service principal/managed identity** as a member → only **Security groups** with **Assigned** membership and **Cloud** source qualify.
     - REJECT Microsoft 365 groups (service principals cannot be members).
     - REJECT Dynamic groups (membership is rule-driven).
     - REJECT AD-synced groups (read-only in the cloud; manage on-premises).
   - IF manually adding a **cloud user** → any **Assigned + Cloud** group qualifies (both Security and Microsoft 365 types).
     - REJECT Dynamic groups and synced groups for the same reasons above.
4. **SC-300 Golden Rule:** *Manual edits need Assigned + Cloud; service principals need Security + Assigned + Cloud; Dynamic and synced groups are hands-off.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/groups/groups-create-rule (dynamic groups) and https://learn.microsoft.com/entra/identity/groups/groups-membership-principal

---

### 24. Self-Service Application Access
1. **Exam Scenario:** Determine which users can request access to an app via My Apps, given group membership and approver designation (Q354).
2. **Key Decision Point:** Whether the user is *already assigned* to the app (directly or via the target group).
3. **Decision Path:**
   - IF user is already a member of the app's assigned group → **cannot** request access (already assigned).
   - IF user is not assigned and not in the group → **can** request access via My Apps.
   - IF user is a designated **approver** but not assigned → they can still **request** access for themselves (approver role doesn't block requestor eligibility).
4. **SC-300 Golden Rule:** *Already assigned = no request; unassigned = request allowed — even if you're the approver.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/enterprise-apps/self-service-access

---

### 25. Deleted Application Registration: Soft-Delete Retention
1. **Exam Scenario:** An app registration deleted on January 1 — when can it last be restored? (Q355)
2. **Key Decision Point:** The 30-day soft-delete window.
3. **Decision Path:**
   - IF app registration is deleted → it is recoverable for **30 days** (soft-deleted state).
   - Deleted January 1 → last restoration day ≈ **January 31** (choose the closest option within the 30-day window, e.g., January 30).
   - REJECT any date beyond 30 days (March 30, June 30) or well inside the window (January 14).
4. **SC-300 Golden Rule:** *Deleted app registrations live 30 days in soft delete — count 30 days from deletion, not more.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity-platform/howto-restore-app

---

### 26. Bulk License Removal with Least Effort
1. **Exam Scenario:** Remove the Microsoft 365 E3 license from 2,500 users with the least administrative effort (Q356).
2. **Key Decision Point:** License-centric vs. user-centric management views.
3. **Decision Path:**
   - IF removing a specific license product from many users → SELECT the **Licenses blade in the Microsoft Entra admin center** (license-centric view → select product → bulk remove).
   - REJECT **Microsoft 365 admin center Licensing node** (Reason: user-centric; cumbersome at scale).
   - REJECT **Set-WindowsProductKey** (Reason: Windows OS activation, unrelated).
   - REJECT **Identity Governance blade** (Reason: access reviews/entitlement/PIM, not license operations).
4. **SC-300 Golden Rule:** *Bulk license ops = license-centric view in Entra admin center — pick the product, not the users.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/users/licensing-groups-assign

---

### 27. Hybrid Identity: Creating Users in a Synchronized Tenant
1. **Exam Scenario:** Create 300 users from a spreadsheet in a tenant synced with Entra Connect, ensuring access to both on-premises and cloud resources (Q349).
2. **Key Decision Point:** Source of authority (on-premises AD DS) and the correct cmdlet pair.
3. **Decision Path:**
   - IF the tenant is Entra Connect-synchronized → users MUST be created **on-premises first** (AD DS is the source of authority) → use **`New-ADUser`**.
   - IF importing structured data from a spreadsheet → save as CSV and use **`Import-Csv`**.
   - REJECT **`New-MgUser`** (Reason: creates cloud-only users that conflict with sync and can't access on-prem resources).
   - REJECT **`Set-MgUser` / `Set-ADUser`** (Reason: modify *existing* users, not create).
   - REJECT **`Get-Content`** for CSV (Reason: returns raw lines, not structured objects like `Import-Csv`).
4. **SC-300 Golden Rule:** *Synced tenant = create in AD DS: `Import-Csv` + `New-ADUser`; `New-MgUser` makes cloud-only orphans.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/identity/hybrid/connect/what-is-sync

---

### 28. Global Secure Access Management Roles
1. **Exam Scenario:** Determine which users can *manage* Microsoft Entra Internet Access (Q340).
2. **Key Decision Point:** Read-only vs. management permissions for GSA.
3. **Decision Path:**
   - IF managing Microsoft Entra Internet Access / Global Secure Access → **Global Administrator** or **Security Administrator** both qualify.
   - IF user holds **Global Reader** → read-only → **cannot** manage/configure.
4. **SC-300 Golden Rule:** *GSA management = Global Admin or Security Admin; Global Reader only looks.*
5. **Microsoft Learn Reference:** https://learn.microsoft.com/entra/global-secure-access/overview-what-is-global-secure-access

---

## Cross-Cutting Logic Rules (Quick Reference)

| Rule | Logic |
|---|---|
| **CA Exclusion Precedence** | Exclude > Include within the same policy; evaluate each policy independently. |
| **Least Privilege Tie-Breaker** | When multiple roles technically work, choose the most specific built-in role (e.g., Permissions Management Administrator > Global Administrator; Privileged Role Administrator > Global Administrator). |
| **Managed Identity Selection** | System-assigned = lifecycle-coupled; user-assigned = shared/standalone; on-premises = certificate. |
| **Group Editability** | Manual edits require Assigned membership + Cloud source; Dynamic and AD-synced groups are immutable in the cloud. |
| **Data Plane vs. Control Plane** | Reader ≠ data access; data roles (Key Vault Secrets User, Reader and Data Access) are scope-bound. |
| **GSA Connectivity** | Client (Windows/macOS/iOS/Android, joined or registered) → else remote network (IPsec); AVD multi-session unsupported. |
| **PIM Prerequisites** | Entra ID P2 required; eligibility assignment precedes activation/approval config; Entra role reviews need Privileged Role Administrator. |
| **Risk Policy Semantics** | User risk ≠ sign-in risk; MFA requirement on unregistered user = block. |
| **Diagnostic Destinations** | Analyze → Log Analytics; Archive → Storage; Stream → Event Hub; SQL → unsupported. |