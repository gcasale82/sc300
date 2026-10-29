# Module 17: Define a privileged access strategy for administrative users - Flashcards

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

### PIM Overview and Licensing

**Q:** What is Privileged Identity Management (PIM)?
**A:** A service in Microsoft Entra ID for managing, controlling, and monitoring access to privileged resources such as Microsoft Entra ID, Azure, Microsoft 365, and Intune.

**Q:** Which license is required to use PIM?
**A:** Microsoft Entra ID Premium P2.

**Q:** You need just-in-time privileged access with approval-based role activation. Which feature should you use?
**A:** Privileged Identity Management (PIM).

**Q:** List key features of PIM.
**A:** Just-in-time access, time-bound access with start/end dates, approval to activate roles, MFA enforcement for activation, justification on activation, notifications, access reviews, and audit history download.

### Privileged Access Strategy

**Q:** Who can bring Azure resources under PIM management during discovery?
**A:** Only owners of subscriptions and management groups.

**Q:** A Global Administrator wants to discover all Azure subscriptions for PIM but lacks access. What should they do?
**A:** Elevate access to manage all Azure subscriptions in the directory.

**Q:** What is the principle of least privilege in the context of Microsoft Entra roles?
**A:** Assign administrators the least powerful, most specific roles needed instead of Global Administrator, and remove unnecessary privileged assignments.

**Q:** Which PIM feature can automate removing users from unnecessary Microsoft Entra roles?
**A:** Access reviews in PIM (set reviewers to "Members (self)" and require reason on approval).

**Q:** What should you do if privileged accounts don't have emails linked for access reviews?
**A:** Populate the secondary email field on those accounts.

**Q:** Which two Microsoft Entra roles does Microsoft recommend managing with PIM first?
**A:** Global Administrator and Security Administrator.

**Q:** Which roles should always be protected with PIM for less-critical Azure subscriptions?
**A:** Owner and User Access Administrator roles.

**Q:** How should service accounts be treated in PIM for Azure resources?
**A:** Exactly the same as regular user accounts (PIM supports time-bound service accounts).

**Q:** When should you assign a role to a group instead of individual users?
**A:** When many users are assigned to a role, or when you want to delegate assigning the role.

**Q:** Who can manage membership of a Microsoft Entra role-assignable group?
**A:** Only the Privileged Role Administrator, the Global Administrator, and the group owners.

**Q:** When a role-assignable group is brought under PIM management, what is it called?
**A:** A privileged access group.

**Q:** In PIM, when a group member activates a role-assignable group, what is activated?
**A:** Only the user's group membership — the group itself is not activated.

**Q:** What is the difference between eligible and permanently active role assignments?
**A:** Eligible roles require an action (MFA, justification, approval) to activate; permanently active roles are always assigned with no activation needed.

**Q:** How many permanently active Global Administrator assignments does Microsoft recommend?
**A:** Zero, except for the two break-glass emergency access accounts.

**Q:** What factors should you consider when deciding eligible vs. permanent role assignment?
**A:** Frequency of elevation (one-time need vs. day-to-day job) and organization-specific cases (distant teams, high-ranking executives).

**Q:** What does Microsoft recommend for users with permanent role assignments?
**A:** Set up recurring access reviews for them.

### PIM Settings

**Q:** Who receives email notifications when an eligible user activates a role (if notification is enabled)?
**A:** Global Administrator, Privileged Role Administrator, and Security Administrator.

**Q:** What is the default set of approvers for a PIM role requiring approval?
**A:** All users who are Privileged Role Administrators (permanent or eligible).

**Q:** If a user is both eligible for a role and an approver of that role, can they approve their own activation?
**A:** No, they cannot approve themselves.

**Q:** What is the purpose of the incident ticket setting in PIM?
**A:** Requires the user to record an incident ticket number at activation, tying activations to internal incident numbers.

**Q:** Which Microsoft Entra roles are most commonly configured with approval required?
**A:** Global Administrator, User Administrator, Exchange Administrator, Security Administrator, and Password Administrator.

**Q:** What is the difference between "Permanent admin" and "Active admin" in PIM settings?
**A:** Permanent admin applies to Microsoft Entra roles (never activate); Active admin applies to Azure resources and can have an expiration time set.

**Q:** What expiration options are available for active and eligible Azure role assignments?
**A:** 15 days, 1 month, 3 months, 6 months, 1 year, or permanently active/eligible.

### Configuring PIM for Azure Resources

**Q:** Is there a limit to the number of Azure resources you can manage with PIM?
**A:** No, there is no limit, though Microsoft recommends starting with the most critical production resources.

**Q:** When you manage a management group or subscription in PIM, what else can you manage?
**A:** Its child resources.

**Q:** You add a new child Azure resource to a PIM-managed management group. How do you bring it under PIM management?
**A:** Search for it in PIM and add it.

**Q:** Which portal path do you use to discover Azure resources in PIM?
**A:** Microsoft Entra admin center > Microsoft Entra Privileged Identity Management > Azure resources > Discover resources.

### Assigning Roles in PIM

**Q:** What is the difference between Eligible and Active assignment types in PIM?
**A:** Eligible requires an action (MFA, justification, or approval) to use the role; Active grants the privileges always, with no action required.

**Q:** Where does a user go to activate an eligible Microsoft Entra role?
**A:** Privileged Identity Management > My roles > select the role > Activate.

**Q:** How often must a user perform MFA verification when activating roles in a session?
**A:** Only once per session.

**Q:** Which roles can PIM manage for Azure resources?
**A:** Built-in Azure resource roles and custom roles, including Owner, User Access Administrator, Contributor, Security Admin, and Security Manager.

**Q:** For which roles can the scope of permissions be restricted in a PIM assignment?
**A:** Certain roles can be scoped to a single administrative unit, service principal, or application.

**Q:** If multiple approvers are configured for a PIM role, how many approvals are needed?
**A:** Only one — approval completes as soon as one approver approves or denies; you cannot require approval from at least two users.

### Privileged Access Groups

**Q:** What is a privileged access group used for?
**A:** Assigning eligibility for membership or ownership of a role-assignable group, giving workload-specific administrators quick access to multiple roles with a single just-in-time request.

**Q:** How can you enforce different activation policies for employees vs. B2B partner guests?
**A:** Create two different privileged access groups with their own policies (less strict for employees, stricter with approval workflow for partners).

### PIM Audit History and Reports

**Q:** Which PIM view shows all activity associated with a privileged access group?
**A:** Resource audit (under Groups > select group > Activity > Resource audit).

**Q:** Which PIM view shows your personal role activity for a privileged access group?
**A:** My audit (under Groups > select group > Activity > My audit).

**Q:** Which role assignments are not shown in PIM audit history for privileged access groups?
**A:** Role assignments authorized by a service provider using Azure delegated resource management.

### Emergency Access Accounts

**Q:** What are emergency access (break-glass) accounts?
**A:** Highly privileged, cloud-only accounts not assigned to specific individuals, used only when normal administrative accounts cannot be used.

**Q:** How many emergency access accounts should an organization create?
**A:** Two or more.

**Q:** What type of accounts should emergency access accounts be?
**A:** Cloud-only accounts using the .onmicrosoft.com domain, not federated or synchronized from on-premises.

**Q:** What role assignment should emergency access accounts have?
**A:** Permanent Global Administrator role assignment.

**Q:** Why should at least one emergency access account be excluded from phone-based MFA and Conditional Access policies?
**A:** So a policy or unavailable MFA mechanism cannot block access during an emergency; a different authentication mechanism should be configured instead.

**Q:** Which tool can monitor sign-in logs of break-glass accounts and trigger alerts?
**A:** Azure Log Analytics (email and SMS alerts to admins on break-glass sign-ins).

**Q:** How often should emergency access accounts be validated?
**A:** At least every 90 days, and after key changes such as IT staff changes or changes to Microsoft Entra subscriptions.

**Q:** Name scenarios where an emergency access account is needed.
**A:** Federation/identity-provider outage, MFA service or device unavailability, last Global Administrator leaving the organization, and unforeseen circumstances like natural disasters.

**Q:** What should you verify about MFA registration on emergency access accounts?
**A:** That users haven't registered MFA or SSPR to any individual user's device or personal details, and that any registered device is accessible to all administrators with at least two network paths that don't share a common failure mode.
