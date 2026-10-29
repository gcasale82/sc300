# Module 06: Administer FIDO2 and passwordless authentication methods - Summary

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

```text
USER AUTHENTICATION IN MICROSOFT ENTRA ID (SC-300)
│
├── 🔐 METHOD STRENGTH & FACTOR TYPE (exam table)
│   ├── HIGH security ──> Windows Hello for Business / Authenticator app / FIDO2 key
│   ├── MEDIUM ──> OATH hardware (preview) & software tokens / SMS / Voice
│   ├── LOW ──> Password ⭐ can NEVER be disabled in Entra ID — only supplement with MFA
│   ├── Primary factor ──> WHfB, FIDO2, password; Authenticator app & SMS (preview)
│   ├── Secondary only ──> OATH tokens + Voice call (usable for MFA and SSPR)
