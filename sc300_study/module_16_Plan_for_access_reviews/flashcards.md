# Module 16: Plan for access reviews - Flashcards

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

### Introduction to Access Reviews

**Q:** What is a Microsoft Entra access review?
**A:** A planned review of users' access needs, rights, and history to ensure the right people have the right access to the right resources, mitigating access risk to critical assets.

---

**Q:** Which licensing is required for Microsoft Entra access reviews?
**A:** A Microsoft Entra ID Governance or Microsoft Entra Suite subscription. Some capabilities work with a Microsoft Entra ID P2 subscription.

---

### Planning Access Reviews

**Q:** Which resource types can be targeted by access reviews?
**A:** User access to Microsoft Entra-integrated (SSO) apps; group membership (incl. Microsoft 365/Teams); access packages; Microsoft Entra roles and Azure resource roles (via PIM); custom data resources (preview).

---

**Q:** Which roles can create and manage access reviews for groups or applications?
**A:** Global Administrator, User Administrator, Identity Governance Administrator, Group owner, or Privileged Role Administrator (only for Microsoft Entra assignable groups).

---

**Q:** Who can create and manage access reviews for access packages?
**A:** Global Administrator, User Administrator, or Identity Governance Administrator.

---

**Q:** Who can create access reviews for Microsoft Entra roles vs. Azure resource roles?
**A:** Microsoft Entra roles: Global Administrator or Privileged Role Administrator. Azure resource roles: Global Administrator, User Access Administrator, or the resource owner.

---

**Q:** Can you change the reviewers after an access review has started?
**A:** No. The reviewer selection is made at creation time and can't be changed once the review is started.

---

**Q:** What are the three reviewer personas for an access review?
**A:** Resource owners (business owners of the resource), individually selected delegates, and end users who self-attest their need for continued access.

---

**Q:** What should you know about access reviews of groups synchronized from on-premises Active Directory?
**A:** They can't have an owner in Microsoft Entra ID, and reviews can't change their membership (source of authority is on-premises). Reviewers act in the on-prem group; results are available as CSV or via Microsoft Graph.

---

**Q:** Why is there no "application owner" option when selecting a reviewer for an application access review?
**A:** Applications in Microsoft Entra ID don't necessarily have an owner, so selecting the application owner as reviewer isn't possible.

---

**Q:** A script runs under a service principal and must query access review information via Microsoft Graph. Which permission does it need?
**A:** AccessReview.Read.All (application context).

---

**Q:** Where are access review activities recorded, and where should they be sent for advanced analysis?
**A:** Microsoft Entra audit logs (category "Policy"; e.g., Create/Delete access review, Approve/Deny/Reset/Apply decision). Export to Azure Log Analytics or Azure Event Hubs for advanced queries and dashboards.

---

**Q:** Which users require a Microsoft Entra ID P2 license for access reviews?
**A:** Each member or guest who is a reviewer, performs a self-review, or is a group/app owner performing a review. Global Administrators and User Administrators who set up, configure, or apply decisions don't need P2.

---

### Creating Access Reviews for Groups and Apps

**Q:** You need to create an access review in the Microsoft Entra admin center. What is the minimum role required?
**A:** Identity Governance Administrator (Global Administrator also works).

---

**Q:** What is the portal path to create a new access review?
**A:** Microsoft Entra admin center > ID Governance > Access reviews > New access review.

---

**Q:** What does the "All Microsoft 365 groups with guest users" review option do?
**A:** Creates recurring reviews of guest users across all Microsoft Teams and Microsoft 365 groups (groups can be excluded). The scope is limited to "Guest users only."

---

**Q:** Which scope options can you choose when creating a group or app access review?
**A:** "Guest users only" (Microsoft Entra B2B guest users) or "Everyone" (all user objects associated with the resource).

---

**Q:** You want to review only group members who haven't been active. What is the maximum inactivity period you can configure?
**A:** Select "Inactive users (on tenant level)" and specify days inactive — up to 730 days.

---

**Q:** What is a fallback reviewer in an access review?
**A:** A reviewer asked to complete the review when the user has no manager in the directory or the group has no owner (used with "Managers of users" or "Group owner(s)").

---

**Q:** What is the maximum duration for a monthly access review, and why?
**A:** 27 days — to avoid overlapping with the next monthly review.

---

**Q:** Which options can be set for "If reviewers don't respond"?
**A:** No change (leave access unchanged), Remove access, Approve access, or Take recommendations (the system's recommendation).

---

**Q:** What are the two options for "Action to apply on denied guest users"?
**A:** Remove the user's membership from the resource (tenant sign-in still works), or block sign-in for 30 days then remove the user from the tenant — deleted if no action is taken within 30 days.

---

**Q:** When is "Action to apply on denied guest users" not configurable, and what happens instead?
**A:** On reviews scoped beyond guest users and on reviews of all Microsoft 365 groups with guest users. The default is applied: remove the user's membership from the resource.

---

**Q:** An access review shows status "Failed." What are the likely causes?
**A:** The review couldn't progress — related to deletion of the tenant, a change in licenses, or other internal tenant changes.

---

**Q:** Which setting removes access from denied users automatically when the review completes?
**A:** Set "Auto apply results to resource" to Enable. If set to Disable, results must be applied manually.

---

### Creating Access Reviews Programmatically

**Q:** What is required to create an access review using the Microsoft Graph API?
**A:** Call the API to create an access review schedule definition. Caller must be a user with at least the Identity Governance Administrator role using an app with delegated AccessReview.ReadWrite.All, or an app with the AccessReview.ReadWrite.All application permission.

---

**Q:** Which PowerShell cmdlet creates an access review?
**A:** New-MgIdentityGovernanceAccessReviewDefinition (Microsoft Graph PowerShell cmdlets for Identity Governance module).

---

### Monitoring Access Review Findings

**Q:** A reviewer lost the notification email. Where do they find their pending access reviews?
**A:** Sign in to the My Access portal (https://myaccess.microsoft.com) and select "Access reviews" from the left menu.

---

**Q:** When is a denied user actually removed from the resource?
**A:** Not immediately — when the review period ends or when an administrator stops the review, if Auto apply is enabled.

---

**Q:** Two reviewers submit different decisions for the same user. Which decision counts?
**A:** The last submitted response is recorded.

---

**Q:** Which two methods does the system use to generate access review recommendations?
**A:** No sign-in within 30 days (last sign-in date is shown), and peer outlier — the user lacks the same access as peers, based on average distance in the reporting structure.

---

### Automating Access Review Management Tasks

**Q:** What happens when "Take recommendations" is used in an access review?
**A:** At the end of the review, the system automatically applies the recommendations for users that reviewers failed to respond to.

---

**Q:** Does the "no sign-in within 30 days" recommendation consider only interactive sign-ins?
**A:** No — it applies to both interactive and non-interactive sign-ins.

---

**Q:** Through which actions can external identities be granted access to company resources?
**A:** Added to a group, invited to Teams, assigned to an enterprise application or access package, or assigned a privileged role in Microsoft Entra ID or an Azure subscription.

---

### Configuring Recurring Access Reviews

**Q:** Which options define when a recurring access review series ends?
**A:** Never, a specific end date, or a set number of occurrences.

---

**Q:** When updating a running recurring review, what is the difference between applying changes to Current vs. Series?
**A:** Current changes only the active review instance; Series applies changes to all future recurrences (e.g., replace a reviewer who left the organization).

---

### Access Review Agent

**Q:** Where do reviewers complete access reviews with the Access Review Agent, and what does it provide?
**A:** In Microsoft Teams via natural-language conversation; it analyzes active reviews, generates approve/deny recommendations with a justification summary, and lets reviewers ask questions before deciding.

---

**Q:** Which signals does the Access Review Agent use for its recommendations?
**A:** User inactivity; user-to-group affiliation; account enabled (accountEnabled); employment status (employeeLeaveDateTime); lifecycle workflow history (mover workflow in past 30 days); decisions from previous reviews; access request history.

---

**Q:** Which user property does the Access Review Agent use to detect that a user's employment ended?
**A:** employeeLeaveDateTime.

---

**Q:** What are the license prerequisites for the Access Review Agent?
**A:** Microsoft Entra ID Governance or Microsoft Entra Suite licenses, plus onboarding to Security Copilot with at least one security compute unit (SCU).

---

**Q:** Which roles must admins have to set up and manage the Access Review Agent?
**A:** Identity Governance Administrator, Lifecycle Workflows Administrator, and Security Copilot Contributor.

---

**Q:** What must reviewers have to use the Access Review Agent?
**A:** Access to Microsoft Teams, an active access review assigned, and the Security Copilot Contributor role.

---

**Q:** Once started, can an Access Review Agent run be stopped or paused?
**A:** No — agents can't be stopped or paused once started; a run may take a few minutes. It's recommended to run the agent from the Microsoft Entra admin center.

---

**Q:** How do you enable the Access Review Agent for an existing access review?
**A:** ID Governance > Access reviews > select the review > Settings under Manage (one-time) or Series (recurring) > Advanced Settings > check "Access Review Agent (Preview)" > Save.
