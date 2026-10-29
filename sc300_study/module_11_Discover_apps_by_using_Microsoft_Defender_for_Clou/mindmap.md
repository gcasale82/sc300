# Module 11: Discover apps by using Microsoft Defender for Cloud Apps and Active Directory Fe - Mind Map

<details><summary>Source lessons</summary>

- Introduction
- Discover apps by using Microsoft Defender for Cloud Apps and Active Directory Fe
- Configure connectors to apps
- Exercise implement access management for apps
- Design and implement app management roles
- Exercise create a custom role to manage app registration
- Configure preintegrated gallery SaaS apps
- Implement and manage policies for OAuth apps
- Summary and resources

</details>

```mermaid
mindmap
  root((Apps access and management))
    Introduction
      Discover apps in environment
      Design access management
      Implement app management roles
      Configure gallery SaaS apps
      Explore connectors and OAuth apps
    Discover apps
      CASB concept
        Security policy enforcement point
        Between consumers and providers
      Microsoft Defender for Cloud Apps
        Deployment modes
          Log collection
          API connectors
          Reverse proxy
        Cloud Discovery feature
          Analyzes traffic logs
          Snapshot report upload
          Continuous reports via log collectors
        Cloud Discovery Dashboard
          High level usage overview
          Top categories and users
          App risk score
      Filtering discovered apps
        App tag sanctioned or not
        Categories
        Compliance risk factor
        Risk score
      Cloud app catalog
        Over 16000 apps
        More than 80 risk factors
        Sanction or unsanction apps
      AD FS app activity report
        Apps active last 30 days
        Migration statuses
          Ready to migrate
          Needs review
          Additional steps required
        Required admin roles
          Global reader
          Report reader
          Security reader
          Application administrator
          Cloud application administrator
        Portal path
          Enterprise applications
          Usage and insights
      App types to migrate
        SaaS applications
        Line of business applications
        Modern protocols SAML and OIDC
        Legacy via Application Proxy
    Configure connectors
      Use provider APIs
      HTTPS encrypted communication
      API limits and throttling
      Multi-instance support
        API connected apps only
      Connection capabilities
        Account information
        Audit trail
        Account governance
        App permissions
        Data scan every 12 hours
        Data governance quarantine
    Access management exercise
      Entra ID Premium P2 trial
      Add gallery app GitHub
      Assign users and groups
    App management roles
      Delegation methods
        Restrict app registration
        Assign owners
        Built-in admin roles
        Custom roles
      Restrict who creates apps
        Users can register applications No
        Users can consent No
        Application Developer role
      Application owners
        First owner automatic
        Enterprise app owners users only
        Registration owners users and service principals
        Groups cannot be owners
      Built-in roles
        Application Administrator
          Includes app proxy
          Consent excluding Microsoft Graph
        Cloud Application Administrator
          No app proxy management
        Neither manages Conditional Access
      Custom roles
        Role definition then assignment
        Directory or single app scope
        Legacy app registrations not supported
    Custom role exercise
      Roles and administrators blade
      New custom role
      Permissions tab
      Select credential permissions
    Gallery SaaS apps
      App properties
        Enabled for users to sign in
        User assignment required
        Visible to users
      Custom logo 215 by 215 pixels
      Notes field
    OAuth app policies
      Portal security.microsoft.com
      Cloud apps OAuth apps
      Community use filter
      Approve or ban apps
      Ban disables enterprise application
      Policy via Control then Policies
    Summary
      Discovery with MDCA or AD FS report
      Access management for apps
      App management roles
      Gallery SaaS configuration
      Connectors and OAuth apps
```
