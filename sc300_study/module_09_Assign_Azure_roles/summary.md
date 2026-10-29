# Module 09: Assign Azure roles - Summary

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

```text
AZURE RBAC, MANAGED IDENTITIES & KEY VAULT (SC-300)
│
├── 🔑 AZURE ROLES (Azure RBAC ──> authorization system for Azure resources)
│   ├── Who gets access? ──> User / Group / Service Principal / Managed Identity
│   ├── Built-in roles ─────> Owner (full + grant access) / Contributor (no grant access)
│   │                        Reader (view only) / User Access Administrator (assign access)
│   ├── Scope levels ───────> Management group > Subscription > Resource group > Resource
│   │                        ⭐ Lower levels INHERIT permissions from higher levels
│   ├── Limits ─────────────> 4,000 role assignments per subscription
│   │                        500 role assignments per management group
│   ├── Portal path ────────> Access control (IAM) blade (user, group, RG, or subscription)
│   ├── PowerShell ─────────> New-AzRoleAssignment -ObjectId -RoleDefinitionName -Scope
│   └── CLI ────────────────> az role assignment create --assignee --role --resource-group
│
├── 🛠️ CUSTOM ROLES (when built-ins don't fit ──> least privilege)
│   ├── Assignable scopes ──> Management group (preview only) / Subscription / Resource group
│   ├── Limits ─────────────> Up to 5,000 custom roles per directory
│   │                        ⭐ Shared across subscriptions, stored in Microsoft Entra ID
│   ├── Portal path ────────> Entra admin center > Identity > Roles and administration
│   │                        > + New custom role
│   └── JSON template ──────> roleName / description / assignableScopes / permissions
│                            (actions + notActions + dataActions + notDataActions,
│                             * = wildcard at any level, e.g. Microsoft.Billing/*/read)
│
├── 🤖 MANAGED IDENTITIES (eliminate credential management ──> Entra tokens, no secrets)
│   ├── System-assigned ────> Tied to the service instance lifecycle
│   │                        ⚠️ Deleted automatically when the resource is deleted
│   │                        Only that resource can use it to request tokens
│   ├── User-assigned ──────> Standalone Azure resource, managed separately
│   │                        ⭐ Can be assigned to ONE OR MORE service instances
│   ├── Cost ───────────────> No extra cost; works with any Entra-auth resource
│   ├── Portal path ────────> App Service > Identity > System assigned / User assigned
│   ├── CLI ────────────────> az webapp identity assign --identities <identity-name>
│   ├── PowerShell ─────────> Update-AzFunctionApp -IdentityType UserAssigned
│   │                        (AZ.ManagedServiceIdentity module)
│   └── Grant access ───────> Target resource > Access control (IAM) > Add role assignment
│                            (pick Owner / Contributor / Reader per least privilege)
│
├── 👤 USER PERMISSIONS (default rights ──> member vs guest)
│   ├── Member users ───────> Enumerate users / invite guests / create Security & M365 groups
│   │                        / register apps / read app properties
│   ├── Guest users ────────> ⚠️ Slightly FEWER default permissions than members
│   ├── Restrict ───────────> Entra ID > User settings (block app registration, portal
│   │                        access, LinkedIn, external collaboration settings)
│   ├── Add permissions ────> Roles and administrators (assign roles to users/groups)
│   └── Audit a role ───────> Roles and administrators > select role > (...) > description
│                            page (Attribute definition reader)
│
├── 🗝️ AZURE KEY VAULT ACCESS (two authorization models ──> pick per security posture)
│   ├── Access policies ────> More granular but harder to manage
│   │                        ⚠️ Limit: 1,024 access policy entries per vault
│   │                        ⭐ Assign policies to GROUPS, not individual users
│   ├── Azure RBAC ─────────> One place to manage permissions across ALL vaults
│   │                        Scopes: management group / subscription / RG / resource
│   │                        Requires: enable RBAC on vault + assign via Identity & Access (IAM)
│   ├── RBAC built-ins ─────> Key Vault Administrator (all data-plane ops, no RBAC mgmt)
│   │                        Certificates Officer / Crypto Officer / Secrets Officer
│   │                        Crypto User (crypto ops) / Crypto Service Encryption User
│   │                        Reader (metadata only) / Secrets User (read contents)
│   └── Best practice ──────> ⭐ One vault per application per environment (Dev / Pre-Prod / Prod)
│
├── 📥 KEY VAULT SECRETS (add & retrieve)
│   ├── Add secret ─────────> Key Vault > Secrets > Generate/Import (Manual / Name / Value)
│   ├── Portal retrieve ────> Open secret > "Show secret value" button
│   ├── CLI ────────────────> az keyvault secret show --name --vault-name --query "value"
│   ├── PowerShell ─────────> Get-AzKeyVaultSecret -VaultName -Name -AsPlainText
│   └── Applications ───────> SDKs available for .NET / Node.js / Python
│
└── 🛡️ GOLDEN RULES (exam mindset)
    ├── Least privilege ────> Narrowest role at narrowest scope (limits blast radius)
    ├── Managed identities ─> Assign to the APP, configure inside the hosting service
    └── Permissions flow ───> Assigned at user/group level, always reach the user in the end
```
