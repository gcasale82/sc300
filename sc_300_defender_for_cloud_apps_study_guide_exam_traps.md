================================================================================
SC-300 EXAM PREP: MICROSOFT DEFENDER FOR CLOUD APPS (MDA / CASB)
================================================================================

--------------------------------------------------------------------------------
1. CORE ARCHITECTURE, ROLES & LICENSING (EXAM TRAPS)
--------------------------------------------------------------------------------
- What is Microsoft Defender for Cloud Apps (MDA)?
  Microsoft's Cloud Access Security Broker (CASB) integrated into the Microsoft 
  Defender XDR portal (`security.microsoft.com`).
- Licensing Traps:
  * **Defender for Cloud Apps (Standalone / E5 / Entra Suite)**: Full CASB 
    capabilities (Session policies, Access policies, API connectors, Cloud 
    Discovery, OAuth app governance).
  * **Defender for Cloud Apps Discovery (Included in Entra ID P1/P2 & MDE P2)**: 
    Discovery and risk scoring ONLY (NO real-time session proxy or API governance!).
- Required RBAC Roles (EXAM TRAP!):
  * Global Administrator / Security Administrator: Full admin access to MDA.
  * Security Operator: Can manage alerts and policies, cannot manage global 
    settings/connectors.
  * Security Reader / Global Reader: Read-only access.
  * *TRAP*: In Defender XDR Unified RBAC, you can also delegate granular 
    permissions specifically for Cloud Apps!

--------------------------------------------------------------------------------
2. CLOUD DISCOVERY & CLOUD APP CATALOG (SHADOW IT)
--------------------------------------------------------------------------------
- Purpose: Discovers Shadow IT (unsanctioned SaaS apps used by employees), 
  assesses risk scores (0-10), and monitors data upload/download volume.

- 3 Ways to Collect Cloud Discovery Data (EXAM COMPARISON):
  1. **Microsoft Defender for Endpoint (MDE) Integration (RECOMMENDED / EASIEST)**:
     - Toggle ON in MDA Settings -> Cloud Discovery -> Microsoft Defender for 
       Endpoint.
     - *TRAP*: Zero firewall configuration needed! Works even when users are 
       **off the corporate network (remote/home)** and captures the **Device + 
       User identity**!
  2. **Automatic Log Upload (Log Collector)**:
     - Docker container installed on-premises (Windows/Linux) that receives 
       Syslog/FTP traffic logs from firewalls/proxies and uploads them to MDA.
  3. **Manual Log Upload (Snapshot Report)**:
     - One-time manual upload of firewall/proxy logs for ad-hoc analysis.

- **Cloud App Catalog & App Tags (EXAM TRAPS!)**:
  * Catalog contains 31,000+ SaaS apps scored on 90+ risk factors (Compliance: 
    SOC2, HIPAA, GDPR; Security: MFA, encryption at rest; Legal).
  * **App Tags (CRITICAL FOR BLOCKING SHADOW IT)**:
    - **Sanctioned**: Approved for corporate use.
    - **Unsanctioned**: Forbidden!
    - **Monitored**: Under evaluation.
  * *HOW DOES BLOCKING AN UNSANCTIONED APP ACTUALLY WORK? (EXAM TRAP!)*:
    - Marking an app as "Unsanctioned" in MDA does **NOT** automatically block 
      network traffic by itself!
    - You MUST either:
      1. Enable **"Enforce app access"** in MDE (uses Defender for Endpoint 
         Network Protection to block the URLs on managed endpoints!).
      2. Export a **Block Script** from MDA and import it into your on-prem 
         firewall/proxy (Zscaler, Palo Alto, BlueCoat, etc.).
  * **Custom Cloud Apps**: If a proprietary/internal app is not in the catalog, 
    you can add a Custom App by specifying its **Domains / IP addresses** so 
    Cloud Discovery tracks traffic to it.

--------------------------------------------------------------------------------
3. CONNECTED APPS (API CONNECTORS)
--------------------------------------------------------------------------------
- Purpose: Connects MDA directly to SaaS apps via **REST APIs** (Microsoft 365, 
  Salesforce, Box, Dropbox, Google Workspace, ServiceNow, AWS, Okta).
- What API Connectors Do:
  * Scans data **at rest** inside the SaaS app (DLP / file sharing permissions, 
    publicly shared links, malware detection).
  * Enforces governance actions (quarantine file, remove external collaborator, 
    suspend user).
  * Enables **Activity Policies** (e.g., "Alert if user downloads > 100 files 
    in 5 minutes" or "Impossible travel" -> populates `mcasImpossibleTravel` 
    in Entra ID Protection!).
- *EXAM TRAP*:
  * API Connectors work **out-of-band (asynchronously / post-activity)**—they 
    CANNOT block a download in real-time! For real-time inline blocking, you 
    MUST use **Conditional Access App Control**!

--------------------------------------------------------------------------------
4. APPLICATION-ENFORCED RESTRICTIONS vs. CONDITIONAL ACCESS APP CONTROL
--------------------------------------------------------------------------------
*CRITICAL SC-300 COMPARISON — DO NOT CONFUSE THESE TWO!*

A. **APPLICATION-ENFORCED RESTRICTIONS (Native App Controls)**
   - How it works: Entra ID passes device compliance/domain-join state to a 
     supported Microsoft app, and the **app itself** natively restricts features.
   - Supported Apps (EXAM TRAP!): **ONLY Exchange Online, SharePoint Online, 
     and OneDrive for Business**! (Does NOT work with 3rd-party SaaS apps!).
   - Typical Exam Scenario:
     * Allow full access from compliant devices, OR allow **"Browser-only 
       limited access (Block download, print, and sync)"** in SharePoint/
       Exchange from unmanaged (BYOD) devices!
   - Setup Requirement: Must be configured in BOTH **SharePoint/Exchange Admin 
     Center** (allow limited web-only access) AND **Entra Conditional Access** 
     (Session -> Use application-enforced restrictions).

B. **CONDITIONAL ACCESS APP CONTROL (Reverse Proxy / Inline Session Control)**
   - How it works: Routes user sessions through the **MDA Reverse Proxy** 
     (`*.mcas.ms` suffix added to the URL!).
   - Supported Apps: **ANY SAML 2.0 or OpenID Connect (OIDC)** app integrated 
     with Entra ID (M365, Salesforce, Box, ServiceNow, custom apps!).
   - Setup Steps (MEMORIZE THE ORDER!):
     1. Create a **Conditional Access Policy** in Entra ID -> Session -> 
        **"Use Conditional Access App Control"** (Monitor only, Block downloads, 
        or Use custom policy).
     2. User signs into the app once via the CA policy -> Onboards the app 
        into MDA (**Connected apps -> Conditional Access App Control apps**).
     3. Create a **Session Policy** or **Access Policy** inside MDA!
   - *EXAM TRAPS*:
     * Only works for **Browser sessions** (does NOT proxy native mobile/desktop 
       thick clients; use **Access Policies** to block native clients!).
     * Adding a new Domain/URL to an app requires re-verifying SSL/SAML routing.

--------------------------------------------------------------------------------
5. ACCESS POLICIES vs. SESSION POLICIES IN MDA
--------------------------------------------------------------------------------
Both require routing through Conditional Access App Control first:

1. **ACCESS POLICIES (Evaluated at Login / Session Start)**
   - Purpose: Controls **WHO** can access the app and **HOW** (before the session 
     even loads), using MDA-specific filters not available in standard Entra CA.
   - Examples:
     * Block **native mobile/desktop apps** while allowing browser proxy access.
     * Allow access ONLY if device has a specific **Client Certificate** or 
       **Intune / Device Tag** (e.g., `Compliant`, `Hybrid Entra Joined`).
     * Block outdated browsers or non-corporate IP ranges.

2. **SESSION POLICIES (Evaluated Continuously During Active Session)**
   - Purpose: Controls **WHAT** the user can do *inside* the browser session in 
     real-time!
   - Control Types:
     * **Monitor only**: Logs all user actions.
     * **Block activities**: Blocks specific actions in real-time (e.g., Block 
       Download, Block Cut/Copy/Paste, Block Print, Block Upload of malware).
     * **Control file download (with Inspection)**:
       - Scans file in real-time using **Microsoft Purview DLP** or built-in 
         DLP engine before allowing download!
       - Actions: Block download, OR **Protect file** (applies a **Microsoft 
         Purview Sensitivity Label / Encryption** on the fly as the user 
         downloads the file!).
     * **Control file upload (with Inspection)**: Blocks uploading sensitive 
       files (e.g., Credit Card numbers) or malware to cloud apps.

--------------------------------------------------------------------------------
6. OAUTH APP POLICIES (APP GOVERNANCE & CONSENT RISKS)
--------------------------------------------------------------------------------
- Purpose: Monitors and revokes third-party **OAuth applications** that users 
  have consented to (protecting against **Illicit Consent Grant attacks**!).
- Supported Platforms: **Microsoft 365 (Entra ID)**, Google Workspace, and 
  Salesforce.
- What OAuth App Policies Monitor:
  * **Permissions / Privileges**: High-severity permissions (e.g., `Mail.ReadWrite`, 
    `Files.ReadWrite.All`, `Directory.ReadWrite.All`).
  * **Community Use**: Apps that are rare/uncommon across other tenants.
  * **User Impact**: Number of users who authorized the app.
  * **Publisher Verification**: Whether the app comes from a Microsoft-verified 
    publisher.
- Automated Governance Actions (EXAM TRAP!):
  * **Ban app**: Revokes OAuth tokens for ALL users and prevents future consent!
  * **Approve app**: Marks app as safe so it doesn't trigger alerts.
  * **Notify user / Admin**: Sends email notification.
- *APP GOVERNANCE ADD-ON TRAP*:
  * For deep anomaly detection on Entra OAuth apps (e.g., detecting an app 
    suddenly downloading massive data via Microsoft Graph API), enable **App 
    Governance** inside MDA settings!

================================================================================
QUICK MEMORY CHEAT SHEET (SC-300 DEFENDER FOR CLOUD APPS)
================================================================================
1. Easiest & best Cloud Discovery method = **Microsoft Defender for Endpoint 
   (MDE) integration** (captures off-network devices + user identity).
2. Blocking an "Unsanctioned" app = Tagging it is NOT enough! Must enable 
   **"Enforce app access" in MDE** OR export a **Firewall/Proxy Block Script**.
3. API Connectors = Work **asynchronously at rest** (DLP scan, file quarantine, 
   activity logs). CANNOT block downloads in real-time!
4. Application-Enforced Restrictions = Works **ONLY with SharePoint, OneDrive, 
   and Exchange Online** (natively restricts BYOD web access).
5. Conditional Access App Control = Reverse proxy (`*.mcas.ms`), works with 
   **ANY SAML/OIDC web app**, browser-only!
6. Block Native Desktop/Mobile Apps in MDA = Use an **Access Policy** (not a 
   Session Policy!).
7. Encrypt/Label a file upon download = **Session Policy -> Control file 
   download -> Protect** (integrates with Microsoft Purview).
8. Stop Illicit Consent Grant / risky 3rd-party OAuth apps = **OAuth App Policy** 
   -> Action: **Ban app**.
================================================================================
