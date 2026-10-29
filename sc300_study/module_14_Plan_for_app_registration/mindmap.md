# Module 14: Plan for app registration - Mind Map

<details><summary>Source lessons</summary>

- Introduction
- Plan for app registration
- Explore application objects and service principals
- Create app registrations
- Configure app authentication
- Configure API permissions
- Create app roles
- Summary

</details>

```mermaid
mindmap
  root((App Registration in Microsoft Entra ID))
    Introduction and Benefits
      Identity configuration for apps
      Custom branding in sign-in dialog
      Tenant configuration choices
      Request scope permissions
      Confidential client applications
    Single versus Multitenant Apps
      Single-tenant apps
        Home tenant only
        Accounts in this directory only
      Multitenant apps
        Users from other tenants
        Service principal per tenant
      Personal Microsoft accounts supported
      Audience setting in admin center
    Application Objects
      Blueprint or template of app
      Resides in home tenant
      One-to-one with the application
      Describes token issuance
      Describes needed resources
      Describes app actions
      Certificates preferred over secrets
    Service Principals
      Instance of the application
      One-to-many with application object
      Types
        Application principal
        Managed identity
          Preferred for Azure workloads
          Eliminates credential management
        Legacy principal
      Managed application in local directory
    Create App Registrations
      Requires Application Developer role
      Admin center path
