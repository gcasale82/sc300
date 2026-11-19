# SC-300 Exam Prep: Identities, Monitoring, Conditional Access & Azure Permissions

## 1. Create, Configure, and Manage Microsoft Entra Identities

### A. Bulk Operations (Portal vs. Microsoft Graph PowerShell)

#### 1. Microsoft Entra Admin Center Bulk Operations (CSV)

* **Supported Actions:** Bulk **Create** users, Bulk **Invite** guests, Bulk **Delete** users, Bulk **Restore** users (within 30-day soft-delete window), Bulk **Add/Remove group members**.

* **Required CSV Columns for Bulk Create:** `Name` (Display Name), `User name` (UPN), `Initial password`, `Block sign in (Yes/No)`.

* > **Exam Trap:** You **CANNOT** bulk-modify existing user attributes (like updating Department or Job Title for 500 users) via CSV in the Entra Admin Center! For bulk *updates*, you **MUST** use **Microsoft Graph PowerShell** (`Update-MgUser`).

#### 2. Microsoft Graph PowerShell (EXAM TRAP: Legacy Modules Retired!)

* **CRITICAL EXAM TRAP:** Legacy `AzureAD` and `MSOnline` (`Get-MsolUser`) PowerShell modules are **DEPRECATED/RETIRED**! On the SC-300 exam, **ALWAYS** pick **Microsoft Graph PowerShell** (`Microsoft.Graph` module) cmdlets:

  * Connect: `Connect-MgGraph -Scopes "User.ReadWrite.All", "Group.ReadWrite.All"`

  * Create user: `New-MgUser`

  * Update user: `Update-MgUser`

  * Delete user: `Remove-MgUser`

  * Restore deleted user: `Restore-MgDirectoryObject`

  * Add user to group: `New-MgGroupMember`

  * Assign license: `Set-MgUserLicense`

* **Least-Privilege Roles for Identity Management:**

  * **User Administrator:** Can create/manage users & groups, reset passwords for non-admins + **Helpdesk Admins** & **Message Center Readers** (cannot reset Global Admin or Security Admin passwords!).

  * **Groups Administrator:** Can create/manage all groups and group settings (including Dynamic Groups & Expiration policies), *cannot* manage users.

### B. Device Join and Device Registration (`Devices -> Device settings`)

| Feature | Microsoft Entra Registered (BYOD) | Microsoft Entra Joined (Cloud-Native) | Microsoft Entra Hybrid Joined | 
| ----- | ----- | ----- | ----- | 
| **Ownership** | Personal (BYOD) or Partner | Organization-owned | Organization-owned (On-Prem AD) | 
| **OS Support** | Windows 10/11, iOS, Android, macOS | Windows 10/11 (Pro/Enterprise/Education — **NOT Home!**) | Windows 10/11, Windows Server 2016+ | 
| **Primary Sign-in** | Local account / Personal MSA + Work account added | Direct Entra ID credentials (`user@domain.com`) | On-Prem AD Domain credentials | 
| **Provisioning** | Settings -> Accounts -> Access work or school | OOBE / Autopilot / Bulk Enrollment (`Windows Configuration Designer`) | Entra Connect Sync (Device Sync + SCP) | 

* **Key Device Settings & Exam Traps (`Devices -> Device settings`):**

  1. **Users may join devices to Microsoft Entra:** `All` (Default), `Selected`, or `None`. *(Requires Entra ID P1/P2 to restrict to "Selected"!).*

  2. **Users may register their devices with Microsoft Entra:** `All` or `None` *(Cannot be scoped to "Selected"; automatically enabled if MDM auto-enrollment is active)*.

  3. **Require Multifactor Authentication to register or join devices:** `Yes` / `No`. *(Trap: Ignored if you already enforce MFA for device registration via Conditional Access -> User Actions -> Register or join devices!).*

  4. **Maximum number of devices per user:** Default = **50** (options: 5, 10, 20, 50, 100, Unlimited).

  5. **Microsoft Entra Local Administrator Role:** Grants local admin rights ONLY on **Entra Joined** Windows devices (does **NOT** work on Hybrid Joined or Registered devices!).

  6. **BitLocker Keys:** Automatically backed up to the Entra ID computer object for **Entra Joined** and **Hybrid Joined** devices (users can view their own BitLocker key in `myaccount.microsoft.com` unless blocked in Device Settings!).

  7. **Stale Devices Cleanup:** Disable/Delete inactive devices (`ApproximateLastSignInDateTime` older than e.g. 90/180 days) via PowerShell (`Get-MgDevice` / `Remove-MgDevice`). *Trap: Deleting a Hybrid Joined device in Entra ID only removes it temporarily unless you also delete/move it out of sync scope in on-prem AD!*

### C. Assign, Modify, and Report on Licenses

* **Assignment Methods:**

  1. **Direct Assignment:** Assigned to an individual user (harder to audit/scale).

  2. **Group-Based Licensing (BEST PRACTICE!):** Assign a license to a **Security Group** (Assigned or Dynamic) or **Microsoft 365 Group** (Security-enabled).

* **CRITICAL LICENSING EXAM TRAPS:**

  1. **Licensing Requirement:** Group-based licensing requires **Microsoft Entra ID P1 or P2**!

  2. **Nested Groups Trap:** Group-based licensing **DOES NOT support nested groups**! Only direct members of the licensed group receive the license.

  3. **Usage Location Mandatory:** You **MUST** populate the `UsageLocation` attribute (e.g., `US`, `IT`, `DE`) on a user *before* assigning any license, or license assignment will fail!

  4. **Conflicting Licenses:** If a user inherits the *same* license from two different groups, Entra consumes **only ONE license** (no double-billing!). If a user has conflicting service plans (e.g., two different Exchange Online plans), group-based licensing enters a **"License assignment error"** state under `Licenses -> Overview -> Audit logs / Failed users`.

  5. **Least-Privilege Role:** **License Administrator** (can assign/remove licenses without needing User Admin or Global Admin!).

## 2. Monitor Identity Activity (Logs, Diagnostic Settings, KQL, Workbooks & Secure Score)

### A. Entra ID Log Types & Retention (CRITICAL EXAM TABLE!)

| Log Type | What It Captures | Free Retention | P1 / P2 Retention | 
| ----- | ----- | ----- | ----- | 
| **Sign-in Logs** | Interactive user sign-ins, **Non-interactive** user sign-ins, **Service Principal (App)** sign-ins, **Managed Identity** sign-ins. | **7 days** | **30 days** | 
| **Audit Logs** | Directory changes (User created, Password reset, Group modified, Role assigned, CA policy edited, App consent granted). | **7 days** | **30 days** | 
| **Provisioning Logs** | Automated SCIM / Cross-tenant sync / HR-driven (Workday/SuccessFactors) user creation, updates, and errors. | **7 days** | **30 days** | 

* > **Exam Traps:**

  * **Roles to Read Logs:** **Reports Reader** (Least-privileged role to view Sign-in & Audit logs!), **Security Reader**, **Global Reader**.

  * **4 Sign-in Log Sub-Tabs:** If an automated daemon app or Azure VM fails to authenticate, look under **Service principal sign-ins** or **Managed identity sign-ins** (NOT *Interactive user sign-ins*!).

  * **Conditional Access Troubleshooting:** Open a specific Sign-in Log event -> **Conditional Access** tab to see which CA policies **Applied**, **Not Applied**, or were in **Report-Only** mode!

### B. Diagnostic Settings (`Monitoring & health -> Diagnostic settings`)

* **Why Configure?** Entra retains logs for max **30 days**. To keep logs longer (e.g., 1 year for compliance), run KQL queries, or trigger SIEM alerts, you must route logs via Diagnostic Settings!

* **Licensing Trap:** Exporting **Sign-in logs** requires **Microsoft Entra ID P1 or P2**! *(Audit logs can be exported on Entra ID Free).*

* **3 Destinations (MEMORIZE WHEN TO PICK EACH!):**

  1. **Log Analytics Workspace (Azure Monitor):** Pick when you need **KQL queries**, **Azure Monitor Alerts**, or **Entra Workbooks**.

  2. **Azure Storage Account:** Pick for **long-term, low-cost cold archival / compliance retention** (e.g., 7-year audit retention).

  3. **Azure Event Hubs:** Pick when streaming logs in real-time to an **external third-party SIEM** (e.g., Splunk, QRadar, ArcSight). *(For Microsoft Sentinel, use the native Entra ID Data Connector -> Log Analytics).*

### C. KQL Queries in Log Analytics (Core Tables to Recognize)

Know the exact Log Analytics table names for SC-300 questions:

* `SigninLogs` -> Interactive user sign-ins (`ResultType == "0"` means success; `"50126"` = invalid password; `"53003"` = blocked by Conditional Access!).

* `AADNonInteractiveUserSignInLogs` -> Non-interactive token refreshes (mobile/desktop apps).

* `AADServicePrincipalSignInLogs` -> App / Service Principal authentications.

* `AADManagedIdentitySignInLogs` -> Azure Managed Identity authentications.

* `AuditLogs` -> Directory modifications (`OperationName`, `InitiatedBy`, `TargetResources`).

* `AADProvisioningLogs` -> SCIM / Cross-tenant sync events.

* `AADUserRiskEvents` & `AADRiskyUsers` -> Entra ID Protection risk events.

### D. Entra Workbooks & Identity Secure Score

* **Entra Workbooks:** Pre-built interactive visual dashboards (e.g., *Conditional Access Insights and Reporting*, *Sign-ins using Legacy Authentication*, *MFA Gaps*, *Sensitive Operations Report*).

  * *Trap:* Requires logs routed to a **Log Analytics Workspace** + **Entra ID P1/P2**.

* **Identity Secure Score (`Protection -> Identity Secure Score`):**

  * Measures your tenant's identity security posture as a percentage (e.g., `68%`) compared to Microsoft best practices and industry peers.

  * Lists prioritized **Improvement Actions** (e.g., *Enable MFA for all users*, *Block legacy authentication*, *Designate more than one Global Admin*, *Turn on user risk policy*).

  * **Status Options:** `To address`, `Completed`, `Planned`, `Risk accepted`, `Resolved through third party` (e.g., if using Okta/Duo MFA instead of Entra MFA!).

  * **Roles:** **Security Reader** / **Global Reader** (View); **Security Administrator** (Update action status/score).

## 3. Plan, Implement, and Manage Microsoft Entra Conditional Access

* **Licensing:** Requires **Microsoft Entra ID P1** (or **P2** for User Risk / Sign-in Risk conditions).

* **Least-Privileged Role:** **Conditional Access Administrator** (or **Security Administrator**).

* **Evaluation Logic (CRITICAL EXAM RULE):**

  * **ALL** selected **Assignments** (Users AND Cloud Apps AND Conditions) must match for a policy to trigger (**AND** logic between categories; **OR** logic within a category).

  * If **multiple** CA policies apply to a sign-in, **ALL** of their Grant controls must be satisfied (**Block ALWAYS wins!**).

### A. Session Management (`Access controls -> Session`)

1. **Use app enforced restrictions:** Passes device state to **SharePoint Online, OneDrive, and Exchange Online** to enforce limited web-only access (block download/print/sync) on unmanaged devices.

2. **Use Conditional Access App Control:** Routes browser sessions through **Microsoft Defender for Cloud Apps** reverse proxy (`*.mcas.ms`).

3. **Sign-in frequency:** Forces users to re-authenticate after a set time (e.g., `4 hours`, `14 days`) or **`Every time`** (required for sensitive apps or Sign-in Risk remediation).

   * *Trap:* Overwrites the default 90-day Primary Refresh Token (PRT) rolling window!

4. **Persistent browser session:** Controls whether users see the *"Stay signed in?"* prompt (`Always persistent` vs. `Never persistent`—useful on shared/kiosk computers!).

5. **Customize continuous access evaluation (CAE):** Strictly enforce or disable CAE for the targeted scope.

6. **Disable resilience defaults:** Disables Entra's outage survival feature (if Entra has an outage, existing sessions won't be extended).

7. **Require token protection for sign-in sessions (Preview/GA):** Binds refresh/access tokens cryptographically to the device's TPM (**Token Binding**) so stolen tokens cannot be replayed from an attacker's machine! *(Supported on Windows 10/11 Entra Joined/Hybrid Joined devices for Exchange/SharePoint desktop apps).*

### B. Device-Enforced Restrictions & Device Filters

* **Grant Controls:**

  * `Require device to be marked as compliant` -> Requires **Intune** enrollment and a passing Intune Compliance Policy.

  * `Require Microsoft Entra hybrid joined device` -> Requires **Entra Connect Device Sync** (does *not* require Intune).

  * *(Trap: If you check BOTH and select "Require ONE of the selected controls", either Intune-compliant BYOD OR Hybrid-Joined corporate PC gets in!)*

* **Filter for Devices (`Conditions -> Filter for devices`):**

  * Uses rule expressions (e.g., `device.isCompliant -eq True`, `device.trustType -eq "AzureAD"`, `device.deviceOwnership -eq "Company"`) to include or **exclude** specific devices (e.g., exclude Privileged Access Workstations [PAWs] from a restrictive policy).

### C. Continuous Access Evaluation (CAE)

* **Problem Solved:** Normally, OAuth access tokens are valid for **~60-90 minutes**, meaning if an admin disables a user or resets their password, the user still keeps access until the token expires!

* **How CAE Works:** Supported apps (**Exchange Online, SharePoint Online, Teams, Microsoft Graph**) subscribe to real-time security events from Entra ID and revoke active sessions **within minutes** when:

  1. User account is **deleted or disabled**.

  2. User **password is changed or reset**.

  3. **MFA is enabled** for the user.

  4. Admin explicitly clicks **"Revoke sessions"** (`Revoke-MgUserSignInSession`).

  5. **High User Risk** is detected by Entra ID Protection.

  6. **IP Address changes** to an untrusted location (Strict Location Enforcement!).

* > **Exam Trap (CAE Strict Location Enforcement):** If a Conditional Access policy uses **Named Locations** (Trusted IPs) and a user moves off the corporate Wi-Fi onto an untrusted network IP, **CAE immediately terminates their active Exchange/SharePoint/Teams token in real-time**!

### D. Authentication Context (`Conditional Access -> Authentication contexts`)

* **Purpose:** Allows you to apply **step-up Conditional Access policies** *inside* an application (granular actions) rather than blocking the entire app at login!

* **Where Used (EXAM SCENARIOS):**

  1. **Microsoft Purview Sensitivity Labels:** Accessing a SharePoint site labeled *"Highly Confidential"* triggers Authentication Context `C1` -> CA requires **Phishing-Resistant MFA + Compliant Device**!

  2. **Microsoft Entra PIM:** Activating a privileged role (e.g., *Global Administrator*) triggers Authentication Context `C2` -> CA requires **Compliant PAW device**!

  3. **Custom Apps:** App sends `acrs` claim in token to trigger step-up auth before viewing sensitive financial records.

### E. Protected Actions (`Roles and administrators -> Protected actions`)

* **Purpose:** Protects **specific high-risk administrative operations** inside Entra ID (even if the user is *already* a Global Admin or Conditional Access Admin!) by requiring them to satisfy a **Conditional Access Authentication Context** right before executing that action!

* **Protected Actions Examples (MEMORIZE THESE!):**

  * Creating, updating, or deleting **Conditional Access policies**!

  * Managing **Named Locations** or **Authentication Contexts**!

  * Modifying **Cross-Tenant Access Settings**!

  * Managing **Authentication Strengths**!

* **How It Works (2-Step Setup):**

  1. Create an **Authentication Context** (e.g., `Admin-StepUp`) and link it to a **Conditional Access Policy** (e.g., Require Phishing-Resistant MFA / FIDO2).

  2. Assign that Authentication Context to the **Protected Action** (e.g., *Modify Conditional Access policies*).

  * *Result:* Even an active Global Admin cannot edit a Conditional Access policy unless their current session satisfies the `Admin-StepUp` CA policy!

### F. Conditional Access Templates & Report-Only Mode

* **CA Templates:** Pre-configured Microsoft best-practice policies across 5 categories: *Secure foundation*, *Zero Trust*, *Remote work*, *Protect administrator*, *Emerging threats*.

  * *Trap:* Templates default to **Report-only** mode (or `Off` if customized) and automatically exclude the current admin user so you don't lock yourself out!

* **Report-Only Mode:** Evaluates the policy against every sign-in and logs the result (`Report-only: Success` or `Failure`) in Sign-in Logs and the **Conditional Access Insights Workbook** *without* actually blocking or prompting users! (Always use before enabling a new policy in production).

## 4. Azure Permissions: RBAC, ABAC, and Azure Key Vault

### A. Azure RBAC vs. Entra ID Roles (CRITICAL BOUNDARY TRAP!)

* **Entra ID Roles:** Manage **identity & M365 resources** (Users, Groups, Enterprise Apps, Conditional Access, Domains). Scope = Tenant or Administrative Unit.

* **Azure RBAC Roles:** Manage **Azure infrastructure resources** (VMs, Storage Accounts, Key Vaults, Subscriptions). Scope hierarchy (top-down inheritance):

  `Management Group -> Subscription -> Resource Group -> Resource`

* **Key Built-in Azure RBAC Roles:**

  * **Owner:** Full access to all resources + **can delegate/assign RBAC roles** (`Microsoft.Authorization/roleAssignments/write`).

  * **Contributor:** Full access to create/manage all resources, **CANNOT assign RBAC roles** to others!

  * **Reader:** View-only access to Azure resources.

  * **User Access Administrator:** Can **ONLY manage RBAC role assignments** (cannot create/modify VMs or resources themselves!).

  * > **Exam Trap (Elevating Access):** A **Global Administrator** in Entra ID does *not* automatically have access to Azure Subscriptions! To take over an orphaned subscription, a Global Admin must go to `Entra ID -> Properties -> Access management for Azure resources = Yes`, which grants them **User Access Administrator** at the Root Management Group (`/`)!

### B. Azure ABAC (Attribute-Based Access Control)

* **What is Azure ABAC?** Extends standard Azure RBAC by adding **Conditions** to role assignments based on attributes of the **Principal (User)**, **Resource (Blob/Container/Tags)**, and **Environment**.

* **Supported Services & Roles (EXAM TRAP!):**

  * Primarily used on **Azure Blob Storage / Azure Data Lake Storage Gen2** (and Azure Key Vault / Event Grid in preview/newer scenarios) with data-plane roles like **Storage Blob Data Reader**, **Storage Blob Data Contributor**, and **Storage Blob Data Owner**.

  * **Example Condition:** *"Allow user to read blobs ONLY IF the blob's index tag `Project` matches the user's Entra **Custom Security Attribute** `Project`!"*

* **Required Role to Configure ABAC Conditions:** Must have `Microsoft.Authorization/roleAssignments/write` (**Owner** or **User Access Administrator**) + **Attribute Assignment Reader** (to select Entra Custom Security Attributes in the condition builder).

### C. Azure Key Vault Access Models (Access Policies vs. Azure RBAC)

Azure Key Vault stores **Secrets** (passwords/API keys), **Keys** (cryptographic encryption keys), and **Certificates**.

#### 1. Two Permission Models (CRITICAL EXAM DISTINCTION!)

1. **Azure Role-Based Access Control (Azure RBAC - RECOMMENDED!):**

   * Unified governance using standard Azure RBAC assignments at the Vault, Resource Group, or Subscription level (can also scope to a *single* secret/key!).

   * **Key Built-in Key Vault Data-Plane Roles (MEMORIZE THESE!):**

     * **Key Vault Administrator:** Full data-plane control over Secrets, Keys, and Certificates (cannot modify the Key Vault firewall/ARM resource itself—that's *Key Vault Contributor*).

     * **Key Vault Secrets Officer:** Create, read, update, delete secrets.

     * **Key Vault Secrets User:** **Read-only** access to secret values (least-privileged role for an app/VM Managed Identity reading a database password!).

     * **Key Vault Crypto Officer / Crypto User:** Manage or use cryptographic keys (encrypt/decrypt/sign).

     * **Key Vault Certificates Officer:** Manage certificates.

   * > **Exam Trap:** Assigning the standard **Owner** or **Contributor** role on a Key Vault grants **Management Plane** access (can delete the vault or change access settings), **NOT Data Plane** access (cannot read secrets unless explicitly granted `Key Vault Secrets User` or an Access Policy!).

2. **Vault Access Policy (Legacy Model):**

   * Configured inside the Key Vault -> *Access policies* blade; grants permissions separately for Keys, Secrets, and Certificates at the **entire vault level only** (cannot scope to individual secrets).

#### 2. Key Vault Authentication & Network Security Traps

* **Managed Identities (BEST PRACTICE!):**

  * Assign a **System-Assigned** or **User-Assigned Managed Identity** to an Azure VM, App Service, or Function App, then grant that Managed Identity the **`Key Vault Secrets User`** RBAC role!

  * *Result:* Zero credentials or secrets stored in code or config files!

* **Soft-Delete & Purge Protection:**

  * **Soft-Delete:** Enabled by default (retains deleted vaults/secrets for 7–90 days so they can be recovered).

  * **Purge Protection:** Prevents anyone (even an Owner!) from permanently deleting a soft-deleted vault/secret before the retention period expires (mandatory for Customer-Managed Keys [CMK]).

## Quick Memory Cheat Sheet (SC-300 Master Part 2)

1. **Bulk Update User Attributes** = Must use **Microsoft Graph PowerShell** (`Update-MgUser`), NOT Portal CSV!

2. **Group-Based Licensing** = Needs **Entra ID P1/P2**; requires **`UsageLocation`** set on users; **NO nested groups**.

3. **Log Retention** = **7 days** (Free) vs. **30 days** (P1/P2). Export Sign-in logs requires **P1/P2**.

4. **Log Destinations** = **Log Analytics** (KQL/Workbooks), **Storage Account** (long-term archival), **Event Hubs** (3rd-party SIEM).

5. **Continuous Access Evaluation (CAE)** = Revokes Exchange/SharePoint/Teams tokens in minutes on user disable, password reset, session revoke, High User Risk, or untrusted IP change.

6. **Protected Actions** = Requires an **Authentication Context + CA Policy** before an admin can edit Conditional Access policies or Named Locations!

7. **Azure Key Vault Least-Privilege Role to Read a Secret** = **`Key Vault Secrets User`** (Management-plane `Contributor` CANNOT read secrets!).

8. **Azure ABAC** = Combines Azure RBAC (e.g., *Storage Blob Data Reader*) with **Conditions** comparing Blob Index Tags against **Entra Custom Security Attributes**.