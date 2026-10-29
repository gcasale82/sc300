# Module 03: Describe guest access and Business to Business accounts - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Describe guest access and Business to Business accounts
- Manage external collaboration
- Exercise - configure external collaboration
- Invite external users - individually and in bulk
- Exercise - add guest users to directory
- Exercise - invite guest users bulk
- Demo - manage guest users in Microsoft Entra ID
- Manage external user accounts in Microsoft Entra ID
- Manage external users in Microsoft 365 workloads
- Exercise - explore dynamic groups
- Implement and manage Microsoft Entra Verified ID
- Configure identity providers
- Implement cross-tenant access controls
- Summary and resources

</details>

### Diagram 1: Guest sign-in method decision - invitation redemption flow
Useful for exam questions asking which identity provider or authentication method a guest user will use.
```mermaid
flowchart TD
A["Guest user redeems invitation"] --> B{"User exists in managed Entra tenant?"}
B -->|"yes"| C["Sign in with Entra account"]
B -->|"no"| D{"SAML or WS-Fed IdP federation enabled for domain?"}
D -->|"yes"| E["Redirect to partner SAML WS-Fed IdP"]
D -->|"no"| F{"Domain is gmail.com or googlemail.com?"}
F -->|"yes"| G["Redirect to Google"]
F -->|"no"| H{"Existing personal MSA?"}
H -->|"yes"| I["Sign in with MSA"]
H -->|"no"| J{"Email one-time passcode enabled?"}
J -->|"yes"| K["Enter passcode from invited email"]
J -->|"no"| L["Create consumer MSA with invited email"]
```

### Diagram 2: Choosing the correct external identity feature
Exam-style decision tree mapping a requirement to the correct feature, with B2B collaboration as the common distractor for direct connect scenarios.
```mermaid
flowchart TD
A["Requirement for external access"] --> B{"User invited to tenant as guest user object?"}
B -->|"yes"| C{"Needs access to apps and resources in your tenant?"}
C -->|"yes"| D["B2B collaboration - invitation and redemption"]
C -->|"no"| E["Consider self-service sign-up user flow"]
B -->|"no"| F{"Seamless access to Teams shared channels?"}
F -->|"yes"| G["B2B direct connect - mutual trust required"]
F -->|"no"| H{"Sign up via app using Facebook or Google?"}
H -->|"Facebook"| I["Self-service sign-up only - cannot redeem invitations"]
H -->|"Gmail"| J["Google federation for B2B guests"]
D -.->|"Wrong for shared channels"| G
G -.->|"Wrong for app access"| D
```

### Diagram 3: Cross-tenant access settings architecture
Helps distinguish inbound, outbound, trust settings, and B2B direct connect defaults on cross-tenant access questions.
```mermaid
flowchart LR
A["Your organization"] --> B["Outbound access settings"]
B --> C["External Entra organization resources"]
D["External Entra organization users"] --> E["Inbound access settings"]
E --> A
F["Trust settings inbound"] --> G["Trust external MFA compliant device hybrid joined"]
H["B2B direct connect"] --> I["Mutual trust - Teams shared channels only"]
J["Organizational settings"] --> K["Per tenant specific config"]
L["Microsoft cloud settings"] --> M["Azure Government or Azure China"]
N["Defaults"] --> O["B2B collaboration enabled"]
N --> P["B2B direct connect blocked"]
```

### Diagram 4: Bulk invite guest users process
Shows the exact portal path and CSV rules frequently tested for bulk invitation questions.
```mermaid
flowchart TD
A["Sign in to Entra admin center as User administrator"] --> B["Identity - Users - All users"]
B --> C["Bulk operations - Bulk invite"]
C --> D["Download CSV template"]
D --> E["Keep first two rows version and headings"]
E --> F["Remove examples row and add guest users"]
F --> G["Upload CSV file"]
G --> H{"Validation passes?"}
H -->|"no"| I["Fix errors and reupload"]
I --> G
H -->|"yes"| J["Submit bulk operation"]
J --> K["Check bulk operation results success and failures"]
```

### Diagram 5: Guest invitation permission decision
Decision tree for selecting the correct external collaboration setting or role when restricting who can invite guests.
```mermaid
flowchart TD
A["Who should invite guest users?"] --> B{"Everyone including guests?"}
B -->|"yes"| C["All users including guests can invite - default"]
B -->|"no"| D{"Members plus admins plus Guest Inviter?"}
D -->|"yes"| E["Admins and Guest Inviter role and members"]
D -->|"no"| F{"Only admins and Guest Inviter role?"}
F -->|"yes"| G["Only users assigned specific admin roles"]
F -->|"no"| H["No one in organization can invite guests"]
G --> I["Guest Inviter role users can invite"]
E --> I
```
