# Module 07: Plan security defaults - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Plan security defaults
- Exercise - Work with security defaults
- Plan Conditional Access policies
- Implement Conditional Access policy controls and assignments
- Exercise - Implement Conditional Access policies roles and assignments
- Test and troubleshoot Conditional Access policies
- Implement application controls
- Implement session management and continuous access evaluation
- Exercise - Configure authentication session controls
- Microsoft Entra Conditional Access Optimization agent
- Summary and resources

</details>

### Decision tree - Security defaults or Conditional Access
Use this to answer exam questions asking which baseline protection fits a given organization and license tier.

```mermaid
flowchart TD
    A["Organization needs baseline identity security"] --> B{"Microsoft Entra ID license tier"}
    B -->|"Free tier"| C["Enable security defaults"]
    B -->|"Premium P1 or P2"| D["Security defaults rejected use CA instead"]
    B -->|"Free but wants granular control"| E["Conditional Access rejected requires P1"]
    C --> F["MFA registration for all users"]
    C --> G["MFA every sign-in for admins"]
    C --> H["Block legacy authentication protocols"]
    C --> I["Protect Azure portal access"]
    C --> J["Enabled by default on new tenants"]
```

### Decision tree - License tier and Conditional Access capability
Use this to match a required capability such as risk-based policies to the correct license and reject the common wrong-tier distractor.

```mermaid
flowchart TD
    A["Conditional Access requirement"] --> B{"License tier"}
    B -->|"Free or Office 365"| C["No Conditional Access available"]
    B -->|"P1 or Microsoft 365 E3"| D["Standard Conditional Access policies"]
    B -->|"P2"| E["Standard CA plus risk-based policies"]
    A -->|"needs sign-in risk policy"| F["Risk policy on P1 rejected needs P2"]
    D --> G["MFA compliant device location controls"]
    E --> H["Sign-in risk and user risk policies"]
```

### Process - Create and roll out a Conditional Access policy
Use this to recall the exact portal path, required exclusions, and the report-only phased rollout order tested on the exam.

```mermaid
flowchart TD
    A["Sign in as Conditional Access Administrator"] --> B["Protection then Conditional Access then New policy"]
    B --> C["Assign users and groups"]
    C --> D["Exclude emergency access accounts"]
    D --> E["Target resources and set conditions"]
    E --> F["Set Grant or Session controls"]
    F --> G["Enable Report-only"]
    G --> H["Monitor sign-in logs"]
    H --> I{"Behavior as expected"}
    I -->|"No"| J["Adjust policy"]
    J --> G
    I -->|"Yes"| K["Enable On for pilot users"]
    K --> L["Expand while excluding administrators"]
    L --> M["Apply to all users after testing"]
```

### Sequence - Continuous Access Evaluation revocation flow
Use this to explain how CAE near-real-time revocation works instead of waiting for token expiry.

```mermaid
sequenceDiagram
    participant C as CAE capable client
    participant E as Microsoft Entra ID
    participant R as Resource provider
    participant A as Administrator
    C->>E: Present credentials or refresh token
    E->>C: Access token issued
    A->>E: Revoke all refresh tokens
    E->>R: Revocation event sent
    C->>R: Present access token
    R->>C: Deny access with 401 claim challenge
    C->>E: Refresh token plus claim challenge
    E->>C: Reevaluate conditions and prompt reauthentication
```

### Decision tree - Troubleshoot a Conditional Access sign-in block
Use this to order the troubleshooting tools and log tabs correctly when a scenario describes a blocked sign-in.

```mermaid
flowchart TD
    A["Sign-in blocked or policy misbehaving"] --> B["Simulate with What If tool"]
    B --> C["Read error page and More Details"]
    C --> D["Open Entra sign-in logs"]
    D --> E{"Sign-in event found"}
    E -->|"No"| F["Filter by Correlation ID user or date"]
    F --> D
    E -->|"Yes"| G["Open Conditional Access tab"]
    G --> H["Review applied policies and policy details"]
    H --> I["Check Troubleshooting and support tab"]
    I --> J{"Cause identified"}
    J -->|"No"| K["Create support request with request ID"]
    J -->|"Yes"| L["Edit policy from Policy Name link"]
```
