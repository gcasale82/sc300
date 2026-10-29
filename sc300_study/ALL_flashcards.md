# SC-300 - All Flashcards

# Module 01: Configure company brand - Flashcards

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

### Introduction

**Q:** What is the SC-300 module on Microsoft Entra tenant configuration covering?
**A:** Company branding, Microsoft Entra roles, delegation via administrative units, custom domains, evaluating permissions, and tenant-wide settings.

**Q:** What prerequisite experience is assumed for this module?
**A:** Prior usage experience with the Azure portal UI or the Microsoft Entra admin center.

---

### Company Branding

**Q:** Which license is required to configure custom company branding on sign-in pages?
**A:** Microsoft Entra ID Premium P1, P2, or Office 365 (for Office 365 apps).

**Q:** Where in the portal do you configure company branding?
**A:** Azure portal > Microsoft Entra ID > Manage menu > Company branding. (The menu option only appears with a premium license.)

**Q:** Which branding setting is automatically set as the default and cannot be changed?
**A:** The language.

**Q:** What are the size and file-size limits for the sign-in page background image?
**A:** Max 1920x1080 pixels and max file size 300,000 bytes; only .png or .jpg supported.

**Q:** What is the maximum length of the username hint text, and what format must it use?
**A:** Max 64 characters, must be Unicode, no links or code. (Not recommended if guests sign in.)

**Q:** What is the maximum character limit for sign-in page text and formatting?
**A:** 1,024 characters; must be Unicode.

**Q:** Where does the banner logo appear?
**A:** On the sign-in page after the user enters a username, and on the My Apps portal page.

---

### Microsoft Entra Roles

**Q:** Microsoft Entra roles vs Azure roles — what do they manage?
**A:** Azure roles control permissions to manage Azure resources; Microsoft Entra roles control permissions to manage Microsoft Entra resources.

**Q:** What is the scope of a Microsoft Entra role?
**A:** Tenant level, or it can be applied to an Administrative Unit.

**Q:** What is the scope of an Azure role?
**A:** Multiple levels: management group, subscription, resource group, or resource.

**Q:** What role does the person who signs up for the Microsoft Entra tenant become?
**A:** The first Global Administrator.

**Q:** What happens when a Global Administrator elevates access via the "Access management for Azure resources" switch in the Azure portal?
**A:** They are granted the User Access Administrator (Azure) role on all subscriptions for that tenant.

**Q:** What can the User Access Administrator role do?
**A:** Grant other users access to Azure resources (useful to regain access to a subscription).

**Q:** Does the Global Administrator have access to Azure resources by default?
**A:** No. But Global and User Administrator roles do span Microsoft Entra ID and Microsoft 365 (e.g., Exchange, SharePoint).

**Q:** Which role can reset the password for any user and all other administrators?
**A:** Global Administrator.

**Q:** What can the User Administrator role manage?
**A:** Create/manage users and groups, manage support tickets, monitor service health, change passwords for users, Helpdesk admins, and other User Administrators.

**Q:** What can the Billing Administrator role do?
**A:** Make purchases, manage subscriptions, manage support tickets, monitor service health.

**Q:** Where can you see the list of Microsoft Entra roles in the Azure portal?
**A:** On the "Roles and administrators" screen.

**Q:** List four ways to assign Microsoft Entra roles.
**A:** To a user/group via Roles and administrators; via Users/Groups > Assigned roles; via Access control (IAM) at broad scopes (subscription, resource group, management group); via PowerShell/Microsoft Graph or PIM.

**Q:** Which experience is used for all role management tasks if you have Microsoft Entra ID P2 and use PIM?
**A:** Privileged Identity Management (PIM) — note you can only assign one role at a time.

**Q:** At what scopes can a custom Microsoft Entra role be assigned?
**A:** Directory-level scope or app registration resource scope only.

**Q:** What portal path creates a custom role?
**A:** Microsoft Entra ID > Roles and administrators > New custom role (Basics > Permissions > Review + create).

**Q:** What is Microsoft's recommendation about the Global Administrator role?
**A:** Avoid using it; follow the principle of least privilege.

---

### Managing Users and Roles (Exercise)

**Q:** In the exercise, which license trial is activated to complete exercises requiring PIM?
**A:** Microsoft Entra ID Premium P2 (via Licenses > Get a free trial).

**Q:** How do you assign a role to a user through the user's profile?
**A:** Users > All Users > select user > Assigned roles > Add assignments > select role > Add.

---

### Administrative Units

**Q:** What is an administrative unit in Microsoft Entra ID?
**A:** A container for other Microsoft Entra resources — it can contain only users, groups, and devices.

**Q:** What problem do administrative units solve?
**A:** They restrict permissions in a role to a portion of the organization, enabling least-privilege delegation (e.g., a User Administrator managing only one region/department).

**Q:** Which five admin roles can manage an administrative unit?
**A:** Authentication administrator, Helpdesk administrator, License administrator, Password administrator, User administrator.

**Q:** Which on-premises Active Directory concept is comparable to administrative units?
**A:** Organizational Units (OUs).

**Q:** What are the three expected stages of administrative unit adoption in an organization?
**A:** Initial adoption, pruning, stabilization.

**Q:** Application Administrator vs Cloud Application Administrator — key difference?
**A:** Cloud Application Administrator has all Application Administrator abilities except Application Proxy settings (no on-premises permission).

**Q:** What does the Application Administrator role NOT grant?
**A:** The ability to manage Conditional Access.

**Q:** How do you selectively grant users the ability to register applications?
**A:** Set "Users can register applications" to No in User settings, then assign users to the Application Developer role.

**Q:** What happens when an Application Developer creates a new application registration?
**A:** They are automatically added as the first owner of the application.

**Q:** What can the Enterprise Application Owner role manage?
**A:** The enterprise apps they own (SSO settings, user/group assignments, adding owners) — but not Application Proxy settings or Conditional Access.

**Q:** What can the Application Registration Owner role manage?
**A:** App registrations they own, including the application manifest and adding other owners.

**Q:** What does Security Defaults enforce on privileged Microsoft Entra accounts?
**A:** Multifactor authentication (available at no extra cost to all organizations).

---

### Role Permissions

**Q:** Name one default permission difference between member users and guest users.
**A:** Members can enumerate the list of users and their contacts and register new applications; guests cannot (guests can only read their own properties and search non-hidden groups by name).

**Q:** In User settings, which user abilities can you restrict?
**A:** Registering applications, accessing the Azure portal, LinkedIn connections, and settings for external collaboration.

**Q:** Where can you view the full list of permissions granted by a role?
**A:** Microsoft Entra ID > Roles and administrators > select role > description page via the ellipsis (...) menu.

**Q:** Which role grants non-admins read access to basic directory information via the admin portal?
**A:** Directory Readers.

**Q:** Does restricting access to the Microsoft Entra administration portal block PowerShell or Visual Studio access?
**A:** No — it only restricts the administration portal, not PowerShell or other clients.

---

### Custom Domains

**Q:** Who can manage domains in Microsoft Entra ID?
**A:** Only a Global Administrator.

**Q:** What is the initial domain name when a tenant is created, and what is its role?
**A:** E.g., contoso.onmicrosoft.com — it is also the primary domain name (default for new users).

**Q:** What is the limit on managed domain names per organization? And if all domains are federated?
**A:** Up to 900 managed domain names; up to 450 domain names if configuring all for federation with on-premises AD.

**Q:** What must you do before adding a subdomain like europe.contoso.com?
**A:** Add and verify the root domain (contoso.com) first; the subdomain is then automatically verified.

**Q:** Can a subdomain of a domain owned by another tenant be verified in a different tenant?
**A:** Yes — you'll be prompted to add a TXT record at the DNS hosting provider.

**Q:** Name three conditions that prevent deleting a custom domain name.
**A:** A user has a username/email/proxy address with the domain; a group has an email/proxy address with it; an app has an app ID URI that includes it.

**Q:** What does ForceDelete do to references to the custom domain?
**A:** Renames UPN, EmailAddress, ProxyAddress (users), group EmailAddress, and application identifierUris to the initial default domain (e.g., user@contoso.onmicrosoft.com) via an asynchronous operation.

**Q:** What are the requirements/limits for using ForceDelete?
**A:** Fewer than 1000 references to the domain; Exchange-provisioned references (mail-enabled security groups, distribution lists) must be updated/removed in the Exchange Admin Center first.

**Q:** When does ForceDelete return an error?
**A:** If the number of objects to rename is greater than 1000, or if one of the applications is a multitenant app. (Also fails if the domain was purchased via Microsoft 365 domain subscription services or if you're a partner administering on behalf of another customer.)

**Q:** Does changing the DNS registrar for a custom domain require configuration changes in Microsoft Entra ID?
**A:** No additional configuration tasks; the domain continues to work without interruption.

---

### Tenant-Wide Settings

**Q:** Where is the tenant display name configured?
**A:** Microsoft Entra admin center > Identity > Overview > Properties > Name.

**Q:** Which tenant property cannot be changed after the tenant is created?
**A:** Country or region.

**Q:** Who does Microsoft contact if there's a data breach and no Global privacy contact is listed?
**A:** The administrator who owns the tenant.

**Q:** What four protections are provided by Security Defaults?
**A:** MFA registration for all users, MFA for administrators, blocking legacy authentication protocols, and MFA when necessary / protecting privileged activities like Azure portal access.

**Q:** Which Conditional Access policy equivalent of Security Defaults requires Microsoft Entra ID Premium P2?
**A:** Require MFA registration (requires Identity Protection, part of P2).

**Q:** Which external collaboration setting controls who can invite guests?
**A:** Guest invite settings (from guests themselves to only admins).


---

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


---

# Module 03: Describe guest access and Business to Business accounts - Flashcards

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

### B2B collaboration and guest access

**Q:** What is Microsoft Entra B2B collaboration?
**A:** A feature of Microsoft Entra External Identities that lets you invite external users (guests) to access your apps and resources with their own credentials, while you keep control of corporate data.

**Q:** How can you identify a B2B collaboration user object in the directory?
**A:** UserType is typically "Guest" and the user principal name contains the #EXT# extension.

### External collaboration settings

**Q:** Who can invite guest users by default?
**A:** All users in the tenant, including guests, can invite guest users — even without an admin role.

**Q:** Which role can be assigned to delegate guest invitations to non-admin users?
**A:** The Guest Inviter role.

**Q:** What can guest users see in the directory by default?
**A:** They cannot list users, groups, or other directory resources, but they can see membership of non-hidden groups.

**Q:** Which guest user access setting is the most restrictive?
**A:** "Guest user access is restricted to properties and memberships of their own directory objects" — guests can only view their own profile.

**Q:** What is the portal path to configure external collaboration settings?
**A:** Microsoft Entra admin center > Identity > External Identities > External collaboration settings.

**Q:** Can you create both an allowlist and a blocklist for collaboration restrictions?
**A:** No — you can create only one type of list, and only one policy per organization.

**Q:** What is the maximum size of the collaboration restrictions policy?
**A:** 25 KB (25,000 characters), including the allow/blocklist and any other configured parameters.

### Inviting external users

**Q:** Do B2B invitations expire?
**A:** No — an invitation of a user does not expire.

**Q:** What must a guest user do after being invited to access resources?
**A:** Redeem the invitation, via the redemption URL in the invitation email or a direct link to the shared app.

**Q:** How do you invite many guest users at once?
**A:** Identity > Users > All users > Bulk operations > Bulk invite; download the CSV template, fill it in, upload, then verify.

**Q:** Which rows of the bulk invite CSV template must never be removed or modified?
**A:** The first two rows: the version number row and the column headings row.

**Q:** What must you do with the examples row in the bulk CSV template?
**A:** Remove it and replace it with your own entries.

**Q:** Which email addresses are not supported for guest invitations?
**A:** Group email addresses; plus (+) addressing is also not supported — omit the + and any text after it up to the @.

**Q:** What are the required values in the bulk invite CSV?
**A:** Email address to invite [inviteeEmail] and the redirection URL the user is forwarded to after accepting the invitation.

### Managing external user accounts

**Q:** What does the UserType property indicate?
**A:** The user's relationship to the host tenant: Member (employee) or Guest (external collaborator). It has no relation to sign-in method or directory role.

**Q:** Can a B2B collaboration user have UserType = Member?
**A:** Yes — change it in the Microsoft Entra user properties, e.g., when the partner belongs to a larger parent organization.

**Q:** What does an Identities value of "mail" mean for a guest user?
**A:** The user signed up using Microsoft Entra Email one-time passcode (OTP).

**Q:** What does an Identities value of google.com or facebook.com mean?
**A:** The user signed up via self-service with a Gmail or Facebook account.

**Q:** What is recommended for granting B2B/guest users privileged roles?
**A:** Follow least privilege and use Privileged Identity Management (PIM) to grant access.

### Microsoft 365 external collaboration

**Q:** Which Microsoft 365 external collaboration activity is disabled by default?
**A:** Shared channel in Teams (it uses an existing Microsoft 365 external account).

**Q:** Which Microsoft 365 external collaboration settings are enabled by default?
**A:** Authenticated file/folder sharing, site sharing, team sharing, external chat and meetings, anonymous meeting join, and unauthenticated file/folder sharing.

### Dynamic groups

**Q:** Which license is required to create and use dynamic groups?
**A:** Microsoft Entra ID Premium P1 or P2.

**Q:** Which dynamic membership rule includes all users, both members and guests?
**A:** user.objectId -ne null (Membership type: Dynamic User).

### Microsoft Entra Verified ID

**Q:** What is Microsoft Entra Verified ID?
**A:** A decentralized identity service to issue and verify verifiable credentials; verifiers get a free REST API to request and accept credentials in apps.

**Q:** What are the prerequisites to deploy Microsoft Entra Verified ID?
**A:** An Azure tenant with a subscription, a Microsoft Entra ID premium license, signed in as global administrator, and a configured Azure Key Vault.

**Q:** What is the purpose of the Domain setting in Verified ID setup?
**A:** It binds your DID to your domain; wallets use it to validate the DID and display a verified symbol.

### Identity providers

**Q:** What is the current name for direct federation?
**A:** SAML/WS-Fed identity provider (IdP) federation.

**Q:** Which protocols must a partner IdP support for SAML/WS-Fed IdP federation?
**A:** SAML 2.0 or WS-Federation (WS-Fed).

**Q:** Does setting up SAML/WS-Fed federation change authentication for guests who already redeemed invitations?
**A:** No — they keep their previous authentication method; only new guest users use the federated IdP.

**Q:** What happens to guest users if you delete a SAML/WS-Fed federation?
**A:** They can no longer sign in; you can update their authentication method by resetting their redemption status.

**Q:** Which NameID format is required in the SAML 2.0 token?
**A:** urn:oasis:names:tc:SAML:2.0:nameid-format:persistent

**Q:** Which WS-Fed identity providers have been tested for compatibility with Microsoft Entra ID?
**A:** AD FS and Shibboleth.

**Q:** Google federation is designed for which users, and what should you use for G Suite domains?
**A:** Gmail users (gmail.com/googlemail.com); for G Suite domains use SAML/WS-Fed (direct) federation.

**Q:** A Google guest user gets a "header too long" error. What is the fix?
**A:** Clear cookies or open a private/incognito window and try to sign in again.

**Q:** Can users redeem invitations with Facebook accounts?
**A:** No — Facebook works only for self-service sign-up user flows, not for invitation redemption.

**Q:** What must be enabled before adding Facebook as an identity provider?
**A:** Self-service sign-up for the tenant; then set up a user flow and select Facebook as a sign-in option.

**Q:** What happens if you delete Google federation?
**A:** Google guests who already redeemed their invitation cannot sign in; restore access by deleting them from the directory and reinviting them.

### Cross-tenant access controls

**Q:** What are the default cross-tenant access settings?
**A:** B2B collaboration with other Microsoft Entra organizations is enabled; B2B direct connect is blocked.

**Q:** Inbound vs outbound access settings — what does each control?
**A:** Inbound controls external users accessing your resources; outbound controls your users accessing external organizations' resources.

**Q:** What do inbound trust settings allow?
**A:** Your Conditional Access policies can trust the external organization's MFA, compliant device, or hybrid Microsoft Entra joined device.

**Q:** What is B2B direct connect and where does it work today?
**A:** A mutual trust between two Microsoft Entra organizations for seamless SSO; currently works with Teams shared channels.

**Q:** What is required to establish B2B direct connect?
**A:** Both organizations must mutually enable B2B direct connect in their cross-tenant access settings.

**Q:** Which settings connect your tenant to Microsoft Azure Government or Microsoft Azure China?
**A:** Microsoft cloud settings in cross-tenant access settings.


---

# Module 04: Plan, design, and implement Microsoft Entra Connect - Flashcards

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

### Hybrid Identity and Microsoft Entra Connect

**Q:** What is hybrid identity?
**A:** An identity solution spanning on-premises and cloud environments, providing unified authentication and authorization to resources regardless of their location.

**Q:** What is Microsoft Entra Connect and what are its five capabilities?
**A:** The Microsoft tool that bridges on-premises AD with Microsoft Entra ID. Capabilities: synchronization, password hash synchronization (PHS), pass-through authentication (PTA), federation integration, and health monitoring.

### Choosing an Authentication Method

**Q:** How many PTA agents are recommended for high availability?
**A:** Three lightweight agents: the first on the Microsoft Entra Connect server plus two extra agents, so authentication continues if one agent is down for maintenance.

**Q:** You need to deploy PTA agents. Can you place them in a perimeter network (DMZ)?
**A:** No. Agents must access on-premises AD DS including domain controllers and need outbound internet access, so perimeter network deployment is not supported.

**Q:** PTA agents cannot validate credentials due to a major on-premises failure. What is the fallback and how is it activated?
**A:** Password hash synchronization can serve as a backup authentication method. Failover is not automatic — you must manually switch the sign-in method using Microsoft Entra Connect.

**Q:** Which sign-in scenarios require federated authentication instead of cloud authentication?
**A:** Requirements Entra ID doesn't support natively: smartcard/certificate authentication, on-premises or third-party MFA servers, third-party authentication solutions, or sign-in with sAMAccountName (DOMAIN\username).

**Q:** Why should you enable PHS even when using PTA or federation?
**A:** Three reasons: high availability/disaster recovery (cloud service scales globally), on-premises outage survival, and Identity Protection leaked-credentials detection (requires Entra ID P2).

### Microsoft Entra Connect Design

**Q:** What is sourceAnchor (also called immutableId)?
**A:** An attribute immutable during the lifetime of an object that uniquely identifies it as the same object on-premises and in Microsoft Entra ID; used for hard match and federation claims.

**Q:** What are the length rules for a sourceAnchor value?
**A:** Fewer than 60 characters; characters other than a-z, A-Z, or 0-9 are encoded and counted as three characters each.

**Q:** Which sourceAnchor attribute should you use in a single-forest environment?
**A:** objectGUID — also the attribute used by Express settings and DirSync.

**Q:** A user's UPN suffix (contoso.com) is not a verified domain in Microsoft Entra ID. What happens during sync?
**A:** Microsoft Entra Connect replaces the UPN suffix with the default tenant domain, e.g., contoso.onmicrosoft.com.

**Q:** What is a Microsoft Entra Connect server in staging mode?
**A:** A second server that reads data from all connected directories but writes nothing to them; it runs the normal sync cycle and serves as an updated standby.

**Q:** What is the maximum group size supported by Microsoft Entra Connect cloud sync, and which filter is recommended for large groups?
**A:** Groups with up to 50,000 members; use only the OU scoping filter when synchronizing large groups.

### Password Hash Synchronization (PHS)

**Q:** What exactly does PHS synchronize from on-premises AD?
**A:** A hash of the hash of the user's password — Entra Connect extracts the AD password hash and applies extra security processing before synchronizing it to Microsoft Entra ID.

**Q:** How often does the PHS process run, and can you change this?
**A:** Every 2 minutes; the frequency cannot be modified. Passwords sync more frequently than the standard directory synchronization window.

**Q:** Which Microsoft Entra Connect installation option enables PHS automatically?
**A:** Express Settings. With custom settings, PHS is available on the User Sign-In page.

**Q:** PHS fails on a FIPS-locked-down server. What is the cause and fix?
**A:** FIPS disables MD5. Fix: in miiserver.exe.config (under %programfiles%\Azure AD Sync\Bin), add `<enforceFIPSPolicy enabled="false"/>` under configuration/runtime.

### Pass-Through Authentication (PTA)

**Q:** How does PTA validate passwords?
**A:** A software agent on one or more on-premises servers validates passwords directly against on-premises Active Directory — password validation does not happen in the cloud.

**Q:** How do you enable pass-through authentication?
**A:** Through Microsoft Entra Connect: choose PTA on the User Sign-In page during custom installation, or use the "Change user sign-in" task on an existing installation.

**Q:** Is PTA a per-domain or tenant-level feature?
**A:** Tenant-level — enabling it affects sign-in for users across all managed domains in the tenant.

**Q:** When switching from AD FS to PTA, how long should you wait before shutting down AD FS, and why?
**A:** At least 12 hours, to ensure users can keep signing in to Exchange ActiveSync during the transition.

### Seamless Single Sign-On (SSO)

**Q:** Which sign-in methods can seamless SSO be combined with?
**A:** Password hash synchronization or pass-through authentication. Seamless SSO is not applicable to AD FS.

**Q:** In the seamless SSO browser flow, which account does the browser request a Kerberos ticket for?
**A:** The AZUREADSSOACC computer account, which represents Microsoft Entra ID; Entra ID decrypts the ticket using a previously shared key.

**Q:** How can seamless SSO be rolled out, and does it require extra on-premises components?
**A:** Rolled out to some or all users using Group Policy; no additional on-premises components are needed.

### Federation with AD FS

**Q:** What is federation?
**A:** A collection of domains that have established trust — typically including authentication and almost always authorization — for shared access to a set of resources.

**Q:** What is the minimum AD FS version supported for federation with Microsoft Entra Connect?
**A:** Windows Server 2012 R2 or later (new or existing farm).

**Q:** Where is the Web Application Proxy server deployed in an AD FS federation, and what is its role?
**A:** In the perimeter network facing the extranet; it supports authentication requests coming from the extranet.

**Q:** Which two service account types can the AD FS service use?
**A:** A group managed service account or a domain user account.

**Q:** What is device writeback used for?
**A:** Enabling device-based conditional access for AD FS-protected devices by synchronizing devices registered in Entra ID back to on-premises AD (e.g., creating the CN=RegisteredDevices container).

### Troubleshooting Synchronization Errors

**Q:** Where do you view sync error reports, and how often are they refreshed?
**A:** In the Azure portal as part of Microsoft Entra Connect Health for sync; reports update every 30 minutes with errors from the latest synchronization attempt.

**Q:** Hard match vs soft match in Microsoft Entra ID?
**A:** Hard match: incoming object's sourceAnchor matches a cloud object's immutableId. Soft match: fallback using ProxyAddresses and UserPrincipalName when no hard match is found (target must have no immutableId).

**Q:** What causes the InvalidSoftMatch sync error?
**A:** The hard match fails and the soft match finds an object whose immutableId differs from the incoming object's sourceAnchor — typically two objects sharing a ProxyAddresses or UPN value.

**Q:** What causes the ObjectTypeMismatch sync error?
**A:** Two objects of different object types (e.g., a user and a mail-enabled security group) share the same ProxyAddresses value, which Microsoft Entra ID does not allow.

**Q:** What causes the AttributeValueMustBeUnique sync error?
**A:** Entra Connect tries to add or update an object with a ProxyAddresses or UserPrincipalName value already assigned to another object in Microsoft Entra ID.

**Q:** Which attributes typically trigger the LargeObject error, and what is the certificate hard limit?
**A:** userCertificate, userSMIMECertificate, thumbnailPhoto, and proxyAddresses; the hard limit is 15 certificates.

**Q:** What causes the FederatedDomainChangeError sync error?
**A:** A synchronized user's UPN suffix is changed from one federated domain to another federated domain (e.g., bob@contoso.com to bob@fabrikam.com), so the UPN doesn't update.

**Q:** An on-premises user cannot soft-match a cloud object that has an admin role assigned. How do you fix it?
**A:** Remove the cloud account from all admin roles, hard-delete the quarantined object, let the next sync soft-match the users, then restore the role memberships.

### Microsoft Entra Connect Health

**Q:** Which license is required to use Microsoft Entra Connect Health?
**A:** Microsoft Entra ID Premium P1.

**Q:** Which role is required to register the Microsoft Entra Connect Health agent?
**A:** Global administrator in Microsoft Entra ID — by default, the global admin account has permissions to register the agent.

**Q:** Which firewall ports must the Health agent be able to use?
**A:** TCP 443 and TCP 5671. The latest agent version requires only port 443.

**Q:** Which Health agent is installed automatically?
**A:** The Microsoft Entra Connect Health agent for Sync — installed automatically with the latest version of Microsoft Entra Connect.

**Q:** Where should you NOT install the Health agent for AD FS?
**A:** On the Microsoft Entra Connect (Sync) server — the AD FS server must be a different machine with a unique host name.

### Managing Microsoft Entra Connect Health

**Q:** What happens when you delete a server from the Microsoft Entra Connect Health service?
**A:** Data collection and monitoring stop for that server; the agent is not uninstalled and collected data isn't deleted (Azure retention policy applies). To re-monitor, uninstall and reinstall the agent.

**Q:** Which built-in Azure RBAC roles does Microsoft Entra Connect Health support?
**A:** Owner (manage access, view all information, change settings), Contributor (view all information, change settings), Reader (view all information only). Other roles have no impact.

**Q:** Which RBAC role is required to use the Diagnose and Remediate feature for duplicated attribute sync errors?
**A:** Contributor permission from Azure RBAC.


---

# Module 04: Plan, design, and implement Microsoft Entra Connect - Flashcards (part 1)

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

### Introduction to Hybrid Identity

**Q:** What is hybrid identity?
**A:** An identity solution spanning on-premises and cloud environments, providing unified authentication and authorization to resources regardless of location.

**Q:** You need to implement a hybrid identity solution. Which two tools are central to this?
**A:** Microsoft Entra ID and Microsoft Entra Connect.

**Q:** Which synchronization-related tasks are covered in the hybrid identity module?
**A:** Plan/design/implement Microsoft Entra Connect, manage PHS and PTA, seamless SSO, federation (excluding manual AD FS deployments), troubleshoot sync errors, and implement Microsoft Entra Connect Health.

### Microsoft Entra Connect: Planning and Design

**Q:** What are the five capabilities of Microsoft Entra Connect?
**A:** Synchronization, password hash synchronization (PHS), pass-through authentication (PTA), federation integration, and health monitoring.

**Q:** Which two sign-in methods fall under "cloud authentication" in a hybrid identity design?
**A:** Password hash synchronization (PHS) and pass-through authentication (PTA).

**Q:** How often does the password hash synchronization process run as part of the Microsoft Entra Connect sync process?
**A:** Every 2 minutes (frequency cannot be modified).

**Q:** You need to enforce on-premises account states, password policies, and sign-in hours immediately at sign-in time. Which authentication method should you use?
**A:** Pass-through authentication (PTA).

**Q:** How many PTA agents are recommended for high availability?
**A:** Three agents (one on the Microsoft Entra Connect server plus two extra).

**Q:** Can PTA agents be deployed in a perimeter network?
**A:** No — agents need access to on-premises domain controllers and outbound internet access, so perimeter network deployment is not supported.

**Q:** What is the backup authentication method for PTA when agents can't validate credentials, and is failover automatic?
**A:** Password hash synchronization; failover is NOT automatic — you must manually switch the sign-in method using Microsoft Entra Connect.

**Q:** Which authentication method is required for smartcard authentication, third-party MFA servers, or sign-in using sAMAccountName (DOMAIN\username)?
**A:** Federated authentication (e.g., AD FS).

**Q:** Why should you enable password hash synchronization regardless of the chosen authentication method?
**A:** For high availability/disaster recovery, on-premises outage survival, and Identity Protection (leaked credentials detection requires Entra ID Premium P2).

**Q:** What is the recommended way to protect against Microsoft Entra Connect server failure?
**A:** Deploy a second Microsoft Entra Connect server in staging mode as a standby.

**Q:** What does a Microsoft Entra Connect server in staging mode do?
**A:** It reads data from all connected directories and runs the normal sync cycle, but writes nothing to connected directories.

**Q:** What is the sourceAnchor attribute also called, and what is its definition?
**A:** immutableId — an attribute immutable during the lifetime of an object that uniquely identifies an object as the same on-premises and in Microsoft Entra ID.

**Q:** Which attribute should be used as sourceAnchor for a single forest, and which is used with express settings?
**A:** objectGuid.

**Q:** What are the length and character rules for the sourceAnchor value?
**A:** Fewer than 60 characters; special characters are not allowed; must be globally unique; string, integer, or binary; not case-sensitive; shouldn't be based on the user's name.

**Q:** Which attribute does Microsoft Entra ID use to authenticate users, and what must its suffix match?
**A:** userPrincipalName (UPN); the suffix must match a verified custom domain in Microsoft Entra ID.

**Q:** The UPN suffix contoso.com is not a verified domain in Microsoft Entra ID. What happens when users sync?
**A:** The tool replaces the UPN suffix with the default domain, e.g., contoso.onmicrosoft.com.

**Q:** You run a non-routable domain like contoso.local. Which installation type does Microsoft Entra Connect suggest, and why?
**A:** Custom settings — so you can specify the attribute to use as UPN for sign-in to Azure.

**Q:** What is the most common supported topology for Microsoft Entra Connect?
**A:** Single on-premises forest (one or multiple domains) with a single Microsoft Entra tenant — the only topology supported by express installation.

**Q:** How many Microsoft Entra Connect sync servers are needed per Microsoft Entra tenant?
**A:** One — there is a 1:1 relationship between a sync server and a tenant.

**Q:** What are the four staging areas/components Microsoft Entra Connect uses for synchronization?
**A:** Connector Space (CS), Metaverse (MV), sync rules, and run profiles.

**Q:** How many Metaverses (MV) exist in a Microsoft Entra Connect deployment?
**A:** Only one.

**Q:** What is Microsoft Entra Connect cloud sync, and what agent does it use?
**A:** A synchronization solution for users, groups, and contacts to Microsoft Entra ID using the cloud provisioning agent, with sync configuration managed in the cloud.

**Q:** What is the maximum group member size supported by Microsoft Entra Connect cloud sync?
**A:** 50,000 members (use only the OU scoping filter for large groups).

**Q:** Which scenario is cloud sync designed for regarding disconnected forests?
**A:** Synchronizing to a Microsoft Entra tenant from multi-forest disconnected AD environments, e.g., mergers and acquisitions.

### Password Hash Synchronization (PHS)

**Q:** What exactly does Microsoft Entra Connect synchronize for PHS?
**A:** A hash of the hash of the user's on-premises AD password to Microsoft Entra ID.

**Q:** Can you revert a password hash to plain text?
**A:** No — a hash is the result of a one-way mathematical function and cannot be reverted.

**Q:** During the first PHS enablement, can you define a subset of users to synchronize?
**A:** No — the initial sync covers all in-scope users; selective password hash sync can be set up only after initial synchronization.

**Q:** Which installation option automatically enables PHS?
**A:** Express Settings during Microsoft Entra Connect installation.

**Q:** On a FIPS-locked-down server, how do you enable MD5 for PHS?
**A:** Edit miiserver.exe.config in %programfiles%\Azure AD Sync\Bin and add `<enforceFIPSPolicy enabled="false"/>` under the runtime node.

**Q:** Which federation product other than AD FS can be configured with Microsoft Entra Connect, and what is its minimum version?
**A:** PingFederate, version 8.4 or later.

### Pass-Through Authentication (PTA)

**Q:** How does PTA validate user passwords?
**A:** A software agent on on-premises servers validates passwords directly against on-premises Active Directory — validation doesn't happen in the cloud.

**Q:** Through which tool do you enable pass-through authentication?
**A:** Microsoft Entra Connect (custom installation path, or the "Change user sign-in" task on an existing install).

**Q:** Is PTA a tenant-level or per-user feature?
**A:** Tenant-level — enabling it affects sign-in for users across all managed domains in the tenant.

**Q:** When switching from AD FS to PTA, how long should you wait before shutting down the AD FS infrastructure?
**A:** At least 12 hours, to ensure users can keep signing in to Exchange ActiveSync during the transition.

### Seamless Single Sign-On (Seamless SSO)

**Q:** Which sign-in methods can be combined with seamless SSO, and which cannot?
**A:** Works with PHS and PTA; NOT applicable to Active Directory Federation Services (ADFS).

**Q:** Which computer account in Active Directory represents Microsoft Entra ID for seamless SSO Kerberos tickets?
**A:** AZUREADSSOACC.

**Q:** In browser-based seamless SSO, what HTTP response does Microsoft Entra ID use to challenge the browser for a Kerberos ticket?
**A:** A 401 Unauthorized response, via JavaScript in the background.

**Q:** In seamless SSO, what does Microsoft Entra ID use to decrypt the Kerberos ticket?
**A:** The previously shared key (the AZUREADSSOACC computer account's secret).

**Q:** What token does Microsoft Entra ID issue to a native client app after validating the Kerberos ticket in seamless SSO?
**A:** A SAML token, which the app submits to the Microsoft Entra ID OAuth2 token endpoint to receive access, refresh, and ID tokens.

### Federation

**Q:** What is the minimum Windows Server version for an AD FS farm used with Microsoft Entra Connect federation?
**A:** Windows Server 2012 R2 or later.

**Q:** Where is the Web Application Proxy (WAP) server deployed in a federation topology?
**A:** In the perimeter network, facing the extranet, to support authentication requests from the extranet.

**Q:** Do AD FS servers need to be domain-joined before federation setup? What about WAP servers?
**A:** AD FS servers must be joined to Active Directory; domain join is not required for Web Application Proxy servers.

**Q:** Which two types of service accounts can the AD FS service use?
**A:** Group managed service account or domain user account.

**Q:** How many domains can you federate during the initial Microsoft Entra Connect installation?
**A:** Only a single domain; more domains can be added later by running Microsoft Entra Connect again.

**Q:** What is AlternateID sign-in used for in federation?
**A:** Allowing users to sign in with an attribute other than UPN (e.g., mail) when the on-premises UPN uses a non-routable domain or can't be changed.

**Q:** What is device writeback used for?
**A:** To enable device-based conditional access for AD FS-protected devices by synchronizing devices registered in Azure back to on-premises Active Directory.

### Troubleshooting Synchronization Errors

**Q:** Where is the Synchronization Errors report available?
**A:** In the Azure portal as part of Microsoft Entra Connect Health for sync.

**Q:** What are the three operation types Microsoft Entra Connect performs on directories?
**A:** Import, Synchronization, and Export.

**Q:** What is a "Hard Match" in Microsoft Entra ID?
**A:** Matching an incoming object's sourceAnchor attribute to the immutableId attribute of objects in Microsoft Entra ID.

**Q:** What is a "Soft Match" and which attributes does it use?
**A:** A fallback match when hard match fails, using ProxyAddresses and UserPrincipalName attributes to match objects already present in Microsoft Entra ID.

**Q:** What causes an InvalidSoftMatch error?
**A:** Hard match finds no object, but soft match finds an object that has a different immutableId value than the incoming object's sourceAnchor.

**Q:** Which attributes must have unique values in the Microsoft Entra directory schema?
**A:** ProxyAddresses, UserPrincipalName, onPremisesSecurityIdentifier, and ObjectId.

**Q:** What does the Microsoft Entra Attribute Duplicate Attribute Resiliency feature do?
**A:** It reduces sync errors by handling duplicated ProxyAddresses and UserPrincipalName more resiliently, allowing new objects to be provisioned — but it doesn't fix the duplicate data itself.

**Q:** What causes an ObjectTypeMismatch error?
**A:** Two objects of different types (User, Group, Contact) have the same value for attributes used in soft match, e.g., the same ProxyAddresses value.

**Q:** What causes an AttributeValueMustBeUnique error?
**A:** Microsoft Entra Connect tries to add/update an object with a ProxyAddresses or UserPrincipalName value already assigned to another object in Microsoft Entra ID.

**Q:** What causes an IdentityDataValidationFailed error?
**A:** The UserPrincipalName attribute contains invalid/unsupported characters or doesn't follow the required format.

**Q:** What causes a FederatedDomainChangeError?
**A:** A synchronized user's UPN suffix is changed from one federated domain to another federated domain (e.g., bob@contoso.com to bob@fabrikam.com).

**Q:** Which attributes typically trigger a LargeObject (ExceededAllowedLength) sync error?
**A:** userCertificate, userSMIMECertificate, thumbnailPhoto, and proxyAddresses.

**Q:** What is the hard limit for certificates in the userCertificate and userSMIMECertificate attributes?
**A:** 15 certificates.

**Q:** What causes an "Existing Admin Role Conflict" sync error?
**A:** An on-premises user object with administrative permissions and the same UPN as an existing Microsoft Entra object — soft-matching is not allowed with cloud objects holding admin roles.

**Q:** How do you fix an Existing Admin Role Conflict?
**A:** Remove the cloud account from all admin roles, hard delete the quarantined cloud object, let the next sync cycle soft-match, then restore the role memberships.

**Q:** How often are sync error reports in Microsoft Entra Connect Health for sync updated?
**A:** Every 30 minutes, including errors from the latest synchronization attempt.

### Microsoft Entra Connect Health

**Q:** What does Microsoft Entra Connect Health provide?
**A:** Monitoring of on-premises identity infrastructure — alerts, performance monitoring, and usage analytics via the Microsoft Entra Connect Health portal.

**Q:** Which license is required to use Microsoft Entra Connect Health?
**A:** Microsoft Entra ID Premium P1.

**Q:** Which administrative role is required to install and configure the Microsoft Entra Connect Health agent?
**A:** Global administrator in Microsoft Entra ID.

**Q:** Which firewall ports must be open for the Microsoft Entra Connect Health agent?
**A:** TCP 443 and TCP 5671 — the latest agent version requires only port 443.

**Q:** Which PowerShell version is required for the Health agent, and what must be disabled (FIPS)?
**A:** PowerShell 4.0 or newer; FIPS (Federal Information Processing Standard) must be disabled.

**Q:** Should the AD FS Health agent be installed on the Sync server?
**A:** No — the AD FS server should be different from the Sync server; don't install the AD FS agent on the Sync server.

**Q:** How is the Microsoft Entra Connect Health agent for Sync installed?
**A:** It is installed automatically with the latest version of Microsoft Entra Connect.

**Q:** Which services verify the Health agent installation for AD FS and for Sync?
**A:** AD FS: Diagnostics, Insights, and Monitoring Services. Sync: Sync Insights Service and Sync Monitoring Service.


---

# Module 04: Plan, design, and implement Microsoft Entra Connect - Flashcards (part 2)

<details><summary>Source lessons</summary>

- Manage Microsoft Entra Health
- Summary and resources

</details>

### Microsoft Entra Connect Health – Alerts and Email Notifications

**Q:** Are Microsoft Entra Connect Health email notifications enabled by default?
**A:** Yes. Email notifications are enabled by default and are sent when an alert is generated and when it's resolved.

**Q:** Where in the portal do you configure email notifications for a Connect Health service?
**A:** Open the Alerts dialog for the service, select Notification Settings in the action bar, set the switch to ON, then select Save.

**Q:** How can you make all global administrators receive Connect Health alert emails?
**A:** In Notification Settings, select the check box to have all global administrators receive email notifications.

**Q:** How do you add extra recipients to Connect Health alert emails?
**A:** Enter the addresses in the Additional Email Recipients box; to remove one, select the entry and select Delete. Save to apply.

**Q:** When do notification setting changes take effect in Connect Health?
**A:** Only after you select Save. Changes take effect only after saving.

**Q:** How does Connect Health report back-end sync processing errors by email?
**A:** Instead of one message per error, it sends a daily digest of all errors to the tenant's administrative contact email address(es).

---

### Microsoft Entra Connect Health – Deleting Servers and Service Instances

**Q:** Which license is required for the steps to delete a server or service instance from Connect Health?
**A:** A Microsoft Entra ID premium license is required for the deletion steps.

**Q:** What happens to monitoring when you delete a server from Connect Health?
**A:** Data collection stops and the server is removed from monitoring — no new alerts, monitoring, or usage analytics data for that server.

**Q:** Does deleting a server in Connect Health uninstall the Health Agent?
**A:** No. The agent is not uninstalled; if it remains installed, you may see Health Agent-related errors on the server.

**Q:** What happens to data already collected from a deleted server?
**A:** It is not deleted immediately; it is deleted in accordance with the Azure data retention policy.

**Q:** What must you do to start monitoring the same server again after deleting it from Connect Health?
**A:** Uninstall and reinstall the Health Agent on that server.

**Q:** What must you do to resume monitoring after deleting an entire service instance?
**A:** Uninstall and reinstall the Health Agent on all the servers that were part of that service instance.

**Q:** How do you delete a server monitored by Connect Health for AD FS or Entra Connect (Sync)?
**A:** Open the Server screen from the Server List, select Delete, type the server name in the confirmation box, then select Delete.

**Q:** How do you delete a domain controller monitored by Connect Health for Microsoft Entra Domain Services?
**A:** Open the Domain Controllers dashboard, select the DC, select Delete Selected, confirm, then select Delete.

**Q:** How do you delete a service instance from Connect Health?
**A:** Open the Service screen from the Service List by service identifier (farm name), select Delete, type the service name (e.g., sts.contoso.com), then select Delete.

---

### Microsoft Entra Connect Health – Access with Azure RBAC

**Q:** Which built-in Azure RBAC roles does Microsoft Entra Connect Health support?
**A:** Owner, Contributor, and Reader. Other roles (e.g., User Access Administrator, DevTest Labs Users) have no impact on access.

**Q:** You need a user to view alerts and change settings (e.g., email notifications) but NOT manage access. Which role?
**A:** Contributor — can view all information and change settings, but not manage access.

**Q:** What can an Owner do in Connect Health that a Contributor cannot?
**A:** Manage access — e.g., assign a role to a user or group. Owners can also view all information and change settings.

**Q:** What can a Reader do in Connect Health?
**A:** Only view all information (e.g., view alerts) from the portal.

**Q:** Who is assigned the Owner role in Connect Health by default, and can this be changed?
**A:** Microsoft Entra global administrators are Owners by default; this assignment can't be changed.

**Q:** What are the two access scopes for managing Connect Health access?
**A:** All service instances (recommended in most cases) and an individual service instance.

**Q:** A user has access at both the directory level and a service instance level. Is permission granted?
**A:** Yes. Permission is granted if the user has access at either the directory or the service instance level.

**Q:** Is the Invite Users feature supported within Microsoft Entra Connect Health?
**A:** No. The Invite Users feature isn't supported.

**Q:** You must grant a user Connect Health access across all monitored role types. What scope and path?
**A:** All service instances scope: open the main Connect Health screen, then Configure > Users > Add, select the role, and add the user or group.

---

### Diagnosing and Remediating Duplicated Attribute Sync Errors

**Q:** Which sync error names indicate duplicated attribute conflicts?
**A:** QuarantinedAttributeValueMustBeUnique and AttributeValueMustBeUnique.

**Q:** Which attribute conflicts are commonly seen with these sync errors?
**A:** UserPrincipalName or Proxy Addresses conflicts; conflicting objects are quarantined in Microsoft Entra ID.

**Q:** Which duplicated attributes does the Connect Health diagnose feature support?
**A:** UserPrincipalName and ProxyAddresses (both error types); SipProxyAddress and OnPremiseSecurityIdentifier (AttributeValueMustBeUnique only).

**Q:** Which Azure RBAC permission is required to use the diagnose feature for sync errors?
**A:** Contributor permission from Azure RBAC.

**Q:** Is any upgrade or configuration required to enable the Connect Health diagnosis feature?
**A:** No. No upgrade or configuration is required.

**Q:** What is the "orphaned object" scenario?
**A:** The on-premises source object was deleted but the deletion never synced to Microsoft Entra ID; the existing user loses the Source Anchor, so a restored/recreated object can't match it.

**Q:** A cloud-only user exists in Microsoft Entra ID and a newly synced duplicate appears. Why can't they be matched?
**A:** The objects have different Source Anchors and there is no direct way to remap the Source Anchor.

**Q:** What does the Diagnose status "Not Started" mean?
**A:** The diagnosis process hasn't been visited; there may be a potential way to fix the sync error directly from the portal.

**Q:** What does the Diagnose status "Manual Fix Required" mean?
**A:** The error doesn't fit portal fix criteria (e.g., conflicting objects aren't users) or diagnostics found no portal fix — on-premises fixes are still an option.

**Q:** What does the Diagnose status "Pending Sync" mean?
**A:** A fix was applied and the portal is waiting for the next sync cycle to clear the error.

**Q:** Which button applies the portal-side fix for an identified orphaned object case?
**A:** Apply Fix — the status changes to Pending sync and the error is removed after the next sync cycle.

**Q:** Diagnostics end with a Close button instead of a fix. What does this mean?
**A:** No quick fix is available from the portal; use the shown on-premises solution. The error status switches to Manual fix required.

**Q:** Two users both have Joe.J@contoso.com as UPN and are quarantined. How can you fix it?
**A:** Update the conflicting source object from the on-premises side; the sync error resolves after the next sync.

---

### Module Summary – Hybrid Identity with Microsoft Entra ID

**Q:** Which sign-in/authentication options does the module cover for Microsoft Entra Connect?
**A:** Password hash synchronization (PHS), pass-through authentication (PTA), Seamless single sign-on (Seamless SSO), and federation (excluding manual AD FS deployments).

**Q:** Beyond hybrid deployment, which management areas does the module cover?
**A:** Managing Microsoft Entra Connect, managing PHS/PTA/Seamless SSO/federation, troubleshooting synchronization errors, and implementing/managing Microsoft Entra Connect Health.


---

# Module 05: What is Microsoft Entra multifactor authentication - Flashcards

<details><summary>Source lessons</summary>

- Introduction
- What is Microsoft Entra multifactor authentication
- Plan your multifactor authentication deployment
- Exercise - Enable Microsoft Entra multifactor authentication
- Configure multifactor authentication methods
- Summary

</details>

### What Is Microsoft Entra Multifactor Authentication

**Q:** What is Microsoft Entra multifactor authentication?
**A:** A security feature that requires two or more authentication elements for full authentication, limiting the impact of password exposure.

**Q:** What are the three categories of authentication elements used in MFA?
**A:** Something you know (password, security answer), something you possess (mobile app, token device), something you are (biometric, e.g., fingerprint or face scan).

**Q:** A hacker obtained a valid username and password via social engineering. Which feature stops the sign-in?
**A:** Microsoft Entra multifactor authentication — the password is useless without also possessing the second factor (trusted device, fingerprint, or face).

**Q:** Which Microsoft Entra feature locks out attackers brute-forcing passwords while treating valid-user sign-ins differently?
**A:** Microsoft Entra smart lockout.

**Q:** Which Microsoft Entra feature blocks commonly used and compromised passwords using a global banned-password list?
**A:** Microsoft Entra password protection.

**Q:** Which feature lets you configure risk-based policies that automatically block risky behaviors or require a password change?
**A:** Microsoft Entra ID Protection.

**Q:** Which Microsoft Entra feature provisions security-enhanced remote access to on-premises web applications?
**A:** Microsoft Entra application proxy.

**Q:** Which tool creates and manages a single identity per user across a hybrid enterprise, keeping users, groups, and devices in sync?
**A:** Microsoft Entra Connect.

**Q:** Which offerings include Microsoft Entra MFA using security defaults?
**A:** Microsoft Entra ID P1 or P2, Microsoft 365 Business, Microsoft Entra ID Free, and standalone Microsoft 365 licenses.

**Q:** Why is the layered approach of MFA effective?
**A:** Even if a hacker learns the password, it's useless without the trusted device; a found device is useless without the password.

### Planning an MFA Deployment

**Q:** How should you roll out an MFA deployment across an organization?
**A:** In waves — start with a small pilot group to find setup issues and unsupported apps/devices, then broaden until fully enrolled.

**Q:** What resources does Microsoft provide to help draft MFA rollout communications?
**A:** Communication templates, including posters and email templates.

**Q:** How is Microsoft Entra MFA enforced?
**A:** With Conditional Access policies, which are IF-THEN statements (IF a user accesses a resource, THEN they must complete an action).

**Q:** Name common access requests that a Conditional Access policy can require MFA for.
**A:** Accessing a specific cloud app, a specific network, a specific client application, or registering a new device.

**Q:** Why should you support more than one MFA authentication method?
**A:** So users have a backup option in case their primary method is unavailable.

**Q:** Which MFA method is preferred for backups?
**A:** Call to phone — Azure calls a supplied number and the user approves via the keypad.

**Q:** What key facts apply to the mobile app OATH verification code method?
**A:** The code changes every 30 seconds, works even with limited connectivity, and doesn't work in China on Android devices.

**Q:** What are FIDO2 security keys?
**A:** An unphishable, standards-based passwordless authentication method — typically USB devices, but can also use Bluetooth or NFC.

**Q:** What does Windows Hello for Business provide?
**A:** Strong two-factor authentication on devices that replaces passwords, using a device-tied credential with a biometric or PIN.

**Q:** What is the easiest approach to get users to register their MFA methods?
**A:** Microsoft Entra ID Protection — with a license, configure it to prompt users to register for MFA at their next sign-in.

**Q:** How can Conditional Access enforce MFA registration for all users?
**A:** Apply the policy to a group containing all users, then manually review the group periodically to remove registered users.

### Enabling MFA in the Portal

**Q:** Which license is required to configure MFA with Conditional Access in this module's exercise?
**A:** Microsoft Entra ID P1 or P2 (a 30-day free trial can be used).

**Q:** What is the portal path to the MFA options page?
**A:** Azure portal > Microsoft Entra ID > Security > Multifactor authentication (under Manage).

**Q:** Where in the portal do you enable or disable app passwords?
**A:** Microsoft Entra ID > Security > Multifactor authentication > Additional cloud-based multifactor authentication settings > Service Settings.

**Q:** What is the purpose of app passwords?
**A:** They let users create unique per-app passwords to authenticate to apps that don't support multifactor authentication.

**Q:** In a Conditional Access policy, which setting enforces MFA?
**A:** Under Grant: select Grant access, then Require multifactor authentication, then set Enable policy to On.

**Q:** In the exercise's guest-user MFA policy, which conditions were configured?
**A:** Locations — set Configure to Yes and select Any location.

**Q:** What happens the next time a user signs into an app protected by a new MFA Conditional Access policy?
**A:** The user is prompted to register for MFA and choose a preferred method.

### Configuring Authentication Methods

**Q:** Which authentication method is the only one you cannot disable?
**A:** Password.

**Q:** Which accounts can use security questions for SSPR?
**A:** Only non-administrative accounts — the method is available only for SSPR.

**Q:** How many predefined security questions does Azure provide, and what is the maximum length of custom questions?
**A:** 35 predefined questions (translated/localized by browser locale); custom questions max 200 characters.

**Q:** Who can read or change a user's security questions and answers?
**A:** Nobody but the user — answers are stored privately on the user object and can only be answered by the user, during registration.

**Q:** Which method is available only in SSPR, and what is the recommendation about it?
**A:** Email address — avoid email accounts that don't require the user's Microsoft Entra password to access.

**Q:** Name the three passwordless authentication solutions.
**A:** Windows Hello for Business, FIDO2 security keys, and the Microsoft Authenticator app.

**Q:** What does FIDO stand for?
**A:** Fast Identity Online — an open standard for passwordless authentication.

**Q:** Where can users sign in with FIDO2 security keys?
**A:** Microsoft Entra ID-joined or Entra hybrid-joined Windows 10 devices, with SSO to cloud and on-premises resources, and supported browsers.

**Q:** Which OATH token type does Microsoft Entra ID support for hardware tokens?
**A:** OATH-TOTP SHA-1 tokens, of the 30-second or 60-second variety.

**Q:** What is the limit on secret keys for OATH hardware tokens?
**A:** 128 characters (which might not be compatible with all tokens).

**Q:** In OATH software tokens, who generates the secret key (seed)?
**A:** Microsoft Entra ID generates the seed that is input into the app and used to generate each OTP.

**Q:** Which MFA method is not available to the free/trial Microsoft Entra tier?
**A:** Voice call — an automated voice system calls the number and the user confirms via keypad.

**Q:** On which platforms is the Microsoft Authenticator app available?
**A:** Android and iOS.

**Q:** Which services can use the Microsoft Authenticator app, FIDO2 keys, text messages, and voice calls?
**A:** Both Microsoft Entra multifactor authentication and SSPR.

**Q:** A user enabled for MFA can't authenticate from a legacy non-browser app. What is the fix?
**A:** Use an app password — it allows authentication to continue for non-browser apps that don't support MFA.

**Q:** How does a user sign in using the Microsoft Authenticator app?
**A:** Approve a push notification, or enter an OATH verification code from the app after providing username and password.

### Monitoring MFA and SSPR

**Q:** Where do you monitor authentication method activity for MFA and SSPR adoption?
**A:** Microsoft Entra ID > Monitoring > Usage & insights.

**Q:** What data window is used for the Usage & insights registration statistics?
**A:** The last 30 days of audit logs from the combined security info registration and SSPR registration experiences.

**Q:** Where can you see a specific user's latest registration audit information?
**A:** Drill down from the Usage & insights chart by clicking it.

**Q:** Where can you learn more about SSPR usage in the organization?
**A:** The Usage tab on the main Usage & insights view.


---

# Module 06: Administer FIDO2 and passwordless authentication methods - Flashcards

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

### Authentication Methods Overview

**Q:** Which three passwordless authentication methods provide the most secure sign-in events in Microsoft Entra ID?
**A:** Windows Hello for Business, FIDO2 security keys, and the Microsoft Authenticator app.

**Q:** Which authentication methods can be used as the PRIMARY factor at sign-in?
**A:** Password, Windows Hello for Business, and FIDO2 security keys (Microsoft Authenticator app and SMS in preview). OATH tokens and voice calls are secondary only (MFA/SSPR).

**Q:** Can you disable the password authentication method in Microsoft Entra ID?
**A:** No. Passwords cannot be disabled; increase sign-in security by adding multifactor authentication.

**Q:** You need to support legacy apps that don't support modern authentication with per-user MFA. Which method should you use?
**A:** App passwords.

**Q:** Which verification methods are used ONLY for SSPR (not for MFA)?
**A:** Security questions and email address.

**Q:** What does Microsoft recommend to simplify onboarding for MFA and SSPR?
**A:** Enable combined security information registration, and require users to register multiple authentication methods for resiliency.

### FIDO2 Security Keys

**


---

# Module 07: Plan security defaults - Flashcards

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

### Introduction and Security Defaults

**Q:** What is Conditional Access and what does it allow you to control?
**A:** Fine-grained control over which users/identities can perform specific activities and access resources, using signals to enforce Zero Trust access decisions.

**Q:** What are security defaults?
**A:** Preconfigured security settings managed by Microsoft, provided at no extra cost, giving organizations a basic level of identity security until they manage their own.

**Q:** Name three settings enforced by security defaults.
**A:** Require all users to register for MFA, require administrators to perform MFA, and block legacy authentication protocols.

**Q:** What MFA method does security defaults registration use to prevent MFA fatigue attacks?
**A:** Number matching—users enter a number displayed on screen into the Microsoft Authenticator app.

**Q:** What is the grace period for MFA registration when security defaults are enabled?
**A:** None—registration is required immediately; users are prompted before accessing any resources.

**Q:** Which admin roles are required to enable or disable security defaults?
**A:** At least a Conditional Access Administrator (Security Administrator also shown in exercises).

**Q:** What is the portal path to manage security defaults?
**A:** Microsoft Entra admin center > Entra ID > Overview > Properties > Manage security defaults.

**Q:** Since when are security defaults potentially already enabled on new tenants?
**A:** Tenants created on or after October 22, 2019; security defaults are enabled on all new tenants at creation.

**Q:** Who should NOT use security defaults?
**A:** Organizations already using Conditional Access policies, organizations with Microsoft Entra ID Premium licenses, or those with complex security requirements.

**Q:** Why is legacy authentication dangerous even with an MFA policy enabled?
**A:** Legacy authentication doesn't support MFA, so attackers can use older protocols to bypass multifactor authentication.

**Q:** Which protocols count as legacy authentication?
**A:** Clients not using modern authentication (e.g., Office 2010) and mail protocols like IMAP, SMTP, or POP3.

**Q:** Does security defaults block Exchange Active Sync basic authentication?
**A:** Yes—after security defaults are enabled, all authentication requests made by older protocols are blocked.

### Plan Conditional Access Policies

**Q:** How are Conditional Access policies structured?
**A:** As if-then statements: if an assignment is met, then apply the access controls. Conditions are called assignments.

**Q:** What are the three components of a CA policy?
**A:** Assignments (users, apps, conditions), Access controls (grant/block), and Session controls.

**Q:** If no assignment is required and no CA policy is in effect, what is the default behavior?
**A:** An access token is issued.

**Q:** What is report-only mode used for?
**A:** Evaluating CA policies before enabling them, to predict affected users without enforcing the policy.

**Q:** How many emergency access accounts should you create before deploying CA policies?
**A:** Two or more, to mitigate accidental administrator lockout from misconfigured policies.

**Q:** Which license tier is required for Conditional Access?
**A:** Microsoft Entra ID Premium P1 (or Microsoft 365 E3 and up); Free and Office 365 subscriptions have no CA.

**Q:** Which license tier adds risk-based sign-in options to Conditional Access?
**A:** Microsoft Entra ID Premium P2, which adds Risky sign-ins, Risky Users, and risk-based options via Identity Protection.

**Q:** How should block access policies be rolled out?
**A:** In phases: communicate changes, start with a small user set, keep administrators excluded, apply to all users only after thorough testing.

**Q:** What can you use named locations for in CA planning?
**A:** To block sign-ins from countries/regions you would never expect, by creating a named location and a policy blocking sign-in from it.

### Implement CA Policy Controls and Assignments

**Q:** What does Microsoft recommend when first creating a CA policy?
**A:** Set Enable policy to Report-only, monitor sign-in logs to verify behavior, then switch it to On.

**Q:** Which license is required to use sign-in risk-based Conditional Access?
**A:** Microsoft Entra ID Premium P2 (Identity Protection sign-in risk detections).

**Q:** Through which two interfaces can a sign-in risk-based policy be assigned?
**A:** Conditional Access itself or Microsoft Entra Identity Protection—organizations should choose one.

**Q:** Which user action in Conditional Access secures MFA and SSPR registration?
**A:** "Register security information" under Cloud apps or actions > User actions (requires combined registration).

**Q:** How do you create a policy requiring security info registration only from trusted locations?
**A:** User action "Register security information"; Conditions > Locations: include Any location, exclude All trusted locations; Grant control: Block access.

**Q:** Where are Named locations configured?
**A:** Microsoft Entra admin center > Protection > Conditional Access > Named locations.

**Q:** What two types of named locations can you define?
**A:** IP ranges (IPv4 address ranges) or Countries/Regions (with optional inclusion of unknown areas).

**Q:** Which accounts should always be excluded from CA policies?
**A:** Emergency access (break-glass) accounts, service accounts (e.g., Microsoft Entra Connect Sync Account), and service principals.

**Q:** Why should service accounts be excluded from CA policies?
**A:** MFA can't be completed programmatically; consider replacing them with managed identities.

**Q:** Which grant control requires Intune and blocks non-compliant devices?
**A:** "Require device to be marked as compliant"—Intune sends compliance info to Microsoft Entra ID for CA decisions.

**Q:** Does the "Require device to be marked as compliant" control block Intune enrollment?
**A:** No—new devices can still enroll to Intune even if the control applies to All users and All cloud apps.

**Q:** What are Terms of Use (TOU) in Conditional Access?
**A:** PDF-based terms created in Identity Governance that users must accept before access; consent can be expired or re-attested.

### Troubleshooting CA Policies

**Q:** Name two configurations that can block your entire organization if applied to all users and all cloud apps.
**A:** Block access, and Require Hybrid Microsoft Entra joined device.

**Q:** Why is "Require app protection policy" on all users/apps dangerous?
**A:** It can block administrators without an Intune app protection policy client app from accessing portals like Intune and Azure.

**Q:** Which two tools should be used to validate CA policies before enabling?
**A:** Conditional Access report-only mode and the What If tool.

**Q:** Which roles can review sign-in events for CA troubleshooting?
**A:** Security Administrator or Global Reader.

**Q:** Where do you find which CA policies applied to a failed sign-in?
**A:** Identity > Monitoring and Health > Sign-ins > select the event > Conditional Access tab.

**Q:** Which filters help investigate a specific sign-in failure?
**A:** Correlation ID, Conditional access (filtered to failures), Username, and Date.

**Q:** What information should be provided when opening a support incident for a sign-in failure?
**A:** The request ID and the time and date from the sign-in event.

**Q:** In the CA policy details view of a sign-in event, what does each side show?
**A:** Left side: details collected at sign-in; right side: whether those details satisfy the applied policies' requirements.

### Application Controls

**Q:** What architecture does Conditional Access App Control use?
**A:** Reverse proxy architecture, integrated with Microsoft Entra Conditional Access, routing users to Microsoft Defender for Cloud Apps.

**Q:** Where are access and session policies for Conditional Access App Control configured?
**A:** In the Microsoft Defender for Cloud Apps portal.

**Q:** Name three things Conditional Access App Control can do in a session.
**A:** Prevent data exfiltration (block download/copy/print), protect on download via Azure Information Protection labels, block upload of unlabeled files, and monitor risky user sessions.

**Q:** What must iOS and Android devices do before you can require approved client apps for them?
**A:** They must first register in Microsoft Entra ID.

**Q:** What are app protection policies (APP)?
**A:** Rules that ensure an organization's data remains safe or contained in a managed app, enforced when users access or move corporate data.

**Q:** What is MAM-WE?
**A:** Mobile Application Management without enrollment—protecting apps with sensitive data on almost any device, including personal BYOD devices.

**Q:** Can Intune app protection policies be used independent of MDM?
**A:** Yes—APP works independent of any mobile-device management solution, protecting data on managed and unmanaged devices.

**Q:** In a MAM policy, what does "Target to all app types = No" allow?
**A:** Applying MAM policies based on managed state—e.g., a less strict policy for Intune-managed devices, a more restrictive one for unenrolled devices.

**Q:** In the approved client app policy for Exchange Online, which client app type requires a separate policy for EAS?
**A:** Exchange ActiveSync clients—configured as a separate policy selecting "Exchange ActiveSync clients."

### Session Management and CAE

**Q:** What is sign-in frequency?
**A:** The time period before a user is asked to sign in again when accessing a resource.

**Q:** What is the default sign-in frequency in Microsoft Entra ID?
**A:** A rolling window of 90 days.

**Q:** Which protocols must apps implement for the sign-in frequency setting to work?
**A:** OAuth2 or OIDC, per the standards; SAML apps also work if they don't drop their own cookies.

**Q:** Name three examples of policy violations that revoke a user's session.
**A:** A password change, an incompliant device, or an account disable.

**Q:** What is the default browser session persistence behavior in Microsoft Entra ID?
**A:** Users on personal devices choose whether to persist the session via a "Stay signed in?" prompt after successful authentication.

**Q:** How long is a default OAuth 2.0 access token valid?
**A:** One hour; after expiration the client is redirected to Microsoft Entra ID to refresh it.

**Q:** What is Continuous Access Evaluation (CAE)?
**A:** A mechanism for two-way conversation between the token issuer and the relying party (enlightened app) to enforce revocations and policy changes in near real time.

**Q:** What does a resource provider send to a CAE-capable client when denying access after a revocation event?
**A:** A 401+ claim challenge; the client then re-authenticates with Microsoft Entra ID.

**Q:** Name two key benefits of CAE.
**A:** Near real-time enforcement of user termination/password change (session revocation) and near real-time enforcement of network location changes.

### Conditional Access Optimization Agent

**Q:** What is the Conditional Access optimization agent?
**A:** An agent that recommends policies and changes based on Zero Trust best practices, ensuring all users are protected by policy.

**Q:** Which licenses are required to use the Conditional Access optimization agent?
**A:** At least Microsoft Entra ID P1, available Security Compute Units (SCU), and Microsoft Intune licenses for device-based controls.

**Q:** Which role is needed to activate the agent the first time?
**A:** Security Administrator or higher.

**Q:** Name three key features of the optimization agent.
**A:** Require MFA for uncovered users, block legacy authentication, policy consolidation, block device code flow, and one-click remediation.

**Q:** What does the agent's policy consolidation feature do?
**A:** Identifies overlapping settings across enabled policies (e.g., same grant controls) and suggests consolidating them into one.


---

# Module 08: Review identity protection basics - Flashcards

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

### Identity Protection Basics

**Q:** Which license is required to use Identity Protection risk policies, risk notifications, and the MFA registration policy?
**A:** Microsoft Entra ID Premium P2.

**Q:** What are the three key tasks organizations can accomplish with Identity Protection?
**A:** Automate detection and remediation of identity-based risks; investigate risk data in the portal; export risk detection data to third-party utilities (e.g., a SIEM).

**Q:** How many signals does Microsoft analyze per day to identify and protect customers from threats?
**A:** 6.5 trillion signals per day.

**Q:** Which risk detection fires when multiple usernames are attacked using common passwords in a unified brute-force manner?
**A:** Password spray.

**Q:** A user signs in from a location atypical compared to their recent sign-ins. Which detection triggers?
**A:** Atypical travel.

**Q:** Which detection flags unusual token characteristics, such as an unusual token lifetime or a token replayed from an unfamiliar location?
**A:** Anomalous token.

**Q:** Which Identity Protection risk detections are discovered by Microsoft Defender for Cloud Apps (MDCA)?
**A:** New country, Activity from anonymous IP address, and Suspicious inbox forwarding.

**Q:** With Microsoft Entra ID P1, what does the Risky users report show?
**A:** Limited information: only users with medium and high risk, no details drawer, no risk history.

### Identity Protection Roles and Permissions

**Q:** Which role has full access to Identity Protection but cannot reset a user's password?
**A:** Security Administrator.

**Q:** Which role can view


---

# Module 09: Assign Azure roles - Flashcards

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

### Azure RBAC Fundamentals

**Q:** What is Azure RBAC?
**A:** The authorization system used to manage access to Azure resources. Access is granted by assigning roles to users, groups, service principals, or managed identities at a particular scope.

**Q:** You need to grant an application access to an Azure resource. Which security principal should you assign the role to?
**A:** A service principal.

**Q:** You want an application to manage its own credentials for authentication. Which security principal should you use?
**A:** A managed identity.

**Q:** Owner vs Contributor: what can Owner do that Contributor cannot?
**A:** Owner has full access to all resources, including granting access. Contributor can create and manage all types of Azure resources but cannot grant access.

**Q:** Which built-in Azure role is used specifically to assign access to Azure resources?
**A:** User Access Administrator.

**Q:** Which built-in Azure role can only view available Azure resources?
**A:** Reader.

### Scope and Role Assignment Limits

**Q:** Name the four levels of scope at which you can assign Azure roles.
**A:** Management group, subscription, resource group, and resource.

**Q:** How do role permissions flow across scope levels?
**A:** Lower levels inherit role permissions from higher levels (parent-child hierarchy); each lower level makes the scope more specific.

**Q:** You assign the Reader role to a user at management group scope. What can the user read?
**A:** Everything in all subscriptions in the management group.

**Q:** What is the maximum number of role assignments per subscription, and what does it include?
**A:** Up to 4,000 — including assignments at the subscription, resource group, and resource scopes.

**Q:** What is the maximum number of role assignments per management group?
**A:** Up to 500.

**Q:** What best practice should you follow when choosing roles and scopes, and why?
**A:** Least privilege — grant the minimum role at the narrowest scope needed, limiting what is at risk if the security principal is compromised.

### Assigning Azure Roles

**Q:** Which Azure portal page do you use to assign roles, whether you are on a user, group, resource group, or subscription?
**A:** Access control (IAM) — identity and access management.

**Q:** Which PowerShell cmdlet assigns an Azure role?
**A:** New-AzRoleAssignment, with -ObjectId, -RoleDefinitionName, and -Scope parameters.

**Q:** Which Azure CLI command creates a role assignment?
**A:** az role assignment create --assignee "{assignee}" --role "{roleNameOrId}" --resource-group "{resourceGroupName}"

### Custom Azure Roles

**Q:** Where are custom Azure roles stored, and can they be shared across subscriptions?
**A:** Stored in Microsoft Entra ID; yes, they can be shared across subscriptions.

**Q:** How many custom roles can each directory have?
**A:** Up to 5,000 custom roles.

**Q:** At which scopes can custom Azure roles be assigned?
**A:** Management group (in preview only), subscription, and resource group.

**Q:** What is the portal path to create a custom role?
**A:** Microsoft Entra admin center > Identity menu > Roles and administration > + New custom role.

**Q:** In a custom role JSON definition, what does Microsoft.Billing/*/read grant?
**A:** All read permissions from the Billing resource — the asterisk (*) is a wildcard and can exist at any level.

### Managed Identities

**Q:** What core problem do managed identities solve?
**A:** They eliminate the need for developers to manage secrets, credentials, certificates, and keys — Microsoft Entra ID automatically manages the identity used to obtain tokens.

**Q:** System-assigned vs user-assigned managed identity: how do their lifecycles differ?
**A:** System-assigned is tied to the service instance's lifecycle and is deleted automatically when the resource is deleted; user-assigned is a standalone resource managed separately from the resources using it.

**Q:** Which managed identity type can, by design, only be used by the Azure resource it is enabled on?
**A:** System-assigned — only that resource can use the identity to request tokens from Microsoft Entra ID.

**Q:** How many service instances can use a user-assigned managed identity?
**A:** One or more instances of an Azure service — it is assigned to one or more instances.

**Q:** What does using managed identities cost?
**A:** Nothing extra — managed identities can be used without any additional cost.

**Q:** What is the portal path to add a managed identity to an App Service app?
**A:** Open the app in the Azure portal > Identity > select System assigned or User assigned > + Add.

**Q:** Which CLI command assigns a managed identity to a web app?
**A:** az webapp identity assign --resource-group


---

# Module 10: Explore Global Secure Access - Flashcards

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

### Introduction to Global Secure Access

**Q:** What is Security Service Edge (SSE)?
**A:** An identity-aware, cloud-delivered network perimeter that secures access for users working from anywhere.

**Q:** Which two products make up the Microsoft SSE solution, and what are they collectively called?
**A:** Microsoft Entra Internet Access and Microsoft Entra Private Access — collectively referred to as Global Secure Access.

**Q:** Which three Zero Trust principles is Global Secure Access founded on?
**A:** Least privilege, explicit verification, and an assumption of breach.

### Explore Global Secure Access

**Q:** Microsoft Entra Internet Access vs Microsoft Entra Private Access — what does each secure?
**A:** Internet Access: Microsoft services, SaaS apps, and public internet apps via the Secure Web Gateway (SWG). Private Access: private corporate resources without a legacy VPN.

**Q:** Microsoft Entra Private Access builds on which existing capability, and what does it add?
**A:** Builds on Microsoft Entra application proxy; extends access to any private resource, port, and protocol across hybrid and multicloud environments.

**Q:** You plan to deploy Global Secure Access. Which licenses do you need?
**A:** Microsoft Entra ID P1 or P2, plus a Microsoft Entra Internet Access license and/or a Microsoft Entra Private Access license.

**Q:** Which role must be assigned to at least one administrator to manage Global Secure Access?
**A:** Global Secure Access Administrator.

### Deploy and Configure Microsoft Entra Internet Access

**Q:** What are the four main steps to deploy Microsoft Entra Internet Access?
**A:** 1) Enable the Microsoft traffic forwarding profile, 2) install the Global Secure Access client, 3) enable tenant restrictions, 4) enable enhanced Global Secure Access signaling and Conditional Access.

**Q:** A Conditional Access policy targets Microsoft traffic. When is it enforced?
**A:** Only when the Global Secure Access client is installed on the device; without the client, Microsoft traffic still flows but the policy isn't enforced.

**Q:** Which three network routing policies are created when you enable the Microsoft traffic forwarding profile?
**A:** 1) Exchange Online, 2) SharePoint Online and OneDrive for Business, 3) Entra ID and Microsoft Graph (managed via FQDNs or IP subnets).

**Q:** How can the Global Secure Access client be deployed on Windows and Android?
**A:** Windows: manually (GlobalSecureAccessClient.exe) or via MDM such as Microsoft Intune. Android: via Intune or Microsoft Defender for Endpoint (client from the Android store).

**Q:** What do tenant restrictions control?
**A:** Which external tenants internal identities (employees) may access on the managed network; external identities (contractors, vendors) can be blocked from all external tenants.

**Q:** Which role is required to configure tenant restrictions in cross-tenant access settings?
**A:** At least Security Administrator (Identity > External Identities > Cross-tenant access settings > Organizational settings).

**Q:** You need to enforce tenant restrictions with Global Secure Access tagging. Which roles and portal path are required?
**A:** Both Global Secure Access Administrator and Security Administrator; Global Secure Access > Global Settings > Session Management > Tenant Restrictions > enable tagging.

**Q:** With tenant restrictions v2, what does data plane protection cover, and what is out of scope?
**A:** Blocks reuse of an Entra ID-issued token against Microsoft Graph and anonymous access to SharePoint Online; third-party apps such as Slack aren't in scope.

**Q:** In a compliant network Conditional Access policy, which cloud app must you NOT select in the app picker?
**A:** The generic "Office 365" cloud app (not supported) — select Office 365 Exchange Online and/or Office 365 SharePoint Online instead.

**Q:** Which named location is created when Global Secure Access signaling is enabled, and what is its location type?
**A:** "All Compliant Network locations" with location type Network Access (Protection > Conditional Access > Named locations); can optionally be marked trusted.

### Deploy and Configure Microsoft Entra Private Access

**Q:** What are the four main steps to deploy Microsoft Entra Private Access?
**A:** 1) Configure a private network connector and connector group, 2) configure Quick Access, 3) enable the Private Access traffic forwarding profile, 4) install the Global Secure Access client.

**Q:** What is a Microsoft Entra private network connector?
**A:** A lightweight agent installed on a server in the private network that facilitates the outbound connection to the Global Secure Access service.

**Q:** What are the requirements for the private network connector server?
**A:** Windows Server 2016 or later, .NET 4.7.2+, TLS 1.2 enabled; more than one server recommended for high availability.

**Q:** Which outbound ports must the private network connector be able to use?
**A:** Port 80 — downloading certificate revocation lists (CRLs); port 443 — all outbound communication with the Application Proxy service.

**Q:** Which role installs the private network connector?
**A:** Global Administrator of the directory that uses Application Proxy (Global Secure Access > Connect > Connectors).

**Q:** What is Quick Access in Microsoft Entra Private Access?
**A:** An app where you define a name, select a connector group, and add application segments (FQDNs and IP addresses) of private resources.

**Q:** Which four destination types can a Quick Access application segment use?
**A:** IP address (IPv4), FQDN (including wildcard FQDNs), IP address range (CIDR), and IP address range (IP to IP).

**Q:** Which name-resolution type is NOT supported in Private Access application segments?
**A:** NetBIOS — use an FQDN such as contoso.local/app1 instead of contoso/app1.

### Global Secure Access Dashboard

**Q:** What does the Global Secure Access snapshot widget display?
**A:** Distinct users (UPN), devices (device ID), and workloads/destinations (FQDNs and IPs) seen in the last 24 hours; filterable by Internet Access, Private Access, or Microsoft traffic.

**Q:** Name four common alerts shown on the Global Secure Access dashboard.
**A:** Unhealthy remote network, increased external tenants activity, token and device inconsistency, and web content blocked.

**Q:** In the cross-tenant access widget, what are "unseen tenants"?
**A:** Distinct tenant IDs seen in the last 24 hours but not seen in the previous seven days.

### Remote Networks

**Q:** What is a remote network in Global Secure Access?
**A:** A remote location (e.g., branch office) connected via an IPsec tunnel between the customer premises equipment (CPE) router and the nearest Global Secure Access endpoint; no client install needed on devices.

**Q:** What are the five steps to configure a remote network?
**A:** Basics (name, region) > Connectivity (router details) > Traffic forwarding profile > Review configuration > Set up on-premises router.

**Q:** Where must the final remote network step be performed, and what happens if you skip it?
**A:** In the on-premises router (CPE) management console; until it's done, IPsec isn't set up because IKE negotiations must complete between both parties.

**Q:** Which two tools can be used to configure remote networks?
**A:** The Microsoft Entra admin center or the Microsoft Graph API.

### Conditional Access with Global Secure Access

**Q:** Which three new Conditional Access checks are introduced with Global Secure Access?
**A:** Compliant network check, Private Access apps, and Source IP restoration.

**Q:** Where do you enable Global Secure Access signaling for Conditional Access?
**A:** Global Secure Access > Global settings > Session management > Adaptive access (as Global Secure Access Administrator); CAE signaling is auto-enabled for Office 365 (preview).

**Q:** At which planes is the compliant network enforced, and which services support the data plane?
**A:** Authentication plane (Microsoft Entra ID) and data plane (preview) via Continuous Access Evaluation — currently only Exchange Online and SharePoint Online.

**Q:** What does Source IP restoration provide?
**A:** The original user source IP for Conditional Access/CAE location policies, Identity Protection risk detections, and sign-in logs; enabled by turning on Global Secure Access signaling.

**Q:** Source IP restoration — which traffic is supported, and what limitation applies?
**A:** Only Microsoft traffic (SharePoint Online, Exchange Online, Teams, Microsoft Graph); the Global Secure Access service IP isn't visible, only the source IP.

**Q:** How do you create a Conditional Access policy for a Quick Access or Private Access app?
**A:** Global Secure Access > Applications > Enterprise applications > select the app > Conditional Access > Create new policy; the app is auto-added as the target resource.

**Q:** Which accounts should be excluded from Conditional Access policies?
**A:** Emergency access (break-glass) accounts to prevent tenant-wide lockout, and service accounts/principals such as the Microsoft Entra Connect Sync account.

### Logs and Monitoring

**Q:** Where can you access Global Secure Access audit logs?
**A:** Global Secure Access > Audit logs (pre-filtered), or Identity > Monitoring & health > Audit logs with the Service filter set to Global Secure Access.

**Q:** In traffic logs, what is the hierarchy of session, connection, and transaction?
**A:** Session starts with the first URL a user accesses; a session contains connections (5-tuple: source/destination IP, source/destination port, FQDN); a connection contains transactions (unique request/response pair).

**Q:** You need to view Global Secure Access traffic logs. Which path and minimum role?
**A:** Global Secure Access > Monitor > Traffic logs; at least the Reports Reader role.

**Q:** Enriched Microsoft 365 logs — which two log tables are combined, and how?
**A:** Microsoft 365 OfficeActivity + Global Secure Access NetworkAccessTraffic, joined via a Unique Token ID; only SharePoint Online logs are available for enrichment.

**Q:** Which diagnostic setting sends Global Secure Access traffic logs, and to which destinations?
**A:** NetworkAccessTrafficLogs — to a Log Analytics workspace, storage account, event hub, or partner solution (requires at least Security Administrator).

**Q:** What are the retention periods for Global Secure Access logs?
**A:** Traffic and remote network health logs: 30 days. Audit logs: varies by Microsoft Entra ID license. Office logs: up to 24 hours.


---

# Module 11: Discover apps by using Microsoft Defender for Cloud Apps and Active Directory Fe - Flashcards

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

### CASB and Microsoft Defender for Cloud Apps (MDCA)

**Q:** What is a CASB?
**A:** Cloud Access Security Broker — an on-premises or cloud-based security policy enforcement point placed between cloud service consumers and cloud service providers to enforce enterprise security policies.

**Q:** What is Microsoft Defender for Cloud Apps (MDCA)?
**A:** Microsoft's implementation of a CASB service that protects data, services, and applications with enterprise policies, providing supplemental reporting and analytics.

**Q:** Which deployment modes does MDCA support?
**A:** Log collection, API connectors, and reverse proxy.

**Q:** Which MDCA feature provides visibility into Shadow IT?
**A:** Cloud Discovery — it uses traffic logs to dynamically discover and analyze the cloud apps your organization is using.

**Q:** How do you create a snapshot report of your organization's cloud use in Cloud Discovery?
**A:** Manually upload log files from your firewalls or proxies for analysis.

**Q:** How do you set up continuous reports in Cloud Discovery?
**A:** Use MDCA log collectors to periodically forward your logs.

**Q:** You need to get a general picture of discovered cloud apps before deeper investigation. Where should you look first?
**A:** The Cloud Discovery Dashboard — starting with the High-level usage overview.

**Q:** What can you check in the Cloud Discovery Dashboard to identify the most dominant users of cloud apps?
**A:** The top users and source IP addresses.

**Q:** Where do you review the risk score of a discovered app and the status of open discovery alerts?
**A:** In the App risk overview on the Cloud Discovery Dashboard.

### Filtering and Sanctioning Apps

**Q:** Which filter lets you search for apps compliant with HIPAA, ISO 27001, SOC 2, or PCI-DSS?
**A:** The Compliance risk factor filter.

**Q:** Which filter indicates whether allowing permission to an app is common, uncommon, or rare?
**A:** The Community use filter (used in OAuth app policies).

**Q:** How many cloud apps are in the MDCA Cloud app catalog, and how are they scored?
**A:** More than 16,000 cloud apps, ranked and scored based on industry standards, regulatory certifications, and best practices.

**Q:** On how many risk factors is MDCA app scoring based?
**A:** More than 80 risk factors.

**Q:** What is the effect of marking an OAuth app as banned in an OAuth app policy?
**A:** It disables the correlating Enterprise Application.

### AD FS Application Migration

**Q:** What is AD FS?
**A:** Active Directory Federation Services — a standards-based on-premises identity service that extends SSO between trusted business partners (federation) without requiring separate sign-ins.

**Q:** Which report helps identify which AD FS applications can migrate to Microsoft Entra ID?
**A:** The AD FS application activity report, available in the Azure portal under Microsoft Entra ID > Enterprise applications > Usage and insights.

**Q:** Which apps are listed in the AD FS application activity report?
**A:** All AD FS applications in the organization that have had an active user logged in within the last 30 days.

**Q:** Which admin roles grant access to the AD FS application activity report data?
**A:** Global reader/administrator, report reader, security reader, application administrator, or cloud application administrator.

**Q:** What does the migration status "Ready to migrate" mean?
**A:** The AD FS application configuration is fully supported in Microsoft Entra ID and can be migrated as-is.

**Q:** What does the migration status "Needs review" mean?
**A:** Some of the application's settings can be migrated to Microsoft Entra ID, but settings that can't be migrated as-is must be reviewed.

**Q:** What does the migration status "Additional steps required" mean?
**A:** Microsoft Entra ID doesn't support some of the application's settings, so the application can't be migrated in its current state.

**Q:** Which two types of applications should be migrated from AD FS to Microsoft Entra ID?
**A:** SaaS applications (procured by the organization) and line-of-business (LOB) applications (developed by the organization).

**Q:** Which authentication protocols should be migrated first, and why?
**A:** Modern authentication protocols such as SAML and OpenID Connect — they can be reconfigured via a built-in gallery connector or by registering the app in Microsoft Entra ID.

**Q:** How do you integrate apps using older (legacy) authentication protocols?
**A:** By using Application Proxy and/or Microsoft Entra Domain Services.

### App Connectors

**Q:** How do app connectors enable greater visibility and control in MDCA?
**A:** They use the APIs of app providers; all communication between MDCA and connected apps is encrypted using HTTPS.

**Q:** Which apps does MDCA multi-instance support apply to?
**A:** Only API connected apps — not Cloud Discovered apps or Proxy connected apps.

**Q:** How often does MDCA perform periodic data scans of connected apps, and what triggers real-time scans?
**A:** Periodic scans every 12 hours; real-time scans are triggered each time a change is detected.

**Q:** What capabilities does the App permissions / App permission governance API connection provide?
**A:** Visibility into issued tokens and their permissions, and the ability to remove tokens.

**Q:** Why might some MDCA policies take hours or days to run?
**A:** Operations like scanning all files in a tenant require numerous API calls, which are spread over a longer period due to API limitations such as throttling and API limits.

### Access Management for Apps

**Q:** Which license is required to complete the exercises in this module?
**A:** Microsoft Entra ID Premium P2 (a free trial can be activated).

**Q:** Which gallery app is added in the exercise to demonstrate user assignment?
**A:** GitHub Enterprise Cloud – Enterprise Account.

### App Management Roles

**Q:** What happens automatically when a user creates a new application registration?
**A:** They're automatically added as the first owner of the application.

**Q:** Which role should you assign to grant users the ability to create application registrations when "Users can register applications" is set to No?
**A:** The Application Developer role (it also grants consent on one's own behalf when user consent is disabled).

**Q:** Which setting disables the default ability for users to create application registrations?
**A:** The "Users can register applications" setting on the User settings page, set to No.

**Q:** What can an enterprise application owner manage?
**A:** Organization-specific configuration of the enterprise app they own, such as SSO configuration, provisioning, and user assignments — but only for apps they own.

**Q:** Which principals can be owners of application registrations vs. enterprise applications?
**A:** Users and service principals can own application registrations; only users can own enterprise applications. Groups can't be owners of either.

**Q:** Application Administrator vs. Cloud Application Administrator — what's the key difference?
**A:** Cloud Application Administrator has the same permissions as Application Administrator except the ability to manage application proxy.

**Q:** Can Application Administrator or Cloud Application Administrator manage Conditional Access settings?
**A:** No — neither role grants the ability to manage Conditional Access settings.

**Q:** Are Application Administrators or Cloud Application Administrators added as owners when creating new app registrations or enterprise apps?
**A:** No — users assigned to these roles aren't added as owners.

**Q:** What are the two separate steps for using a custom role?
**A:** Create a custom role definition with permissions, then create a role assignment to assign the custom role at a scope.

**Q:** At which scopes can a custom role be assigned?
**A:** Organization-wide (directory) scope or the scope of a single Microsoft Entra object, such as a single app registration.

**Q:** Where do role assignments for app registrations appear in the App registrations page?
**A:** Only in the All applications tab — they don't show up in the Owned applications tab.

**Q:** Custom roles don't grant access to the Microsoft Entra ID portal when which user setting is enabled?
**A:** When the "Restrict access to Microsoft Entra ID administration portal" user setting is set to Yes.

### Gallery SaaS App Configuration

**Q:** What does the "Enabled for users to sign in?" app property control?
**A:** Whether users assigned to the application can sign in.

**Q:** What does the "User assignment required?" property control?
**A:** Whether users who aren't assigned to the application can sign in.

**Q:** What does the "Visible to users?" property control?
**A:** Whether assigned users can see the app in My Apps and the Microsoft 365 app launcher.

**Q:** Which SSO protocol do apps added through App registrations use by default, and which field do they lack compared to SAML apps?
**A:** OIDC-based apps by default; they don't have the User access URL field that SAML-based SSO apps have.

**Q:** What are the requirements for a custom app logo?
**A:** 215 by 215 pixels, saved in .png format.

### OAuth App Policies

**Q:** Where do you create and manage OAuth app policies in MDCA?
**A:** At https://security.microsoft.com, under the Cloud apps section, select OAuth apps.

**Q:** How can you create an OAuth app policy besides using the "New policy from search" button?
**A:** Select Control > Policies > Create policy > OAuth app policy.

**Q:** You need to revoke uncommon apps requesting high permissions, but only when authorized by an administrator. How do you scope the policy?
**A:** Set the policy based on group memberships of the users who authorized the apps — e.g., only if the user is a member of the Administrators group.


---

# Module 12: Implement token customizations - Flashcards

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

### Token Customizations

**Q:** What object in Microsoft Entra ID represents a set of rules enforced on individual applications or on all applications in an organization?
**A:** A policy object. Each policy type has a unique structure with properties applied to the objects it's assigned to.

**Q:** You need to restrict authentication sessions for users accessing sensitive data from an unmanaged device. Which feature should you use?
**A:** Conditional Access sign-in frequency / authentication session management controls, targeting specific use cases without affecting all users.

**Q:** What does the refresh token sliding window lifetime define?
**A:** The maximum time period before which a refresh token can be used to acquire a new access token; after the period elapses, the user is forced to reauthenticate.

**Q:** What are three uses of optional claims in Microsoft Entra ID applications?
**A:** Select additional claims to include in tokens, change the behavior of certain claims returned in tokens, and add access custom claims for the application.

**Q:** Why are optional claims especially valuable when moving from v1.0 to v2.0 tokens?
**A:** v2.0 tokens are smaller for performance; several claims formerly included by default are no longer present and must be requested per application.

**Q:** Are optional claims supported in SAML tokens?
**A:** Yes — optional claims are supported in v1.0 and v2.0 format tokens, and in SAML tokens.

**Q:** For which entities can you set token lifetimes in Microsoft Entra ID?
**A:** All apps in the organization, a multitenant application, or a specific service principal in your organization.

**Q:** What happens to a default policy when a policy with higher priority is assigned to an application?
**A:** The default policy is overridden by the higher-priority policy for that application.

### Consent Settings

**Q:** By default, which permissions can users consent to?
**A:** Only permissions that don't require administrator consent (e.g., access to their own mailbox, not unfettered read/write to all org files).

**Q:** What is the recommended way to reduce the risk of malicious apps tricking users into granting access?
**A:** Allow user consent only for applications published by a verified publisher.

**Q:** What do app consent policies describe?
**A:** Conditions that must be met before an app can be consented to, including conditions on the app requesting access and the permissions requested.

**Q:** What happens when user consent is disabled (users can't grant permissions)?
**A:** Users can still sign in to apps they previously consented to or that admins consented to, but can't consent to new permissions or apps; only roles with consent permission can consent.

**Q:** Under the "verified publishers or your organization" consent setting, which permissions can users consent to?
**A:** Only permissions you have classified as low impact; you must classify permissions to choose which ones users can consent to.

**Q:** What is risk-based step-up consent?
**A:** If Microsoft detects a risky end-user consent request, the request requires step-up to admin consent instead. Enabled by default; only changes behavior when end-user consent is enabled.

**Q:** Which error appears when a risky consent request is blocked and the admin consent request workflow is not enabled?
**A:** AADSTS90094 — the app needs permission to access resources in your organization that only an admin can grant.

**Q:** What audit event details are logged when a risky application is detected during consent?
**A:** Category: ApplicationManagement; Activity Type: Consent to application; Status Reason: Risky application detected.

### Application Proxy

**Q:** What is Microsoft Entra application proxy?
**A:** A Microsoft Entra ID feature enabling users to access on-premises web applications from a remote client, via a cloud Application Proxy service and an on-premises connector.

**Q:** What are the two components of Application Proxy and where do they run?
**A:** The Application Proxy service (runs in the cloud) and the Application Proxy connector (runs on an on-premises server).

**Q:** What does Application Proxy replace the need for?
**A:** A VPN or reverse proxy, for giving remote users access to internal resources.

**Q:** Should Application Proxy be used for internal users on the corporate network?
**A:** No — it's not intended for internal users; unnecessary use can introduce unexpected and undesirable performance issues.

**Q:** Which types of applications work with Application Proxy?
**A:** Web apps using Integrated Windows Authentication, form-based or header-based access, web APIs exposed to rich apps, apps behind a Remote Desktop Gateway, and rich client apps integrated with MSAL.

**Q:** After successful sign-in, what does the Application Proxy service retrieve from the token sent by the client?
**A:** The user principal name (UPN) and security principal name (SPN); it then sends the request to the Application Proxy connector.

**Q:** In the Application Proxy flow, who performs any additional authentication required for single sign-on?
**A:** The Application Proxy connector, on behalf of the user, before sending the request to the on-premises application.

### Custom SaaS Apps for SSO

**Q:** Where are pre-configured apps published that can be set up with minimal effort?
**A:** The Microsoft Entra ID App Gallery.

**Q:** OAuth vs. OpenID Connect — which is used for authorization and which for authentication?
**A:** OAuth is used for authorization; OpenID Connect (OIDC) is used for authentication. OIDC is built on top of OAuth 2.0.

**Q:** OAuth vs. SAML — which is used for authorization and which for authentication?
**A:** OAuth is used for authorization; SAML is used for authentication.

**Q:** OpenID Connect vs. SAML — when is each commonly used?
**A:** SAML is commonly used with identity providers like AD FS federated to Microsoft Entra ID (enterprise applications); OIDC is commonly used for purely cloud apps such as mobile apps, websites, and web APIs.

**Q:** Which industry-standard protocols does the Microsoft identity platform support for developers?
**A:** OAuth 2.0 and OpenID Connect, plus open-source libraries for different platforms.

**Q:** What benefit does SSO delegation of authentication to Microsoft Entra ID enable?
**A:** Scenarios such as Conditional Access policies requiring a specific location or multifactor authentication; users sign in once and are signed in to all web apps sharing the centralized directory.

### Application-Based User Provisioning

**Q:** What does app provisioning refer to in Microsoft Entra ID?
**A:** Automatically creating user identities and roles in cloud (SaaS) apps users need access to, including maintenance and removal as status or roles change.

**Q:** What does automatic deprovisioning do?
**A:** Automatically deactivates accounts in the right systems when people leave a team or organization.

**Q:** What does manual provisioning mean for a gallery app?
**A:** No automatic Microsoft Entra provisioning connector exists; accounts must be created manually, e.g., via the app's admin portal or a spreadsheet upload.

**Q:** How are apps supporting automatic provisioning designated in the Microsoft Entra ID gallery?
**A:** With a Provisioning icon; the supported provisioning mode is also visible on the Provisioning tab after adding the app to Enterprise apps.

**Q:** What problem does SCIM solve?
**A:** Apps expose proprietary user/group APIs that implement the same actions differently (endpoints, methods, schemas); SCIM provides a common user schema for provisioning and deprovisioning.

**Q:** Which two endpoints does the SCIM specification standardize?
**A:** A /Users endpoint and a /Groups endpoint, using common REST verbs and a predefined schema for common attributes.

**Q:** Which SCIM version defines the standard user object schema and REST APIs?
**A:** SCIM 2.0.

**Q:** How does SCIM combine with federation standards?
**A:** Used with SAML or OpenID Connect, SCIM provides an end-to-end, standards-based solution for access management.

**Q:** Which provisioning capability helps integrate systems where users already exist in the target system?
**A:** Seamless deployment in brown field scenarios — matching existing identities between systems.

### Monitoring and Auditing

**Q:** What view does the usage and insights report provide?
**A:** An application-centric view of sign-in data: top used applications, apps with the most failed sign-ins, and top sign-in errors per application.

**Q:** What is the portal path to access the usage and insights report?
**A:** Microsoft Entra admin center → Identity → Applications → Enterprise applications → Activity section → Usage & insights.

**Q:** Which roles can access Microsoft Entra audit logs?
**A:** Security Administrator, Security Reader, Report Reader, Global Reader, or Administrator roles.

**Q:** Where do you access the audit report in Microsoft Entra ID?
**A:** Select Audit logs in the Monitoring section of Microsoft Entra ID.

**Q:** What does the default audit log list view show?
**A:** Date and time, the service that logged it, category and activity name (what), status (success/failure), target, and initiator/actor (who).

**Q:** How can you customize the audit log list view?
**A:** Click Columns in the toolbar to display other fields or remove displayed fields.

**Q:** Where do you find a filtered audit view for application-related data?
**A:** Audit logs in the Activity section of the Enterprise applications screen, with Enterprise applications preselected as the Application Type.

### Application Collections

**Q:** Which license is required to set up app collections in the My Apps portal?
**A:** Microsoft Entra ID Premium P1 or P2.

**Q:** What does a collection essentially do for users?
**A:** Applies a filter to applications a user can already access, so the user sees only apps in the collection assigned to them, displayed on a separate tab.

**Q:** What is the portal path to create an admin application collection?
**A:** Microsoft Entra admin center → Identity → Applications → Enterprise Applications → Manage → App Launchers → New collection.

**Q:** If you assign users or groups as owners of an admin collection, how can they manage it?
**A:** Only through the Azure portal.

**Q:** What is the URL of the My Apps portal?
**A:** https://myapps.microsoft.com

**Q:** Does the My Apps portal require an Azure or Microsoft 365 subscription?
**A:** No — it is separate from the Azure portal and doesn't require an Azure subscription or Microsoft 365 subscription.

**Q:** What can users do in the My Apps portal?
**A:** Discover apps they have access to, request new apps for self-service, create personal collections, and manage access to applications.

**Q:** How do you create a collection via the My Apps portal?
**A:** Select the ellipsis (...) on the apps screen → Manage collections → Create collection → + Add apps → Add selected apps → name the collection → Create collection.


---

# Module 13: Plan your line of business application registration strategy - Flashcards

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

### Application Registration Strategy

**Q:** What are the two representations of an application in Microsoft Entra ID?
**A:** Application objects and service principals.

**Q:** Where do you manage application objects in the Azure portal?
**A:** Through the App registrations experience.

**Q:** Where do you manage service principals in the Azure portal?
**A:** Through the Enterprise applications experience.

**Q:** How many application objects can an application have, and where does it exist?
**A:** At most one application object, which exists only in its home directory (even for multitenant apps).

**Q:** What does a service principal represent in your directory?
**A:** The instance of the application in your directory; it governs the app's connection to Microsoft Entra ID.

**Q:** Which property links a service principal back to its application object?
**A:** The application ID property.

**Q:** Which service principal records are only available in the UI (not programmatically)?
**A:** Claims transformation rules and attribute mappings (user provisioning).

**Q:** Which API requires an application object before creating a service principal?
**A:** Microsoft Graph API (PowerShell can create service principals without an application object).

**Q:** Who creates the first service principal when a third-party app is used in your tenant?
**A:** The first user to give consent to the application causes the service principal to be created.

**Q:** Which roles can be assigned to allow users to add applications to Microsoft Entra ID?
**A:** Application Administrator and Cloud Application Administrator.

**Q:** By default, can regular users register application objects in Microsoft Entra ID?
**A:** Yes — by default all users have rights to register applications they develop and consent on their own behalf.

**Q:** Which setting prevents users from consenting to apps accessing company data on their behalf?
**A:** In Enterprise applications > User settings, set "Users can consent to apps accessing company data on their behalf" to No.

**Q:** Which setting prevents users from registering their own applications?
**A:** In Microsoft Entra ID > User settings, set "Users can register applications" to No.

**Q:** What happens to a user's access to an application when they leave the organization?
**A:** They automatically lose access to their account in the application.

**Q:** Where can you audit how an application was added to the directory?
**A:** In the Audit Reports within the Azure portal.

### Single-Tenant vs Multitenant Apps

**Q:** Single-tenant vs multitenant app: what is the difference?
**A:** Single-tenant apps are only available in the tenant where they were registered (home tenant); multitenant apps are available in the home tenant and other tenants.

**Q:** Which audience setting targets only internal organizational users?
**A:** "Accounts in this directory only" (single tenant).

**Q:** Which audience setting targets business or educational customers using Microsoft 365?
**A:** "Accounts in any Microsoft Entra directory" (multitenant).

**Q:** Which audience setting targets the widest set of Microsoft accounts, including personal accounts (Skype, Xbox)?
**A:** "Accounts in any Microsoft Entra directory and personal Microsoft accounts" (multitenant).

**Q:** Name two best practices for building multitenant apps.
**A:** Test the app in a tenant with Conditional Access policies configured, and follow least user access (request only needed permissions).

### Implementing App Registration

**Q:** What does registering an application establish?
**A:** A trust relationship between your app and the Microsoft identity platform (unidirectional — the app trusts the platform, not vice versa).

**Q:** What is a redirect URI?
**A:** The location where the Microsoft identity platform redirects a user's client and sends security tokens after authentication.

**Q:** Which portal path is used to configure platform settings and redirect URIs for an app registration?
**A:** App registrations > (app) > Manage > Authentication > Platform configurations > Add a platform.

**Q:** Which platform tile is used for a client-side web app built in JavaScript, Angular, Vue.js, React.js, or Blazor WebAssembly?
**A:** Single-page application.

**Q:** Which platform tile is used for mobile apps not using the latest MSAL or a broker, and for desktop apps?
**A:** Mobile and desktop applications.

**Q:** Which platform tiles generate a redirect URI automatically for you?
**A:** iOS/macOS (from Bundle ID) and Android (from package name and signature hash).

**Q:** Which credential type is recommended for production confidential client apps?
**A:** Certificates (public key) — higher assurance than client secrets.

**Q:** Which file types are accepted when adding a certificate credential?
**A:** .cer, .pem, and .crt.

**Q:** What is a client secret also known as, and when is it typically used?
**A:** An application password; easier to use but less secure — often used during development.

**Q:** What must you do immediately after creating a client secret?
**A:** Record the secret's value — it is never displayed again after you leave the page.

**Q:** Does a web API app registration need a redirect URI?
**A:** No — no user signs in interactively, so redirect URI and platform settings are skipped.

**Q:** What is the purpose of an Application ID URI when exposing an API?
**A:** It acts as the prefix for the scopes referenced in the API's code and must be globally unique (default form: api://).

**Q:** What is the full string of a scope composed of?
**A:** The web API's Application ID URI concatenated with the scope name (e.g., https://contoso.com/api/Employees.Read.All).

**Q:** What is a pre-authorized client application (PCA)?
**A:** A trusted client app added under Authorized client applications, so users are not prompted for consent to the selected scopes.

### Scopes and Permissions

**Q:** Delegated permissions vs application permissions?
**A:** Delegated permissions are used by apps with a signed-in user present (app acts as the user); application permissions are used by apps without a signed-in user (background services/daemons) and only an admin can consent.

**Q:** What are the effective permissions for delegated permissions?
**A:** The least privileged intersection of the granted delegated permissions and the privileges of the signed-in user — the app can never have more privileges than the user.

**Q:** What are the effective permissions for application permissions?
**A:** The full level of privileges implied by the permission (e.g., User.ReadWrite.All application permission can update every user's profile).

**Q:** Which OAuth 2.0 concept represents a permission as a string value (e.g., Calendars.Read)?
**A:** Scopes (also called permissions).

**Q:** Which OpenID Connect scopes are supported by the Microsoft identity platform?
**A:** openid, email, profile, and offline_access (address and phone are not supported).

**Q:** Which OIDC scope must an app request to perform sign-in and receive the sub claim?
**A:** openid.

**Q:** Which scope gives an app refresh tokens for long-term access on behalf of a user?
**A:** offline_access ("Maintain access to data you have given it access to").

**Q:** Which two permissions are automatically included in the initial consent to an application?
**A:** offline_access and user.read.

**Q:** For SPAs, is the refresh token always provided?
**A:** Yes — when using a Single Page Application, the refresh token is always provided.

### Admin Consent

**Q:** Which roles can grant tenant-wide admin consent?
**A:** Privileged Role Administrator (also Company Administrator, or a custom role including the permission to grant permissions to applications).

**Q:** What is the warning about granting tenant-wide admin consent via App registrations?
**A:** It revokes any permissions previously granted tenant-wide; permissions granted by users on their own behalf are not affected.

**Q:** What is the URL format for granting tenant-wide admin consent?
**A:** https://login.microsoftonline.com/{tenant-id}/adminconsent?client_id={client-id}

**Q:** What are admin-restricted permissions?
**A:** High-privilege permissions (e.g., User.Read.All, Directory.ReadWrite.All, Groups.Read.All) that organizational users cannot consent to — only an administrator can grant them.

**Q:** Which endpoint must be used to request application permissions?
**A:** The admin consent endpoint (application permissions cannot be requested using the authorize endpoint).

**Q:** In the admin consent endpoint request, which tenant value should you NOT use and why?
**A:** "common" — personal accounts can't provide admin consent except in the context of a tenant; use the tenant ID when possible.

**Q:** What is the /.default scope used for?
**A:** To request the statically defined permissions configured in the app registration's API permissions section.

### App Roles and Authorization

**Q:** What do app roles enable in Microsoft Entra ID?
**A:** Assigning permissions to users; when a user signs in, Microsoft Entra ID emits a roles claim for each role granted individually or via group membership.

**Q:** What are the two ways to declare app roles in the Azure portal?
**A:** The App roles UI (preview) and the app manifest editor.

**Q:** Which portal path is used to assign users and groups to app roles?
**A:** Enterprise applications > (app) > Manage > Users and groups > Add user/group.

**Q:** Which API can be used to assign users and groups to app roles programmatically?
**A:** Microsoft Graph.

### App Governance

**Q:** What is app governance?
**A:** A security and policy management add-on to Defender for Cloud Apps for OAuth-enabled apps that access Microsoft 365 data through Microsoft Graph APIs.

**Q:** What are the four capability areas app governance provides?
**A:** Insights, Governance, Detection, and Remediation.

**Q:** What must be ensured before enabling app governance sync with Defender for Cloud Apps?
**A:** Office 365 is connected in Defender for Cloud Apps and Office 365 Microsoft Entra ID apps are enabled.

**Q:** Which built-in app governance policies appear in Defender for Cloud Apps after enabling integration?
**A:** Microsoft 365 OAuth app Reputation, Microsoft 365 OAuth Phishing Detection, and Microsoft 365 OAuth App Governance.


---

# Module 14: Plan for app registration - Flashcards

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

### Introduction to App Registration

**Q:** What is application registration in Microsoft Entra ID?
**A:** The process of ensuring the identity system is aware of what applications are used, confirming users have access to the app and the app has access to needed resources.

**Q:** You need to customize the branding of your application's sign-in dialog. Which process enables this?
**A:** Registering the application with Microsoft Entra ID, which provides an identity configuration integrating it with the Microsoft identity platform.

**Q:** Which identity platform capabilities does app registration enable?
**A:** Custom branding, tenant configuration (single/multitenant), permission management (scopes), and secure authentication (secrets, certificates, managed identities).

**Q:** What credential options are recommended for confidential client applications?
**A:** Client secrets, certificates, or managed identities — with certificates preferred over passwords and managed identities eliminating credential management entirely.

---

### Single-Tenant vs Multitenant Apps

**Q:** What is a single-tenant application?
**A:** An app available only to users and resources in the tenant where it was registered (its home tenant).

**Q:** What is a multitenant application?
**A:** An app available to users in both its home tenant and other tenants; used intentionally when needed.

**Q:** When is a service principal created for a multitenant app in other tenants?
**A:** At app registration in the source tenant, and during the first user authentication in other tenants.

**Q:** Which supported account type setting makes an app single-tenant?
**A:** "Accounts in this directory only" — all user and guest accounts in your directory can use the app or API.

**Q:** Which audience setting allows any work or school account from any Microsoft Entra directory?
**A:** "Accounts in any Microsoft Entra directory" — a multitenant configuration.

**Q:** Which audience setting allows work/school accounts AND personal Microsoft accounts (Skype, Xbox, Outlook.com)?
**A:** "Accounts in any Microsoft Entra directory and personal Microsoft accounts" — a multitenant configuration.

---

### Application Objects and Service Principals

**Q:** Application object vs service principal — what is the difference?
**A:** The application object is the definition (template/blueprint) of the app in its home tenant; the service principal is an instance of the app created in each tenant where the app is used.

**Q:** What is the relationship between an application object and its service principals?
**A:** One-to-one with the software application; one-to-many with its corresponding service principal objects.

**Q:** If you register an app in the Microsoft Entra admin center, what objects are created?
**A:** An application object AND a service principal object, both automatically created in your home tenant.

**Q:** If you create an application using Microsoft Graph APIs, what extra step is required?
**A:** Creating the service principal object is a separate step (unlike admin center registration, which creates both automatically).

**Q:** What three aspects of an application does the application object describe?
**A:** How the service issues tokens to access the app, the resources the app might need to access, and the actions the app can take.

**Q:** What are the three types of service principals?
**A:** Application (local instance of a global app object), Managed identity (recommended for Azure workloads), and Legacy (apps created before app registrations).

**Q:** Which service principal type is preferred for Azure-hosted applications and why?
**A:** Managed identity — it eliminates the need for developers to manage credentials, reducing security risks and operational overhead.

**Q:** What does a service principal include that records local permissions and policies?
**A:** Records of local user/group app-role assignments, user and admin consent grants, local policies including Conditional Access policy, and alternate local settings.

**Q:** You delete an application object in its home tenant. What happens to its service principal?
**A:** The home tenant service principal is also deleted; however, restoring the application object does NOT restore the service principal.

**Q:** For a multitenant app, what happens to service principals in other tenants if the home tenant's application object is deleted?
**A:** Service principals in other tenants remain independent of the home tenant's application object.

**Q:** Where in the Microsoft Entra admin center do you find service principals associated with an app registration?
**A:** In the application registration overview, select "Managed application in local directory."

**Q:** What is consent in the Microsoft identity platform?
**A:** The process of a resource owner granting authorization for a client app to access protected resources, under specific permissions, on behalf of the resource owner.

---

### Creating App Registrations

**Q:** What is the minimum role required to create an app registration in the Microsoft Entra admin center?
**A:** At least the Application Developer role.

**Q:** What are the portal steps to register a new app?
**A:** Microsoft Entra admin center → Identity → Applications → App registrations → New registration → enter Name, choose Supported account types → Register.

**Q:** Which two IDs should you record from the app registration Overview page?
**A:** The Application (client) ID and the Directory (tenant) ID.

**Q:** Which flow does the Single-page application platform configuration automatically enable?
**A:** Authorization code flow with PKCE (and CORS) — more secure than the legacy implicit grant flow.

**Q:** When adding a SPA platform, which checkboxes should you NOT select under Implicit grant and hybrid flows?
**A:** Neither checkbox — legacy implicit grant patterns are no longer recommended.

**Q:** New app registrations are hidden from users by default. How do you make an app visible on the My Apps page?
**A:** Go to Enterprise apps → Properties and set "Visible to users?" to Yes.

---

### Configure App Authentication

**Q:** Where are platform-specific settings like redirect URIs configured?
**A:** Under Platform configurations in the app registration's Authentication blade (App registrations → your app → Manage → Authentication).

**Q:** Which platforms require you to manually enter a redirect URI?
**A:** Web and Single-page application platforms; mobile and desktop platforms can use generated redirect URIs.

**Q:** What must you enter to configure the iOS/macOS platform, and is a redirect URI manual?
**A:** The app Bundle ID (found in Build Settings or Xcode Info.plist); a redirect URI is automatically generated.

**Q:** What must you enter to configure the Android platform?
**A:** The app Package name (from AndroidManifest.xml) and a generated Signature hash; the redirect URI is auto-generated.

**Q:** What is a redirect URI (reply URL)?
**A:** The location where the authorization server sends the user (and the authorization code or access token) after successful authorization.

**Q:** What scheme must redirect URIs use, and what exception exists?
**A:** They must begin with https, with exceptions for localhost redirect URIs during development.

**Q:** Are redirect URIs case-sensitive?
**A:** Yes — they must match the case of the URL path of your running application.

**Q:** How is the trailing slash handled for redirect URIs?
**A:** URIs without a path segment are returned WITH a trailing slash; URIs with a path segment are NOT appended with a trailing slash.

**Q:** Which special characters are NOT supported in redirect URIs?
**A:** ! $ ' ( ) , ;

---

### Configure API Permissions

**Q:** Which authorization protocol does the Microsoft identity platform implement?
**A:** OAuth 2.0 — allowing an external app to access web-hosted resources on behalf of a user.

**Q:** What is an Application ID URI?
**A:** The resource identifier of any web-hosted resource that integrates with the Microsoft identity platform.

**Q:** What are permissions (scopes) in the context of a resource/API?
**A:** Sets that divide the functionality of a resource into smaller chunks, giving the resource fine-grained control over its data and exposed functionality.

**Q:** Delegated permissions vs application permissions — how do delegated permissions operate?
**A:** On behalf of the signed-in user; the app can only access data the user themselves could access.

**Q:** Portal path to add delegated Microsoft Graph permissions to an app?
**A:** App registrations → your app → API permissions → Add a permission → Microsoft Graph → Delegated permissions.

**Q:** Which four basic OpenID Connect scopes are commonly requested by most applications?
**A:** email, offline_access, openid, and profile.

**Q:** What does the offline_access scope enable?
**A:** Maintaining access to data you gave it access to — enabling refresh tokens for long-term access.

**Q:** How can an administrator eliminate the need for individual user consent to an app?
**A:** By granting admin consent on behalf of all users in the organization.

**Q:** What permission principle should developers follow when requesting API permissions?
**A:** Least privilege — request only the permissions needed for the application to function.

---

### App Roles

**Q:** What is an app role?
**A:** A custom claim applied to users, groups, or applications that appears in the token generated when a user authenticates, used for authorization in the app.

**Q:** In which token types does the roles claim appear?
**A:** ID tokens for applications and access tokens for APIs.

**Q:** What is a key licensing-related advantage of app roles over group claims?
**A:** App roles avoid group overage issues and don't require Microsoft Entra ID P1 licensing.

**Q:** When creating an app role for users and groups, which Allowed member types value do you select?
**A:** Users/Groups.

**Q:** What baseline role should you always define when creating app roles?
**A:** A baseline user role with no elevated authorization permissions (e.g., "user" or "reader").

**Q:** Why is a baseline user role important when app assignment is required?
**A:** Without it, all assigned users and groups would automatically get elevated roles like "admin," violating the principle of least privilege.

**Q:** What is a code-level advantage of app roles over group-based authorization?
**A:** The app can simply check for a role claim like "admin" instead of iterating through group GUIDs and mapping group IDs to permissions.

---

### Summary / Security Best Practices

**Q:** What is the preferred credential type for application (workload identity) authentication?
**A:** Certificates instead of passwords; for Azure workloads, managed identities are preferred as they eliminate credential management entirely.

**Q:** What is a legacy service principal and what should be done with it?
**A:** A service principal representing an app created before app registrations existed or via legacy experiences; it should be migrated to modern app registrations when possible.


---

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


---

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


---

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


---

# Module 18: Analyze and investigate sign-in logs to troubleshoot access issues - Flashcards

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

### Reporting Architecture

**Q:** Which three log types make up the "Activity" component of the Microsoft Entra ID reporting architecture?
**A:** Sign-ins, Audit logs, and Provisioning logs.

**Q:** What is a risky sign-in?
**A:** An indicator for a sign-in attempt by someone who isn't the legitimate owner of a user account.

**Q:** What is a risky user?
**A:** An indicator that a user account might have been compromised.

**Q:** Which built-in roles can access sign-in and risk reporting data?
**A:** Security Administrator, Security Reader, Global Reader, and Report Reader.

**Q:** Can non-admin users access sign-in data?
**A:** Yes — any user (non-admin) can access their own sign-ins.

---

### Sign-in Logs

**Q:** Which license is required to access the sign-in activity report?
**A:** None specifically — the sign-in activity report is available in all editions of Microsoft Entra ID (also via Microsoft Graph API).

**Q:** How long does it take for sign-in records to appear in the portal?
**A:** Up to two hours.

**Q:** You need to view service-to-service (non-interactive) authentication in the Sign-ins report. What should you tell the admin?
**A:** Not possible — the sign-ins report only displays interactive sign-ins; non-interactive sign-ins are not displayed.

**Q:** You need to export sign-in logs to work with data outside the Azure portal. Which formats and limits apply?
**A:** Download as CSV or JSON, up to the most recent 250,000 records (constrained by report retention policies).

**Q:** In sign-in logs, what does Conditional Access status "Failure" mean?
**A:** The sign-in satisfied the user and application condition of at least one CA policy, and grant controls were not satisfied or set to block access.

**Q:** In sign-in logs, what does Conditional Access status "Not applied" mean?
**A:** No policy applied to the user and application during the sign-in.

**Q:** Which sign-in log filter shows whether a user signed in via browser, mobile app/desktop client, or Exchange ActiveSync?
**A:** The Client app filter.

**Q:** Why might an IP address map to a wrong physical location in Microsoft Entra reports?
**A:** Location conversion is best effort; mobile providers and VPNs issue IP addresses from central pools far from the client device.

**Q:** You need an application-centric view of sign-in data for your top three applications. Where do you look?
**A:** Enterprise applications > Overview > last 30 days report (app-usage graphs, weekly aggregation, default 30 days).

**Q:** Where is the only place to get a full view of Microsoft 365 activity logs?
**A:** The Microsoft 365 admin center (also programmatically via Office 365 Management APIs).

**Q:** What is the default time period for the Identity Protection user sign-in graph?
**A:** 30 days (weekly aggregations of sign-ins).

---

### Audit Logs

**Q:** Where do you access the Microsoft Entra audit logs report?
**A:** Microsoft Entra ID > Monitoring > Audit logs.

**Q:** What does an audit log default list view show?
**A:** Date/time, service that logged it, category and activity name (what), status (success/failure), target, and initiator/actor (who).

**Q:** Which audit log category is preselected when viewing audit logs from the Users tab?
**A:** UserManagement.

**Q:** Which audit log category is preselected when viewing audit logs from the Groups tab?
**A:** GroupManagement.

**Q:** Which filter is preselected when viewing audit logs from Enterprise applications?
**A:** Enterprise applications as the Application Type.

**Q:** What are the possible values of the audit log Status filter?
**A:** All, Success, Failure.

**Q:** Which audit log filters are case-sensitive?
**A:** Target and Initiated by (Actor) — both name and UPN are case-sensitive.

**Q:** What are the Date range filter options for audit logs?
**A:** 7 days, 24 hours, or Custom (with configurable start and end time).

**Q:** How can you retrieve the full list of audit activities programmatically?
**A:** Via the Graph API: graph.windows.net/<tenantdomain>/activities/auditActivityTypesV2?api-version=beta

---

### Microsoft Sentinel Integration

**Q:** What is Microsoft Sentinel?
**A:** A scalable, cloud-native SIEM and SOAR solution that collects, detects, investigates, and responds to threats across the enterprise.

**Q:** Which license is required to ingest sign-in logs into Microsoft Sentinel?
**A:** Microsoft Entra ID P1 or P2.

**Q:** Which licenses are sufficient to ingest non-sign-in Microsoft Entra log types into Sentinel?
**A:** Any Microsoft Entra ID license (Free/O365/P1/P2).

**Q:** Which two roles must a user have to connect Microsoft Entra logs to Microsoft Sentinel?
**A:** Microsoft Sentinel Contributor (on the workspace) and Security Administrator (on the tenant streaming the logs).

**Q:** Which log types can be streamed via the Microsoft Sentinel Microsoft Entra ID connector?
**A:** Sign-in logs and audit logs.

---

### Exporting to Third-Party SIEM

**Q:** What is the Microsoft recommended approach for integrating Azure with SIEM tools?
**A:** Route data via Azure Monitor to Azure Event Hubs, where SIEM connectors consume the data.

**Q:** Which tool should a Splunk customer use for SIEM integration?
**A:** The Azure Monitor Add-On for Splunk (migrate away from AzLog).

**Q:** Which components should IBM QRadar customers use for Azure integration?
**A:** Microsoft Azure DSM and Microsoft Azure Event Hubs Protocol (from the IBM support website).

**Q:** Which Microsoft Entra log type was directly integrated with AzLog but not yet available in Azure Monitor?
**A:** Microsoft Entra logs.

**Q:** What is AzLog?
**A:** The Azure Log Integration tool, used to consolidate, translate, and forward logs from Azure services to a SIEM tool before Azure Monitor existed.

---

### Workbooks and Usage & Insights

**Q:** Which license is required to access the usage and insights report?
**A:** Microsoft Entra ID P1 or P2.

**Q:** Which roles can access the usage and insights report?
**A:** Security Administrator, Security Reader, or Report Reader.

**Q:** Where do you find the usage and insights report?
**A:** Azure portal > Microsoft Entra ID > Enterprise applications > Activity > Usage and insights.

**Q:** What can you sort by in the usage and insights report?
**A:** Number of successful sign-ins, failed sign-ins, and success rate.

---

### Identity Secure Score

**Q:** What is the identity secure score?
**A:** A percentage indicating how aligned your identity security is with Microsoft's best practice recommendations.

**Q:** Which license is required for identity secure score?
**A:** None — it's available in all editions of Microsoft Entra ID.

**Q:** What is the portal path to access the identity secure score?
**A:** Azure portal > Microsoft Entra ID > Security > Identity Secure Score.

**Q:** What does the identity secure score dashboard's comparison graph show?
**A:** How your score compares to other tenants in the same industry and of similar size.

**Q:** How are identity secure score controls scored?
**A:** Two ways — binary (100% if configured per recommendation) or as a percentage of total configuration (partial credit, e.g., MFA for some users).

**Q:** Besides configuring security features, what other task type improves your secure score?
**A:** Security-related tasks, like reading reports.
