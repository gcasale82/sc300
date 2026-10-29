# Module 11: Discover apps by using Microsoft Defender for Cloud Apps and Active Directory Fe - Flashcards

<details><summary>Source lessons</summary>

- Introduction
- Discover apps by using Microsoft Defender for Cloud Apps and Active Directory Fe
- Configure connectors to apps
- Exercise implement access management for apps
- Design and implement app management roles
- Exercise create a custom role to manage app registration
- Configure preintegrated gallery SaaS apps
- Implement and manage policies for OAuth apps
- Summary and resources

</details>

### CASB and Microsoft Defender for Cloud Apps (MDCA)

**Q:** What is a CASB?
**A:** Cloud Access Security Broker — an on-premises or cloud-based security policy enforcement point placed between cloud service consumers and cloud service providers to enforce enterprise security policies.

**Q:** What is Microsoft Defender for Cloud Apps (MDCA)?
**A:** Microsoft's implementation of a CASB service that protects data, services, and applications with enterprise policies, providing supplemental reporting and analytics.

**Q:** Which deployment modes does MDCA support?
**A:** Log collection, API connectors, and reverse proxy.

**Q:** Which MDCA feature provides visibility into Shadow IT?
**A:** Cloud Discovery — it uses traffic logs to dynamically discover and analyze the cloud apps your organization is using.

**Q:** How do you create a snapshot report of your organization's cloud use in Cloud Discovery?
**A:** Manually upload log files from your firewalls or proxies for analysis.

**Q:** How do you set up continuous reports in Cloud Discovery?
**A:** Use MDCA log collectors to periodically forward your logs.

**Q:** You need to get a general picture of discovered cloud apps before deeper investigation. Where should you look first?
**A:** The Cloud Discovery Dashboard — starting with the High-level usage overview.

**Q:** What can you check in the Cloud Discovery Dashboard to identify the most dominant users of cloud apps?
**A:** The top users and source IP addresses.

**Q:** Where do you review the risk score of a discovered app and the status of open discovery alerts?
**A:** In the App risk overview on the Cloud Discovery Dashboard.

### Filtering and Sanctioning Apps

**Q:** Which filter lets you search for apps compliant with HIPAA, ISO 27001, SOC 2, or PCI-DSS?
**A:** The Compliance risk factor filter.

**Q:** Which filter indicates whether allowing permission to an app is common, uncommon, or rare?
**A:** The Community use filter (used in OAuth app policies).

**Q:** How many cloud apps are in the MDCA Cloud app catalog, and how are they scored?
**A:** More than 16,000 cloud apps, ranked and scored based on industry standards, regulatory certifications, and best practices.

**Q:** On how many risk factors is MDCA app scoring based?
**A:** More than 80 risk factors.

**Q:** What is the effect of marking an OAuth app as banned in an OAuth app policy?
**A:** It disables the correlating Enterprise Application.

### AD FS Application Migration

**Q:** What is AD FS?
**A:** Active Directory Federation Services — a standards-based on-premises identity service that extends SSO between trusted business partners (federation) without requiring separate sign-ins.

**Q:** Which report helps identify which AD FS applications can migrate to Microsoft Entra ID?
**A:** The AD FS application activity report, available in the Azure portal under Microsoft Entra ID > Enterprise applications > Usage and insights.

**Q:** Which apps are listed in the AD FS application activity report?
**A:** All AD FS applications in the organization that have had an active user logged in within the last 30 days.

**Q:** Which admin roles grant access to the AD FS application activity report data?
**A:** Global reader/administrator, report reader, security reader, application administrator, or cloud application administrator.

**Q:** What does the migration status "Ready to migrate" mean?
**A:** The AD FS application configuration is fully supported in Microsoft Entra ID and can be migrated as-is.

**Q:** What does the migration status "Needs review" mean?
**A:** Some of the application's settings can be migrated to Microsoft Entra ID, but settings that can't be migrated as-is must be reviewed.

**Q:** What does the migration status "Additional steps required" mean?
**A:** Microsoft Entra ID doesn't support some of the application's settings, so the application can't be migrated in its current state.

**Q:** Which two types of applications should be migrated from AD FS to Microsoft Entra ID?
**A:** SaaS applications (procured by the organization) and line-of-business (LOB) applications (developed by the organization).

**Q:** Which authentication protocols should be migrated first, and why?
**A:** Modern authentication protocols such as SAML and OpenID Connect — they can be reconfigured via a built-in gallery connector or by registering the app in Microsoft Entra ID.

**Q:** How do you integrate apps using older (legacy) authentication protocols?
**A:** By using Application Proxy and/or Microsoft Entra Domain Services.

### App Connectors

**Q:** How do app connectors enable greater visibility and control in MDCA?
**A:** They use the APIs of app providers; all communication between MDCA and connected apps is encrypted using HTTPS.

**Q:** Which apps does MDCA multi-instance support apply to?
**A:** Only API connected apps — not Cloud Discovered apps or Proxy connected apps.

**Q:** How often does MDCA perform periodic data scans of connected apps, and what triggers real-time scans?
**A:** Periodic scans every 12 hours; real-time scans are triggered each time a change is detected.

**Q:** What capabilities does the App permissions / App permission governance API connection provide?
**A:** Visibility into issued tokens and their permissions, and the ability to remove tokens.

**Q:** Why might some MDCA policies take hours or days to run?
**A:** Operations like scanning all files in a tenant require numerous API calls, which are spread over a longer period due to API limitations such as throttling and API limits.

### Access Management for Apps

**Q:** Which license is required to complete the exercises in this module?
**A:** Microsoft Entra ID Premium P2 (a free trial can be activated).

**Q:** Which gallery app is added in the exercise to demonstrate user assignment?
**A:** GitHub Enterprise Cloud – Enterprise Account.

### App Management Roles

**Q:** What happens automatically when a user creates a new application registration?
**A:** They're automatically added as the first owner of the application.

**Q:** Which role should you assign to grant users the ability to create application registrations when "Users can register applications" is set to No?
**A:** The Application Developer role (it also grants consent on one's own behalf when user consent is disabled).

**Q:** Which setting disables the default ability for users to create application registrations?
**A:** The "Users can register applications" setting on the User settings page, set to No.

**Q:** What can an enterprise application owner manage?
**A:** Organization-specific configuration of the enterprise app they own, such as SSO configuration, provisioning, and user assignments — but only for apps they own.

**Q:** Which principals can be owners of application registrations vs. enterprise applications?
**A:** Users and service principals can own application registrations; only users can own enterprise applications. Groups can't be owners of either.

**Q:** Application Administrator vs. Cloud Application Administrator — what's the key difference?
**A:** Cloud Application Administrator has the same permissions as Application Administrator except the ability to manage application proxy.

**Q:** Can Application Administrator or Cloud Application Administrator manage Conditional Access settings?
**A:** No — neither role grants the ability to manage Conditional Access settings.

**Q:** Are Application Administrators or Cloud Application Administrators added as owners when creating new app registrations or enterprise apps?
**A:** No — users assigned to these roles aren't added as owners.

**Q:** What are the two separate steps for using a custom role?
**A:** Create a custom role definition with permissions, then create a role assignment to assign the custom role at a scope.

**Q:** At which scopes can a custom role be assigned?
**A:** Organization-wide (directory) scope or the scope of a single Microsoft Entra object, such as a single app registration.

**Q:** Where do role assignments for app registrations appear in the App registrations page?
**A:** Only in the All applications tab — they don't show up in the Owned applications tab.

**Q:** Custom roles don't grant access to the Microsoft Entra ID portal when which user setting is enabled?
**A:** When the "Restrict access to Microsoft Entra ID administration portal" user setting is set to Yes.

### Gallery SaaS App Configuration

**Q:** What does the "Enabled for users to sign in?" app property control?
**A:** Whether users assigned to the application can sign in.

**Q:** What does the "User assignment required?" property control?
**A:** Whether users who aren't assigned to the application can sign in.

**Q:** What does the "Visible to users?" property control?
**A:** Whether assigned users can see the app in My Apps and the Microsoft 365 app launcher.

**Q:** Which SSO protocol do apps added through App registrations use by default, and which field do they lack compared to SAML apps?
**A:** OIDC-based apps by default; they don't have the User access URL field that SAML-based SSO apps have.

**Q:** What are the requirements for a custom app logo?
**A:** 215 by 215 pixels, saved in .png format.

### OAuth App Policies

**Q:** Where do you create and manage OAuth app policies in MDCA?
**A:** At https://security.microsoft.com, under the Cloud apps section, select OAuth apps.

**Q:** How can you create an OAuth app policy besides using the "New policy from search" button?
**A:** Select Control > Policies > Create policy > OAuth app policy.

**Q:** You need to revoke uncommon apps requesting high permissions, but only when authorized by an administrator. How do you scope the policy?
**A:** Set the policy based on group memberships of the users who authorized the apps — e.g., only if the user is a member of the Administrators group.
