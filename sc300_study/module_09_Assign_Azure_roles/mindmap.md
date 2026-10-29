# Module 09: Assign Azure roles - Mind Map

<details><summary>Source lessons</summary>

- Introduction
- Assign Azure roles
- Configure custom Azure roles
- Create and configure managed identities
- Access Azure resources with managed identities
- Analyze Azure role permissions
- Configure Azure Key Vault RBAC policies
- Retrieve objects from Azure Key Vault
- Summary and resources

</details>

```mermaid
mindmap
  root((Azure roles managed identities Key Vault))
    Module objectives
      Assign Azure and custom roles
      Manage application access
      Configure and use Key Vault
    Azure RBAC
      Security principals
        User
        Group
        Service principal
        Managed identity
      Built-in roles
        Owner
        Contributor
        Reader
        User Access Administrator
      Scope levels
        Management group
        Subscription
        Resource group
        Resource
        Lower levels inherit permissions
      4000 per subscription 500 per management group
      PowerShell New-AzRoleAssignment
      CLI az role assignment create
    Custom Azure roles
      Subscription
      Resource group
      Management group preview
      Stored in Microsoft Entra ID
      5000 custom roles per directory
      Entra admin center Roles and administrators
      PowerShell CLI REST API
      JSON definition
        roleName
        assignableScopes
        actions and notActions
        wildcard asterisk
    Managed identities
      No credential management no cost
      System-assigned
        Lifecycle tied to resource
        Deleted with resource
      User-assigned
        Standalone Azure resource
        Shared across instances
      Configure App Service Identity menu
      Zero trust least privilege
    Access other resources
      Open resource IAM page
      Add role assignment
      Select least privilege role and identity
      Review and assign
    Analyze permissions
      Member versus guest defaults
      Members register applications
      Restrict via user settings
        Block app registration
        Block portal access
      Attribute definition reader
    Key Vault access
      Access policy model
        1024 entries maximum
        Assign policies to groups
      Azure RBAC model
        Enable RBAC on vault
        Assign roles via IAM
      Built-in Key Vault roles
        Key Vault Administrator
        Certificates Officer
        Crypto Officer
        Secrets Officer
        Secrets User
        Key Vault Reader
    Retrieve secrets
      Add secret Generate Import
      Portal show secret value
      CLI secret show command
      PowerShell Get-AzKeyVaultSecret
```
