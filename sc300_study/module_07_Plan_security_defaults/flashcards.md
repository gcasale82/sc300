# Module 07: Plan security defaults - Flashcards

<details><summary>Source lessons</summary>

- Introduction
- Plan security defaults
- Exercise - Work with security defaults
- Plan Conditional Access policies
- Implement Conditional Access policy controls and assignments
- Exercise - Implement Conditional Access policies roles and assignments
- Test and troubleshoot Conditional Access policies
- Implement application controls
- Implement session management and continuous access evaluation
- Exercise - Configure authentication session controls
- Microsoft Entra Conditional Access Optimization agent
- Summary and resources

</details>

### Introduction and Security Defaults

**Q:** What is Conditional Access and what does it allow you to control?
**A:** Fine-grained control over which users/identities can perform specific activities and access resources, using signals to enforce Zero Trust access decisions.

**Q:** What are security defaults?
**A:** Preconfigured security settings managed by Microsoft, provided at no extra cost, giving organizations a basic level of identity security until they manage their own.

**Q:** Name three settings enforced by security defaults.
**A:** Require all users to register for MFA, require administrators to perform MFA, and block legacy authentication protocols.

**Q:** What MFA method does security defaults registration use to prevent MFA fatigue attacks?
**A:** Number matching—users enter a number displayed on screen into the Microsoft Authenticator app.

**Q:** What is the grace period for MFA registration when security defaults are enabled?
**A:** None—registration is required immediately; users are prompted before accessing any resources.

**Q:** Which admin roles are required to enable or disable security defaults?
**A:** At least a Conditional Access Administrator (Security Administrator also shown in exercises).

**Q:** What is the portal path to manage security defaults?
**A:** Microsoft Entra admin center > Entra ID > Overview > Properties > Manage security defaults.

**Q:** Since when are security defaults potentially already enabled on new tenants?
**A:** Tenants created on or after October 22, 2019; security defaults are enabled on all new tenants at creation.

**Q:** Who should NOT use security defaults?
**A:** Organizations already using Conditional Access policies, organizations with Microsoft Entra ID Premium licenses, or those with complex security requirements.

**Q:** Why is legacy authentication dangerous even with an MFA policy enabled?
**A:** Legacy authentication doesn't support MFA, so attackers can use older protocols to bypass multifactor authentication.

**Q:** Which protocols count as legacy authentication?
**A:** Clients not using modern authentication (e.g., Office 2010) and mail protocols like IMAP, SMTP, or POP3.

**Q:** Does security defaults block Exchange Active Sync basic authentication?
**A:** Yes—after security defaults are enabled, all authentication requests made by older protocols are blocked.

### Plan Conditional Access Policies

**Q:** How are Conditional Access policies structured?
**A:** As if-then statements: if an assignment is met, then apply the access controls. Conditions are called assignments.

**Q:** What are the three components of a CA policy?
**A:** Assignments (users, apps, conditions), Access controls (grant/block), and Session controls.

**Q:** If no assignment is required and no CA policy is in effect, what is the default behavior?
**A:** An access token is issued.

**Q:** What is report-only mode used for?
**A:** Evaluating CA policies before enabling them, to predict affected users without enforcing the policy.

**Q:** How many emergency access accounts should you create before deploying CA policies?
**A:** Two or more, to mitigate accidental administrator lockout from misconfigured policies.

**Q:** Which license tier is required for Conditional Access?
**A:** Microsoft Entra ID Premium P1 (or Microsoft 365 E3 and up); Free and Office 365 subscriptions have no CA.

**Q:** Which license tier adds risk-based sign-in options to Conditional Access?
**A:** Microsoft Entra ID Premium P2, which adds Risky sign-ins, Risky Users, and risk-based options via Identity Protection.

**Q:** How should block access policies be rolled out?
**A:** In phases: communicate changes, start with a small user set, keep administrators excluded, apply to all users only after thorough testing.

**Q:** What can you use named locations for in CA planning?
**A:** To block sign-ins from countries/regions you would never expect, by creating a named location and a policy blocking sign-in from it.

### Implement CA Policy Controls and Assignments

**Q:** What does Microsoft recommend when first creating a CA policy?
**A:** Set Enable policy to Report-only, monitor sign-in logs to verify behavior, then switch it to On.

**Q:** Which license is required to use sign-in risk-based Conditional Access?
**A:** Microsoft Entra ID Premium P2 (Identity Protection sign-in risk detections).

**Q:** Through which two interfaces can a sign-in risk-based policy be assigned?
**A:** Conditional Access itself or Microsoft Entra Identity Protection—organizations should choose one.

**Q:** Which user action in Conditional Access secures MFA and SSPR registration?
**A:** "Register security information" under Cloud apps or actions > User actions (requires combined registration).

**Q:** How do you create a policy requiring security info registration only from trusted locations?
**A:** User action "Register security information"; Conditions > Locations: include Any location, exclude All trusted locations; Grant control: Block access.

**Q:** Where are Named locations configured?
**A:** Microsoft Entra admin center > Protection > Conditional Access > Named locations.

**Q:** What two types of named locations can you define?
**A:** IP ranges (IPv4 address ranges) or Countries/Regions (with optional inclusion of unknown areas).

**Q:** Which accounts should always be excluded from CA policies?
**A:** Emergency access (break-glass) accounts, service accounts (e.g., Microsoft Entra Connect Sync Account), and service principals.

**Q:** Why should service accounts be excluded from CA policies?
**A:** MFA can't be completed programmatically; consider replacing them with managed identities.

**Q:** Which grant control requires Intune and blocks non-compliant devices?
**A:** "Require device to be marked as compliant"—Intune sends compliance info to Microsoft Entra ID for CA decisions.

**Q:** Does the "Require device to be marked as compliant" control block Intune enrollment?
**A:** No—new devices can still enroll to Intune even if the control applies to All users and All cloud apps.

**Q:** What are Terms of Use (TOU) in Conditional Access?
**A:** PDF-based terms created in Identity Governance that users must accept before access; consent can be expired or re-attested.

### Troubleshooting CA Policies

**Q:** Name two configurations that can block your entire organization if applied to all users and all cloud apps.
**A:** Block access, and Require Hybrid Microsoft Entra joined device.

**Q:** Why is "Require app protection policy" on all users/apps dangerous?
**A:** It can block administrators without an Intune app protection policy client app from accessing portals like Intune and Azure.

**Q:** Which two tools should be used to validate CA policies before enabling?
**A:** Conditional Access report-only mode and the What If tool.

**Q:** Which roles can review sign-in events for CA troubleshooting?
**A:** Security Administrator or Global Reader.

**Q:** Where do you find which CA policies applied to a failed sign-in?
**A:** Identity > Monitoring and Health > Sign-ins > select the event > Conditional Access tab.

**Q:** Which filters help investigate a specific sign-in failure?
**A:** Correlation ID, Conditional access (filtered to failures), Username, and Date.

**Q:** What information should be provided when opening a support incident for a sign-in failure?
**A:** The request ID and the time and date from the sign-in event.

**Q:** In the CA policy details view of a sign-in event, what does each side show?
**A:** Left side: details collected at sign-in; right side: whether those details satisfy the applied policies' requirements.

### Application Controls

**Q:** What architecture does Conditional Access App Control use?
**A:** Reverse proxy architecture, integrated with Microsoft Entra Conditional Access, routing users to Microsoft Defender for Cloud Apps.

**Q:** Where are access and session policies for Conditional Access App Control configured?
**A:** In the Microsoft Defender for Cloud Apps portal.

**Q:** Name three things Conditional Access App Control can do in a session.
**A:** Prevent data exfiltration (block download/copy/print), protect on download via Azure Information Protection labels, block upload of unlabeled files, and monitor risky user sessions.

**Q:** What must iOS and Android devices do before you can require approved client apps for them?
**A:** They must first register in Microsoft Entra ID.

**Q:** What are app protection policies (APP)?
**A:** Rules that ensure an organization's data remains safe or contained in a managed app, enforced when users access or move corporate data.

**Q:** What is MAM-WE?
**A:** Mobile Application Management without enrollment—protecting apps with sensitive data on almost any device, including personal BYOD devices.

**Q:** Can Intune app protection policies be used independent of MDM?
**A:** Yes—APP works independent of any mobile-device management solution, protecting data on managed and unmanaged devices.

**Q:** In a MAM policy, what does "Target to all app types = No" allow?
**A:** Applying MAM policies based on managed state—e.g., a less strict policy for Intune-managed devices, a more restrictive one for unenrolled devices.

**Q:** In the approved client app policy for Exchange Online, which client app type requires a separate policy for EAS?
**A:** Exchange ActiveSync clients—configured as a separate policy selecting "Exchange ActiveSync clients."

### Session Management and CAE

**Q:** What is sign-in frequency?
**A:** The time period before a user is asked to sign in again when accessing a resource.

**Q:** What is the default sign-in frequency in Microsoft Entra ID?
**A:** A rolling window of 90 days.

**Q:** Which protocols must apps implement for the sign-in frequency setting to work?
**A:** OAuth2 or OIDC, per the standards; SAML apps also work if they don't drop their own cookies.

**Q:** Name three examples of policy violations that revoke a user's session.
**A:** A password change, an incompliant device, or an account disable.

**Q:** What is the default browser session persistence behavior in Microsoft Entra ID?
**A:** Users on personal devices choose whether to persist the session via a "Stay signed in?" prompt after successful authentication.

**Q:** How long is a default OAuth 2.0 access token valid?
**A:** One hour; after expiration the client is redirected to Microsoft Entra ID to refresh it.

**Q:** What is Continuous Access Evaluation (CAE)?
**A:** A mechanism for two-way conversation between the token issuer and the relying party (enlightened app) to enforce revocations and policy changes in near real time.

**Q:** What does a resource provider send to a CAE-capable client when denying access after a revocation event?
**A:** A 401+ claim challenge; the client then re-authenticates with Microsoft Entra ID.

**Q:** Name two key benefits of CAE.
**A:** Near real-time enforcement of user termination/password change (session revocation) and near real-time enforcement of network location changes.

### Conditional Access Optimization Agent

**Q:** What is the Conditional Access optimization agent?
**A:** An agent that recommends policies and changes based on Zero Trust best practices, ensuring all users are protected by policy.

**Q:** Which licenses are required to use the Conditional Access optimization agent?
**A:** At least Microsoft Entra ID P1, available Security Compute Units (SCU), and Microsoft Intune licenses for device-based controls.

**Q:** Which role is needed to activate the agent the first time?
**A:** Security Administrator or higher.

**Q:** Name three key features of the optimization agent.
**A:** Require MFA for uncovered users, block legacy authentication, policy consolidation, block device code flow, and one-click remediation.

**Q:** What does the agent's policy consolidation feature do?
**A:** Identifies overlapping settings across enabled policies (e.g., same grant controls) and suggests consolidating them into one.
