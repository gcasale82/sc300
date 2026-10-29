# Module 10: Explore Global Secure Access - Flashcards

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

### Introduction to Global Secure Access

**Q:** What is Security Service Edge (SSE)?
**A:** An identity-aware, cloud-delivered network perimeter that secures access for users working from anywhere.

**Q:** Which two products make up the Microsoft SSE solution, and what are they collectively called?
**A:** Microsoft Entra Internet Access and Microsoft Entra Private Access — collectively referred to as Global Secure Access.

**Q:** Which three Zero Trust principles is Global Secure Access founded on?
**A:** Least privilege, explicit verification, and an assumption of breach.

### Explore Global Secure Access

**Q:** Microsoft Entra Internet Access vs Microsoft Entra Private Access — what does each secure?
**A:** Internet Access: Microsoft services, SaaS apps, and public internet apps via the Secure Web Gateway (SWG). Private Access: private corporate resources without a legacy VPN.

**Q:** Microsoft Entra Private Access builds on which existing capability, and what does it add?
**A:** Builds on Microsoft Entra application proxy; extends access to any private resource, port, and protocol across hybrid and multicloud environments.

**Q:** You plan to deploy Global Secure Access. Which licenses do you need?
**A:** Microsoft Entra ID P1 or P2, plus a Microsoft Entra Internet Access license and/or a Microsoft Entra Private Access license.

**Q:** Which role must be assigned to at least one administrator to manage Global Secure Access?
**A:** Global Secure Access Administrator.

### Deploy and Configure Microsoft Entra Internet Access

**Q:** What are the four main steps to deploy Microsoft Entra Internet Access?
**A:** 1) Enable the Microsoft traffic forwarding profile, 2) install the Global Secure Access client, 3) enable tenant restrictions, 4) enable enhanced Global Secure Access signaling and Conditional Access.

**Q:** A Conditional Access policy targets Microsoft traffic. When is it enforced?
**A:** Only when the Global Secure Access client is installed on the device; without the client, Microsoft traffic still flows but the policy isn't enforced.

**Q:** Which three network routing policies are created when you enable the Microsoft traffic forwarding profile?
**A:** 1) Exchange Online, 2) SharePoint Online and OneDrive for Business, 3) Entra ID and Microsoft Graph (managed via FQDNs or IP subnets).

**Q:** How can the Global Secure Access client be deployed on Windows and Android?
**A:** Windows: manually (GlobalSecureAccessClient.exe) or via MDM such as Microsoft Intune. Android: via Intune or Microsoft Defender for Endpoint (client from the Android store).

**Q:** What do tenant restrictions control?
**A:** Which external tenants internal identities (employees) may access on the managed network; external identities (contractors, vendors) can be blocked from all external tenants.

**Q:** Which role is required to configure tenant restrictions in cross-tenant access settings?
**A:** At least Security Administrator (Identity > External Identities > Cross-tenant access settings > Organizational settings).

**Q:** You need to enforce tenant restrictions with Global Secure Access tagging. Which roles and portal path are required?
**A:** Both Global Secure Access Administrator and Security Administrator; Global Secure Access > Global Settings > Session Management > Tenant Restrictions > enable tagging.

**Q:** With tenant restrictions v2, what does data plane protection cover, and what is out of scope?
**A:** Blocks reuse of an Entra ID-issued token against Microsoft Graph and anonymous access to SharePoint Online; third-party apps such as Slack aren't in scope.

**Q:** In a compliant network Conditional Access policy, which cloud app must you NOT select in the app picker?
**A:** The generic "Office 365" cloud app (not supported) — select Office 365 Exchange Online and/or Office 365 SharePoint Online instead.

**Q:** Which named location is created when Global Secure Access signaling is enabled, and what is its location type?
**A:** "All Compliant Network locations" with location type Network Access (Protection > Conditional Access > Named locations); can optionally be marked trusted.

### Deploy and Configure Microsoft Entra Private Access

**Q:** What are the four main steps to deploy Microsoft Entra Private Access?
**A:** 1) Configure a private network connector and connector group, 2) configure Quick Access, 3) enable the Private Access traffic forwarding profile, 4) install the Global Secure Access client.

**Q:** What is a Microsoft Entra private network connector?
**A:** A lightweight agent installed on a server in the private network that facilitates the outbound connection to the Global Secure Access service.

**Q:** What are the requirements for the private network connector server?
**A:** Windows Server 2016 or later, .NET 4.7.2+, TLS 1.2 enabled; more than one server recommended for high availability.

**Q:** Which outbound ports must the private network connector be able to use?
**A:** Port 80 — downloading certificate revocation lists (CRLs); port 443 — all outbound communication with the Application Proxy service.

**Q:** Which role installs the private network connector?
**A:** Global Administrator of the directory that uses Application Proxy (Global Secure Access > Connect > Connectors).

**Q:** What is Quick Access in Microsoft Entra Private Access?
**A:** An app where you define a name, select a connector group, and add application segments (FQDNs and IP addresses) of private resources.

**Q:** Which four destination types can a Quick Access application segment use?
**A:** IP address (IPv4), FQDN (including wildcard FQDNs), IP address range (CIDR), and IP address range (IP to IP).

**Q:** Which name-resolution type is NOT supported in Private Access application segments?
**A:** NetBIOS — use an FQDN such as contoso.local/app1 instead of contoso/app1.

### Global Secure Access Dashboard

**Q:** What does the Global Secure Access snapshot widget display?
**A:** Distinct users (UPN), devices (device ID), and workloads/destinations (FQDNs and IPs) seen in the last 24 hours; filterable by Internet Access, Private Access, or Microsoft traffic.

**Q:** Name four common alerts shown on the Global Secure Access dashboard.
**A:** Unhealthy remote network, increased external tenants activity, token and device inconsistency, and web content blocked.

**Q:** In the cross-tenant access widget, what are "unseen tenants"?
**A:** Distinct tenant IDs seen in the last 24 hours but not seen in the previous seven days.

### Remote Networks

**Q:** What is a remote network in Global Secure Access?
**A:** A remote location (e.g., branch office) connected via an IPsec tunnel between the customer premises equipment (CPE) router and the nearest Global Secure Access endpoint; no client install needed on devices.

**Q:** What are the five steps to configure a remote network?
**A:** Basics (name, region) > Connectivity (router details) > Traffic forwarding profile > Review configuration > Set up on-premises router.

**Q:** Where must the final remote network step be performed, and what happens if you skip it?
**A:** In the on-premises router (CPE) management console; until it's done, IPsec isn't set up because IKE negotiations must complete between both parties.

**Q:** Which two tools can be used to configure remote networks?
**A:** The Microsoft Entra admin center or the Microsoft Graph API.

### Conditional Access with Global Secure Access

**Q:** Which three new Conditional Access checks are introduced with Global Secure Access?
**A:** Compliant network check, Private Access apps, and Source IP restoration.

**Q:** Where do you enable Global Secure Access signaling for Conditional Access?
**A:** Global Secure Access > Global settings > Session management > Adaptive access (as Global Secure Access Administrator); CAE signaling is auto-enabled for Office 365 (preview).

**Q:** At which planes is the compliant network enforced, and which services support the data plane?
**A:** Authentication plane (Microsoft Entra ID) and data plane (preview) via Continuous Access Evaluation — currently only Exchange Online and SharePoint Online.

**Q:** What does Source IP restoration provide?
**A:** The original user source IP for Conditional Access/CAE location policies, Identity Protection risk detections, and sign-in logs; enabled by turning on Global Secure Access signaling.

**Q:** Source IP restoration — which traffic is supported, and what limitation applies?
**A:** Only Microsoft traffic (SharePoint Online, Exchange Online, Teams, Microsoft Graph); the Global Secure Access service IP isn't visible, only the source IP.

**Q:** How do you create a Conditional Access policy for a Quick Access or Private Access app?
**A:** Global Secure Access > Applications > Enterprise applications > select the app > Conditional Access > Create new policy; the app is auto-added as the target resource.

**Q:** Which accounts should be excluded from Conditional Access policies?
**A:** Emergency access (break-glass) accounts to prevent tenant-wide lockout, and service accounts/principals such as the Microsoft Entra Connect Sync account.

### Logs and Monitoring

**Q:** Where can you access Global Secure Access audit logs?
**A:** Global Secure Access > Audit logs (pre-filtered), or Identity > Monitoring & health > Audit logs with the Service filter set to Global Secure Access.

**Q:** In traffic logs, what is the hierarchy of session, connection, and transaction?
**A:** Session starts with the first URL a user accesses; a session contains connections (5-tuple: source/destination IP, source/destination port, FQDN); a connection contains transactions (unique request/response pair).

**Q:** You need to view Global Secure Access traffic logs. Which path and minimum role?
**A:** Global Secure Access > Monitor > Traffic logs; at least the Reports Reader role.

**Q:** Enriched Microsoft 365 logs — which two log tables are combined, and how?
**A:** Microsoft 365 OfficeActivity + Global Secure Access NetworkAccessTraffic, joined via a Unique Token ID; only SharePoint Online logs are available for enrichment.

**Q:** Which diagnostic setting sends Global Secure Access traffic logs, and to which destinations?
**A:** NetworkAccessTrafficLogs — to a Log Analytics workspace, storage account, event hub, or partner solution (requires at least Security Administrator).

**Q:** What are the retention periods for Global Secure Access logs?
**A:** Traffic and remote network health logs: 30 days. Audit logs: varies by Microsoft Entra ID license. Office logs: up to 24 hours.
