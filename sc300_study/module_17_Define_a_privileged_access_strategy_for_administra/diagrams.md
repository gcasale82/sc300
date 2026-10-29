# Module 17: Define a privileged access strategy for administrative users - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Define a privileged access strategy for administrative users
- Configure Privileged Identity Management for Azure resources
- Exercise configure Privileged Identity Management for Microsoft Entra roles
- Exercise assign Microsoft Entra roles in Privileged Identity Management
- Exercise assign Azure resource roles in Privileged Identity Management
- Plan and configure Privileged Access Groups
- Analyze Privileged Identity Management audit history and reports
- Create and manage emergency access accounts
- Summary and resources

</details>

### Diagram 1: Eligible vs Active vs Permanent Assignment Decision Tree
Use this when the exam asks which assignment type fits a user who needs a role rarely, daily, or for emergencies, and to eliminate the standing-access distractor.
```mermaid
flowchart TD
    A["User needs privileged role"] --> B{"How often is access needed?"}
    B -->|"once or rarely"| C["Eligible assignment in PIM"]
    B -->|"day-to-day job"| D{"Can elevation be enforced?"}
    D -->|"yes"| C
    D -->|"distant team or executive"| E["Consider permanent active role"]
    B -->|"break glass account"| F["Permanent Global Administrator"]
    A --> G["Distractor: permanent assignment for rare use"]
    G -->|"rejected"| H["Zero standing admins except break glass"]
```

### Diagram 2: Which Roles to Protect with PIM Decision Tree
Use this to select the roles Microsoft recommends onboarding to PIM first and to reject the distractor that reader roles never need protection.
```mermaid
flowchart TD
    A["Select roles to protect with PIM"] --> B{"Microsoft Entra or Azure?"}
    B -->|"Microsoft Entra roles"| C{"Which role?"}
    C -->|"Global Administrator"| D["Protect first with PIM"]
    C -->|"Security Administrator"| D
    C -->|"has guest users assigned"| E["Protect with PIM"]
    C -->|"reader role"| F{"Reads sensitive data?"}
    F -->|"yes"| E
    F -->|"no"| G["Protection optional"]
    B -->|"Azure roles"| H{"Subscription criticality?"}
    H -->|"critical"| I["Protect all roles with PIM"]
    H -->|"less critical"| J["Protect Owner and User Access Administrator"]
    A --> K["Distractor: protect only write roles"]
    K -->|"rejected"| L["Reader roles can expose sensitive data"]
```

### Diagram 3: Onboarding Azure Resources into PIM Process
Use this to recall the exact portal path and steps for discovering and managing Azure resources in PIM.
```mermaid
flowchart TD
    A["Sign in to Microsoft Entra admin center"] --> B["Open Privileged Identity Management"]
    B --> C["Select Azure resources"]
    C --> D{"First time with PIM for Azure?"}
    D -->|"yes"| E["Discover resources page shown"]
    D -->|"no"| F["List of managed resources shown"]
    E --> G["Select Discover resources"]
    F --> G
    G --> H["Filter resources with write permission"]
    H --> I["Select management group or subscription"]
    I --> J["Select Manage resource"]
    J --> K["Confirm onboarding"]
    K --> L["Child resources can be managed too"]
```

### Diagram 4: Activating an Eligible Role in PIM
Use this to remember the activation flow including mandatory MFA, justification, single-approver completion, and activation notifications.
```mermaid
sequenceDiagram
    participant U as Eligible admin
    participant P as PIM
    participant A as Approver
    participant N as Administrators
    U->>P: Open My roles
    U->>P: Select Activate on eligible role
    P->>U: Require MFA additional verification
    U->>P: Provide verification and justification
    alt Approval required
        P->>A: Send activation request
        A->>P: One approval completes the request
    end
    P->>U: Role active for activation duration
    P->>N: Notify privileged administrators by email
    Note over A: Approvers cannot approve themselves
```

### Diagram 5: Emergency Access Accounts Scenarios and Requirements
Use this to answer break-glass account questions about when they are needed, how they must be configured, and how often to validate them.
```mermaid
flowchart LR
    A["Break glass scenarios"] --> B["Federation or identity provider outage"]
    A --> C["MFA methods unavailable"]
    A --> D["Last Global Administrator lost"]
    A --> E["Natural disaster"]
    F["Account requirements"] --> G["Two or more cloud-only accounts"]
    F --> H["onmicrosoft.com not federated or synced"]
    F --> I["Permanent Global Administrator"]
    F --> J["One excluded from Conditional Access"]
    F --> K["One excluded from phone MFA"]
    F --> L["Distinct authentication mechanism"]
    F --> M["Monitor sign-ins and audit logs"]
    F --> N["Validate every 90 days"]
```
