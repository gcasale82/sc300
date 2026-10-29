# Module 12: Implement token customizations - Mind Map

<details><summary>Source lessons</summary>

- Introduction
- Implement token customizations
- Implement and configure consent settings
- Integrate on-premises apps with Microsoft Entra application proxy
- Integrate custom SaaS apps for single sign-on
- Implement application-based user provisioning
- Monitor and audit access to Microsoft Entra integrated enterprise applications
- Create and manage application collections
- Summary and resources

</details>

```mermaid
mindmap
  root((Enterprise app integration))
    Token customizations
      Token lifetime policies
        Set for all apps
        Set for multitenant apps
        Set for specific service principal
      Authentication session management
        Conditional Access controls
        High impact users
      Optional claims
        Add claims to tokens
        Supported in v1.0 and v2.0
        Smaller tokens in v2.0
    Consent settings
      User consent options
        Disable user consent
        Verified publishers only
        Low impact permissions
        Consent to all apps
        Custom app consent policies
      Risk based step up consent
        Risky requests need admin
        Error AADSTS90094
        Audit category ApplicationManagement
    Application Proxy
      Components
        Cloud proxy service
        On premises connector
      Supported app types
        Integrated Windows Authentication
        Form based and header based
        Remote Desktop Gateway apps
      How it works
        Entra sign in page
        Connector reads UPN and SPN
      Replaces VPN and reverse proxy
    Custom SaaS SSO
      App Gallery preconfigured apps
      SSO protocols
        SAML for authentication
        OIDC for authentication
        OAuth 2.0 for authorization
      SAML common with ADFS federation
      OIDC for pure cloud apps
    User provisioning
      Provisioning capabilities
        Create accounts automatically
        Deprovision when leaving
        Synchronize data between systems
        Provision groups
      Manual vs automatic
        Manual has no connector
        Automatic has connector
      SCIM standard
        Common user schema
        Users endpoint
        Groups endpoint
        SCIM 2.0 with SAML or OIDC
    Monitoring and auditing
      Usage and insights report
        Top used applications
        Most failed sign ins
        Path Enterprise applications Activity
      Audit logs
        Roles to read logs
          Security Administrator
          Security Reader
          Report Reader
          Global Reader
        Shows date service activity status
      Enterprise apps audit logs
        Consent to application events
    Application collections
      My Apps portal
        myapps.microsoft.com
        Discover and request apps
      Admin collections
        Requires Premium P1 or P2
        Path Enterprise Applications App Launchers
        Assign owners and apps
      Collections filter assigned apps
```
