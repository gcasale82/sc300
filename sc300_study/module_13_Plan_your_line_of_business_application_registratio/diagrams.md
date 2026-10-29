# Module 13: Plan your line of business application registration strategy - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Plan your line of business application registration strategy
- Implement application registration
- Register an application
- Configure permission for an application
- Grant tenant-wide admin consent to applications
- Implement application authorization
- Exercise add app roles to an application and receive tokens
- Manage and monitor application by using app governance
- Summary and resources

</details>

### Application object versus service principal architecture
Use this to answer exam questions about where each object lives, which portal blade manages it, and how they relate across tenants.

```mermaid
flowchart LR
    A["Application object"] -->|"one per app in home tenant"| B["Managed under App registrations"]
    A -->|"referenced by Application ID"| C["Service principal"]
    C -->|"instance of the app per tenant"| D["Managed under Enterprise applications"]
    C -->|"stores local"| E["Consent grants role assignments policies"]
    F["Microsoft services directory"] -->|"publishes"| G["Microsoft first-party apps"]
    H["App gallery directory"] -->|"publishes"| I["Preintegrated third-party apps"]
    J["Publisher SaaS directory"] -->|"publishes"| K["Vendor multitenant apps"]
    L["Creating a service principal"] -->|"via PowerShell"| M["App object not required"]
    L -->|"via Microsoft Graph"| N["App object required first"]
```

### Choosing between delegated and application permissions
Use this decision tree to pick the correct permission type when a question describes how the app runs, with the typical wrong answer shown as rejected.

```mermaid
flowchart TD
    A["App must call a protected API"] --> B{"Signed-in user present at runtime?"}
    B -->|"yes"| C["Delegated permissions"]
    B -->|"no daemon or background service"| D["Application permissions"]
    C --> E["Effective permission is intersection with user privileges"]
    C -->|"example"| F["User.ReadWrite.All limited by user rights"]
    D --> G["Effective permission is full privilege level"]
    D -->|"example"| H["User.ReadWrite.All updates every profile"]
    D --> I["Only an administrator can consent"]
    B -->|"wrong choice for daemon"| X["Rejected delegated permissions"]
    B -->|"wrong choice for user app"| Y["Rejected application permissions"]
```

### Choosing the correct consent path
Use this decision tree to select the right consent endpoint and portal path when a question mentions admin-restricted scopes or tenant-wide consent.

```mermaid
flowchart TD
    A["App requests permissions"] --> B{"Which permission type?"}
    B -->|"delegated low privilege"| C["User can consent at sign-in"]
    B -->|"delegated admin-restricted"| D["User consent blocked with error"]
    B -->|"application permissions"| E["Admin consent endpoint only"]
    C --> F["Requested with scope parameter"]
    D --> G["Tenant-wide admin consent"]
    E --> G
    G --> H["App registrations then API permissions"]
    G --> I["Enterprise applications then Permissions"]
    G --> J["adminconsent URL with tenant and client id"]
    G --> K["Requires Privileged Role Administrator or custom role"]
    G --> L["Revokes prior tenant-wide grants not user grants"]
    G --> M["Delegated admin consent covers all users"]
    E --> N["Grant is to the app itself not a user"]
    B -->|"wrong choice"| X["Rejected user consent for application permissions"]
```

### Selecting the supported account type audience
Use this decision tree to map a described target audience to the correct single-tenant or multitenant setting during app registration.

```mermaid
flowchart TD
    A["Supported account type at registration"] --> B{"Who is the target audience?"}
    B -->|"internal organization only"| C["Accounts in this directory only"]
    C --> C1["Single tenant app"]
    B -->|"business or education customers"| D["Accounts in any Microsoft Entra directory"]
    D --> D1["Multitenant app"]
    B -->|"widest reach with personal accounts"| E["Any directory plus personal Microsoft accounts"]
    E --> E1["Multitenant app"]
    D1 --> F["Test with Conditional Access policies"]
    E1 --> F
    F --> G["Request least privileges"]
    G --> H["Give permissions clear names"]
    B -->|"wrong choice"| X["Rejected single tenant for external customers"]
```

### End-to-end registration consent and token flow
Use this sequence to recall the exact order of registration, service principal creation, consent, and token evaluation in exam scenarios.

```mermaid
sequenceDiagram
    participant Dev as Developer or Admin
    participant Reg as App registrations
    participant Ten as Home tenant
    participant Usr as Signed-in user
    participant API as Web API
    Dev->>Reg: Register app and expose scopes
    Reg->>Ten: Application object created in home tenant
    Ten->>Ten: Service principal instantiated
    Usr->>Ten: First sign-in and consent
    Ten->>Ten: Consent recorded on service principal
    Ten-->>Usr: Security token issued with roles and scp claims
    Usr->>API: Call API with access token
    API->>API: Evaluate scp claim scopes
    API-->>Usr: Operation allowed only if scope present
```
