# Module 18: Analyze and investigate sign-in logs to troubleshoot access issues - Mind Map

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

```mermaid
mindmap
  root((Monitoring Microsoft Entra ID))
    Reporting architecture
      Activity
        Sign-ins
        Audit logs
        Provisioning logs
      Security
        Risky sign-ins
        Users flagged for risk
    Sign-in logs
      All editions and Graph API
      Up to two hours delay
      Interactive sign-ins only
      Filters
        User UPN and status
        IP address and location
        Client app
        Conditional Access
      CA tab shows policy results
      Download 250,000 records
      Reader roles
        Security Administrator
        Security Reader
        Global Reader
        Report Reader
    Audit logs
      System activity for compliance
      Filters
        Service and category
        Activity and status
        Target and initiator
      Date range
        7 days or 24 hours
        Custom range
      Preselected filtered views
        Users tab UserManagement
        Groups tab GroupManagement
        Enterprise applications
      Download 250,000 records
    Microsoft Sentinel
      Cloud-native SIEM and SOAR
      Collect detect investigate respond
      Sign-in logs need P1 or P2
      Other logs any edition
      Sentinel Contributor role
      Security Administrator role
      Connect via data connectors
      Stream sign-in and audit logs
    Export to third-party SIEM
      Azure Monitor single pipeline
      Route via Event Hubs
      Multiple diagnostic settings
      AzLog deprecated tool
      Splunk Azure Monitor Add-On
      QRadar DSM and protocol
      ArcSight smart connector
    Workbooks and reporting
      Usage and insights report
      Application-centric sign-in view
      Needs P1 or P2 license
      Security or Report Reader roles
      Enterprise applications path
      Sort successes failures rate
      Top errors per application
    Identity Secure Score
      Percentage of best practices
      All editions available
      Path Security Identity Secure Score
      Comparison and trend graphs
      Improvement actions list
      Binary or partial scoring
      Balance security usability
    Summary
      Troubleshoot sign-in issues
      Monitor audit logs
      Integrate Log Analytics Sentinel
      Export logs to SIEM
      Analyze workbooks reports
      Track identity secure score
      Configure notifications
```
