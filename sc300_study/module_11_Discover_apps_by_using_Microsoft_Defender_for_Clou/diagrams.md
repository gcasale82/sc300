# Module 11: Discover apps by using Microsoft Defender for Cloud Apps and Active Directory Fe - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Discover apps by using Microsoft Defender for Cloud Apps and Active Directory Fe
- Configure connectors to apps
- Exercise implement access management for apps
- Design and implement app management roles
- Exercise create a custom role to manage app registration
- Configure preintegrated gallery SaaS apps
- Implement and manage policies for OAuth apps
- Summary and resources

</details>

### Diagram 1: Choosing the right app discovery tool
Use this to match a discovery requirement to the correct tool and eliminate the classic distractors that swap Cloud Discovery with the AD FS report.
```mermaid
flowchart TD
    A["Exam requirement - discover apps"] --> B{"What must be discovered?"}
    B -->|"Shadow IT cloud apps in use"| C["Cloud Discovery in MDCA"]
    B -->|"AD FS apps to migrate"| D["AD FS application activity report"]
    B -->|"Modern protocols SAML OIDC"| E["Gallery connector or app registration"]
    B -->|"Legacy protocols on-premises app"| F["Application Proxy or Domain Services"]
    C -->|"wrong choice for migration"| X1["Distractor - AD FS report"]
    D -->|"wrong choice for shadow IT"| X2["Distractor - Cloud Discovery"]
```

### Diagram 2: Delegating application management - role selection
Use this to pick the least-privilege role or delegation method in app management scenarios, including the app proxy and Conditional Access gotchas.
```mermaid
flowchart TD
    A["Delegate application management"] --> B{"What must the delegate do?"}
    B -->|"Manage app proxy settings"| C["Application Administrator role"]
    B -->|"Manage apps without proxy"| D["Cloud Application Administrator role"]
    B -->|"Manage one specific app"| E["Application owner"]
    B -->|"Granular permissions at object scope"| F["Custom role"]
    B -->|"Register apps when default disabled"| G["Application Developer role"]
    C -->|"distractor for proxy tasks"| X1["Rejected - Cloud Application Administrator"]
    C -->|"limitation"| H["Neither admin role manages Conditional Access"]
    D --> H
    E -->|"limitation"| I["Only users own enterprise apps"]
```

### Diagram 3: AD FS application activity report workflow and migration statuses
Use this to recall the exact portal path and the three migration statuses tested in AD FS migration questions.
```mermaid
flowchart TD
    A["Sign in with allowed admin role"] --> B["Microsoft Entra ID blade"]
    B --> C["Enterprise applications"]
    C --> D["Usage and insights under Activity"]
    D --> E["AD FS application activity"]
    E --> F["Apps with sign-ins in last 30 days"]
    F --> G{"Migration status?"}
    G -->|"Ready to migrate"| H["Migrate as-is"]
    G -->|"Needs review"| I["Review settings that cannot migrate"]
    G -->|"Additional steps required"| J["Cannot migrate in current state"]
```

### Diagram 4: App connector connection flow in Defender for Cloud Apps
Use this to remember the connector scan order and the 12-hour plus real-time data scan cadence.
```mermaid
sequenceDiagram
    participant Admin
    participant MDCA as Defender for Cloud Apps
    participant App as Connected app API
    Admin->>MDCA: Connect app using provider API
    MDCA->>App: Scan and save authentication permissions
    MDCA->>App: Request user list
    App-->>MDCA: Users returned after first scan
    loop Periodic scans
        MDCA->>App: Scan users groups activities files
    end
    MDCA-->>Admin: Activities visible after first full scan
