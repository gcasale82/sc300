# Module 04: Plan, design, and implement Microsoft Entra Connect - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Plan, design, and implement Microsoft Entra Connect
- Implement manage password hash synchronization (PHS)
- Implement manage pass-through authentication (PTA)
- Explore pass-through authentication and seamless single sign-on (SSO)
- Implement and manage federation
- Trouble-shoot synchronization errors
- Implement Microsoft Entra Connect Health
- Manage Microsoft Entra Health
- Summary and resources

</details>

### Authentication method decision path
Use this to quickly answer SC-300 questions that give a requirement and ask which sign-in method to choose, including the common distractors.
```mermaid
flowchart TD
A["Requirement: choose sign-in method"] --> B["Must enforce on-prem password policy at sign-in time"]
A --> C{"Any unsupported auth need?"}
A --> D["Simplest option with least infrastructure"]
B --> E["Pass-through authentication"]
C -->|"smartcards third-party MFA"| F["Federation with AD FS"]
C -->|"no special needs"| D
D --> G["Password hash synchronization"]
E --> H["Rejected: PHS does not enforce account state immediately"]
F --> I["Rejected: PHS and PTA cannot meet the requirement"]
G --> J["Rejected: PTA needs agents on servers"]
D --> K["Tip: enable PHS as backup for any method"]
```

### Password hash synchronization flow
Use this to recall how the hash of the hash travels and that the sync runs every two minutes.
```mermaid
flowchart LR
A["User password in on-prem AD"] --> B["AD stores password as one-way hash"]
B --> C["Entra Connect extracts password hash"]
C --> D["Extra security processing applied"]
D --> E["Synced to Entra ID every 2 minutes"]
E --> F["Cloud sign-in with same password"]
G["FIPS locked down server"] --> H["Enable MD5 in miiserver config"]
```

### Seamless SSO browser sign-in sequence
Use this to answer questions about the Kerberos flow and the AZUREADSSOACC computer account.
```mermaid
sequenceDiagram
participant U as User on domain-joined device
participant B as Browser
participant E as Microsoft Entra ID
participant A as Active Directory
U->>B: Open cloud app like Outlook Web
B->>E: Redirect to sign-in page
U->>E: Types username only
E-->>B: 401 challenge for Kerberos ticket
B->>A: Request ticket for AZUREADSSOACC account
A-->>B: Kerberos ticket encrypted with account secret
B->>E: Forwards Kerberos ticket
E->>E: Decrypts ticket with shared key
E-->>U: Token or extra proof like MFA
```

### Sync error troubleshooting decision tree
Use this to map a described error scenario to the correct error name and fix, while avoiding the distractor errors.
```mermaid
flowchart TD
A["Export error to Entra ID reported"] --> B{"Same attribute value reused?"}
B -->|"different object types like user vs group"| C["ObjectTypeMismatch"]
B -->|"same type duplicate ProxyAddresses or UPN"| D{"immutableId set on matching object?"}
D -->|"yes and hard match failed"| E["InvalidSoftMatch"]
D -->|"no immutableId conflict"| F["AttributeValueMustBeUnique"]
A --> G{"Attribute too large or too many?"}
G -->|"more than 15 certificates"| H["LargeObject"]
A --> I{"UPN moved between two federated domains?"}
I -->|"yes"| J["FederatedDomainChangeError"]
A --> K{"Invalid characters or format in UPN?"}
K -->|"yes"| L["IdentityDataValidationFailed"]
E --> M["Fix: remove duplicate value in source directory"]
F --> M
C --> M
H --> N["Fix: reduce attribute size or count"]
J --> O["Fix: avoid changing UPN between federated domains"]
```

### Entra Connect sync architecture and topologies
Use this to remember the staging areas, the single metaverse, and which topologies are supported versus the distractor options.
```mermaid
flowchart LR
A["Connected directory AD forest"] -->|"Import"| B["Connector Space CS"]
B -->|"Sync rules evaluate"| C["Metaverse MV only one exists"]
C -->|"Sync rules and run profiles"| D["Entra Connector Space"]
D -->|"Export"| E["Microsoft Entra ID tenant"]
F["Staging server second server"] -->|"Reads data writes nothing"| A
G["Single forest single tenant"] --> H["Express installation supported"]
I["Multiple forests one tenant"] --> J["One sync server reaches all forests"]
K["Two tenants"] --> L["Rejected: one sync server per tenant 1 to 1"]
```

### Connect Health access and diagnose decision path
Use this for questions about required licenses, RBAC roles, and the duplicated attribute self-service fix workflow.
```mermaid
flowchart TD
A["Task in Entra Connect Health"] --> B{"License present?"}
B -->|"Entra ID Premium P1 or P2"| C["Proceed"]
B -->|"Free tier"| D["Rejected: Health requires Premium"]
C --> E{"What access is needed?"}
E -->|"Manage access and settings"| F["Owner role default for global admins"]
E -->|"View and change settings"| G["Contributor role needed for Diagnose and Apply Fix"]
E -->|"View only"| H["Reader role"]
C --> I{"Duplicated attribute sync error?"}
I -->|"fixable scenario found"| J["Diagnose then Apply Fix then Pending Sync"]
I -->|"no portal fix available"| K["Manual fix required in on-prem AD"]
```
