# Module 15: Define access packages - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Define access packages
- Exercise create and manage a resource catalog with Microsoft Entra entitlement m
- Configure entitlement management
- Exercise add terms of use acceptance report
- Exercise manage the lifecycle of external users with Microsoft Entra identity go
- Configure and manage connected organizations
- Review per-user entitlements
- Summary and resources

</details>

### Entitlement management object hierarchy
Use this to quickly recall how catalogs, access packages, policies, resources and assignments nest, a frequent terminology question on the exam.

```mermaid
flowchart LR
    A["Catalog"] -->|"contains"| B["Access package"]
    A -->|"holds"| C["Resources"]
    C -->|"types"| D["Groups apps SharePoint sites"]
    B -->|"bundles"| E["Resources with roles"]
    B -->|"governed by"| F["Policy"]
    F -->|"defines"| G["Eligible requestors"]
    F -->|"defines"| H["Approvers"]
    F -->|"defines"| I["Expiry duration"]
    F -->|"targets"| J["Connected organization"]
    F -->|"approved request yields"| K["Assignment"]
    L["Catalog creator"] -->|"creates and owns"| A
```

### Decision tree choosing the correct entitlement feature
Use this to map an exam requirement to the correct feature while avoiding the classic dynamic group and manual B2B distractors.

```mermaid
flowchart TD
    A["Access requirement"] -->|"Time limited task access"| B["Access package"]
    A -->|"Baseline mailbox for all staff"| C["Rejected dynamic group and licensing"]
    A -->|"Manager approval required"| D["Access package with policy"]
    A -->|"Departments self manage resources"| E["Catalog with delegated owners"]
    A -->|"Partner organization users"| F["Connected organization in policy"]
    A -->|"One off guest invitation"| G["Rejected manual B2B invite"]
```

### Access package request and approval workflow
Use this to trace the end to end request, approval, assignment and expiry flow that scenario questions describe.

```mermaid
sequenceDiagram
    participant U as Requestor
    participant P as Access package policy
    participant V as Approver
    participant E as Entitlement management
    U->>P: Submits access request
    P->>V: Routes request for approval
    V->>P: Approves or denies
    P->>E: Approved request creates assignment
    E->>U: All resource roles provisioned
    Note over U,E: Assignment is time limited and expires if not renewed
    Note over E: Approved external requestor is auto invited to the directory
```

### Decision tree external user lifecycle after last assignment
Use this to answer lifecycle questions about what happens when an external user loses their last access package assignment, including the default 30 day removal.

```mermaid
flowchart TD
    A["External user loses last assignment"] --> B["Block sign in setting"]
    B -->|"Yes default"| C["Sign in blocked cannot request again"]
    B -->|"No"| D["Can still request other packages"]
    C --> E["Remove external user setting"]
    E -->|"Yes default 30 days"| F["Guest account removed after 30 days"]
    E -->|"Yes set to 0 days"| G["Guest account removed immediately"]
    E -->|"No"| H["Guest account remains"]
    I["Guest existed before assignments"] -->|"Never auto removed"| H
```

### Decision tree terms of use configuration and edits
Use this to distinguish which terms of use settings can be edited versus which force creating a new terms of use.

```mermaid
flowchart TD
    A["Terms of use task"] -->|"Rename or add language"| B["Edit terms is allowed"]
    A -->|"Enforce at sign in"| C["Conditional Access grant control"]
    A -->|"Upload revised PDF"| D["Update version in language options"]
    D -->|"Force reacceptance"| E["Require reaccept toggle on"]
    D -->|"No forced reacceptance"| F["Only new or expired users see version"]
    A -->|"Change expiry or Conditional Access policy"| G["Rejected create new terms of use"]
    A -->|"Change consent every device"| G
    H["Expire consents on"] -->|"Same schedule for all"| I["Expire starting on plus frequency"]
    H -->|"Per user schedule"| J["Duration before reacceptance in days"]
```
