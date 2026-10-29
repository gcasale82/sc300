# Module 18: Analyze and investigate sign-in logs to troubleshoot access issues - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Analyze and investigate sign-in logs to troubleshoot access issues
- Review and monitor Microsoft Entra audit logs
- Exercise connect data from Microsoft Entra ID to Microsoft Sentinel
- Export logs to third-party security information and event management system
- Analyze Microsoft Entra workbooks and reporting
- Monitor security posture with Identity Secure Score
- Summary and resources

</details>

### Choosing the correct Entra log type
Use this to map an exam scenario describing a monitoring need to the correct log type while avoiding the classic distractor of picking audit logs for sign-in troubleshooting.
```mermaid
flowchart TD
    A["Scenario: troubleshoot a user sign-in"] -->|"correct"| B["Sign-ins report"]
    A -->|"distractor"| C["Audit logs - rejected"]
    D["Scenario: track who changed a group"] -->|"correct"| E["Audit logs"]
    D -->|"distractor"| F["Sign-ins report - rejected"]
    G["Scenario: monitor Workday provisioning"] -->|"correct"| H["Provisioning logs"]
    I["Scenario: find possibly compromised users"] -->|"correct"| J["Identity Protection risk reports"]
```

### Deciding where to route Entra logs
Use this when a question asks where sign-in or audit data should be delivered, since the exam tests Sentinel versus third-party SIEM versus manual download and the deprecated AzLog distractor.
```mermaid
flowchart TD
    A["Requirement: cloud SIEM and SOAR"] -->|"correct"| B["Microsoft Sentinel"]
    A -->|"distractor"| C["AzLog tool - rejected legacy"]
    D["Requirement: Splunk QRadar or ArcSight"] -->|"correct"| E["Azure Monitor Event Hubs connectors"]
    F["Requirement: analyze data outside the portal"] -->|"correct"| G["Download CSV or JSON up to 250000 records"]
    H["Requirement: full Microsoft 365 activity view"] -->|"correct"| I["Microsoft 365 admin center"]
```

### Reporting architecture and access
Use this to recall the reporting architecture components and which roles and licenses unlock each report.
```mermaid
flowchart LR
    A["Microsoft Entra reporting"] --> B["Activity"]
    A --> C["Security"]
    B --> B1["Sign-ins"]
    B --> B2["Audit logs"]
    B --> B3["Provisioning logs"]
    C --> C1["Risky sign-ins"]
    C --> C2["Users flagged for risk"]
    B1 --> D["Security Admin, Security Reader, Global Reader, Report Reader"]
    B1 --> E["All editions plus Microsoft Graph API"]
    B1 --> F["Interactive sign-ins only"]
    B1 --> G["Non-admins see own sign-ins"]
```

### Connecting Microsoft Entra ID to Microsoft Sentinel
Use this to memorize the exact portal click path and role prerequisites for streaming Entra logs into Sentinel.
```mermaid
sequenceDiagram
    participant Admin
    participant Portal as Azure portal
    participant Sen as Microsoft Sentinel
    participant WS as Log Analytics workspace
    Admin->>Portal: Sign in as tenant administrator
    Admin->>Portal: Search and select Microsoft Sentinel
    Admin->>Sen: Select Add
    Sen->>WS: Create workspace pay-as-you-go
    Admin->>Sen: Open Data connectors
    Admin->>Sen: Select Microsoft Entra ID connector
    Admin->>Sen: Check Sign-in logs and Audit logs
    Admin->>Sen: Select Apply changes
    Note over Admin,Sen: Needs Security Administrator and Sentinel Contributor roles
```

### License and role requirements per task
Use this to lock in the license tier and role combinations that the exam frequently mixes up as distractors.
```mermaid
flowchart TD
    A["Task: open usage and insights report"] -->|"correct"| B["Entra ID P1 or P2"]
    A -->|"distractor"| C["Free edition - rejected"]
    B --> D["Security Admin, Security Reader, Report Reader"]
    E["Task: stream sign-in logs to Sentinel"] -->|"correct"| F["Entra ID P1 or P2"]
    G["Task: stream audit logs to Sentinel"] -->|"correct"| H["Any license tier"]
    I["Task: view identity secure score"] -->|"correct"| J["All editions"]
```
