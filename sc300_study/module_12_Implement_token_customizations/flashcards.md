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
