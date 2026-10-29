# Module 04: Plan, design, and implement Microsoft Entra Connect - Mind Map (part 1)

<details><summary>Source lessons</summary>

- Introduction
- Plan, design, and implement Microsoft Entra Connect
- Implement manage password hash synchronization (PHS)
- Implement manage pass-through authentication (PTA)
- Explore pass-through authentication and seamless single sign-on (SSO)
- Implement and manage federation
- Trouble-shoot synchronization errors
- Implement Microsoft Entra Connect Health

</details>

```mermaid
mindmap
  root((Hybrid Identity with Microsoft Entra ID))
    Introduction
      Hybrid identity spans on-premises and cloud
      Unified authentication and authorization
      Microsoft Entra Connect bridges directories
      Learning objectives
        Plan and implement Entra Connect
        Manage PHS and PTA
        Seamless SSO and federation
        Troubleshoot sync errors
        Entra Connect Health
    Plan and Implement Entra Connect
      Capabilities
        Synchronization of users and groups
        Password hash synchronization
        Pass-through authentication
        Federation integration
        Health monitoring
      Authentication methods
        Cloud authentication
          PHS simplest option
          PTA validates against on-premises AD
          Seamless SSO improves experience
        Federated authentication
          AD FS or PingFederate
          Smartcards and third-party MFA
          sAMAccountName sign-in
      Recommendations
          Enable PHS with any method
          Survive on-premises outages
          Identity Protection needs P2
      Design concepts
        sourceAnchor immutableId
          Fewer than 60 characters
          objectGuid for single forest
          Must be globally unique
        UPN sign-in
          Must match verified domain
          Non-routable domains need custom settings
      Topologies
        Single forest single tenant
        Multiple forests single tenant
        Account-resource forest
        Staging server standby
        One sync server per tenant
      Components
        Connector Space
        Metaverse
        Sync rules
        Run profiles
      Cloud sync
        Lightweight provisioning agents
        Multi-forest disconnected environments
        Large groups up to 50000 members
        Sync runs every 2 minutes
    Password Hash Synchronization
      How it works
        Hash of the hash synchronized
        Runs every 2 minutes
        Cannot modify frequency
        Overwrites existing cloud password
        Selective PHS after initial sync
      Enablement
        Enabled by Express Settings
        Available in custom settings
      FIPS consideration
        MD5 disabled on FIPS servers
        Edit miiserver.exe.config
        enforceFIPSPolicy disabled
      PingFederate
        Version 8.4 or later
        TLS SSL certificate required
    Pass-through Authentication
      Validates passwords against on-premises AD
      Enable via Entra Connect
        Custom installation path
        Change user sign-in task
      Agent requirements
        Recommend three agents
        Not supported in perimeter network
        Needs outbound internet access
      Tenant-level feature
        Affects all managed domains
        Wait 12 hours before AD FS shutdown
      PHS as backup method
        Manual failover only
    Seamless Single Sign-On
      Automatic sign-in on corporate desktops
      Combines with PHS or PTA
      Not applicable to AD FS
      Benefits
        No repeated password prompts
        No extra on-premises components
        Roll out with Group Policy
      Browser flow
        Kerberos ticket challenge
        AZUREADSSOACC computer account
        Entra ID decrypts ticket
      Native client flow
        WS-Trust MEX endpoint
        SAML token issued
        Access and refresh tokens
    Federation
      Trust between domains
      All authentication occurs on-premises
      AD FS Windows Server 2012 R2 or later
      Deployment requirements
        Local admin credentials
        Windows Remote Management access
      Setup steps
        Specify AD FS servers
        Specify Web Application Proxy servers
        Perimeter network placement
        Group managed service account
        Select domain to federate
      Management tasks
        Repair the trust
        Alternate sign-in ID
        Add federated domain
        Device writeback
          Conditional Access for ADFS devices
          RegisteredDevices container
    Troubleshoot Synchronization Errors
      Operations
        Import
        Synchronization
        Export
      Report in Entra Connect Health
        Updated every 30 minutes
      Data mismatch errors
        InvalidSoftMatch
          Hard Match uses sourceAnchor
          Soft Match uses ProxyAddresses and UPN
          Duplicate immutableId conflict
        ObjectTypeMismatch
          Different object types same proxyAddress
      Duplicate attributes
        AttributeValueMustBeUnique
          ProxyAddresses must be unique
          UserPrincipalName must be unique
      Data validation failures
        IdentityDataValidationFailed
          Invalid UPN characters or format
        FederatedDomainChangeError
          UPN suffix between federated domains
      LargeObject
        ExceededAllowedLength
        userCertificate limit 15 certificates
        thumbnailPhoto too large
      Admin role conflict
        No soft-match for admin users
        Remove roles and hard delete
    Entra Connect Health
      Monitors on-premises identity infrastructure
      Requires Entra ID Premium P1
      Portal features
        Alerts
        Performance monitoring
        Usage analytics
      Requirements
        Global administrator role
        Agent on each targeted server
        Outbound connectivity to endpoints
        PowerShell 4.0 or newer
        FIPS disabled
      Firewall ports
        TCP port 443
        TCP port 5671 for older agents
      Agent installation
        AD FS agent
          Separate from sync server
          Diagnostics Insights Monitoring services
        Sync agent
          Installed automatically with Connect
          Insights and Monitoring services
```
