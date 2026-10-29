# Module 06: Administer FIDO2 and passwordless authentication methods - Mind Map

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

```mermaid
mindmap
  root((Microsoft Entra ID Authentication))
    Authentication Methods Overview
      Windows Hello Authenticator FIDO2 high
      OATH and SMS medium
      Voice medium password low
      Primary versus secondary factors
        FIDO2 and password primary
        SMS primary in preview
        OATH voice secondary only
      Password cannot be disabled
      Combined registration for MFA and SSPR
    FIDO2 Passwordless Authentication
      FIDO Alliance open specification
      WebAuthn based and unphishable
      USB Bluetooth or NFC keys
      Enable under Authentication methods policy
        Target all or selected users
      Register via Security Info portal
        Add method then security key
      Prerequisites MFA and Windows 10
    Authenticator App and OATH
      Push notifications and OATH codes
      Used for passwordless MFA SSPR
      OATH TOTP open standard
      HOTP standard not supported
      Up to five tokens total
    Windows Hello for Business
      Strong two-factor biometric or PIN
      Certificate or asymmetric key pair
      Keys generated in TPM
      Private key never leaves device
      Deployment security groups
        KeyCredential Admins
        Windows Hello for Business Users
        KeyAdmins for Server 2016 DCs
      Pluton security built into CPU
    Self-Service Password Reset
      Users reset without helpdesk
      Licensing requirements
        Premium P1 P2 or Business Standard
        Premium P1 P2 or Business Premium
      Enable for Selected group
        Global administrator account
        Protection then Password reset
      Configure methods registration notifications
      Register at aka.ms/ssprsetup
    Password Protection
      Global banned list free for cloud users
      Custom banned list needs Premium P1 P2
      On-premises components
        DC agent and proxy service
        Policy cached in sysvol
      Audit mode first then Enforce
      DCs never contact internet directly
      Server 2012 R2 with .NET 4.7.2
      DFSR required for sysvol
      Global Administrator registers proxy
      Two proxies for high availability
    Smart Lockout
      Default 10 failed attempts
      One minute initial lockout
      Tracks last three bad hashes
      Always on for all tenants
      Customization needs Premium P1 or higher
      Familiar and unfamiliar location counters
      Works with PHS and pass-through
      Entra threshold below AD DS threshold
      Entra duration above AD DS duration
      Configure via Password protection settings
    Kerberos and VM Authentication
      Application Proxy single sign-on
      Kerberos Constrained Delegation
        Connector impersonates users
        UPN and SPN exchanged
      Apps need integrated Windows authentication and SPNs
      Entra sign-in for Azure VMs
        Windows Server 2022 and later
        Windows 11 24H2 or later
        Supported Linux distributions
      System assigned managed identity
```
