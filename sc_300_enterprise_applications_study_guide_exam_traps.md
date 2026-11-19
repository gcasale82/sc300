# SC-300 Exam Prep: Enterprise Applications & Application Proxy

---

## 1. Enterprise Apps vs. App Registrations (Core Distinction)
* **App Registrations (Application Object):** The global blueprint/definition of an app created in your home tenant (defines redirect URIs, client secrets/certificates, API permissions).
* **Enterprise Applications (Service Principal):** The local instance/representation of an app in a specific tenant where you configure **SSO, User/Group Assignments, Conditional Access, and Provisioning**.
* > **Exam Trap:** Deleting an Enterprise Application removes access in that tenant; deleting the App Registration breaks the app globally across *all* tenants using it.

---

## 2. RBAC Roles for Managing Enterprise Applications (Least Privilege)
Memorize the exact role boundaries for SC-300 scenario questions:

* **Application Administrator:**
  * Can create and manage **ALL** aspects of Enterprise Apps, App Registrations, AND **Application Proxy** settings.
  * *Trap:* Cannot manage Conditional Access policies.
* **Cloud Application Administrator:**
  * Same as Application Administrator, **EXCEPT** they **CANNOT manage Application Proxy** (connectors/on-prem apps)!
  * > **Exam Trap:** If a question asks for the least-privileged role to manage SaaS apps -> **Cloud Application Administrator**. If the question mentions **Application Proxy** -> **Application Administrator**!
* **Application Developer:**
  * Can ONLY create **App Registrations** (when tenant setting *"Users can register applications"* is set to **No**).
  * Automatically becomes the **Owner** of the apps they register, allowing them to manage *only* those specific apps.
* **App Owner (Object-level permission):**
  * Non-admin user assigned as Owner of a specific Enterprise App/App Registration; can manage SSO, certificates, and assignments *only* for that app.

---

## 3. Tenant-Level vs. Application-Level Settings
### A. Tenant-Level Settings (`Enterprise applications -> User settings`)
1. **Users can register applications:** `Yes` (Default) / `No`. (Set to `No` in hardened tenants; delegate via *Application Developer* role).
2. **Users can consent to apps accessing company data on their behalf:** Controlled under *Consent and permissions*.
3. **Users can add gallery apps to My Apps:** `Yes` / `No`.
4. **Admin consent workflow:** Enables users to request admin approval when blocked by consent settings.

### B. Application-Level Settings (`Enterprise app -> Properties`)
1. **Enabled for users to sign-in?** (`Yes` / `No`) -> Set to `No` to temporarily disable SSO without deleting the app or assignments.
2. **Assignment required?** (`Yes` / `No` - Default is `No`!):
   * **`No`:** *Any* user in the tenant (including B2B guests!) can sign into the app if they have the URL (unless blocked by Conditional Access).
   * **`Yes`:** **ONLY** users/groups explicitly listed under *Users and groups* can authenticate (and only those users see the tile on `myapps.microsoft.com`).
   * > **Exam Trap:** Does *NOT* affect whether the app shows up in the My Apps portal unless visibility is toggled, AND if set to `Yes`, unassigned users receive error `AADSTS50105`.
3. **Visible to users?** (`Yes` / `No`) -> Controls whether the app icon appears on the **My Apps** portal and Microsoft 365 App Launcher. (Does *not* block direct URL access!).
4. **User assignment required for OAuth/API:** Also blocks users from consenting to the app if set to `Yes`.

---

## 4. On-Premises Apps via Microsoft Entra Application Proxy
* **Purpose:** Publishes internal on-prem web apps (HTTP/HTTPS) securely to external users without opening inbound firewall ports or using a VPN.
* **Licensing:** Requires **Microsoft Entra ID P1 or P2**.

### Architecture & Exam Traps
1. **Private Network Connector:**
   * Lightweight Windows agent installed on **Windows Server 2012 R2 or later** inside the corporate network.
   * Uses **outbound-only** connections over **TCP 443 (TLS)** (and TCP 80 for CRL checks) to Azure Service Bus. **NO inbound ports required!**
   * High Availability: Install **at least 2 connectors** in a **Connector Group** (automatic round-robin load balancing).
2. **Pre-Authentication vs. Passthrough:**
   * **Microsoft Entra ID (Recommended):** Authenticates user in the cloud (enforcing MFA & Conditional Access) *before* traffic ever touches your on-prem network!
   * **Passthrough:** Traffic passes straight to the on-prem app without Entra auth (only used if the app cannot handle redirects, e.g., legacy basic auth prompts).
3. **Backend Application SSO (Internal Auth):**
   * **Integrated Windows Authentication (IWA) via Kerberos Constrained Delegation (KCD):** Connector impersonates the user via Kerberos to sign into legacy Windows/IIS apps.
     * *Trap:* Connector server and target app server MUST be in the **same Active Directory forest** (or trusting forests) and require **Service Principal Name (SPN)** delegation configured in AD!
   * **Header-based authentication:** Connector injects HTTP headers (`X-Forwarded-User`, etc.) for apps like PingAccess/SiteMinder.
   * **Password-based / SAML SSO:** Also supported.
4. **Custom Domains & Certificates:**
   * Default external URL ends in `*.msappproxy.net`.
   * If you use a custom domain (e.g., `https://app.contoso.com`), you **MUST upload a TLS/SSL certificate (`.pfx`)** matching that custom domain and create a DNS `CNAME` pointing to `*.msappproxy.net`.

---

## 5. SaaS Apps: Single Sign-On (SSO) & Automated Provisioning
### A. SaaS SSO Protocols (Exam Selection Rules)
* **OIDC / OAuth 2.0:** Modern cloud/mobile apps, SPAs, APIs (uses JSON/JWT tokens).
* **SAML 2.0:** Traditional enterprise SaaS apps (uses XML assertions).
  * > **SAML Token Signing Certificate Trap:** SAML certificates expire (typically every 3 years)! You must renew the cert in Entra ID **AND** upload the new certificate/metadata to the SaaS provider *before* expiration, or SSO will break completely!
* **Password-based SSO (Password Vaulting):** Used for legacy web apps that don't support SAML/OIDC. Requires the **My Apps Secure Sign-in Browser Extension**; Entra securely stores and auto-fills credentials.
* **Linked SSO:** App is just a bookmark link on My Apps (authentication is handled externally by the app/AD FS).

### B. Automated User Provisioning (SCIM)
* **Protocol:** **SCIM 2.0** (System for Cross-domain Identity Management).
* **Sync Interval:** Every **40 minutes** (incremental sync).
* **Capabilities:** Automatically creates, updates, and de-provisions (soft-deletes/disables) users and groups in SaaS apps based on **Users and groups** assignments in Entra ID.
* **Scoping Filters:** Can filter provisioning by user attributes (e.g., `department EQUALS "Sales"`).
* > **Licensing Trap:** Provisioning for **Gallery apps** is included in Entra ID Free; provisioning for **Non-Gallery (custom) SCIM apps** requires **Entra ID P1/P2**!

---

## 6. Assigning Users, Groups, and App Roles
* **Direct User vs. Group Assignment:**
  * Assigning a **Security Group** to an Enterprise App requires **Microsoft Entra ID P1 or P2**! (Entra ID Free only supports direct individual user assignment).
  * **Nested Groups Trap:** Entra ID Enterprise App assignment **DOES NOT support nested groups** for SSO/Provisioning! Users must be direct members of the assigned group.
* **Custom App Roles:**
  * Defined in the **App Registration -> App roles** manifest (e.g., `Admin`, `Editor`, `Viewer`).
  * When assigning a user or group to the **Enterprise App**, you select which **App Role** they receive, which is emitted inside the `roles` claim of the SAML/JWT token!

---

## 7. User and Admin Consent Management (Critical Security Topic)
* **User Consent Settings (`Enterprise applications -> Consent and permissions -> User consent settings`):**
  1. **Do not allow user consent:** Safest; blocks all user consent (users must use Admin Consent Workflow).
  2. **Allow user consent for apps from verified publishers, for selected permissions (Recommended):** Users can ONLY consent if:
     * The publisher has a **Verified Publisher** blue badge (MPN ID verified), **AND**
     * The requested permissions are classified as **Low Impact** (configured under *Permission classifications*, e.g., `User.Read`, `openid`, `profile`, `email`).
  3. **Allow user consent for all apps:** Risky; exposes tenant to **Illicit Consent Grant** phishing attacks.
* **Admin Consent Workflow (`User settings -> Admin consent requests`):**
  * When enabled, blocked users see a **"Request approval"** screen with a justification box instead of a hard error.
  * Requests expire after **30 days** if not reviewed.
  * Reviewers (Admins or designated non-admin Reviewers) can approve/deny, **EXCEPT** reviewers **CANNOT approve requests for their own account**!

---

## 8. Application Collections (My Apps Portal Organization)
* **Purpose:** Groups apps into logical tabs/folders on the **My Apps** (`myapps.microsoft.com`) portal (e.g., *HR Apps*, *Finance Apps*, *Engineering*).
* **Licensing:** Requires **Microsoft Entra ID P1 or P2**.
* **Configuration & Traps:**
  * Created under `Enterprise applications -> Collections`.
  * You assign **Apps** to the Collection AND assign **Users/Groups** to the Collection.
  * > **CRITICAL EXAM TRAP:** Assigning a user to an **Application Collection** **DOES NOT grant them access/SSO permissions** to the apps inside it! The user must *still* be assigned directly (or via group) to each individual Enterprise Application! Collections are purely for **UI visual grouping**.

---

## Quick Memory Cheat Sheet (SC-300 Enterprise Apps)
1. **Least-Privilege Role for App Proxy** = **Application Administrator** (Cloud App Admin *cannot* manage App Proxy).
2. **Application Proxy Outbound Ports** = **TCP 443** only (no inbound firewall ports).
3. **App Proxy Kerberos SSO (KCD)** = Connector & App must be in the **same/trusting AD forest** + requires **SPN**.
4. **Group-based App Assignment** = Requires **Entra ID P1/P2**; **Nested groups are NOT supported**.
5. **"Assignment required = No"** = Any user in the tenant (including Guests) can sign in!
6. **Verified Publisher Consent** = Requires BOTH a **Verified Publisher** AND permissions marked **Low Impact** in *Permission classifications*.
7. **Admin Consent Reviewers** = Cannot approve their own consent requests; requests expire in **30 days**.
8. **Application Collections** = Visual UI grouping in My Apps ONLY; does **NOT** grant app permissions.