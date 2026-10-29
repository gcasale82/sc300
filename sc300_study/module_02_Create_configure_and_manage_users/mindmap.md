# Module 02: Create, configure, and manage users - Mind Map

<details><summary>Source lessons</summary>

- Introduction
- Create, configure, and manage users
- Exercise - assign licenses to users
- Exercise - restore or remove deleted users
- Create, configure, and manage groups
- Exercise - add groups in Microsoft Entra ID
- Configure and manage device registration
- Manage licenses
- Exercise - change group license assignments
- Exercise - change user license assignments
- Create custom security attributes
- Explore automatic user creation
- Summary and resources

</details>

```mermaid
mindmap
  root((Manage Users Groups and Devices))
    Module Overview
      Microsoft Entra ID cloud identity service
      End to end identity management
      Single sign on and multifactor authentication
      Managing licenses and device registration
    Users
      User account authenticates and builds token
      Microsoft Entra admin center
      Three user identities
        Cloud identities
        Directory synchronized identities
          Entra Cloud Sync recommended
          Entra Connect Sync complex scenarios
        Guest users invited
      Deleted users retained 30 days
        Restore within suspended window
        Permanent deletion cannot be restored
      Roles to restore users
        User administrator
        Global administrator
        Partner Tier 1 and 2 support
    Groups
      Security groups manage resource access
      Microsoft 365 groups collaboration
      Membership types
        Assigned manual
        Dynamic User
        Dynamic Device security groups only
      Dynamic groups need Entra ID P1
      Rules evaluate attributes automatically
      Exercise creates Marketing and Northwest Sales
    Licenses
      Group based licensing
      Requirements
        Entra ID Premium P1
        Office 365 Enterprise E3 or greater
        One license per unique member
      Managed in Microsoft 365 admin center
      License assignment errors
        CountViolation not enough licenses
        MutuallyExclusiveViolation conflicting service plans
        DependencyViolation dependent products
        ProhibitedInUsageLocationViolation usage location
        Duplicate proxy addresses
      Usage location inherited from directory
      Reprocess group or user
      Deleted group converts to direct assignment
      Migrate direct to group licenses
    Device Registration
      Balance productivity and asset protection
      Entra registered devices
        BYOD and mobile scenarios
        Signed in with local account
        Conditional Access via Intune
      Entra joined devices
        Cloud first or cloud only
        Organizational account sign in
        Windows Home editions excluded
        OOBE bulk enrollment Autopilot
      Hybrid Entra joined
        Joined on-premises AD plus Entra
        Group Policy and Configuration Manager
      Device writeback no longer supported
      Cloud Kerberos Trust replaces writeback
    Custom Security Attributes
      Business specific key value pairs
      Assign to users and applications
      Fine grained access control
      Data types Boolean integer string
      Single or multiple values
      Not supported in Domain Services
      Not supported in SAML or JWT
    Automatic User Provisioning
      SCIM open standard protocol
      Uses SCIM 2.0
      HCM system HR source
      Entra Provisioning Service
      Target system SCIM endpoint
      Deprovisioning reduces breach risk
      API driven inbound provisioning
        General availability March 2024
        Workday and SAP SuccessFactors supported
```
