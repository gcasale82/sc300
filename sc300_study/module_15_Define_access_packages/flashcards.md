# Module 15: Define access packages - Flashcards

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

### Entitlement Management Basics

**Q:** What is an access package in Microsoft Entra entitlement management?
**A:** A bundle of all the resources with the access a user needs to work on a project or task, governed with policies. An access package is always contained in a catalog.

**Q:** What is a catalog in entitlement management?
**A:** A container of related resources and access packages, used for delegation so non-administrators can create their own access packages.

**Q:** You need to let non-administrators create access packages for their own teams. Which entitlement management concept should you use?
**A:** Catalogs — catalog owners can add resources they own and delegate access package creation within their catalog.

**Q:** What is a policy in entitlement management?
**A:** A set of rules defining the access lifecycle: who can request, who approves, and how long access lasts. A policy is linked to an access package.

**Q:** What happens after an external user's access package request is approved?
**A:** The user is automatically invited into your directory (B2B) and assigned the access package.

**Q:** What happens to an external user's B2B account when their access expires and they have no other access package assignments?
**A:** It can be automatically removed from your directory.

**Q:** Name four resource types you can manage directly with access packages.
**A:** Microsoft Entra security groups, Microsoft 365 Groups and Teams, Microsoft Entra enterprise applications (SaaS or federated), and SharePoint Online sites.

**Q:** How can you provide Microsoft 365 licenses through an access package?
**A:** Include a security group in the access package and configure group-based licensing for that group.

**Q:** What are two common access-management problems entitlement management solves?
**A:** Users don't know what access they need or who approves it; users retain access longer than required for business purposes.

### Access Package Scenarios

**Q:** When are access packages most appropriate vs. other assignment mechanisms?
**A:** For time-limited access for tasks, access requiring manager/sponsor approval, departmental self-management of policies, and multi-organization B2B collaboration.

**Q:** An access package should be available to both internal employees and external partner users. How do you configure this?
**A:** Create two policies on the same access package: one for internal users, one for external users.

**Q:** In an access package, what does an assignment guarantee the user receives?
**A:** All the resource roles of that access package, typically time-limited before expiring.

**Q:** What roles can a group resource have in an access package?
**A:** Member and owner (two roles).

**Q:** What resource roles do SharePoint Online sites typically have?
**A:** Three roles, plus possibly additional custom roles.

### Catalogs

**Q:** Who becomes the owner when a new catalog is created?
**A:** The user who created the catalog becomes the first catalog owner (and can add other owners).

**Q:** Who is a catalog creator?
**A:** A collection of users authorized to create new catalogs; a non-administrator creator automatically becomes owner of the catalog they create.

**Q:** What resource types can be added to a catalog?
**A:** Groups (cloud-created Microsoft 365 or Entra security groups), applications (Entra enterprise apps), and SharePoint Online sites/site collections.

**Q:** What must be true before you can delete a catalog?
**A:** The catalog must not contain any access packages.

**Q:** Portal path to create a catalog in the Microsoft Entra admin center?
**A:** Microsoft Entra ID > ID Governance > Entitlement management > Catalogs > + New catalog.

**Q:** What does the "Enabled for external users" catalog setting do?
**A:** Allows users in selected external directories to request access packages in that catalog.

**Q:** Which roles are prerequisites for configuring entitlement management catalogs in the exercise?
**A:** Security Administrator or Conditional Access Administrator (with Microsoft Entra ID P1/P2, EMS E3, or EMS E5).

### Configuring Entitlement Management

**Q:** Which delegated role assigns the access package manager role to others?
**A:** Catalog owner.

**Q:** Which Microsoft Graph permission is needed for programmatic management of access packages?
**A:** EntitlementManagement.ReadWrite.All (delegated, via the entitlement management API).

**Q:** Which role can directly assign specific users to an access package?
**A:** Access package manager.

### Terms of Use

**Q:** What file format must terms of use documents use, and what is the recommended font size for mobile users?
**A:** PDF format; recommended font size is 24 point.

**Q:** Where do you create terms of use in the Microsoft Entra admin center?
**A:** ID Governance > Entitlement management > Terms of use > + New terms (requires Global administrator).

**Q:** What does "Require users to consent on every device" require before granting access?
**A:** Users must register each device with Microsoft Entra ID before getting access.

**Q:** You need users to reaccept terms of use on their own schedule rather than a fixed schedule. Which setting?
**A:** Duration before reacceptance required (days) — e.g., 30 days means consent expires 30 days after each user's own acceptance.

**Q:** With "Expire starting on" set to a future date and Monthly frequency, when must users first accept?
**A:** Immediately (starting today); consents then expire on the future date and reacceptance is required monthly thereafter.

**Q:** How are terms of use enforced on users?
**A:** Through a Conditional Access policy that grants access using the terms of use as a grant control.

**Q:** Which Conditional Access template choice makes terms of use appear later as a grant control?
**A:** "Create Conditional Access policy later" — the ToU then appears in the grant control list.

**Q:** Where can users review the terms of use they accepted?
**A:** https://myaccount.microsoft.com > VIEW SETTINGS AND PRIVACY > Privacy tab > Organization's notice.

**Q:** How do users see the correct language version of a terms of use document?
**A:** Multiple ToU versions can be uploaded in different languages; users see the version based on their browser preferences.

**Q:** Can you modify an existing terms of use document in place?
**A:** No — you upload a new version via Update in Language Options; some settings (like consent on every device or expirations) require creating a new terms of use.

**Q:** What happens to existing user consents when you upload a new ToU version without requiring reacceptance?
**A:** Previous consents stay current; only new users or those whose consent expires see the new version.

**Q:** Where do you view who accepted or declined terms of use, and for how long is data kept?
**A:** In ID Governance > Terms of use, select the Accepted or Declined numbers; counts and identities are stored for the life of the terms of use.

**Q:** Which terms of use template lets you select specific users, groups, and apps?
**A:** Custom policy.

### External User Lifecycle

**Q:** By default, what happens when an external user loses their last access package assignment?
**A:** They are blocked from signing in to the directory, and after 30 days their guest account is removed.

**Q:** You want an external user's guest account removed immediately when they lose their last assignment. Which setting and value?
**A:** Set "Remove external user" to Yes and "Number of days before removing external user from this directory" to 0.

**Q:** Why should you avoid blocking an external user from signing in if they may need future access?
**A:** A blocked user cannot re-request the access package or request other access in that directory.

**Q:** Which guest accounts does entitlement management remove when assignments expire?
**A:** Only accounts invited through entitlement management; guests present in the directory before receiving assignments remain.

**Q:** Which role is required to manage the lifecycle of external users in entitlement settings?
**A:** User administrator.

### Connected Organizations

**Q:** What is a connected organization?
**A:** An external Microsoft Entra directory or domain you have a relationship with, whose users can be allowed by policy to request access packages.

**Q:** What are the three ways to specify users that form a connected organization?
**A:** Users in another Microsoft Entra directory (any Microsoft cloud), users in a non-Microsoft Entra directory configured for direct federation, or users of a non-Microsoft directory whose email addresses share the same domain name.

**Q:** How many directories or domains can you add per connected organization?
**A:** Only one directory or domain per connected organization.

**Q:** What is a sponsor for a connected organization?
**A:** An internal or external user already in your directory who acts as the point of contact for the relationship.

**Q:** What state is automatically set when you create a new connected organization?
**A:** Configured.

**Q:** Which roles are prerequisites to add a connected organization?
**A:** Identity Governance administrator or User administrator.

### Reviewing Per-User Entitlements

**Q:** Which roles can view who has an assignment to an access package?
**A:** Identity Governance administrator, User administrator, Catalog owner, Access package manager, or Access package assignment manager.

**Q:** How do you identify assignments that did not have all resource roles properly provisioned?
**A:** Filter the Assignments status to "Delivering"; check delivery errors on the user's request in the Requests page.

**Q:** Which Microsoft Graph PowerShell scope is used to query access package assignments?
**A:** EntitlementManagement.Read.All (e.g., Connect-MgGraph -Scopes "EntitlementManagement.Read.All").

**Q:** How do you download a list of access package assignments for offline review?
**A:** Filter the Assignments list, then select Download to export a CSV file.

**Q:** Following zero trust, how often should you review entitlement packages?
**A:** Regularly, using the built-in review tools in the system.
