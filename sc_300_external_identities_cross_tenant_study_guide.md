# SC-300 Exam Prep: External Identities & Cross-Tenant Access

## 1. External Collaboration Settings (`External Identities -> External collaboration settings`)

Controls **who** can invite B2B guests, **which domains** are allowed/blocked, and **what guests can see** inside your directory:

### A. Guest User Access Restrictions (Visibility Trap!)

Determines what directory objects (users, groups, membership) a B2B guest can enumerate via Microsoft Graph, PowerShell, or the portal:

1. **Guest users have the same access as members (most inclusive):** Guests can enumerate all users and group memberships.

2. **Guest users have limited access to properties and memberships of directory objects (Default):** Guests can only read basic properties of users/groups; cannot browse the full directory freely.

3. **Guest user access is restricted to properties and memberships of their own directory objects (most restrictive - Best Practice!):** Guests can ONLY view their own profile and the specific groups/apps explicitly assigned to them.

### B. Guest Invite Settings (Who Can Send Invitations?)

1. **Anyone in the organization can invite guest users including guests and non-admins (most inclusive):** Even existing *guests* can invite *other guests*!

2. **Member users and users assigned to specific admin roles can invite guest users including guests with member permissions:** Standard internal members can invite guests, but guests cannot.

3. **Only users assigned to specific admin roles can invite guest users:** Restricts invites strictly to **Global Admin**, **User Admin**, and **Guest Inviter** (least-privileged role!).

4. **No one in the organization can invite guest users including admins (most restrictive):** Completely blocks all B2B invitations across the tenant.

### C. Collaboration Restrictions (Domain Allow/Block Lists)

* **Allow invitations to be sent to any domain (most inclusive - Default).**

* **Deny invitations to the specified domains (Blacklist):** Blocks specific competitor/consumer domains (e.g., `gmail.com`, `competitor.com`).

* **Allow invitations only to the specified domains (Whitelist):** Strictly allows invites ONLY to approved partner domains.

* > **Exam Trap:** Collaboration restrictions apply to **B2B Invitations AND Self-Service Sign-Up**, *HOWEVER*, they **DO NOT block users added via Cross-Tenant Synchronization**!

## 2. Inviting & Managing External User Accounts (B2B)

### A. Invitation Methods & Least-Privilege Roles

* **Individual Invite:** Entra Portal (`Users -> New user -> Invite external user`), Microsoft Teams, SharePoint, or Entra Entitlement Management (Access Packages).

* **Bulk Invite:** Entra Portal (`Users -> Bulk operations -> Bulk invite`) using a **CSV file** (requires `User name`, `Email address`, `Invite redirect URL`, and optional `Send invite message`).

* **Least-Privileged Role:** **Guest Inviter** (can invite guests without being a User Admin!).

### B. Managing External User Accounts (Lifecycle & Traps)

* **User Type (`Member` vs. `Guest`):**

  * B2B invited users default to `UserType = Guest` (`UPN` formatted as `alice_partner.com#EXT#@contoso.onmicrosoft.com`).

  * You can convert an external user's `UserType` from **Guest to Member** (or vice versa) in their user profile (common in M&A/subsidiary scenarios so they get Member-level visibility while still authenticating against their home tenant!).

* **Reset Redemption Status (CRITICAL EXAM TRAP!):**

  * **Scenario:** A B2B guest accepted their invite, then their home company changed their email address/UPN, or the guest account was deleted/recreated in their home tenant, breaking their login to your tenant.

  * **Solution:** Open the guest user's profile in Entra ID -> Click **"Reset redemption status"** (or re-invite with the new email). This preserves their existing group memberships, app assignments, and object ID while letting them re-accept the invitation!

* **Licensing Trap (MAU Billing):**

  * Entra ID External Identities uses **Monthly Active Users (MAU)** billing: the **first 50,000 MAUs per month are FREE** (including Entra ID P1/P2 features for guests if linked to an Azure subscription!).

## 3. Cross-Tenant Access Settings (`External Identities -> Cross-tenant access settings`)

Replaces legacy "B2B Direct Connect / Trust" settings with granular **Inbound** and **Outbound** controls for B2B Collaboration and B2B Direct Connect (Teams Shared Channels):

### A. Default Settings vs. Organizational Settings

* **Default settings:** Applies globally to *all* external Entra tenants unless overridden.

* **Organizational settings:** Add a specific partner tenant (by **Tenant ID** or **Domain Name**) to override the default rules for that partner alone!

### B. Inbound Access Settings (External Users Coming INTO Your Tenant)

1. **B2B Collaboration:** Control which external users/groups from the partner tenant can be invited/access your apps (Allow/Block all or specific users/groups).

2. **B2B Direct Connect:** Required for **Microsoft Teams Shared Channels**! Allows external users to collaborate in your Teams channels *without* creating a B2B Guest object in your directory.

   * > **Exam Trap:** For Teams Shared Channels to work, **BOTH** tenants must configure Cross-Tenant Access: Your tenant must allow **Inbound** B2B Direct Connect, AND the partner tenant must allow **Outbound** B2B Direct Connect!

3. **Trust Settings (HUGE EXAM TRAP!):**

   * **Trust multifactor authentication (MFA) from Microsoft Entra tenants:** If checked, your Conditional Access policies will **accept the MFA claim already satisfied in the guest's home tenant** instead of forcing the guest to register a *second* MFA method in your resource tenant!

   * **Trust compliant devices:** Accepts Intune device compliance status from the guest's home tenant.

   * **Trust Microsoft Entra hybrid joined devices:** Accepts Hybrid Join status from the guest's home tenant.

   * > **Exam Trap:** By default, **NONE of the Trust Settings are checked**! If a question states *"Guests are prompted to register for MFA a second time when accessing our apps, even though they already use MFA at their home company"* -> Enable **Trust multifactor authentication from Microsoft Entra tenants** in **Inbound Cross-Tenant Access Settings**!

### C. Outbound Access Settings (Your Users Going OUT to External Tenants)

* Controls whether *your* internal users can be invited as guests into external tenants or join external Teams Shared Channels.

* **Automatic Redemption (Seamless B2B):**

  * Located under `Cross-tenant access settings -> Organizational settings -> Inbound/Outbound -> Trust settings -> Automatically redeem invitations with the tenant`.

  * When enabled on **BOTH** tenants, users are **NOT prompted to click "Accept" on the B2B consent screen** when accessing resources in the partner tenant!

## 4. Cross-Tenant Synchronization (`External Identities -> Cross-tenant synchronization`)

* **Purpose:** Automates **creating, updating, and deleting B2B Guest (or Member) accounts** across multiple Entra ID tenants owned by the *same organization* (e.g., parent company + subsidiaries / M&A) so users can access apps across tenants seamlessly without manual invites!

* **Architecture & Licensing:**

  * Uses the **Microsoft Entra Provisioning Engine** (syncs every **40 minutes**).

  * Requires **Microsoft Entra ID P1 or P2** for every user synchronized.

* **Configuration Steps & Exam Traps:**

  1. **Target Tenant (Receiving users):** Must enable **"Allow users to sync into this tenant"** under `Cross-tenant access settings -> Organizational settings -> [Source Tenant] -> Cross-tenant sync` (Inbound).

  2. **Source Tenant (Sending users):** Creates the Cross-Tenant Sync configuration, scopes **Users/Groups**, configures **Attribute Mappings** (can map `userType` as `Guest` or `Member`!), and enables **Automatic Redemption** so synced users never see a consent prompt!

  3. > **CRITICAL EXAM TRAP:** Cross-Tenant Synchronization **ONLY works between Microsoft Entra ID tenants** (it cannot sync users from Google, Okta, or on-prem AD directly—on-prem AD users must first sync to the Source Entra tenant via Entra Connect/Cloud Sync).

## 5. External Identity Providers (SAML / WS-Fed, Social & Email OTP)

Configured under `External Identities -> All identity providers` so external users without an Entra ID account can still collaborate as B2B guests:

### A. Built-in Identity Providers

1. **Microsoft Account (MSA):** Enabled by default (`outlook.com`, `hotmail.com`, `live.com`).

2. **Email One-Time Passcode (Email OTP):** Enabled by default! If a guest's email domain is *not* an Entra tenant, MSA, Google, or federated SAML/WS-Fed partner, Entra automatically emails them a **30-minute temporary passcode** so they don't have to create a Microsoft account!

3. **Google / Facebook:**

   * Google can be configured for B2B guest sign-in (`gmail.com` or Google Workspace domains) by registering an app in Google Cloud Console and entering the **Client ID** and **Client Secret** in Entra ID.

### B. Direct Federation with SAML / WS-Fed Identity Providers (B2B Federation)

* **Purpose:** Allows external partners who use a non-Microsoft IdP (e.g., **Okta, PingFederate, Salesforce, Shibboleth, on-prem AD FS**) to sign into your Entra tenant using their **existing corporate credentials** via **SAML 2.0** or **WS-Fed**!

* **Configuration Requirements (EXAM TRAPS!):**

  1. You can configure **ONLY ONE** SAML/WS-Fed IdP per partner domain name (e.g., `partner.com`).

  2. You must obtain the partner's **Metadata XML file** (or manually enter **Issuer URI**, **Passive authentication endpoint**, and **Token signing certificate**).

  3. **DNS Validation Trap:** To verify domain ownership when configuring Direct Federation, the partner must either host a federation metadata URL matching the domain OR add a **`TXT` record** to their public DNS!

  4. **Authentication Priority Order (MEMORIZE THIS!):**

     When a guest signs in, Entra evaluates authentication providers in this strict order:

     1. **Microsoft Entra ID** (if the domain is verified in another Entra tenant).

     2. **SAML / WS-Fed Direct Federation** (if configured for that domain).

     3. **Google Federation** (if configured).

     4. **Microsoft Account (MSA)** (if an MSA exists for that email).

     5. **Email One-Time Passcode (OTP)** (fallback!).

## Quick Memory Cheat Sheet (SC-300 External Identities)

1. **Least-Privileged Role to Invite Guests** = **Guest Inviter**.

2. **Most Restrictive Guest Visibility** = *"Guest user access is restricted to properties and memberships of their own directory objects"*.

3. **Guest Changed Email / Broken Login** = Click **"Reset redemption status"** on the guest user's profile (preserves permissions & object ID).

4. **Stop Guests Being Prompted for MFA Twice** = Enable **"Trust multifactor authentication from Microsoft Entra tenants"** in **Inbound Cross-Tenant Access Settings**.

5. **Teams Shared Channels (B2B Direct Connect)** = Requires **Outbound** enabled in Tenant A AND **Inbound** enabled in Tenant B.

6. **Automate B2B User Creation Across Subsidiaries** = **Cross-Tenant Synchronization** (Target tenant must allow inbound sync; ignores domain invite blocklists!).

7. **Suppress B2B Consent Prompt Between Partner Tenants** = Enable **Automatic Redemption** on **BOTH** tenants in Cross-Tenant Access Trust Settings.

8. **Partner Uses Okta / PingFederate (Non-Entra) for B2B** = Configure **SAML / WS-Fed Direct Federation** under *All identity providers* (requires DNS `TXT` record or metadata URL).