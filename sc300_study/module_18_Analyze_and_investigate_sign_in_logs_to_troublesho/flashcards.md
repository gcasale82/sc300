# Module 18: Analyze and investigate sign-in logs to troubleshoot access issues - Flashcards

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

### Reporting Architecture

**Q:** Which three log types make up the "Activity" component of the Microsoft Entra ID reporting architecture?
**A:** Sign-ins, Audit logs, and Provisioning logs.

**Q:** What is a risky sign-in?
**A:** An indicator for a sign-in attempt by someone who isn't the legitimate owner of a user account.

**Q:** What is a risky user?
**A:** An indicator that a user account might have been compromised.

**Q:** Which built-in roles can access sign-in and risk reporting data?
**A:** Security Administrator, Security Reader, Global Reader, and Report Reader.

**Q:** Can non-admin users access sign-in data?
**A:** Yes — any user (non-admin) can access their own sign-ins.

---

### Sign-in Logs

**Q:** Which license is required to access the sign-in activity report?
**A:** None specifically — the sign-in activity report is available in all editions of Microsoft Entra ID (also via Microsoft Graph API).

**Q:** How long does it take for sign-in records to appear in the portal?
**A:** Up to two hours.

**Q:** You need to view service-to-service (non-interactive) authentication in the Sign-ins report. What should you tell the admin?
**A:** Not possible — the sign-ins report only displays interactive sign-ins; non-interactive sign-ins are not displayed.

**Q:** You need to export sign-in logs to work with data outside the Azure portal. Which formats and limits apply?
**A:** Download as CSV or JSON, up to the most recent 250,000 records (constrained by report retention policies).

**Q:** In sign-in logs, what does Conditional Access status "Failure" mean?
**A:** The sign-in satisfied the user and application condition of at least one CA policy, and grant controls were not satisfied or set to block access.

**Q:** In sign-in logs, what does Conditional Access status "Not applied" mean?
**A:** No policy applied to the user and application during the sign-in.

**Q:** Which sign-in log filter shows whether a user signed in via browser, mobile app/desktop client, or Exchange ActiveSync?
**A:** The Client app filter.

**Q:** Why might an IP address map to a wrong physical location in Microsoft Entra reports?
**A:** Location conversion is best effort; mobile providers and VPNs issue IP addresses from central pools far from the client device.

**Q:** You need an application-centric view of sign-in data for your top three applications. Where do you look?
**A:** Enterprise applications > Overview > last 30 days report (app-usage graphs, weekly aggregation, default 30 days).

**Q:** Where is the only place to get a full view of Microsoft 365 activity logs?
**A:** The Microsoft 365 admin center (also programmatically via Office 365 Management APIs).

**Q:** What is the default time period for the Identity Protection user sign-in graph?
**A:** 30 days (weekly aggregations of sign-ins).

---

### Audit Logs

**Q:** Where do you access the Microsoft Entra audit logs report?
**A:** Microsoft Entra ID > Monitoring > Audit logs.

**Q:** What does an audit log default list view show?
**A:** Date/time, service that logged it, category and activity name (what), status (success/failure), target, and initiator/actor (who).

**Q:** Which audit log category is preselected when viewing audit logs from the Users tab?
**A:** UserManagement.

**Q:** Which audit log category is preselected when viewing audit logs from the Groups tab?
**A:** GroupManagement.

**Q:** Which filter is preselected when viewing audit logs from Enterprise applications?
**A:** Enterprise applications as the Application Type.

**Q:** What are the possible values of the audit log Status filter?
**A:** All, Success, Failure.

**Q:** Which audit log filters are case-sensitive?
**A:** Target and Initiated by (Actor) — both name and UPN are case-sensitive.

**Q:** What are the Date range filter options for audit logs?
**A:** 7 days, 24 hours, or Custom (with configurable start and end time).

**Q:** How can you retrieve the full list of audit activities programmatically?
**A:** Via the Graph API: graph.windows.net/<tenantdomain>/activities/auditActivityTypesV2?api-version=beta

---

### Microsoft Sentinel Integration

**Q:** What is Microsoft Sentinel?
**A:** A scalable, cloud-native SIEM and SOAR solution that collects, detects, investigates, and responds to threats across the enterprise.

**Q:** Which license is required to ingest sign-in logs into Microsoft Sentinel?
**A:** Microsoft Entra ID P1 or P2.

**Q:** Which licenses are sufficient to ingest non-sign-in Microsoft Entra log types into Sentinel?
**A:** Any Microsoft Entra ID license (Free/O365/P1/P2).

**Q:** Which two roles must a user have to connect Microsoft Entra logs to Microsoft Sentinel?
**A:** Microsoft Sentinel Contributor (on the workspace) and Security Administrator (on the tenant streaming the logs).

**Q:** Which log types can be streamed via the Microsoft Sentinel Microsoft Entra ID connector?
**A:** Sign-in logs and audit logs.

---

### Exporting to Third-Party SIEM

**Q:** What is the Microsoft recommended approach for integrating Azure with SIEM tools?
**A:** Route data via Azure Monitor to Azure Event Hubs, where SIEM connectors consume the data.

**Q:** Which tool should a Splunk customer use for SIEM integration?
**A:** The Azure Monitor Add-On for Splunk (migrate away from AzLog).

**Q:** Which components should IBM QRadar customers use for Azure integration?
**A:** Microsoft Azure DSM and Microsoft Azure Event Hubs Protocol (from the IBM support website).

**Q:** Which Microsoft Entra log type was directly integrated with AzLog but not yet available in Azure Monitor?
**A:** Microsoft Entra logs.

**Q:** What is AzLog?
**A:** The Azure Log Integration tool, used to consolidate, translate, and forward logs from Azure services to a SIEM tool before Azure Monitor existed.

---

### Workbooks and Usage & Insights

**Q:** Which license is required to access the usage and insights report?
**A:** Microsoft Entra ID P1 or P2.

**Q:** Which roles can access the usage and insights report?
**A:** Security Administrator, Security Reader, or Report Reader.

**Q:** Where do you find the usage and insights report?
**A:** Azure portal > Microsoft Entra ID > Enterprise applications > Activity > Usage and insights.

**Q:** What can you sort by in the usage and insights report?
**A:** Number of successful sign-ins, failed sign-ins, and success rate.

---

### Identity Secure Score

**Q:** What is the identity secure score?
**A:** A percentage indicating how aligned your identity security is with Microsoft's best practice recommendations.

**Q:** Which license is required for identity secure score?
**A:** None — it's available in all editions of Microsoft Entra ID.

**Q:** What is the portal path to access the identity secure score?
**A:** Azure portal > Microsoft Entra ID > Security > Identity Secure Score.

**Q:** What does the identity secure score dashboard's comparison graph show?
**A:** How your score compares to other tenants in the same industry and of similar size.

**Q:** How are identity secure score controls scored?
**A:** Two ways — binary (100% if configured per recommendation) or as a percentage of total configuration (partial credit, e.g., MFA for some users).

**Q:** Besides configuring security features, what other task type improves your secure score?
**A:** Security-related tasks, like reading reports.
