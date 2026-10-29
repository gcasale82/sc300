# Module 04: Plan, design, and implement Microsoft Entra Connect - Diagrams (part 2)

<details><summary>Source lessons</summary>

- Manage Microsoft Entra Health
- Summary and resources

</details>

### RBAC role selection for Microsoft Entra Connect Health
Useful when a question asks which built-in role grants a specific level of access, including the trap that other Azure roles have no impact.

```mermaid
flowchart TD
    A["Need access to Connect Health"] -->|"manage access and assign roles"| B["Owner"]
    A -->|"view all info and change settings"| C["Contributor"]
    A -->|"view all info only"| D["Reader"]
    A -->|"assign User Access Administrator"| E["Rejected no impact on Connect Health"]
    B --> F["Default role for global administrators"]
    C --> G["Minimum role for Diagnose and Apply Fix"]
```

### Duplicated attribute sync error fixability decision
Helps you decide whether a duplicated attribute sync error can be fixed from the portal or requires manual on-premises remediation, a common scenario question.

```mermaid
flowchart TD
    A["Duplicated attribute sync error"] --> B{"Which attribute?"}
    B -->|"UserPrincipalName or ProxyAddresses"| C["Portal diagnose supported"]
    B -->|"SipProxyAddress or OnPremiseSecurityIdentifier"| D{"Which error type?"}
    D -->|"AttributeValueMustBeUnique"| C
    D -->|"QuarantinedAttributeValueMustBeUnique"| E["Diagnose not supported"]
    B -->|"any other attribute"| E
    C --> F{"Assigned role?"}
    F -->|"Contributor or Owner"| G["Run Diagnose then Apply Fix"]
    F -->|"Reader only"| H["Rejected upgrade to Contributor"]
    E --> I["Manual Fix Required on premises"]
```

### Delete server versus delete service instance
Useful for comparing the effects of deleting a server versus a service instance, including the premium license requirement and Health Agent behavior.

```mermaid
flowchart LR
    A["Stop monitoring in Connect Health"] --> B["Delete one server"]
    A --> C["Delete service instance"]
    B --> D["Stops collecting new data"]
    B --> E["Old data kept per retention policy"]
    B --> F["Agent remains reinstall to resume"]
    C --> G["Removes whole farm from monitoring"]
    C --> H["All instance data deleted per policy"]
    C --> I["Reinstall agents on all servers"]
    B --> J["Requires Entra ID premium license"]
    C --> J
```

### Sync error remediation workflow and statuses
Maps the diagnose status lifecycle so you can predict which status appears after each action in remediation questions.

```mermaid
flowchart TD
    A["Sync error appears in Connect Health"] --> B["Status Not Started"]
    B --> C["Select Diagnose and answer questions"]
    C --> D{"Orphaned object case found?"}
    D -->|"yes"| E["Select Apply Fix"]
    E --> F["Status Pending Sync"]
    F --> G["Next sync cycle clears the error"]
    D -->|"no Close button shown"| H["Status Manual Fix Required"]
    H --> I["Resolve from on-premises side"]
```

### Email notification configuration steps
Summarizes the exact portal steps and default behavior of email notifications, which is testable as an ordering or true or false item.

```mermaid
sequenceDiagram
    participant Admin
    participant P as Connect Health
    Admin->>P: Open Alerts dialog
    Admin->>P: Select Notification Settings
    Admin->>P: Set email notification switch ON
    Admin->>P: Optionally notify all global administrators
    Admin->>P: Add additional email recipients
    Admin->>P: Select Save
    Note over P: Notifications enabled by default
    Note over P: Sync error emails sent as daily digest
```
