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
