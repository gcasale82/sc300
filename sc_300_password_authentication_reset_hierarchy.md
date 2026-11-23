# SC-300 Exam Prep: Password & Authentication Reset Hierarchy

## 1. Visual Mind Map

```text
ENTRA ID PASSWORD & AUTHENTICATION RESET HIERARCHY
│
├── 🎯 TARGET: WHO NEEDS THE RESET?
│   │
│   ├── 🟢 TIER 1: STANDARD USERS & "BASIC" NON-PRIVILEGED ROLES
│   │   ├── Standard Users (No roles)
│   │   └── Basic Non-Privileged Roles (THE ONLY EXCEPTIONS):
│   │       ├── Directory Readers
│   │       ├── Guest Inviter
│   │       ├── Message Center Reader
│   │       └── Reports Reader
│   │
│   └── 🔴 TIER 2: PRIVILEGED & SENSITIVE ROLES (PROTECTED)
│       ├── Global Reader  <-- ⚠️ #1 EXAM TRAP! (Read-only, yet PROTECTED)
│       ├── Security Reader / Security Admin
│       ├── User Admin / Groups Admin / Application Admin
│       ├── Exchange / SharePoint / Intune / Conditional Access Admin
│       └── Global Admin / Privileged Role Admin
│
└── 🛠️ ACTOR: WHO CAN PERFORM THE RESET? (Least Privilege Order)
    │
    ├── 1. Helpdesk Administrator
    │   ├── Action: Password Reset ONLY (No MFA)
    │   └── Targets: Standard Users + Other Helpdesk Admins
    │
    ├── 2. Password Administrator
    │   ├── Action: Password Reset ONLY (No MFA)
    │   └── Targets: Standard Users + Other Password Admins + [TIER 1 Basic Roles]
    │
    ├── 3. User Administrator
    │   ├── Action: Password Reset ONLY (No MFA)
    │   └── Targets: Standard Users + User/Helpdesk Admins + [TIER 1 Basic Roles]
    │
    ├── 4. Authentication Administrator
    │   ├── Action: Password Reset + MFA / FIDO2 / Phone Methods
    │   └── Targets: Standard Users + Helpdesk/Password Admins + [TIER 1 Basic Roles]
    │
    └── 5. Privileged Authentication Administrator (and Global Admin)
        ├── Action: Password Reset + MFA / FIDO2 / Phone Methods
        └── Targets: EVERYONE (All TIER 1 + All TIER 2, including Global Reader & Global Admin)
```

---

## 2. Quick Decision Matrix (SC-300 Exam Cheat Sheet)

| Target User's Role | Reset **Password ONLY** (Least Privilege) | Reset **MFA / Authenticator / FIDO2** (Least Privilege) |
| :--- | :--- | :--- |
| **No Admin Role** (Regular User) | **Helpdesk Administrator** *(or Password Admin)* | **Authentication Administrator** |
| **Directory Readers / Guest Inviter** | **Password Administrator** | **Authentication Administrator** |
| **Global Reader** *(Exam Trap!)* | **Privileged Authentication Administrator** | **Privileged Authentication Administrator** |
| **Any Admin Role** *(User, Security, App, Global)* | **Privileged Authentication Administrator** | **Privileged Authentication Administrator** |

---

## 3. Three Golden Rules to Spot the Right Answer in 5 Seconds

1. **The "Global Reader" Trap:** Even though `Global Reader` cannot modify anything, Microsoft classifies it as a **privileged role** because it can read all tenant configurations. Helpdesk, Password, User, and Authentication Admins **CANNOT** reset its password. Always pick **Privileged Authentication Administrator** (or *Global Administrator*).
2. **Password vs. MFA Split:** 
   * `Helpdesk Admin`, `Password Admin`, and `User Admin` = **Password ONLY** (zero access to MFA/FIDO2 settings).
   * `Authentication Admin` & `Privileged Authentication Admin` = **Password + MFA/FIDO2**.
3. **The 4 "Safe" Target Roles:** If the question states the target user holds `Directory Readers`, `Guest Inviter`, `Message Center Reader`, or `Reports Reader`, they are treated like standard non-privileged users $\rightarrow$ **Password Administrator** (or *Authentication Admin* for MFA) is sufficient.