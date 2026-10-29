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
