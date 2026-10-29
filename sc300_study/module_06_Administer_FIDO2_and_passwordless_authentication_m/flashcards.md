# Module 06: Administer FIDO2 and passwordless authentication methods - Flashcards

<details><summary>Source lessons</summary>

- Introduction
- Administer FIDO2 and passwordless authentication methods
- Explore Authenticator app and OATH tokens
- Implement an authentication solution based on Windows Hello for Business
- Exercise configure and deploy self-service password reset
- Deploy and manage password protection
- Configure smart lockout thresholds
- Exercise - Manage Microsoft Entra smart lockout values
- Implement Kerberos and certificate-based authentication in Microsoft Entra ID
- Configure Microsoft Entra user authentication for virtual machines
- Summary and resources

</details>

### Authentication Methods Overview

**Q:** Which three passwordless authentication methods provide the most secure sign-in events in Microsoft Entra ID?
**A:** Windows Hello for Business, FIDO2 security keys, and the Microsoft Authenticator app.

**Q:** Which authentication methods can be used as the PRIMARY factor at sign-in?
**A:** Password, Windows Hello for Business, and FIDO2 security keys (Microsoft Authenticator app and SMS in preview). OATH tokens and voice calls are secondary only (MFA/SSPR).

**Q:** Can you disable the password authentication method in Microsoft Entra ID?
**A:** No. Passwords cannot be disabled; increase sign-in security by adding multifactor authentication.

**Q:** You need to support legacy apps that don't support modern authentication with per-user MFA. Which method should you use?
**A:** App passwords.

**Q:** Which verification methods are used ONLY for SSPR (not for MFA)?
**A:** Security questions and email address.

**Q:** What does Microsoft recommend to simplify onboarding for MFA and SSPR?
**A:** Enable combined security information registration, and require users to register multiple authentication methods for resiliency.

### FIDO2 Security Keys

**
