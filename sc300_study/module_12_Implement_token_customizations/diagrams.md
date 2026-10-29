# Module 12: Implement token customizations - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Implement token customizations
- Implement and configure consent settings
- Integrate on-premises apps with Microsoft Entra application proxy
- Integrate custom SaaS apps for single sign-on
- Implement application-based user provisioning
- Monitor and audit access to Microsoft Entra integrated enterprise applications
- Create and manage application collections
- Summary and resources

</details>

### Choosing remote access for on-premises apps
Use this to answer exam questions asking which solution provides secure remote access to on-premises web apps and to eliminate the VPN and reverse proxy distractors.
```mermaid
flowchart TD
    A["Requirement - remote users need on-premises web apps"] --> B{"Who needs access?"}
    B -->|"remote users"| C{"Choose the access solution"}
    C -->|"correct answer"| D["Microsoft Entra application proxy"]
    C -->|"common distractor"| E["VPN or reverse proxy - rejected"]
    D --> F["Supports IWA, form-based and header-based apps"]
    D --> G["Supports Remote Desktop Gateway and MSAL clients"]
    B -->|"internal users on corporate network"| H["Rejected - not intended for internal users"]
```

### Selecting the correct SSO protocol
Use this when a question describes an app scenario and you must pick between OAuth 2.0, OpenID Connect and SAML without mixing up authentication and authorization.
```mermaid
flowchart TD
    A{"What must the protocol do?"} -->|"authorize access to resources"| B["OAuth 2.0"]
    A -->|"authenticate - purely cloud apps"| C["OpenID Connect"]
    A -->|"authenticate - enterprise ADFS federated apps"| D["SAML"]
    A -->|"distractor - OAuth for sign-in"| E["Rejected - OAuth is authorization only"]
    C --> F["Built on top of OAuth 2.0"]
    C --> G["Used for mobile apps, web sites, web APIs"]
    D --> H["Frequently used in enterprise applications"]
```

### Application Proxy sign-in flow
Use this to reconstruct the exact order of the Application Proxy sign-in sequence, a common process-order question on the exam.
```mermaid
sequenceDiagram
    participant U as User
    participant E as Microsoft Entra ID
    participant S as Application Proxy service
    participant C as Application Proxy connector
    participant A as On-premises app
    U->>E: Access application through endpoint
    E->>U: Direct to Microsoft Entra sign-in page
    U->>E: Successful sign-in
    E->>U: Send token to client device
    U->>S: Send token to Application Proxy service
    S->>S: Extract UPN and SPN from token
    S->>C: Send request to connector
    C->>C: Perform additional single sign-on authentication
    C->>A: Send request to application
    A->>C: Return response
    C->>S: Return response
    S->>U: Deliver response to user
```

### Choosing user consent settings and risky consent handling
Use this to match a consent requirement to the correct setting and to recognize the AADSTS90094 error and the related audit event that appear in exam questions.
```mermaid
flowchart TD
    A{"Choose the user consent setting"} -->|"no user consent allowed"| B["Disable user consent"]
    A -->|"only verified publishers and low impact"| C["Verified publisher and low impact option"]
    A -->|"any permission not requiring admin"| D["Users can consent to all apps"]
    A -->|"need more granular conditions"| E["Custom app consent policy"]
    C --> F["Requires classifying permissions as low impact"]
    B --> G["Previously consented apps keep working"]
    H["Microsoft detects risky consent request"] -->|"step-up consent"| I["Admin approval required"]
    I -->|"consent request workflow enabled"| J["User sends request to admin from prompt"]
    I -->|"workflow not enabled"| K["Error AADSTS90094 shown"]
    K --> L["Audit event ApplicationManagement - Risky application detected"]
```

### Choosing a user provisioning method
Use this to decide between manual provisioning, automatic gallery provisioning and a custom SCIM endpoint, and to reject the proprietary API distractor.
```mermaid
flowchart TD
    A{"How should app users be provisioned?"} -->|"gallery app shows provisioning icon"| B["Automatic provisioning connector"]
    A -->|"gallery app without connector"| C["Manual provisioning - create accounts yourself"]
    A -->|"custom app with SCIM 2.0 REST API"| D["Build SCIM endpoint for provisioning"]
    A -->|"distractor - custom proprietary API per app"| E["Rejected - SCIM replaces proprietary APIs"]
    B --> F["Automates create, update, deprovision and groups"]
    D --> G["Uses Users and Groups endpoints with REST verbs"]
    D --> H["Combine with SAML or OIDC for end-to-end solution"]
```
