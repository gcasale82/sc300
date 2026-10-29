# Module 05: What is Microsoft Entra multifactor authentication - Diagrams

<details><summary>Source lessons</summary>

- Introduction
- What is Microsoft Entra multifactor authentication
- Plan your multifactor authentication deployment
- Exercise - Enable Microsoft Entra multifactor authentication
- Configure multifactor authentication methods
- Summary

</details>

### Choosing the identity protection feature for a stated requirement
Use this when the exam gives a threat scenario and you must pick between MFA and its look-alike features like Smart Lockout, password protection, SSPR and ID Protection.

```mermaid
flowchart TD
    A["Identity protection requirement"] --> B{"Attacker already has valid password"}
    B -->|"yes"| C{"Require second factor at sign-in"}
    C -->|"yes"| D["Microsoft Entra multifactor authentication"]
    C -->|"no"| E["Rejected smart lockout only stops guessing"]
    B -->|"no"| F{"Brute force sign-in attempts"}
    F -->|"yes"| G["Microsoft Entra smart lockout"]
    F -->|"no"| H{"Block commonly breached passwords"}
    H -->|"yes"| I["Microsoft Entra password protection"]
    H -->|"no"| J{"Auto respond to risky behavior"}
    J -->|"yes"| K["Microsoft Entra ID Protection"]
    J -->|"no"| L["SSPR or password rules"]
```

### Selecting the MFA authentication method
Use this to match method requirements to features, including the FIDO2 versus Windows Hello distractor and the free-tier voice call limitation.

```mermaid
flowchart TD
    A["Select MFA authentication method"] --> B{"Unphishable passwordless hardware key"}
    B -->|"yes"| C["FIDO2 security key USB Bluetooth NFC"]
    B -->|"no"| D{"Biometric or PIN tied to device"}
    D -->|"yes"| E["Windows Hello for Business"]
    D -->|"no"| F{"Must work with limited connectivity"}
    F -->|"yes"| G["Mobile app verification code 30 second OATH"]
    F -->|"no"| H{"Android device in China"}
    H -->|"yes"| I["Rejected verification code unsupported there"]
    H -->|"no"| J{"Non-browser app lacks MFA support"}
    J -->|"yes"| K["App password"]
    J -->|"no"| L{"Free or trial tenant"}
    L -->|"yes"| M["Rejected voice call unavailable"]
    L -->|"no"| N["Voice call preferred for backup"]
```

### Deciding how users register for MFA
Use this when the exam asks for the easiest or the most automated registration approach and which license each option requires.

```mermaid
flowchart TD
    A["Plan user registration for MFA"] --> B{"Identity Protection license owned"}
    B -->|"yes"| C["Easiest option prompt at next sign-in"]
    B -->|"no"| D{"Enforce registration for everyone"}
    D -->|"yes"| E["Conditional Access on all users group"]
    E --> F["Periodically remove registered users manually"]
    D -->|"no"| G["Prompt when app requires MFA"]
    B --> H["Rejected security defaults are all or nothing"]
```

### Portal path to configure MFA settings and a Conditional Access policy
Use this to memorize the exact blade navigation and the required role for the enable MFA exercise.

```mermaid
flowchart LR
    A["Sign in as Authentication Administrator"] --> B["Microsoft Entra ID blade"]
    B --> C["Security"]
    C --> D["Multifactor authentication"]
    D --> E["Additional cloud-based MFA settings"]
    E --> F["Service settings and app passwords"]
    C --> G["Conditional Access"]
    G --> H["Create new policy"]
    H --> I["Assign users and groups"]
    I --> J["Target resources cloud apps"]
    J --> K["Grant access require MFA"]
    K --> L["Enable policy On then Create"]
```

### First sign-in registration versus later MFA sign-ins
Use this to recall the user experience flow tested in scenario questions about first-time registration prompts and repeat prompts.

```mermaid
sequenceDiagram
    participant U as User
    participant E as Microsoft Entra ID
    participant A as App requiring MFA
    U->>A: First sign-in
    A->>E: MFA required
    E->>U: Prompt to register preferred method
    U->>E: Registers authentication method
    E->>A: Access granted
    U->>A: Later sign-in
    A->>E: MFA required
    E->>U: Prompt for registered factor
    U->>E: Approves notification or enters code
    E->>A: Access granted
```
