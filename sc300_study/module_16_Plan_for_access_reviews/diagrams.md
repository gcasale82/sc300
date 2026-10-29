# Module 16: Plan for access reviews - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Plan for access reviews
- Create access reviews for groups and apps
- Create and configure access reviews programmatically
- Monitor access review findings
- Automate access review management tasks
- Configure recurring access reviews
- Explore the Access Review Agent in Microsoft Entra
- Summary and resources

</details>

### Decision tree: Admin roles required per resource type
Use this to instantly match the resource being reviewed to the roles that can create the review and to reject the Security Administrator distractor.
```mermaid
flowchart TD
A["Need to create an access review"] --> B{"Resource type to review"}
B -->|"Group or application"| C["Identity Governance Administrator"]
C --> C1["Also Global Admin User Admin Group Owner"]
C --> C2["Privileged Role Admin only for Entra assignable groups"]
C --> C3["Readers add Global Reader and Security Reader"]
B -->|"Microsoft Entra role"| D["Global Administrator or Privileged Role Administrator"]
B -->|"Azure resource role"| E["Global Administrator or User Access Administrator"]
B -->|"Access package"| F["Identity Governance Administrator"]
F --> F1["Also Global Admin and User Administrator"]
A -->|"Common distractor"| X["Security Administrator as creator - rejected"]
```

### Decision tree: Licensing for access reviews
Use this to resolve licensing questions, since only reviewers and self-reviewers need Microsoft Entra ID P2 while admins who configure reviews do not.
```mermaid
flowchart TD
A["Who needs an access review license"] --> B{"What does the user do"}
B -->|"Assigned as reviewer"| C["Microsoft Entra ID P2 license required"]
B -->|"Performs self-review"| D["Microsoft Entra ID P2 license required"]
B -->|"Group owner performing review"| E["Microsoft Entra ID P2 license required"]
B -->|"Application owner performing review"| F["Microsoft Entra ID P2 license required"]
B -->|"Only sets up configures or applies decisions"| G["No license required"]
G --> G1["Global Administrator or User Administrator"]
A -->|"Common distractor"| X["Admins configuring reviews need licenses - rejected"]
```

### Decision tree: Choosing what to review
Use this to pick the correct review target from an exam requirement and to avoid the classic trap that access reviews can change on-premises synced groups.
```mermaid
flowchart TD
A["Choose what to review"] --> B{"Access scenario"}
B -->|"Users have direct app access"| C["Review the application"]
B -->|"Access assigned through groups"| D["Review group membership"]
B -->|"Bundle of groups apps and sites"| E["Review the access package"]
B -->|"Privileged role assignments"| F["PIM review of Entra or Azure roles"]
B -->|"Only external guests need cleanup"| G["Scope Guest users only"]
B -->|"Group synced from on-premises"| H["Review only - changes stay on-premises"]
A -->|"Common distractor"| X["Access review edits synced group membership - rejected"]
```

### Process: Creating an access review in the portal
Use this to memorize the exact portal path, creation steps, and the upon-completion settings that are heavily tested.
```mermaid
flowchart TD
A["Sign in as Identity Governance Administrator"] --> B["ID Governance then Access reviews"]
B --> C["Select New access review"]
C --> D["Select what to review"]
D --> E["Choose scope Guest users only or Everyone"]
E --> F["Select reviewers and fallback reviewer"]
F --> G["Set frequency duration and dates"]
G --> H["Configure upon completion settings"]
H --> I{"Auto apply results to resource"}
I -->|"Enable"| J["Denied users removed automatically"]
I -->|"Disable"| K["Apply results manually"]
H --> L["If reviewers do not respond"]
L --> L1["No change Remove Approve or Take recommendations"]
H --> M["Denied guests - remove membership or block 30 days"]
M --> M1["Blocked users deleted if no action in 30 days"]
H --> N["Enable decision helpers reminders justifications"]
N --> O["Name review then Create then Start"]
```

### Sequence: Reviewer completes an access review
Use this to remember the My Access portal reviewer flow, the two recommendation types, and the last-decision-wins rule.
```mermaid
sequenceDiagram
participant R as Reviewer
participant P as My Access portal
participant E as Microsoft Entra ID
E->>R: Review start email with Start review link
R->>P: Or browse to myaccess.microsoft.com
R->>P: Select Access reviews in left menu
P->>R: Show pending access reviews
R->>P: Open the assigned review
R->>P: Approve or deny users individually
R->>P: Or select Accept recommendations
P->>E: Save decisions
Note over R,P: Don't know keeps access and is audited
Note over P: Recommendations use 30 day no sign-in and peer outlier
Note over E: Last submitted reviewer decision wins
E->>P: When review ends apply results
P->>R: Denied users removed if auto apply enabled
```
