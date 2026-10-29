# Module 14: Plan for app registration - Diagrams

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

### Decision tree - Choosing supported account types
Useful when the exam gives an audience scenario and asks which supported account type to select, including the classic multitenant distractor.
```mermaid
flowchart TD
    A["Which users must sign in"] -->|"Only users in my organization"| B["Accounts in this directory only"]
    B --> B1["Single-tenant app"]
    A -->|"Any work or school account"| C["Accounts in any Microsoft Entra directory"]
    C --> C1["Multitenant app"]
    A -->|"Work school or personal accounts"| D["Any directory plus personal Microsoft accounts"]
    D --> D1["Multitenant app including personal accounts"]
    A -->|"Internal app but picks multitenant"| X["Distractor - rejected"]
    X --> X1["Unneeded audience expansion"]
```

### Decision tree - Choosing the credential type for an app
Useful for questions on how a confidential client proves its identity and why certificates or managed identities beat client secrets.
```mermaid
flowchart TD
    A["App needs to prove its identity"] --> B{"Where does the app run"}
    B -->|"Azure workload"| C["Managed identity"]
    C --> C1["Preferred - no credentials to manage"]
    B -->|"Outside Azure"| D{"Can it hold credentials securely"}
    D -->|"Yes - confidential client"| E{"Pick credential type"}
    E -->|"Recommended"| F["Certificate"]
    E -->|"Distractor"| G["Client secret"]
    G --> G1["Rejected - only when certificate not feasible"]
    D -->|"No - public client"| H["No client secret required"]
```

### Decision tree - App roles versus group-based authorization
Useful when a question contrasts app roles with group claims, group overage, code complexity and licensing implications.
```mermaid
flowchart TD
    A["App needs authorization claims in tokens"] --> B{"Choose authorization model"}
    B -->|"Define app roles"| C["App roles on the app registration"]
    C --> C1["Roles claim in ID and access tokens"]
    C --> C2["No group overage - no P1 needed"]
    C --> C3["Always define a baseline user role"]
    B -->|"Rely on group claims"| D["Group-based authorization"]
    D --> D1["Risk group overage and GUID mapping"]
    B -->|"Distractor - hardcode group IDs"| E["Rejected - complex and less portable"]
```

### Architecture - Application object versus service principals
Useful for questions on where each object lives, how service principals are created per tenant, and what deletion or restore does.
```mermaid
flowchart LR
    A["Application object - home tenant"] -->|"Template - one to many"| B["Service principal - home tenant"]
    A -->|"Created at first user consent"| C["Service principal - tenant two"]
    A -->|"Created at first user consent"| D["Service principal - tenant three"]
    B --> E["Local roles consent and Conditional Access"]
    C --> F["Independent of home tenant changes"]
    D --> F
    A -->|"Single-tenant app"| G["Only one service principal"]
    A -->|"Delete application object"| H["Home tenant SP deleted and not restored"]
```

### Process - Registering a single-page application
Useful to memorize the exact admin-center path, the values to record, and the platform settings for a SPA registration.
```mermaid
sequenceDiagram
    participant Admin
    participant Entra as Microsoft Entra admin center
    Admin->>Entra: Sign in with at least Application Developer role
    Admin->>Entra: Applications then App registrations then New registration
    Admin->>Entra: Enter name and supported account types
    Note over Admin,Entra: No redirect URI at this stage
    Admin->>Entra: Select Register
    Entra-->>Admin: Application client ID and Directory tenant ID
    Admin->>Entra: Open Authentication and Add a platform
    Admin->>Entra: Select Single-page application tile
    Admin->>Entra: Enter redirect URI then Save
    Note over Admin,Entra: Leave implicit grant checkboxes cleared
    Entra-->>Admin: Authorization code flow with PKCE and CORS enabled
```
