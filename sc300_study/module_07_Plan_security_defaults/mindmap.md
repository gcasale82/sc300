# Module 07: Plan security defaults - Mind Map

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

```mermaid
mindmap
  root((Conditional Access in Microsoft Entra ID))
    Security Defaults
      Microsoft managed preconfigured settings
      Enabled on new tenants
      Enforced policies
        All users register MFA
        Number matching prevents fatigue
        Admin roles require MFA
        Blocks legacy authentication protocols
      Blocks ActiveSync basic authentication
      Disable when using CA policies
    Plan Conditional Access
      Analyzes user device location signals
      Assignments
        Users and groups
        Cloud apps or actions
        Conditions
      Grant and session controls
      Best practices
        Emergency access accounts
        Report-only mode
      Common policies
        Require MFA
        Require managed devices
        Block access
    Implement Policies
      Create under Protection Conditional Access
      Risk-based policies
        Sign-in and user risk
        Requires P2 license
      Secure registration from trusted location
      Location blocking with named locations
      Require Intune compliant devices
      Exclusions
        Service accounts
        Agent identities
      Terms of Use consent
    Test and Troubleshoot
      Avoid blocking all users and apps
      Sign-in interrupt error page
      Sign-in logs
        Conditional Access tab
      Troubleshooting and support tab
      What If tool
    Application Controls
      Conditional Access App Control
        Reverse proxy architecture
        Defender for Cloud Apps
      Prevent data exfiltration
      Approved client apps
        Require approved client app
        Android and iOS platforms
      App protection policies
        MAM without enrollment
        Works without MDM
    Session Management and CAE
      Sign-in frequency
        Default rolling 90 days
        OAuth2 OIDC SAML apps
      Persistent browser session
      Continuous Access Evaluation
        Tokens valid one hour
        Near real time revocation
    Optimization Agent
      Requires P1 license
      Requires Security Compute Units
      Security Administrator activates
      Require MFA coverage
      Device-based controls
      Block legacy authentication
      Policy consolidation
      One-click remediation
    Licensing and Roles
      Free no Conditional Access
      P1 standard Conditional Access
      P2 risk-based policies
      Intune for device controls
      Conditional Access Administrator
      Security Administrator
      Global Reader for sign-in logs
```
