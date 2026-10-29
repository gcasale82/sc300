# Module 09: Assign Azure roles - Diagrams

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

### Decision tree - Choosing the security principal for a role assignment
Useful when an exam question asks which principal should receive an Azure role for a given scenario, including the hard-coded-credentials distractor.

```mermaid
flowchart TD
    A["Who needs access to the resource"] --> B{"Single person only"}
    B -->|"yes"| C["Assign role to a User"]
    B -->|"no"| D{"Same role for many users"}
    D -->|"yes"| E["Assign role to a Group"]
    D -->|"no"| F{"Application needs resource access"}
    F -->|"yes"| G{"App should manage its own credentials"}
    G -->|"yes"| H["Managed Identity"]
    G -->|"no"| I["Service Principal"]
    F -->|"distractor"| J["Rejected - hard coded app credentials"]
```

### Decision tree - Key Vault RBAC versus access policies
Useful for questions asking which Key Vault access model fits a requirement, including the trap of assigning policies to individual users.

```mermaid
flowchart TD
    A["Grant access to Key Vault objects"] --> B{"Manage permissions across all vaults"}
    B -->|"yes"| C["Use Azure RBAC roles"]
    B -->|"no"| D{"Need granular per object control"}
    D -->|"yes"| E["Use Key Vault access policies"]
    D -->|"no"| C
    C --> F["Separate roles for keys secrets certificates"]
    E --> G["Assign policies to groups not users"]
    A -->|"distractor"| H["Rejected - access policy per individual user"]
```

### Decision tree - System-assigned versus user-assigned managed identity
Useful for distinguishing the two managed identity types by lifecycle and rejecting the service-principal-with-stored-secret distractor.

```mermaid
flowchart TD
    A["App needs Entra tokens with no credentials"] --> B{"Identity tied to one resource lifecycle"}
    B -->|"yes"| C["System assigned managed identity"]
    B -->|"no"| D{"Shared across multiple resources"}
    D -->|"yes"| E["User assigned managed identity"]
    D -->|"no"| C
    C --> C1["Deleted automatically with the resource"]
    E --> E1["Managed separately from the resources"]
    A -->|"distractor"| F["Rejected - service principal with stored secret"]
```

### Process - The five steps to assign an Azure role
Useful for recalling the ordered role assignment steps, the supported tools, and the assignment limits tested on the exam.

```mermaid
flowchart TD
    A["Start - grant access to Azure resource"] --> B["Step 1 - pick the security principal"]
    B --> C["Step 2 - choose built-in or custom role"]
    C --> D["Step 3 - pick the scope level"]
    D --> E["Step 4 - confirm your own assignment rights"]
    E --> F["Step 5 - assign the role"]
    F --> G["Portal - Access control IAM page"]
    F --> H["PowerShell - New-AzRoleAssignment"]
    F --> I["CLI - az role assignment create"]
    F --> J["Limit - 4000 assignments per subscription"]
    F --> K["Limit - 500 assignments per management group"]
```

### Architecture - Azure scope hierarchy and inheritance
Useful for visualizing the four scope levels and how lower levels inherit role permissions, a recurring exam topic.

```mermaid
flowchart LR
    A["Management group"] -->|"role inherits down"| B["Subscription"]
    B -->|"role inherits down"| C["Resource group"]
    C -->|"role inherits down"| D["Resource"]
    E["Each level is more specific"] --> A
    F["Least privilege - narrow scope"] --> D
```
