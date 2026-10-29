# Module 10: Explore Global Secure Access - Mind Map

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

```mermaid
mindmap
  root((Global Secure Access))
    Overview and SSE
      Zero Trust principles
        Least privilege
        Explicit verification
        Assume breach
      Microsoft Entra Internet Access
        Secure Web Gateway
      Microsoft Entra Private Access
        VPN-less private access
      Entra ID P1 or P2 license
      Global Secure Access Administrator role
      Microsoft Entra admin center portal
    Internet Access deployment
      Enable Microsoft traffic forwarding
        Exchange Online
        SharePoint and OneDrive
        Entra ID and MSGraph
      Install Global Secure Access client
      Configure tenant restrictions
        Cross-tenant access settings
        Enable tagging enforcement
      Enable signaling and Conditional Access
        Compliant network named location
    Private Access deployment
      Private network connector
        Windows Server 2016 or later
        .NET 4.7.2 or higher
        Outbound ports 80 and 443
      Connector groups
      Quick Access app
        Application segments
        FQDN IP and CIDR destinations
      Enable Private Access forwarding profile
    Dashboard
      Snapshot users devices workloads
      Alerts and notifications
        Unhealthy remote network
        Token and device inconsistency
      Top used destinations
      Cross-tenant access
      Web category filtering
    Remote networks
      IPsec tunnel to Global Secure Access
      Customer premises equipment router
      Five configuration steps
        Basics name and region
        Connectivity device details
        Traffic forwarding profile
        Review configuration
        On-premises router setup
    Conditional Access integration
      Enable Global Secure Access signaling first
      Compliant network check
        Authentication plane enforcement
        Data plane via CAE
      Private Access apps policies
      Source IP restoration
        Microsoft traffic only
      Exclude emergency and service accounts
    Logs and monitoring
      Audit logs
      Traffic logs preview
        Session
        Connection 5-tuple
        Transaction
      Enriched Office 365 logs
        Unique Token ID correlation
        SharePoint Online only
      Diagnostic settings destinations
        Log Analytics workspace
      Retention
        Traffic logs 30 days
        Office logs 24 hours
    Summary
      Traffic forwarding profiles
      Tenant restrictions
      Compliant network policies
      Zero Trust strategy
```
