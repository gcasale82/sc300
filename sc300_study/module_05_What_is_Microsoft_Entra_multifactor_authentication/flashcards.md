# Module 05: What is Microsoft Entra multifactor authentication - Flashcards

<details><summary>Source lessons</summary>

- Introduction
- What is Microsoft Entra multifactor authentication
- Plan your multifactor authentication deployment
- Exercise - Enable Microsoft Entra multifactor authentication
- Configure multifactor authentication methods
- Summary

</details>

### What Is Microsoft Entra Multifactor Authentication

**Q:** What is Microsoft Entra multifactor authentication?
**A:** A security feature that requires two or more authentication elements for full authentication, limiting the impact of password exposure.

**Q:** What are the three categories of authentication elements used in MFA?
**A:** Something you know (password, security answer), something you possess (mobile app, token device), something you are (biometric, e.g., fingerprint or face scan).

**Q:** A hacker obtained a valid username and password via social engineering. Which feature stops the sign-in?
**A:** Microsoft Entra multifactor authentication — the password is useless without also possessing the second factor (trusted device, fingerprint, or face).

**Q:** Which Microsoft Entra feature locks out attackers brute-forcing passwords while treating valid-user sign-ins differently?
**A:** Microsoft Entra smart lockout.

**Q:** Which Microsoft Entra feature blocks commonly used and compromised passwords using a global banned-password list?
**A:** Microsoft Entra password protection.

**Q:** Which feature lets you configure risk-based policies that automatically block risky behaviors or require a password change?
**A:** Microsoft Entra ID Protection.

**Q:** Which Microsoft Entra feature provisions security-enhanced remote access to on-premises web applications?
**A:** Microsoft Entra application proxy.

**Q:** Which tool creates and manages a single identity per user across a hybrid enterprise, keeping users, groups, and devices in sync?
**A:** Microsoft Entra Connect.

**Q:** Which offerings include Microsoft Entra MFA using security defaults?
**A:** Microsoft Entra ID P1 or P2, Microsoft 365 Business, Microsoft Entra ID Free, and standalone Microsoft 365 licenses.

**Q:** Why is the layered approach of MFA effective?
**A:** Even if a hacker learns the password, it's useless without the trusted device; a found device is useless without the password.

### Planning an MFA Deployment

**Q:** How should you roll out an MFA deployment across an organization?
**A:** In waves — start with a small pilot group to find setup issues and unsupported apps/devices, then broaden until fully enrolled.

**Q:** What resources does Microsoft provide to help draft MFA rollout communications?
**A:** Communication templates, including posters and email templates.

**Q:** How is Microsoft Entra MFA enforced?
**A:** With Conditional Access policies, which are IF-THEN statements (IF a user accesses a resource, THEN they must complete an action).

**Q:** Name common access requests that a Conditional Access policy can require MFA for.
**A:** Accessing a specific cloud app, a specific network, a specific client application, or registering a new device.

**Q:** Why should you support more than one MFA authentication method?
**A:** So users have a backup option in case their primary method is unavailable.

**Q:** Which MFA method is preferred for backups?
**A:** Call to phone — Azure calls a supplied number and the user approves via the keypad.

**Q:** What key facts apply to the mobile app OATH verification code method?
**A:** The code changes every 30 seconds, works even with limited connectivity, and doesn't work in China on Android devices.

**Q:** What are FIDO2 security keys?
**A:** An unphishable, standards-based passwordless authentication method — typically USB devices, but can also use Bluetooth or NFC.

**Q:** What does Windows Hello for Business provide?
**A:** Strong two-factor authentication on devices that replaces passwords, using a device-tied credential with a biometric or PIN.

**Q:** What is the easiest approach to get users to register their MFA methods?
**A:** Microsoft Entra ID Protection — with a license, configure it to prompt users to register for MFA at their next sign-in.

**Q:** How can Conditional Access enforce MFA registration for all users?
**A:** Apply the policy to a group containing all users, then manually review the group periodically to remove registered users.

### Enabling MFA in the Portal

**Q:** Which license is required to configure MFA with Conditional Access in this module's exercise?
**A:** Microsoft Entra ID P1 or P2 (a 30-day free trial can be used).

**Q:** What is the portal path to the MFA options page?
**A:** Azure portal > Microsoft Entra ID > Security > Multifactor authentication (under Manage).

**Q:** Where in the portal do you enable or disable app passwords?
**A:** Microsoft Entra ID > Security > Multifactor authentication > Additional cloud-based multifactor authentication settings > Service Settings.

**Q:** What is the purpose of app passwords?
**A:** They let users create unique per-app passwords to authenticate to apps that don't support multifactor authentication.

**Q:** In a Conditional Access policy, which setting enforces MFA?
**A:** Under Grant: select Grant access, then Require multifactor authentication, then set Enable policy to On.

**Q:** In the exercise's guest-user MFA policy, which conditions were configured?
**A:** Locations — set Configure to Yes and select Any location.

**Q:** What happens the next time a user signs into an app protected by a new MFA Conditional Access policy?
**A:** The user is prompted to register for MFA and choose a preferred method.

### Configuring Authentication Methods

**Q:** Which authentication method is the only one you cannot disable?
**A:** Password.

**Q:** Which accounts can use security questions for SSPR?
**A:** Only non-administrative accounts — the method is available only for SSPR.

**Q:** How many predefined security questions does Azure provide, and what is the maximum length of custom questions?
**A:** 35 predefined questions (translated/localized by browser locale); custom questions max 200 characters.

**Q:** Who can read or change a user's security questions and answers?
**A:** Nobody but the user — answers are stored privately on the user object and can only be answered by the user, during registration.

**Q:** Which method is available only in SSPR, and what is the recommendation about it?
**A:** Email address — avoid email accounts that don't require the user's Microsoft Entra password to access.

**Q:** Name the three passwordless authentication solutions.
**A:** Windows Hello for Business, FIDO2 security keys, and the Microsoft Authenticator app.

**Q:** What does FIDO stand for?
**A:** Fast Identity Online — an open standard for passwordless authentication.

**Q:** Where can users sign in with FIDO2 security keys?
**A:** Microsoft Entra ID-joined or Entra hybrid-joined Windows 10 devices, with SSO to cloud and on-premises resources, and supported browsers.

**Q:** Which OATH token type does Microsoft Entra ID support for hardware tokens?
**A:** OATH-TOTP SHA-1 tokens, of the 30-second or 60-second variety.

**Q:** What is the limit on secret keys for OATH hardware tokens?
**A:** 128 characters (which might not be compatible with all tokens).

**Q:** In OATH software tokens, who generates the secret key (seed)?
**A:** Microsoft Entra ID generates the seed that is input into the app and used to generate each OTP.

**Q:** Which MFA method is not available to the free/trial Microsoft Entra tier?
**A:** Voice call — an automated voice system calls the number and the user confirms via keypad.

**Q:** On which platforms is the Microsoft Authenticator app available?
**A:** Android and iOS.

**Q:** Which services can use the Microsoft Authenticator app, FIDO2 keys, text messages, and voice calls?
**A:** Both Microsoft Entra multifactor authentication and SSPR.

**Q:** A user enabled for MFA can't authenticate from a legacy non-browser app. What is the fix?
**A:** Use an app password — it allows authentication to continue for non-browser apps that don't support MFA.

**Q:** How does a user sign in using the Microsoft Authenticator app?
**A:** Approve a push notification, or enter an OATH verification code from the app after providing username and password.

### Monitoring MFA and SSPR

**Q:** Where do you monitor authentication method activity for MFA and SSPR adoption?
**A:** Microsoft Entra ID > Monitoring > Usage & insights.

**Q:** What data window is used for the Usage & insights registration statistics?
**A:** The last 30 days of audit logs from the combined security info registration and SSPR registration experiences.

**Q:** Where can you see a specific user's latest registration audit information?
**A:** Drill down from the Usage & insights chart by clicking it.

**Q:** Where can you learn more about SSPR usage in the organization?
**A:** The Usage tab on the main Usage & insights view.
