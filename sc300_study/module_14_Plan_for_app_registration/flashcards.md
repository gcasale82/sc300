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
