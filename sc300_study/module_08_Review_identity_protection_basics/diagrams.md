# Module 08: Review identity protection basics - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Review identity protection basics
- Implement and manage user risk policy
- Exercise enable sign-in risk policy
- Exercise configure Microsoft Entra multifactor authentication registration polic
- Monitor, investigate, and remediate elevated risky users
- Implement security for workload identities
- Explore Microsoft Defender for Identity
- Explore the Identity Risk Management Agent
- Summary and resources

</details>

### Identity Protection capability and license decision tree
Use this to answer classic SC-300 license-tier questions by matching the required capability to Microsoft Entra ID Premium P2 and eliminating the Free or P1 distractor.
```mermaid
flowchart TD
    A["Exam requirement"] --> B{"Capability needed?"}
    B -->|"Automate risk response"| C["Risk policies"]
    B -->|"Full risk reports and alerts"| D["Security reports and notifications"]
    B -->|"Enforce MFA registration"| E["MFA registration policy"]
    C --> F{"License tier?"}
    D --> F
    E --> F
    F -->|"Premium P2"| G["Correct - feature available"]
    F -->|"Free or P1"| H["Rejected distractor - needs P2"]
    G --> I{"Who configures policies?"}
    I -->|"Security Administrator"| J["Correct - full access"]
    I -->|"Security Operator or Reader"| K["Rejected - cannot configure policies"]
```

### Remediation decision path for a risky user
Use this to pick the correct remediation or unblocking action in scenario questions, with dismiss user risk exposed as the distractor that does not secure the account.
```mermaid
flowchart TD
    A["User flagged at risk"] --> B{"Registered for MFA and SSPR?"}
    B -->|"Yes"| C["Self-remediation with risk policy"]
    B -->|"No"| D{"Can admin reset password?"}
    D -->|"Yes"| E{"User registered for SSPR?"}
    E -->|"Yes"| F["Require user to reset password"]
    E -->|"No"| G["Generate temporary password"]
    D -->|"No - user deleted"| H["Dismiss user risk"]
    G --> I["User sets new password at next sign-in"]
    H --> J["Rejected as full remediation - password unchanged"]
```

### Choosing the correct Identity Protection report
Use this to match an investigation requirement to the risky users, risky sign-ins, or risk detections report and recall the 30 day, 90 day and download limits.
```mermaid
flowchart TD
    A["Investigation requirement"] --> B{"What must you review?"}
    B -->|"User risk level and history"| C["Risky users report"]
    B -->|"Sign-in risk and MFA details"| D["Risky sign-ins report"]
    B -->|"Detection type and location"| E["Risk detections report"]
    B -->|"Sign-in data older than 30 days"| F["Rejected - risky sign-ins limit 30 days"]
    F --> E
    C --> G["Download max 2500 entries"]
    D --> G
    E --> H["Download max 5000 records"]
```

### Configuring the three Identity Protection policies
Use this to recall the exact portal path and the recommended thresholds and access controls for the user risk, sign-in risk, and MFA registration policies.
```mermaid
flowchart TD
    A["Sign in as Global administrator"] --> B["Identity - Protection - Identity Protection"]
    B --> C{"Which policy to configure?"}
    C -->|"User risk policy"| D["User risk threshold High"]
    D --> E["Require password change"]
    C -->|"Sign-in risk policy"| F["Sign-in risk Medium and above"]
    F --> G["Require MFA"]
    C -->|"MFA registration policy"| H["Fixed control - Require MFA registration"]
    E --> I["Exclude emergency access accounts"]
    G --> I
    H --> I
    I --> J["Enforce policy On then Save"]
```

### Defender for Identity architecture and data flow
Use this to match Defender for Identity components to their functions, a common component-to-purpose matching question on the exam.
```mermaid
flowchart LR
    A["Domain controllers"] -->|"Sensor - no port mirroring"| D["Defender for Identity cloud service"]
    B["AD FS servers"] -->|"Sensor"| D
    F["Microsoft threat intelligence"] --> D
    D -->|"Alerts and attack timeline"| E["Microsoft Defender portal"]
    E --> G["SecOp analysts triage and investigate"]
```
