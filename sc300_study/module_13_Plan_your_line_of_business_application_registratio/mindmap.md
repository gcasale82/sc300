# Module 13: Plan your line of business application registration strategy - Mind Map

<details><summary>Source lessons</summary>

- Introduction
- Plan your line of business application registration strategy
- Implement application registration
- Register an application
- Configure permission for an application
- Grant tenant-wide admin consent to applications
- Implement application authorization
- Exercise add app roles to an application and receive tokens
- Manage and monitor application by using app governance
- Summary and resources

</details>

```mermaid
mindmap
  root((App registrations and permissions))
    Module summary
      Plan registration strategy
      Implement app registrations
      Configure app permissions
      Maintain app governance
    Why integrate apps
      Authentication and authorization
      Single sign on
      User provisioning and sync
      OAuth authorization services
      Application proxy publishing
    Application objects
      Managed in App registrations
      Define app for token issuance
      Exist only in home directory
      Redirect URIs secrets roles
      Created via portal or Graph
    Service principals
      Managed in Enterprise applications
      Instance of app in directory
      Local role and consent records
      Created on first user consent
      Graph API needs application object
    Registration strategy
      Application or Cloud Application Administrator
      Users can register apps by default
      Disable user consent or registration
      Single tenant versus multitenant
      Test with Conditional Access
    Implement registration
      Add redirect URI
      Platform configurations
        Web and single-page apps
        iOS Android bundle IDs
        Mobile and desktop apps
      Credentials
        Certificates recommended
        Client secret shown once
      Web API scopes
        Expose an API
        Application ID URI unique
        Admins only for privileged scopes
        Pre-authorized client apps
    Permissions and consent
      OAuth 2.0 scopes and resource URIs
      Delegated permissions
        Act as signed-in user
        Effective least privilege
      Application permissions
        Run without signed-in user
        Admin consent only
      OpenID Connect scopes
        openid email profile
        offline_access refresh tokens
        Access token expires one hour
    Tenant-wide admin consent
      Privileged Role Administrator role
      Grant from App registrations
      Revokes prior tenant-wide grants
      Grant from Enterprise applications
      Admin consent URL format
      Admin-restricted permissions
        User.Read.All
        Directory.ReadWrite.All
        Groups.Read.All
      Admin consent endpoint
    App roles
      App roles UI or manifest
      Roles claim in tokens
      Assign users and groups
      Enterprise applications Users and groups
    App governance
      Defender for Cloud Apps add-on
      OAuth apps via Microsoft Graph
      Insights dashboard
      Governance policies
      Detection alerts
      Remediation controls
      Enable sync in settings
```
