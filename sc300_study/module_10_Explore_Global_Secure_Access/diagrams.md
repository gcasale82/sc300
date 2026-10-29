# Module 10: Explore Global Secure Access - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- Explore Global Secure Access
- Deploy and configure Microsoft Entra Internet Access
- Deploy and configure Microsoft Entra Private Access
- Explore how to use the Dashboard to drive Global Secure Access
- Create remote networks for use with Global Secure Access
- Use Conditional Access with Global Secure Access
- Explore logs and monitoring options with Global Secure Access
- Summary and resources

</details>

### Choosing Between Microsoft Entra Internet Access and Private Access
Use this decision tree to pick the correct Global Secure Access component when an exam question describes a resource type or scenario, including the classic VPN distractor.

```mermaid
flowchart TD
    A["What needs secure access?"] -->|"Microsoft SaaS and public internet"| B["Microsoft Entra Internet Access"]
    A -->|"Private corporate resources"| C["Microsoft Entra Private Access"]
    B --> D["Secure Web Gateway protection"]
    B --> E["Universal tenant restrictions"]
    B --> F["Web category filtering"]
    C --> G["Quick Access app segments"]
    C --> H["Per app Conditional Access"]
    A -->|"Connect users with legacy VPN"| X["Rejected - VPN not required"]
    A -->|"Replace existing third party SSE"| Y["Rejected - deploy side by side instead"]
```

### Conditional Access Decision Path with Global Secure Access
Use this to match a Conditional Access requirement to the correct Global Secure Access capability, and to remember the signaling prerequisite.

```mermaid
flowchart TD
    A["Security goal with Global Secure Access"] -->|"Stop stolen token replay"| B["Compliant network check"]
    A -->|"Keep source IP location policies"| C["Source IP restoration"]
    A -->|"Protect Quick Access private apps"| D["Conditional Access on Private Access apps"]
    A -->|"Block unallowed external tenants"| E["Universal tenant restrictions"]
    B --> F["Prerequisite - enable Global Secure Access signaling"]
    C --> F
    F --> G["Named location All Compliant Network locations"]
    E --> H["Configure in cross tenant access settings"]
    A -->|"Maintain list of office IP addresses"| I["Rejected - use compliant network location instead"]
```

### Global Secure Access Traffic Flow Architecture
Use this architecture view to answer questions about how client traffic and branch office traffic reach Microsoft, internet, and private resources.

```mermaid
flowchart LR
    U["Remote user with Global Secure Access client"] --> IA["Microsoft Entra Internet Access"]
    U --> PA["Microsoft Entra Private Access"]
    IA --> SWG["Secure Web Gateway"]
    SWG --> RES["Microsoft 365 SaaS and internet"]
    PA --> CON["Private network connector"]
    CON --> PRIV["Private apps and data centers"]
    IA --> WAN["Microsoft Wide Area Network"]
    PA --> WAN
    CPE["Branch office CPE router"] -->|"IPsec tunnel"| WAN
```

### Deployment Steps for Internet Access and Private Access
Use this side-by-side process map to memorize the four ordered deployment steps of each component and their key configuration details.

```mermaid
flowchart TD
    A["Deploy Global Secure Access"] --> B["Internet Access path"]
    A --> C["Private Access path"]
    B --> B1["Step 1 Enable Microsoft traffic forwarding profile"]
    B1 --> B2["Step 2 Install Global Secure Access client"]
    B2 --> B3["Step 3 Enable tenant restrictions"]
    B3 --> B4["Step 4 Enable signaling and Conditional Access"]
    B4 --> B5["Block policy excludes break glass accounts"]
    C --> C1["Step 1 Install private network connector"]
    C1 --> C2["Step 2 Configure Quick Access app"]
    C2 --> C3["Step 3 Enable Private Access traffic forwarding"]
    C3 --> C4["Step 4 Install Global Secure Access client"]
    C1 --> C5["Windows Server 2016 or later with TLS 1.2"]
    C2 --> C6["FQDN IP CIDR or IP range segments"]
```

### Remote Network Configuration Workflow
Use this sequence to remember that the final remote network step happens on the on-premises router, not in the Microsoft Entra admin center.

```mermaid
sequenceDiagram
    participant Admin as Global Secure Access Administrator
    participant Portal as Microsoft Entra admin center
    participant CPE as On premises router
    Admin->>Portal: Create remote network with name and region
    Admin->>Portal: Enter connectivity details of CPE
    Admin->>Portal: Add traffic forwarding profile
    Portal->>Admin: Review configuration shows Microsoft side settings
    Admin->>CPE: Enter connection details in router console
    CPE->>Portal: IKE negotiation establishes IPsec tunnel
```
