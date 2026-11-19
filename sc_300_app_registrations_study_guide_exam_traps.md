# SC-300 Exam Prep: Plan and Implement App Registrations

## 1. Planning & Creating App Registrations (Core Concepts & Traps)

### A. Supported Account Types (Audience)
When creating an App Registration, you must define who can sign in (this determines the `signInAudience` property in the manifest):
1. **Accounts in this organizational directory only (Single tenant):**
   * Only users/guests in your specific Entra tenant can sign in.
   * Endpoint uses your specific `{tenant-id}` (e.g., `https://login.microsoftonline.com/{tenant-id}/v2.0`).
2. **Accounts in any organizational directory (Any Microsoft Entra ID tenant - Multitenant):**
   * Any work or school account from *any* Entra tenant can sign in.
   * Endpoint uses `/organizations` or `/common`.
   * > **Exam Trap:** For a multitenant app to work in *other* organizations, those organizations must grant consent, which automatically creates an **Enterprise Application (Service Principal)** in *their* tenant!
3. **Accounts in any organizational directory and personal Microsoft accounts (e.g., Skype, Xbox):**
   * Supports both Entra work/school accounts and personal MSAs (`outlook.com`, `hotmail.com`).
   * Endpoint uses `/common`.
4. **Personal Microsoft accounts only:**
   * Endpoint uses `/consumers`.

### B. Who Can Create App Registrations? (RBAC & Settings Traps)
* **Tenant Setting:** `Entra ID -> User settings -> App registrations -> Users can register applications` (`Yes` by default).
* If set to **`No`** (Best Practice), ONLY users with these roles can register apps:
  * **Application Developer** (Least-privileged role! Automatically becomes **Owner** of the newly registered app so they can manage it, but *cannot* manage other apps).
  * **Cloud Application Administrator** / **Application Administrator** / **Global Administrator**.

### C. Key Identifiers to Memorize
* **Application (client) ID:** Globally unique GUID identifying the app across *all* tenants.
* **Object ID:** Unique GUID of the *Application Object* in the home tenant (different from the *Service Principal Object ID* in Enterprise Apps!).
* **Directory (tenant) ID:** GUID of the Entra tenant where the app is registered.

---

## 2. Configure App Authentication (Platforms, Tokens & Credentials)

### A. Platform Configurations & Redirect URIs
You must select the correct platform architecture so Entra ID enforces the right OAuth 2.0 / OIDC security rules:

| Platform Type | App Architecture | OAuth Flow & Security Rules (Exam Traps) |
| :--- | :--- | :--- |
| **Web** | Server-side web apps (ASP.NET, Node.js, Java) | **Confidential client** (can safely store a Client Secret/Cert on the backend server). Uses Authorization Code flow. |
| **Single-page application (SPA)** | Client-side browser apps (React, Angular, Vue) | **Public client** (runs in browser; CANNOT safely store secrets!). MUST use **Authorization Code Flow with PKCE** (replaces legacy Implicit Grant). |
| **Mobile and desktop applications** | Native installed apps (iOS, Android, Windows desktop) | **Public client** (runs on user's device; CANNOT store secrets!). Uses Authorization Code Flow with PKCE + system browser/broker (WAM). |

* > **Redirect URI Exam Traps:**
  * Redirect URIs **MUST use `https://`**, with **ONE exception**: `http://localhost` (allowed for local developer testing!).
  * Wildcards (e.g., `https://*.contoso.com`) are **NOT supported** in App Registration Redirect URIs.

### B. Implicit Grant and Hybrid Flows (Legacy Trap!)
* **Access tokens (used for implicit flows)** and **ID tokens (used for implicit and hybrid flows)** checkboxes:
  * **Default & Best Practice:** Leave **UNCHECKED**! Modern SPAs should use **Auth Code + PKCE**, NOT Implicit Grant (Implicit flow exposes tokens in the browser URL fragment `#`).

### C. Application Credentials (How Confidential Apps Prove Their Identity)
Used by daemon apps, background services, and server-side Web apps to request tokens:

1. **Client Secrets (Application Passwords):**
   * String password generated in Entra ID (max lifetime: **24 months / 2 years**).
   * > **Exam Trap:** The secret **Value** is displayed **ONLY ONCE** immediately upon creation! If you leave the blade without copying it, you cannot retrieve it later (you must generate a new secret). Least secure option.
2. **Certificates (Recommended over Secrets):**
   * Upload public key (`.cer`, `.pem`, `.crt`); app signs assertions using its private key.
   * Much stronger security than client secrets.
3. **Federated Identity Credentials (Passwordless / Zero Secrets - BEST PRACTICE!):**
   * Allows external workloads (e.g., **GitHub Actions, Azure DevOps, Kubernetes/AKS pods, Google Cloud, AWS**) to exchange their own OIDC token for an Entra ID access token **with ZERO secrets or certificates stored anywhere**!
   * > **Exam Trap:** If a question asks how a GitHub Action or Kubernetes workload should authenticate to Entra ID/Azure *without managing or rotating secrets/certificates* -> Pick **Federated Identity Credentials**!

### D. Advanced Settings (`Authentication -> Advanced settings`)
* **Allow public client flows (`Yes` / `No`):**
  * Set to `Yes` ONLY when using **Device Code Flow** (for headless/input-constrained devices like smart TVs/CLI tools) or **ROPC (Resource Owner Password Credentials)** (legacy, blocks MFA—avoid unless explicitly required!).
* **Supported account types:** Can be changed *after* creation (e.g., converting Single-tenant to Multitenant), provided all Redirect URIs meet multitenant rules.

---

## 3. Configure API Permissions (Delegated vs. Application & Consent)

### A. Permission Types (CRITICAL EXAM DISTINCTION!)

| Feature | Delegated Permissions (`Scopes`) | Application Permissions (`App Roles`) |
| :--- | :--- | :--- |
| **Scenario** | App acts **on behalf of a signed-in user** (interactive user present). | App runs **as itself (daemon / background service)** with **NO signed-in user**. |
| **Effective Access** | Intersection of the **App's permissions** AND the **User's own privileges** (app can never exceed what the signed-in user is allowed to do!). | Full permissions granted to the app across the entire tenant (or scoped resource), regardless of any user! |
| **Admin Consent** | Required *only* for high-privilege scopes (e.g., `Directory.ReadWrite.All`), or if tenant blocks user consent. | **ALWAYS requires Admin Consent!** (Regular users can *never* consent to Application permissions). |

### B. Configuring & Granting Consent (Exam Traps)
* **Configuring a permission** (`Add a permission -> Microsoft Graph -> Select permissions`) only *declares* what the app needs—it does **NOT** grant access yet!
* **Grant admin consent for {Tenant}:**
  * Clicking this button grants tenant-wide consent on behalf of all users, suppressing individual user consent prompts.
  * **Who can click "Grant admin consent"?**
    * **Global Administrator**
    * **Privileged Role Administrator** (can grant *any* permission, including `AppRoleAssignment.ReadWrite.All`).
    * **Application Administrator** or **Cloud Application Administrator** (can grant consent for permissions *except* Microsoft Graph app roles that grant privileged directory access like `RoleManagement.ReadWrite.Directory`!).

---

## 4. Create App Roles & Expose an API

### A. Creating App Roles (`App registrations -> App roles`)
Used to define granular RBAC roles *inside* your custom application (e.g., `Task.Read`, `Task.Write`, `Billing.Admin`):
* **Allowed member types (CRITICAL EXAM TRAP!):**
  1. **Users/Groups:** App role can be assigned to human users or security groups under *Enterprise Applications -> Users and groups*. (Emitted in the user's `roles` token claim).
  2. **Applications:** App role can be assigned to *other daemon/client applications* as an **Application Permission**!
  3. **Both (Users/Groups and Applications):** Supports both interactive user assignment and daemon app-to-app permission assignment.
* **Value:** The exact string emitted in the JWT/SAML `roles` claim (e.g., `Orders.Manager` — cannot contain spaces!).
* **Enable this app role:** Must be checked (`True`) to be assignable; uncheck (`False`) before deleting an app role!
* > **Licensing Trap:** Assigning an App Role to a **User** works in Entra ID Free; assigning an App Role to a **Security Group** requires **Entra ID P1 or P2**!

### B. Expose an API (For Custom Web APIs)
If you are registering a custom backend API that other client apps will call:
1. **Application ID URI:** Must be set first (e.g., `api://{client-id}` or verified custom domain `https://api.contoso.com`).
2. **Add a scope:** Defines **Delegated permissions** (e.g., `Files.Read`) that client apps can request on behalf of a user.
3. **Authorized client applications:** Pre-authorizes trusted first-party/internal client apps so users are **not prompted for consent** when calling this API!

---

## Quick Memory Cheat Sheet (SC-300 App Registrations)

1. **App Registration vs. Enterprise App** = App Registration is the **global blueprint** (home tenant); Enterprise App is the **local tenant instance** (SSO, assignments, Conditional Access).
2. **Least-privileged role to create App Registrations** = **Application Developer** (becomes Owner of created apps).
3. **SPA & Mobile/Desktop Apps** = Public clients; MUST use **Authorization Code Flow with PKCE** (NEVER store secrets in SPAs/mobile apps!).
4. **HTTP Redirect URI** = Allowed **ONLY for `http://localhost`**; all other URLs must be `https://` with no wildcards.
5. **Zero-secret auth for GitHub Actions / AKS / DevOps** = **Federated Identity Credentials**.
6. **Background Daemon Service (no user signed in)** = Must use **Application Permissions** (ALWAYS requires Admin Consent).
7. **Delegated Permission Effective Rights** = Intersection of **App permissions + Signed-in User's rights**.
8. **App Role "Allowed member types"** = Pick **Applications** (or **Both**) if another service/daemon app needs to call your API without a user!