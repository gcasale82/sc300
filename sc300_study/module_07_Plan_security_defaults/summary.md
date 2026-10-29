# Module 07: Plan security defaults - Summary

<details><summary>Source lessons</summary>

- Introduction
- Plan security defaults
- Exercise - Work with security defaults
- Plan Conditional Access policies
- Implement Conditional Access policy controls and assignments
- Exercise - Implement Conditional Access policies roles and assignments
- Test and troubleshoot Conditional Access policies
- Implement application controls
- Implement session management and continuous access evaluation
- Exercise - Configure authentication session controls
- Microsoft Entra Conditional Access Optimization agent
- Summary and resources

</details>

```text
CONDITIONAL ACCESS & SECURITY DEFAULTS (SC-300)
│
├── 🏢 SECURITY DEFAULTS (works on Free tier, no extra cost)
│   ├── Auto-enabled ──────> tenants created on/after Oct 22, 2019 (all new tenants)
│   ├── Portal path ───────> Entra admin center > Overview > Properties > Manage security defaults
│   ├── Required role ─────> Conditional Access Administrator or Security Administrator
│   ├── Enforces ──────────> MFA registration for ALL users (no grace period, number
│   │                         matching), MFA every sign-in for 16 admin roles,
│   │                         protect privileged activity (Azure portal)
│   └── ⚠️ Legacy auth ────> blocked (IMAP/SMTP/POP3, Exchange ActiveSync basic) —
│                             legacy protocols bypass MFA entirely
│
├── 🧩 CA POLICY ANATOMY (if-then: assignments -> controls)
│   ├── Assignments ───────> Users/groups • Cloud apps or actions • Conditions
│   ├── Access controls ───> Grant (MFA, compliant device, hybrid joined) or Block access
│   ├── Session controls ──> App enforced permissions, CA App Control, sign-in frequency
│   ├── Licenses ──────────> Free / Office 365 = NO CA • P1 (M365 E3+) = standard CA
│   │                         • P2 = + risk-based policies (Identity Protection)
│   ├── ⚠️ Token default ──> no assignment matched + no policy = access token IS issued
│   └── ⭐ Block access ───> overrides ALL other assignments; exclude break-glass
│                             accounts or you lock out the whole tenant
│
├── 📍 CONDITIONS & RISK (risk-based policies require P2)
│   ├── Named locations ───> Protection > CA > Named locations (IP ranges / countries,
│   │                         optional "include unknown areas")
│   ├── ⭐ Best practice ──> block a named location of never-expected countries,
│   │                         but EXEMPT administrators from that policy
│   ├── Sign-in risk ──────> probability the request isn't by the identity owner (P2)
│   ├── User risk ─────────> leaked credentials -> force secure password change (P2)
│   ├── User actions ──────> "Register security info": block unless trusted location
│   │                         or compliant / hybrid Entra joined device state
│   └── ⭐ Always exclude ─> emergency/break-glass accounts + service accounts (MFA
│                             can't run programmatically); CA never blocks service
│                             principals — replace scripts with managed identities
│
├── 📱 DEVICE & APP CONTROLS (Entra ID P1/P2 + Intune)
│   ├── Compliant device ──> Intune reports compliance (PIN, encryption, OS version,
│   │                         no jailbreak) -> CA grants/blocks
│   │                         ⚠️ does NOT block Intune enrollment
│   ├── Approved client app ─> requires Entra registration first (iOS/Android),
│   │                         works for modern auth + Exchange ActiveSync clients
│   ├── CA App Control ────> Defender for Cloud Apps (reverse proxy): block download/
│   │                         cut/copy/print, label on download, block unlabeled
│   │                         uploads, monitor risky sessions in real time
│   └── App protection (MAM) ─> app-level rules (PIN, block data transfer), works
│                             WITHOUT MDM (MAM-WE for BYOD); target by managed state
│
├── ⏱️ SESSION MANAGEMENT & CAE
│   ├── Sign-in frequency ─> default = rolling 90-day window; OAuth2/OIDC apps + SAML
│   │                         (if not dropping own cookies); device unlock/sign-in
│   │                         satisfies the requirement
│   ├── Browser session ───> "Stay signed in?" prompt on personal/unmanaged devices
│   ├── ⭐ Don't over-prompt ─> users trained to type credentials blindly fall for
│                             phishing; policy violations still revoke sessions
│   └── CAE ───────────────> near real-time enforcement (disable, password change,
│                             location change) via 401 + claim challenge -> reauth
│
├── 🧪 TESTING & TROUBLESHOOTING
│   ├── Report-only ───────> Microsoft recommends ALL new policies start here
│   ├── What If tool ──────> simulate sign-in; session controls appear in results
│   ├── Sign-in logs ──────> Identity > Monitoring > Sign-ins > Conditional Access tab
│   ├── Roles ─────────────> Security Administrator or Global Reader
│   ├── Error page ────────> "More Details" -> correlation ID / request ID for support
│   └── ⚠️ Forbidden ──────> All users + all cloud apps: Block, require hybrid joined,
│                             or require app protection policy = org-wide lockout
│
├── 🤖 CA OPTIMIZATION AGENT
│   ├── Requirements ──────> Microsoft Entra ID P1 minimum + Security Compute Units (SCU)
│   ├── Activation role ───> Security Administrator or higher (first activation);
│   │                         CA Admins can be granted Security Copilot access
│   ├── Features ──────────> require MFA for uncovered users, device controls (needs
│   │                         Intune), block legacy auth, block device code flow,
│   │                         consolidate overlapping policies, one-click remediation
│   └── Entra Agent ID ────> AI agents = first-class principals; target in CA like
│                             workload identities, exclude trusted/emergency agents
│
└── 📜 TERMS OF USE (TOU)
    ├── Created in ────────> Identity Governance > Terms of use (PDF upload required;
    │                         set expiry, require open before accept)
    └── Enforced via ──────> Conditional Access grant control; consent can expire and
                              users must re-attest
```
