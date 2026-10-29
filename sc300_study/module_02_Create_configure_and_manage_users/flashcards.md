# Module 02: Create, configure, and manage users - Flashcards

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

### User Management

**Q:** What are the three ways Microsoft Entra ID typically defines users?
**A:** Cloud identities, directory-synchronized identities, and guest users.

**Q:** What is the "Source" value of a directory-synchronized user in Microsoft Entra ID?
**A:** Windows Server AD.

**Q:** What is the "Source" value of a guest user account?
**A:** Invited user.

**Q:** What is the recommended synchronization tool for most organizations syncing on-premises AD to Microsoft Entra ID?
**A:** Microsoft Entra Cloud Sync — lightweight cloud-managed agent, supports multiple disconnected forests.

**Q:** When should you use Microsoft Entra Connect Sync instead of Microsoft Entra Cloud Sync?
**A:** For complex scenarios such as device synchronization or groups with more than 50,000 members.

**Q:** You need to switch between directories in the Microsoft Entra admin center. Which panel do you use?
**A:** The Directory + Subscription panel (or the Switch directory button in the toolbar).

**Q:** What happens to a cloud identity user when the account is removed from the primary directory?
**A:** It is deleted.

**Q:** Which admin center is used to work with user objects in Microsoft Entra ID?
**A:** The Microsoft Entra admin center.

**Q:** What does Microsoft Entra ID build after a user is authenticated to determine resource access?
**A:** An access token.

### Deleted Users

**Q:** How long does a deleted user account remain in a suspended (restorable) state?
**A:** 30 days; after that, permanent deletion starts automatically.

**Q:** Which roles can restore or permanently delete users?
**A:** Global administrator, Partner Tier-1 Support, Partner Tier-2 Support, or User administrator.

**Q:** You deleted a user 45 days ago and need to restore the account. What is the outcome?
**A:** Restoration is impossible — after the 30-day window, permanent deletion has started and a permanently deleted user cannot be restored.

**Q:** Where in the Microsoft Entra admin center do you restore a deleted user?
**A:** Identity > Users > Deleted users > select user > Restore user.

### Groups

**Q:** What are the two group types in Microsoft Entra ID?
**A:** Security groups and Microsoft 365 groups.

**Q:** Which members can a security group contain?
**A:** Users, devices, and service principals.

**Q:** What do Microsoft 365 groups provide that security groups do not?
**A:** Collaboration features — shared mailbox, calendar, files, SharePoint site — and access for people outside the organization.

**Q:** Which group type is available to end users (not just admins) to create?
**A:** Microsoft 365 groups; security groups require a Microsoft Entra administrator.

**Q:** What are the three group membership types?
**A:** Assigned, Dynamic User, and Dynamic Device.

**Q:** Can Microsoft 365 groups use dynamic device membership?
**A:** No — Dynamic Device applies to security groups only; Microsoft 365 groups support dynamic users but not dynamic devices.

**Q:** What license is required for dynamic group membership?
**A:** Microsoft Entra ID P1 (or Intune for Education for device-based rules).

**Q:** A user's department attribute changes from Sales to Marketing. What happens to dynamic membership rules?
**A:** All dynamic membership rules in the tenant are re-evaluated, and the user is added to or removed from groups accordingly.

### Device Registration

**Q:** What is the primary audience for Microsoft Entra registered devices?
**A:** Bring your own device (BYOD) and mobile device scenarios using personal devices.

**Q:** How do users sign in to a Microsoft Entra registered device?
**A:** With a local account (e.g., a Microsoft account), with a Microsoft Entra account attached for access to organizational resources.

**Q:** How do users sign in to a Microsoft Entra joined device?
**A:** With an organizational Microsoft Entra account.

**Q:** Microsoft Entra registered vs Microsoft Entra joined — which requires an organizational account to sign in to the device?
**A:** Microsoft Entra joined; registered devices only require a local credential with the Entra account attached.

**Q:** Which Windows editions are excluded from Microsoft Entra join?
**A:** Home editions.

**Q:** What are the three methods to deploy Microsoft Entra joined devices?
**A:** Out of Box Experience (OOBE), bulk enrollment, or Windows Autopilot.

**Q:** What defines a hybrid Microsoft Entra joined device?
**A:** A device joined to on-premises Active Directory and registered with Microsoft Entra ID.

**Q:** You have Win32 apps relying on AD machine authentication and want to keep using Group Policy. Which device option should you use?
**A:** Microsoft Entra hybrid joined devices.

**Q:** What replaced device writeback for hybrid identity scenarios?
**A:** Cloud Kerberos Trust — allows Entra joined and hybrid joined devices to authenticate to on-premises resources without device objects written back to AD.

**Q:** Which MDM tool is used to enforce configurations like storage encryption and password complexity on registered devices?
**A:** Microsoft Intune (Mobile Device Management).

### Licenses

**Q:** Which licenses enable group-based licensing?
**A:** Paid or trial Microsoft Entra ID Premium P1 or greater, or paid/trial Office 365 Enterprise E3 or greater.

**Q:** You have 1,000 unique members in licensed groups. How many licenses must you have?
**A:** At least 1,000 — one license for each unique member of licensed groups.

**Q:** Where is group-based licensing managed?
**A:** Only through the Microsoft 365 admin center (Billing > Licenses > Groups).

**Q:** A user is assigned the same license both directly and via a group. How many licenses are consumed?
**A:** Only one — the license is consumed once.

**Q:** What happens to users without a specified usage location during group license assignment?
**A:** They inherit the usage location of the directory.

**Q:** Which PowerShell cmdlet is used to assign licenses to a user?
**A:** Set-MgUserLicense.

**Q:** PowerShell error "CountViolation" indicates which group licensing problem?
**A:** Not enough licenses available for the product.

**Q:** PowerShell error "MutuallyExclusiveViolation" indicates which problem?
**A:** Conflicting service plans — two service plans that cannot be assigned to the same user.

**Q:** PowerShell error "DependencyViolation" indicates which problem?
**A:** Other products depend on the license — a service plan required by another service plan is being removed.

**Q:** PowerShell error "ProhibitedInUsageLocationViolation" indicates which problem?
**A:** The user's usage location isn't allowed for the service being assigned.

**Q:** Example: user has Office 365 E1 directly assigned; you add them to a group with E3. Assignment fails. Why?
**A:** Conflicting service plans — e.g., SharePoint Online Plan 2 conflicts with Plan 1, Exchange Online Plan 2 conflicts with Plan 1.

**Q:** Does Microsoft Entra ID automatically resolve license conflicts?
**A:** No — the decision about how to resolve conflicting product licenses always belongs to the administrator.

**Q:** What exception appears in audit logs when the same license is concurrently assigned to a user via multiple groups?
**A:** LicenseAssignmentAttributeConcurrencyException — Entra ID retries automatically; no customer action required.

**Q:** What must you do before deleting a licensed group?
**A:** Remove all licenses assigned to the group.

**Q:** When a licensed group is deleted and a user has a dependent license (e.g., Audio Conferencing), what happens to the group-assigned license?
**A:** It is converted from inherited to a direct license assignment.

**Q:** Microsoft Workplace Analytics requires which prerequisite service plan?
**A:** Exchange Online (Plan 1) or Exchange Online (Plan 2).

**Q:** How do you manually reprocess a group or user to resolve license errors?
**A:** Open Licenses on the group or user pane and select the Reprocess button on the toolbar.

**Q:** During migration to group-based licensing, why should you keep direct assignments until group licenses are verified?
**A:** To avoid users temporarily losing licenses; when the same license is assigned directly and via group, only one license is consumed.

**Q:** What message confirms group license processing has completed?
**A:** "Latest license changes have been applied to all users."

### Custom Security Attributes

**Q:** What are custom security attributes in Microsoft Entra ID?
**A:** Business-specific key-value pairs you can define and assign to Microsoft Entra objects for storing information, categorizing objects, or fine-grained access control.

**Q:** Which data types do custom security attributes support?
**A:** Boolean, integer, and string — with single or multiple values, free-form or predefined values.

**Q:** Which objects can custom security attributes be assigned to?
**A:** Microsoft Entra users and enterprise applications (service principals).

**Q:** Are custom security attributes supported in SAML token claims or JWT claims?
**A:** No — they are not supported in Microsoft Entra Domain Services, SAML token claims, or JWT claims.

### Automatic User Provisioning (SCIM)

**Q:** What does SCIM stand for?
**A:** System for Cross-Domain Identity Management — an open standard protocol for automating exchange of user identity information between identity domains and IT systems.

**Q:** Which SCIM version does the Microsoft Entra Provisioning Service use?
**A:** SCIM 2.0.

**Q:** What is an HCM system in the SCIM provisioning context?
**A:** A Human Capital Management system that automates HR processes throughout the employee lifecycle (the source of user data).

**Q:** What is API-driven inbound provisioning used for?
**A:** For HR systems without a SCIM endpoint — any automation tool or script can send workforce data to the Microsoft Entra provisioning API (GA March 2024).

**Q:** Name supported authoritative sources for API-driven inbound provisioning.
**A:** Workday, SAP SuccessFactors, and any custom HR system integrated via the API.
