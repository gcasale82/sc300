# Module 10: Explore Global Secure Access - Summary

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

```text
GLOBAL SECURE ACCESS — SSE (SC-300)
│
├── 🌐 FOUNDATION ──> Security Service Edge = Entra Internet Access + Entra Private Access
│   ├── Zero Trust basis ──> least privilege, explicit verification, assume breach
│   ├── Delivery ──> Microsoft WAN (global regions + edge locations) | portal: entra.microsoft.com
│   ├── Licensing ⭐ ──> Entra ID P1 or P2 + Internet Access and/or Private Access license
│   └── Plan via ──> Zero Trust Guidance Center (recommended before rollout)
│
├── 🛡️ INTERNET ACCESS ──> Secure Web Gateway (SWG) for M365, SaaS & public internet
│   ├── Key features ──> token-replay prevention, universal tenant restrictions, enriched logs
│   ├── Web filtering ──> allow/block sites by content category + domain name
│   ├── Tenant restrictions v2 ──> auth plane: Entra ID blocks unsanctioned external accounts
│   │   └── Data plane ──> Graph + SPO anonymous blocked; ⚠️ 3rd-party apps (Slack) NOT in scope
│   ├── Setup path ──> Identity > External Identities > Cross-tenant access > Organizational
│   │   settings ──> Add organization (domain or tenant ID) ──> edit inherited settings
│   └── ⚠️ Enable tagging to enforce ──> needs BOTH Global Secure Access AND Security Admin
│
├── 🔒 PRIVATE ACCESS ──> VPN-less access to private resources (any IP, FQDN, port, protocol)
│   ├── Connector ⭐ ──> Windows Server 2016+ | .NET 4.7.2+ | TLS 1.2 | OUTBOUND 80 + 443 only
│   ├── URLs for connector ──> *.msappproxy.net + *.servicebus.windows.net (443/HTTPS)
│   ├── High availability ──> install on >1 server; verify services Running (connector + Updater)
│   ├── Quick Access app ──> segments: IPv4 / FQDN+wildcards / CIDR / IP-to-IP range + ports
│   │   ├── ⚠️ NetBIOS NOT supported ──> use contoso.local/app1, never contoso/app1
│   │   └── Ports ──> 22 SSH | 80 HTTP | 443 HTTPS | 445 SMB | 3389 RDP (comma / hyphen ranges)
│   └── Deploy (4 steps) ──> connector+group > Quick Access > Private profile > GSA client
│
├── 📲 CLIENT & TRAFFIC FORWARDING
│   ├── Client download ──> Global Secure Access > Connect > Client download (GSA Admin)
│   ├── Platforms ──> Windows (manual or Intune) | Android (Intune or Defender for Endpoint)
│   ├── Profiles path ──> Global Secure Access > Connect > Traffic forwarding (toggle profile)
│   ├── Microsoft profile creates ──> routing policies (Exchange, SPO+OneDrive, Entra+MSGraph)
│   └── ⚠️ CA on Microsoft traffic ──> enforced ONLY when GSA client installed on device
│
├── 🌍 REMOTE NETWORKS ──> branch offices via IPsec tunnel from CPE router to nearest edge
│   ├── Steps 1-4 (portal or Graph) ──> Basics (name+region) > Connectivity (device, IP,
│   │   redundancy, bandwidth) > Traffic forwarding > Review configuration
│   └── ⚠️ Step 5 = on-prem router ──> enter MS settings in CPE console or NO tunnel (IKE
│       is bidirectional — do not skip)
│
├── 🛂 CONDITIONAL ACCESS + GSA ──> first enable signaling (one time, GSA Admin)
│   ├── Signaling path ──> GSA > Global settings > Session management > Adaptive access
│   │   └── Auto-enables ──> CAE signaling for Office 365 (preview)
│   ├── Compliant network check ──> auto Named location "All Compliant Network locations"
│   │   ├── Enforcement ──> auth plane (Entra ID) + data plane via CAE (preview)
│   │   └── ⚠️ CAE data plane ──> only Exchange Online + SharePoint Online today
│   ├── Policy recipe ──> Include All users > Target specific apps > Location Any >
│   │   Exclude compliant network location > Grant: Block access
│   ├── Source IP restoration ──> keeps original user IP; only Microsoft traffic (EXO, SPO,
│   │   Teams, Graph); enabled automatically with signaling
│   │   └── ⚠️ With CAE strict location ──> users blocked even in trusted IP range
│   ├── CA on Private/Quick Access apps ──> GSA > Applications > Enterprise applications >
│   │   app > Conditional Access (app auto-set as target resource)
│   ├── ⚠️ App picker trap ──> generic "Office 365" cloud app NOT supported — pick specific apps
│   └── ⭐ Always exclude ──> break-glass accounts + service accounts (Entra Connect sync)
│
├── 📊 DASHBOARD & MONITORING ──> GSA > Dashboard (Global Secure Access Administrator)
│   ├── Snapshot widget ──> Users (UPN) / Devices (device ID) / Workloads (FQDN+IP) in 24h
│   ├── Alerts ──> unhealthy remote network, more external tenants, token-device mismatch,
│   │   web content blocked
│   ├── Other widgets ──> usage profiling (transactions/users/devices/bytes), top destinations,
│   │   cross-tenant access, web category filtering, device status (active 24h / inactive 7d)
│   └── Cross-tenant widget ──> sign-ins, distinct + unseen tenants (24h vs prior 7 days)
│
├── 📜 LOGS ──> GSA > Audit logs (pre-filtered) | Identity > Monitoring & health > Audit logs
│   ├── Traffic logs hierarchy ⭐ ──> Session > Connection (5-tuple: IPs, ports, FQDN) >
│   │   Transaction (unique request + response) | view: GSA > Monitor > Traffic logs
│   ├── Enriched O365 logs ──> join OfficeActivity + NetworkAccessTraffic via Unique Token ID
│   │   (Sentinel recommended workbook) | ⚠️ only SharePoint Online available for enrichment
│   ├── Diagnostic settings (Security Admin) ──> NetworkAccessTrafficLogs to Log Analytics /
│   │   storage account / event hub / partner (SIEM)
│   └── Retention ⚠️ ──> traffic + remote network health: 30 days | Office logs: 24 hours |
│       audit logs: varies by Microsoft Entra ID license
│
└── 🔑 ROLES CHEAT SHEET
    ├── Global Secure Access Administrator ──> profiles, client, remote nets, signaling, dashboard
    ├── Security Administrator ──> tenant restrictions setup + diagnostic settings
    ├── Conditional Access Administrator ──> compliant network + private app CA policies
    └── Global Administrator ──> install/register private network connector (App Proxy tenant)
```
