# Module 09: Assign Azure roles - Flashcards

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

### Azure RBAC Fundamentals

**Q:** What is Azure RBAC?
**A:** The authorization system used to manage access to Azure resources. Access is granted by assigning roles to users, groups, service principals, or managed identities at a particular scope.

**Q:** You need to grant an application access to an Azure resource. Which security principal should you assign the role to?
**A:** A service principal.

**Q:** You want an application to manage its own credentials for authentication. Which security principal should you use?
**A:** A managed identity.

**Q:** Owner vs Contributor: what can Owner do that Contributor cannot?
**A:** Owner has full access to all resources, including granting access. Contributor can create and manage all types of Azure resources but cannot grant access.

**Q:** Which built-in Azure role is used specifically to assign access to Azure resources?
**A:** User Access Administrator.

**Q:** Which built-in Azure role can only view available Azure resources?
**A:** Reader.

### Scope and Role Assignment Limits

**Q:** Name the four levels of scope at which you can assign Azure roles.
**A:** Management group, subscription, resource group, and resource.

**Q:** How do role permissions flow across scope levels?
**A:** Lower levels inherit role permissions from higher levels (parent-child hierarchy); each lower level makes the scope more specific.

**Q:** You assign the Reader role to a user at management group scope. What can the user read?
**A:** Everything in all subscriptions in the management group.

**Q:** What is the maximum number of role assignments per subscription, and what does it include?
**A:** Up to 4,000 — including assignments at the subscription, resource group, and resource scopes.

**Q:** What is the maximum number of role assignments per management group?
**A:** Up to 500.

**Q:** What best practice should you follow when choosing roles and scopes, and why?
**A:** Least privilege — grant the minimum role at the narrowest scope needed, limiting what is at risk if the security principal is compromised.

### Assigning Azure Roles

**Q:** Which Azure portal page do you use to assign roles, whether you are on a user, group, resource group, or subscription?
**A:** Access control (IAM) — identity and access management.

**Q:** Which PowerShell cmdlet assigns an Azure role?
**A:** New-AzRoleAssignment, with -ObjectId, -RoleDefinitionName, and -Scope parameters.

**Q:** Which Azure CLI command creates a role assignment?
**A:** az role assignment create --assignee "{assignee}" --role "{roleNameOrId}" --resource-group "{resourceGroupName}"

### Custom Azure Roles

**Q:** Where are custom Azure roles stored, and can they be shared across subscriptions?
**A:** Stored in Microsoft Entra ID; yes, they can be shared across subscriptions.

**Q:** How many custom roles can each directory have?
**A:** Up to 5,000 custom roles.

**Q:** At which scopes can custom Azure roles be assigned?
**A:** Management group (in preview only), subscription, and resource group.

**Q:** What is the portal path to create a custom role?
**A:** Microsoft Entra admin center > Identity menu > Roles and administration > + New custom role.

**Q:** In a custom role JSON definition, what does Microsoft.Billing/*/read grant?
**A:** All read permissions from the Billing resource — the asterisk (*) is a wildcard and can exist at any level.

### Managed Identities

**Q:** What core problem do managed identities solve?
**A:** They eliminate the need for developers to manage secrets, credentials, certificates, and keys — Microsoft Entra ID automatically manages the identity used to obtain tokens.

**Q:** System-assigned vs user-assigned managed identity: how do their lifecycles differ?
**A:** System-assigned is tied to the service instance's lifecycle and is deleted automatically when the resource is deleted; user-assigned is a standalone resource managed separately from the resources using it.

**Q:** Which managed identity type can, by design, only be used by the Azure resource it is enabled on?
**A:** System-assigned — only that resource can use the identity to request tokens from Microsoft Entra ID.

**Q:** How many service instances can use a user-assigned managed identity?
**A:** One or more instances of an Azure service — it is assigned to one or more instances.

**Q:** What does using managed identities cost?
**A:** Nothing extra — managed identities can be used without any additional cost.

**Q:** What is the portal path to add a managed identity to an App Service app?
**A:** Open the app in the Azure portal > Identity > select System assigned or User assigned > + Add.

**Q:** Which CLI command assigns a managed identity to a web app?
**A:** az webapp identity assign --resource-group
