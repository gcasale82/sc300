# Module 06: Administer FIDO2 and passwordless authentication methods - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Administer FIDO2 and passwordless authentication methods
- Explore Authenticator app and OATH tokens
- Implement an authentication solution based on Windows Hello for Business
- Exercise configure and deploy self-service password reset
- Deploy and manage password protection
- Configure smart lockout thresholds
- Exercise - Manage Microsoft Entra smart lockout values
- Implement Kerberos and certificate-based authentication in Microsoft Entra ID
- Configure Microsoft Entra user authentication for virtual machines
- Summary and resources

</details>

### Decision tree: Choosing the right authentication method
Use this to map exam requirements like unphishable or passwordless to the correct method while spotting the classic SMS and password-only distractors.

```mermaid
flowchart TD
    A["Select authentication method for scenario"] --> B{"Unphishable hardware passwordless sign in?"}
    B -->|"yes"| C["FIDO2 security key"]
    B -->|"no"| D{"Passwordless phone sign in with push?"}
    D -->|"yes"| E["Microsoft Authenticator app"]
    D -->|"no"| F{"Windows sign in with PIN or biometric?"}
    F -->|"yes"| G["Windows Hello for Business"]
    F -->|"no"| H{"Only for SSPR verification?"}
    H -->|"yes"| I["Security questions or email"]
    H -->|"no"| J{"Legacy app without modern auth?"}
    J -->|"yes"| K["App passwords with per user MFA"]
    J -->|"no"| L{"SMS as strong factor?"}
    L -->|"yes"| M["Rejected SMS is medium security"]
    L -->|"no"| N["Password alone rejected low security"]
```

### Decision tree: SSPR licensing requirements
Use this to recall the exact license tiers that unlock SSPR for cloud-only versus on-premises accounts, a frequent licensing trap on the exam.

```mermaid
flowchart TD
    A["Enable self service password reset"] --> B{"Where are user accounts?"}
    B -->|"cloud only"| C{"Which license?"}
    B -->|"synced from on premises AD"| D{"Which license?"}
    C -->|"Entra P1 or P2"| E["SSPR supported"]
    C -->|"Microsoft 365 Business Standard"| E
    C -->|"Free"| F["Rejected upgrade license needed"]
    D -->|"Entra P1 or P2"| E
    D -->|"Microsoft 365 Business Premium"| E
    D -->|"Business Standard only"| F
```

### Sequence diagram: Kerberos Constrained Delegation via Application Proxy
Use this to memorize the exact order of the KCD flow, which is tested in Application Proxy single sign-on questions.

```mermaid
sequenceDiagram
    participant U as User
    participant P as Application Proxy
    participant E as Microsoft Entra ID
    participant C as Connector
    participant A as Active Directory
    participant S as App server
    U->>P: Request application URL
    P->>E: Redirect for preauthentication
    E->>U: Token issued after MFA policies
    U->>P: Present token
    P->>C: UPN and SPN over secure channel
    C->>A: KCD negotiation impersonating user
    A->>C: Kerberos token for application
    C->>S: Original request with Kerberos token
    S->>C: Application response
    C->>P: Response
    P->>U: Response delivered
```

### Architecture: Microsoft Entra Password Protection on-premises components
Use this to remember that the proxy service is the only outbound internet path, that policies cache in sysvol, and how audit versus enforce mode behaves.

```mermaid
flowchart LR
    A["Writable DC with DC agent"] -->|"requests password policy"| B["Proxy service on member server"]
    B -->|"only outbound internet path"| C["Microsoft Entra ID"]
    C -->|"global and custom banned lists"| B
    B -->|"policy response"| A
    A -->|"stores policy in sysvol"| D["Cached policy refreshed hourly"]
    D -->|"password change or set event"| E{"Deployment mode"}
    E -->|"audit"| F["Logged only not blocked"]
    E -->|"enforce"| G["Weak password blocked"]
    H["RODC"] -->|"forwards changes no agent"| A
```

### Decision tree: Smart lockout configuration for hybrid deployments
Use this to recall the threshold and duration rules for pass-through authentication scenarios and the P1 licensing requirement for customization.

```mermaid
flowchart TD
    A["Smart lockout configuration needed"] --> B{"Premium P1 or higher licensed?"}
    B -->|"no"| C["Defaults only 10 attempts 1 minute lock"]
    B -->|"yes"| D{"Pass through authentication hybrid?"}
    D -->|"no"| E["Customize in Password protection settings"]
    D -->|"yes"| F["Set Entra threshold below AD DS threshold"]
    F --> G{"AD DS threshold 2 to 3 times larger?"}
    G -->|"yes"| H["Correct attacks filtered before AD DS"]
    G -->|"no"| I["Rejected risk of locking on premises users"]
    F --> J{"Entra duration longer than AD DS?"}
    J -->|"yes"| K["Correct seconds versus minutes"]
    J -->|"no"| L["Rejected align durations properly"]
```
