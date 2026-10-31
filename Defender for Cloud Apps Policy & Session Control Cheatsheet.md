# Microsoft Defender for Cloud Apps (MDCA) & Conditional Access Cheatsheet (SC-300 Exam Focus)

This guide summarizes MDCA policies, common exam scenarios, and Conditional Access App Control mechanics into a high-yield, high-retention format designed specifically for the SC-300 exam.

---

## Part 1: MDCA Policy Types & Exam Triggers

| Policy Type | Core Objective | Exam Keyword / Trigger | Action / Outcome |
| :--- | :--- | :--- | :--- |
| **App Discovery Policy** | Uncover & handle Shadow IT based on traffic logs. | *Risk score*, *traffic volume* (e.g., >200 GB), *unsanctioned apps*. | Block app via Defender for Endpoint, flag alert, tag app as Unsanctioned. |
| **Activity Policy** | Detect specific user actions in real-time or near real-time. | *Download rate* (e.g., >50 files in 10 min), *Impossible travel*, *Specific IP range*. | Alert admin, force re-authentication, suspend user. |
| **Anomaly Detection Policy** | Automated behavioral baseline monitoring via Machine Learning. | *Out of the box*, *Zero setup*, *Unusual pattern compared to user baseline*. | Adjust alert sensitivity thresholds (Low/Medium/High). |
| **File Policy** | Inspect content within stored files (Data Loss Prevention / DLP). | *Sensitivity label*, *Credit Card / PII*, *External sharing* ("Anyone with link"). | Remove external sharing, apply MIP label, quarantine file. |
| **Access Policy** | Control or block initial access to cloud apps. | *Unmanaged device*, *Untrusted location*, *Browser access restriction*. | Block access or force session proxy redirection. |
| **Session Policy** | In-session controls for user actions during an active app session. | *Prevent copy/paste*, *Block download*, *Inspect file upload*, *Force watermark*. | Enforce real-time action block or DLP inspection via proxy. |

---

## Part 2: Quick Exam Scenarios (Match Pattern)

### Scenario 1: Shadow IT Control
* **Requirement:** Automatically identify and block unsanctioned cloud apps with a risk score $\le 5$ generating $>200\text{ GB}$ daily traffic.
* **Policy to Use:** **App Discovery Policy** (built from the *New risky app* template).

### Scenario 2: Preventing Data Leakage on Unmanaged Devices
* **Requirement:** Allow users on personal (BYOD) laptops to view documents in Microsoft 365, but prevent them from downloading or printing them.
* **Policy to Use:** **Conditional Access Session Policy** (using the *Block Download* template).

### Scenario 3: Real-Time Malware Upload Prevention
* **Requirement:** Inspect files being uploaded to Salesforce in real time and block any files infected with malware or containing sensitive financial data.
* **Policy to Use:** **Session Policy** (Control type: *File upload with DLP/Malware inspection*).

### Scenario 4: Stale External Sharing Enforcement
* **Requirement:** Audit OneDrive for files tagged as "Internal Only" that have been shared with external guests, and automatically revoke the share link.
* **Policy to Use:** **File Policy** (Scope: *At-rest content* in connected apps).

---

## Part 3: Real-Time Session Control Mechanics

Conditional Access App Control leverages a **Reverse Proxy Architecture** to inspect traffic between the user and the cloud application.

```
[ User Device / Browser ]
           │
           ▼
[ Microsoft Entra ID ]  ───► Conditional Access Policy
           │                 (Session: Use Conditional Access App Control)
           ▼
[ MDCA Reverse Proxy ]  ───► Applies MDCA Access & Session Policies in Real Time
 (cas.ms / mcas.ms URL)
           │
           ▼
[ Target Cloud App ]   ───► Salesforce, Box, Workday, M365, Custom SAML App
```

### How the Redirection Works (Step-by-Step)
1. **Authentication:** User attempts to log into a SaaS app (e.g., Salesforce) via Entra ID.
2. **Evaluation:** Entra ID evaluates the **Conditional Access Policy**.
3. **Session Routing:** If the Conditional Access policy includes **Session $\rightarrow$ Use Conditional Access App Control**, Entra ID rewrites the user's target URL.
4. **Proxy Redirection:** The user's traffic is redirected through the **MDCA Reverse Proxy** (URLs appear with `.cas.ms` or `.mcas.ms` appended).
5. **In-Session Inspection:** MDCA monitors all session activity (click, upload, download, copy/paste) live and applies configured **MDCA Session Policies**.

---

## Part 4: Key Exam Distinctions to Remember

1. **At-Rest vs. In-Transit:**
   * **File Policy:** Evaluates content **at rest** stored within connected apps (SharePoint, Box, Google Workspace).
   * **Session Policy:** Evaluates content **in transit** during an active session (upload/download moments).

2. **Entra ID CA vs. MDCA Policies:**
   * Entra ID CA determines **if** a user can access an app (or if they must be proxied).
   * MDCA Session Policies determine **what** a user can do inside the app once connected.

3. **Prerequisites for Conditional Access App Control:**
   * Requires **Microsoft Entra ID P1** (or P2).
   * Requires **Microsoft Defender for Cloud Apps** license.
   * Apps must be configured with SAML 2.0 or OpenID Connect SSO.