# Module 18: Analyze and investigate sign-in logs to troubleshoot access issues - Summary

<details><summary>Source lessons</summary>

- Introduction
- Analyze and investigate sign-in logs to troubleshoot access issues
- Review and monitor Microsoft Entra audit logs
- Exercise connect data from Microsoft Entra ID to Microsoft Sentinel
- Export logs to third-party security information and event management system
- Analyze Microsoft Entra workbooks and reporting
- Monitor security posture with Identity Secure Score
- Summary and resources

</details>

```text
MONITORING & SECURITY REPORTING IN MICROSOFT ENTRA ID (SC-300)
│
├── 📚 REPORTING ARCHITECTURE (log components)
│   ├── Activity ──────> Sign-ins | Audit logs | Provisioning logs (Workday / ServiceNow)
│   ├── Security ──────> Risky sign-ins + Users flagged for risk (Identity Protection)
│   └── Reader roles ──> Security Admin | Security Reader | Global Reader | Report Reader
│
├── 🔐 SIGN-IN LOGS ──> Entra ID > Monitoring > Sign-ins
│   ├── License ───────> ALL editions (also accessible via Microsoft Graph API)
│   ├── ⚠️ Latency ─────> Up to 2 HOURS before sign-in records appear in the portal
│   ├── ⚠️ Scope ───────> INTERACTIVE sign-ins only (non-interactive are NOT displayed)
│   ├── Default view ──> Date | User | App | Status | Risk detection | MFA requirement
│   ├── Filters ───────> User/UPN, App, Status (Success/Failure/Interrupted), IP, Location,
│   │                    Client app, OS, Correlation ID, Conditional Access status
│   ├── CA troubleshooting ─> "Conditional Access" tab = policy list + result per sign-in
│   ├── Columns rule ──> No multi-value fields as column (auth details, CA, network location)
│   ├── Download ──────> CSV or JSON, max 250,000 records (limited by retention policies)
│   └── Shortcuts ─────> Identity Protection (30-day graph) | Enterprise apps (top 3 apps)
│
├── 🧾 AUDIT LOGS ──> Entra ID > Monitoring > Audit logs
│   ├── Default view ──> Date/time | Service | Category+Activity (what) | Status | Target | Actor
│   ├── Filters ───────> Service, Category (UserManagement, GroupManagement...), Activity, Status
│   ├── Date range ────> 7 days | 24 hours | Custom (start + end time)
│   ├── ⚠️ Case-sensitive ─> Target and "Initiated by" name / UPN searches
│   ├── Graph API ──> graph.windows.net/<tenant>/activities/auditActivityTypesV2?api-version=beta
│   ├── Download ──────> Up to 250,000 records, CSV or JSON
│   └── Shortcuts ─────> Users (UserManagement preselected) | Groups (GroupManagement) | Ent. apps
│
├── 🛡️ MICROSOFT SENTINEL ──> Scalable cloud-native SIEM + SOAR
│   ├── ⚠️ Sign-in logs ─> Ingestion into Sentinel requires Microsoft Entra ID P1 or P2
│   ├── Other log types ─> ANY license is enough (Free / O365 / P1 / P2)
│   ├── Required roles ──> Sentinel Contributor (workspace) + Security Administrator (tenant)
│   │                     + read/write rights on Entra diagnostic settings
│   ├── Connect ─────────> Sentinel > Data connectors > Microsoft Entra ID > tick Sign-in +
│   │                     Audit logs checkboxes > Apply changes
│   └── Workspace ───────> New Log Analytics workspace, globally unique name, Pay-as-you-go
│
├── 🔌 EXPORT TO THIRD-PARTY SIEM ──> Azure Monitor + Event Hubs (⭐ recommended approach)
│   ├── Flow ───────────> Diagnostic settings route logs to Event Hubs; SIEM connectors consume
│   ├── Splunk ─────────> Azure Monitor Add-On for Splunk
│   ├── IBM QRadar ─────> Microsoft Azure DSM + Microsoft Azure Event Hubs Protocol
│   ├── ArcSight ───────> ArcSight Azure Event Hubs smart connector
│   └── AzLog (legacy) ─> Old Azure Log Integration tool; migrate to Azure Monitor integrations
│
├── 📈 USAGE & INSIGHTS REPORT (app-centric workbook view)
│   ├── ⚠️ License ─────> Microsoft Entra ID P1 or P2 REQUIRED
│   ├── Roles ──────────> Security Administrator | Security Reader | Report Reader
│   ├── Path ───────────> Entra ID > Enterprise applications > Activity > Usage and insights
│   └── Answers ────────> Most used apps? Most failed sign-ins? Top sign-in errors per app?
│
├── 🔢 IDENTITY SECURE SCORE
│   ├── License ────────> Available in ALL editions of Microsoft Entra ID
│   ├── Path ───────────> Azure portal > Entra ID > Security > Identity Secure Score
│   ├── Dashboard ──────> Score | comparison vs same-industry / similar-size tenants | trend
│   └── Scoring ────────> Binary (100%/0%) or partial % (MFA for 5/100 users = 0.53% of 10.71%)
│
└── 🎯 SCOPE & KEY RULES (Introduction + Summary)
    ├── Objectives ─────> Investigate sign-ins | Audit logs | Diagnostic logs to Log Analytics /
    │                     Sentinel | Export to SIEM | Workbooks | Secure score | Notifications
    ├── ⭐ Self sign-ins ─> Any NON-admin user can access their OWN sign-ins
    ├── ⚠️ M365 logs ────> Full Microsoft 365 activity view ONLY in M365 admin center
    │                     (also via Office 365 Management APIs)
    └── KQL note ────────> Review activity via Log Analytics / Sentinel WITHOUT KQL in this module
```
