# Module 04: Plan, design, and implement Microsoft Entra Connect - Diagrams (part 1)

<details><summary>Source lessons</summary>

- Introduction
- Plan, design, and implement Microsoft Entra Connect
- Implement manage password hash synchronization (PHS)
- Implement manage pass-through authentication (PTA)
- Explore pass-through authentication and seamless single sign-on (SSO)
- Implement and manage federation
- Trouble-shoot synchronization errors
- Implement Microsoft Entra Connect Health

</details>

### Choose the hybrid authentication method
Use this when an exam question describes authentication requirements and you must pick between PHS, PTA and federation while avoiding the classic distractors.

```mermaid
flowchart TD
A["Choose hybrid authentication method"] --> B{"Smartcard third party MFA or sAMAccountName sign-in required"}
B -->|"yes"| G["Federation with AD FS or PingFederate"]
B -->|"no"| C{"Must enforce on-prem account state at sign-in"}
C -->|"yes"| F["Pass-through authentication PTA"]
C -->|"no"| E["Password hash synchronization PHS"]
B -.->|"distractor"| X1["PHS and PTA rejected - not supported natively"]
C -.->|"distractor"| X2["PHS rejected - account state sync is delayed"]
F --> F2["Deploy three agents for high availability"]
F --> F3["Enable PHS as manual backup method"]
G --> G2["AD FS farm plus WAP in perimeter network"]
E --> E2["Add seamless SSO for better user experience"]
E --> E3["Enable PHS always for Identity Protection"]
```

### Troubleshoot synchronization errors by symptom
Use this to map the symptom described in a troubleshooting question to the exact sync error name the exam expects.

```mermaid
flowchart TD
A["Sync export error to Microsoft Entra ID"] --> R["Diagnose with Connect Health sync report - updates every 30 minutes"]
A --> B{"Duplicate ProxyAddresses or UserPrincipalName value"}
B -->|"yes"| C{"Conflicting objects are different types"}
C -->|"yes"| C1["ObjectTypeMismatch"]
C -->|"no"| D{"Matched object already has immutableId set"}
D -->|"yes"| D1["InvalidSoftMatch"]
D -->|"no"| D2["AttributeValueMustBeUnique"]
B -->|"no"| E{"UPN suffix moved between two federated domains"}
E -->|"yes"| E1["FederatedDomainChangeError"]
E -->|"no"| F{"Attribute over limit like 15 certificates"}
F -->|"yes"| F1["LargeObject also called ExceededAllowedLength"]
F -.->|"distractor"| X1["Reject AttributeValueMustBeUnique - no duplicate here"]
F -->|"no"| G{"Cloud user holds an admin role"}
G -->|"yes"| G1["Existing Admin Role Conflict"]
G -.->|"distractor"| X2["Reject InvalidSoftMatch - role blocks the soft match"]
G -->|"no"| H["IdentityDataValidationFailed - check UPN format"]
```

### Select the sync topology or tool
Use this for questions asking which installation type, topology or sync tool fits a described on-premises environment.

```mermaid
flowchart TD
A["Plan Microsoft Entra Connect deployment"] --> B{"Single forest and single Microsoft Entra tenant"}
B -->|"yes"| C["Express installation supports this topology only"]
B -.->|"distractor"| X1["Reject express settings for multi forest - use custom"]
B -->|"no"| D{"Multiple forests one tenant"}
D -->|"yes"| E["One domain joined sync server reaches all forests"]
D -->|"no"| F{"Disconnected forests like merger or acquisition"}
F -->|"yes"| G["Use Microsoft Entra Connect cloud sync"]
F -->|"no"| H["Full mesh with optional GALSync via MIM"]
E --> I{"Need high availability for sync"}
I -->|"yes"| J["Install second server in staging mode"]
I -->|"no"| K["Single sync server is enough"]
J --> J2["Staging reads all directories but writes nothing"]
G --> G2["Lightweight cloud provisioning agents - sync every 2 minutes"]
A --> N{"Multiple Microsoft Entra tenants"}
N -->|"yes"| O["One sync server per tenant with filtering"]
O -.->|"distractor"| X2["Reject one server serving two tenants"]
```

### Seamless SSO browser sign-in flow
Use this to remember the Kerberos flow and the AZUREADSSOACC computer account, a frequent exam detail.

```mermaid
sequenceDiagram
    participant U as User on domain joined device
    participant B as Browser
    participant E as Microsoft Entra ID
    participant A as Active Directory
    U->>B: Open app like Outlook Web App
    B->>E: Redirect to Microsoft Entra sign-in page
    U->>E: Type username only
    E->>B: 401 Unauthorized challenge for Kerberos ticket
    B->>A: Request ticket for AZUREADSSOACC computer account
    A->>B: Return Kerberos ticket encrypted with account secret
    B->>E: Forward Kerberos ticket
    E->>E: Decrypt ticket with shared key
    E->>B: Return token or request more proof like MFA
    B->>U: User signed in and accesses the app
    Note over A,E: AZUREADSSOACC computer account represents Microsoft Entra ID
```

### Where password validation happens per method
Use this to compare the three sign-in methods and recall where validation occurs and which components are required on-premises.

```mermaid
flowchart LR
subgraph PHS["Password hash synchronization"]
A1["On-premises AD"] -->|"hash of hash every 2 minutes"| A2["Microsoft Entra ID"]
A2 --> A3["Password validated in the cloud"]
end
subgraph PTA["Pass-through authentication"]
B3["Microsoft Entra ID"] -->|"sign-in request"| B1["PTA agents - recommend three"]
B1 -->|"validate password"| B2["Domain controllers"]
B1 -.->|"manual failover via Entra Connect"| A2
end
subgraph FED["Federation"]
C1["Microsoft Entra ID"] -->|"hands off authentication"| C2["Web Application Proxy in perimeter network"]
C2 --> C3["AD FS farm in internal network"]
C3 --> C4["Smartcards or third party MFA supported"]
end
```
