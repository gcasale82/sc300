# Module 01: Configure company brand - Mind Map

<details><summary>Source lessons</summary>

- Introduction
- Configure company brand
- Configure and manage Microsoft Entra roles
- Exercise manage users roles
- Configure delegation by using administrative units
- Analyze Microsoft Entra role permissions
- Configure and manage custom domains
- Configure tenant-wide setting
- Exercise - setting tenant-wide properties
- Summary and resources

</details>

```mermaid
mindmap
  root((Configure and Manage Microsoft Entra Tenant))
    Company Branding
      License Requirements
        Microsoft Entra ID P1 or P2
        Office 365 for Office 365 apps
      Portal Path
        Microsoft Entra ID page
        Manage menu Company branding
      Configurable Settings
        Sign-in page background image
          Max 1920x1080 pixels
          Max 300000 bytes file size
        Banner logo on sign-in and My Apps
        Username hint max 64 characters
        Sign-in page text max 1024 characters
        Language fixed to default
    Microsoft Entra Roles
      Purpose
        Manage Microsoft Entra resources
        Tenant level or administrative unit scope
      Key Built-in Roles
        Global Administrator
          First creator of tenant
          Resets all passwords
          No Azure resource access by default
        User Administrator
          Manage users and groups
          Manage support tickets
        Billing Administrator
          Purchases and subscriptions
      Azure Roles Compared
          Azure roles control Azure resources
          Multiple scopes management group to resource
          Both support custom roles
      Elevation
        Global Admin can elevate to User Access Administrator
        Access management for Azure resources switch
      Assigning Roles
        Roles and administrators blade
        User or group Assigned roles page
        Access control IAM for broad scopes
        PowerShell and Microsoft Graph
        Privileged Identity Management
          Requires P2 license
          One role at a time
      Custom Roles
        Created in Roles and administrators
        Directory or app registration scope only
        Example permissions credentials and basic update
    Exercise Manage User Roles
      Get P2 free trial in Licenses
      Create user Adele Vance
      Assign Application Administrator role
      Remove role assignment
    Administrative Units
      Concept
        Containers for users groups and devices
        Restrict role scope to part of organization
        Least privilege delegation
      Managing Roles in Admin Units
        Authentication administrator
        Helpdesk administrator
        License administrator
        Password administrator
        User administrator
      Lifecycle Stages
        Initial adoption
        Pruning
        Stabilization
      Delegation Planning
        Define roles by frequency and risk
        Emergency access accounts
        Security defaults enforce MFA
      App Delegation Roles
        Application Administrator full scope
        Cloud Application Administrator no Application Proxy
        Application Developer registration and consent
        Enterprise Application Owner
        Application Registration Owner
    Analyze Role Permissions
      Member vs Guest Defaults
        Guests cannot enumerate all users
        Guests can invite other guests
        Members can register applications
      Restricting via User Settings
        Register applications
        Access Microsoft Entra admin portal
        LinkedIn connections
        External collaboration settings
      Viewing Permissions
        Roles and administrators role description
        Role permissions set
        Guest and service principal read permissions
    Custom Domains
      Primary Domain
        Initial contoso.onmicrosoft.com
        Make primary on a verified domain
        Does not change existing user names
      Limits
        Up to 900 managed domain names
        Up to 450 if federated
        Global Administrator manages domains
      Subdomains
        Verify root domain first
        Subdomain auto-verified
        TXT record for another tenant
      Deletion
        Blocked if users groups or apps reference it
        ForceDelete renames references to default domain
          Fewer than 1000 references required
          Fails for multitenant app rename
    Tenant-wide Settings
      User Settings
        Users can register applications
        Restrict admin portal access
          Directory Readers role grants portal access
        Sign in with LinkedIn
      External Collaboration Settings
        Guest user access levels
        Guest invite settings
        Guest self-service options
      Security Defaults
        MFA for all users and administrators
        Block legacy authentication
        Free for all organizations
      Tenant Properties
        Name and country or region
          Country set at creation and cannot change
        Tenant ID unique identifier
        Technical contact
        Global privacy contact
        Privacy statement URL
    Summary
      Roles and custom domains management
      Evaluate permissions by assignments
      Delegation with administrative units
      Tenant-wide configuration
```
