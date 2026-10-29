# Module 04: Plan, design, and implement Microsoft Entra Connect - Flashcards

<details><summary>Source lessons</summary>

- Introduction
- Plan, design, and implement Microsoft Entra Connect
- Implement manage password hash synchronization (PHS)
- Implement manage pass-through authentication (PTA)
- Explore pass-through authentication and seamless single sign-on (SSO)
- Implement and manage federation
- Trouble-shoot synchronization errors
- Implement Microsoft Entra Connect Health
- Manage Microsoft Entra Health
- Summary and resources

</details>

### Hybrid Identity and Microsoft Entra Connect

**Q:** What is hybrid identity?
**A:** An identity solution spanning on-premises and cloud environments, providing unified authentication and authorization to resources regardless of their location.

**Q:** What is Microsoft Entra Connect and what are its five capabilities?
**A:** The Microsoft tool that bridges on-premises AD with Microsoft Entra ID. Capabilities: synchronization, password hash synchronization (PHS), pass-through authentication (PTA), federation integration, and health monitoring.

### Choosing an Authentication Method

**Q:** How many PTA agents are recommended for high availability?
**A:** Three lightweight agents: the first on the Microsoft Entra Connect server plus two extra agents, so authentication continues if one agent is down for maintenance.

**Q:** You need to deploy PTA agents. Can you place them in a perimeter network (DMZ)?
**A:** No. Agents must access on-premises AD DS including domain controllers and need outbound internet access, so perimeter network deployment is not supported.

**Q:** PTA agents cannot validate credentials due to a major on-premises failure. What is the fallback and how is it activated?
**A:** Password hash synchronization can serve as a backup authentication method. Failover is not automatic — you must manually switch the sign-in method using Microsoft Entra Connect.

**Q:** Which sign-in scenarios require federated authentication instead of cloud authentication?
**A:** Requirements Entra ID doesn't support natively: smartcard/certificate authentication, on-premises or third-party MFA servers, third-party authentication solutions, or sign-in with sAMAccountName (DOMAIN\username).

**Q:** Why should you enable PHS even when using PTA or federation?
**A:** Three reasons: high availability/disaster recovery (cloud service scales globally), on-premises outage survival, and Identity Protection leaked-credentials detection (requires Entra ID P2).

### Microsoft Entra Connect Design

**Q:** What is sourceAnchor (also called immutableId)?
**A:** An attribute immutable during the lifetime of an object that uniquely identifies it as the same object on-premises and in Microsoft Entra ID; used for hard match and federation claims.

**Q:** What are the length rules for a sourceAnchor value?
**A:** Fewer than 60 characters; characters other than a-z, A-Z, or 0-9 are encoded and counted as three characters each.

**Q:** Which sourceAnchor attribute should you use in a single-forest environment?
**A:** objectGUID — also the attribute used by Express settings and DirSync.

**Q:** A user's UPN suffix (contoso.com) is not a verified domain in Microsoft Entra ID. What happens during sync?
**A:** Microsoft Entra Connect replaces the UPN suffix with the default tenant domain, e.g., contoso.onmicrosoft.com.

**Q:** What is a Microsoft Entra Connect server in staging mode?
**A:** A second server that reads data from all connected directories but writes nothing to them; it runs the normal sync cycle and serves as an updated standby.

**Q:** What is the maximum group size supported by Microsoft Entra Connect cloud sync, and which filter is recommended for large groups?
**A:** Groups with up to 50,000 members; use only the OU scoping filter when synchronizing large groups.

### Password Hash Synchronization (PHS)

**Q:** What exactly does PHS synchronize from on-premises AD?
**A:** A hash of the hash of the user's password — Entra Connect extracts the AD password hash and applies extra security processing before synchronizing it to Microsoft Entra ID.

**Q:** How often does the PHS process run, and can you change this?
**A:** Every 2 minutes; the frequency cannot be modified. Passwords sync more frequently than the standard directory synchronization window.

**Q:** Which Microsoft Entra Connect installation option enables PHS automatically?
**A:** Express Settings. With custom settings, PHS is available on the User Sign-In page.

**Q:** PHS fails on a FIPS-locked-down server. What is the cause and fix?
**A:** FIPS disables MD5. Fix: in miiserver.exe.config (under %programfiles%\Azure AD Sync\Bin), add `<enforceFIPSPolicy enabled="false"/>` under configuration/runtime.

### Pass-Through Authentication (PTA)

**Q:** How does PTA validate passwords?
**A:** A software agent on one or more on-premises servers validates passwords directly against on-premises Active Directory — password validation does not happen in the cloud.

**Q:** How do you enable pass-through authentication?
**A:** Through Microsoft Entra Connect: choose PTA on the User Sign-In page during custom installation, or use the "Change user sign-in" task on an existing installation.

**Q:** Is PTA a per-domain or tenant-level feature?
**A:** Tenant-level — enabling it affects sign-in for users across all managed domains in the tenant.

**Q:** When switching from AD FS to PTA, how long should you wait before shutting down AD FS, and why?
**A:** At least 12 hours, to ensure users can keep signing in to Exchange ActiveSync during the transition.

### Seamless Single Sign-On (SSO)

**Q:** Which sign-in methods can seamless SSO be combined with?
**A:** Password hash synchronization or pass-through authentication. Seamless SSO is not applicable to AD FS.

**Q:** In the seamless SSO browser flow, which account does the browser request a Kerberos ticket for?
**A:** The AZUREADSSOACC computer account, which represents Microsoft Entra ID; Entra ID decrypts the ticket using a previously shared key.

**Q:** How can seamless SSO be rolled out, and does it require extra on-premises components?
**A:** Rolled out to some or all users using Group Policy; no additional on-premises components are needed.

### Federation with AD FS

**Q:** What is federation?
**A:** A collection of domains that have established trust — typically including authentication and almost always authorization — for shared access to a set of resources.

**Q:** What is the minimum AD FS version supported for federation with Microsoft Entra Connect?
**A:** Windows Server 2012 R2 or later (new or existing farm).

**Q:** Where is the Web Application Proxy server deployed in an AD FS federation, and what is its role?
**A:** In the perimeter network facing the extranet; it supports authentication requests coming from the extranet.

**Q:** Which two service account types can the AD FS service use?
**A:** A group managed service account or a domain user account.

**Q:** What is device writeback used for?
**A:** Enabling device-based conditional access for AD FS-protected devices by synchronizing devices registered in Entra ID back to on-premises AD (e.g., creating the CN=RegisteredDevices container).

### Troubleshooting Synchronization Errors

**Q:** Where do you view sync error reports, and how often are they refreshed?
**A:** In the Azure portal as part of Microsoft Entra Connect Health for sync; reports update every 30 minutes with errors from the latest synchronization attempt.

**Q:** Hard match vs soft match in Microsoft Entra ID?
**A:** Hard match: incoming object's sourceAnchor matches a cloud object's immutableId. Soft match: fallback using ProxyAddresses and UserPrincipalName when no hard match is found (target must have no immutableId).

**Q:** What causes the InvalidSoftMatch sync error?
**A:** The hard match fails and the soft match finds an object whose immutableId differs from the incoming object's sourceAnchor — typically two objects sharing a ProxyAddresses or UPN value.

**Q:** What causes the ObjectTypeMismatch sync error?
**A:** Two objects of different object types (e.g., a user and a mail-enabled security group) share the same ProxyAddresses value, which Microsoft Entra ID does not allow.

**Q:** What causes the AttributeValueMustBeUnique sync error?
**A:** Entra Connect tries to add or update an object with a ProxyAddresses or UserPrincipalName value already assigned to another object in Microsoft Entra ID.

**Q:** Which attributes typically trigger the LargeObject error, and what is the certificate hard limit?
**A:** userCertificate, userSMIMECertificate, thumbnailPhoto, and proxyAddresses; the hard limit is 15 certificates.

**Q:** What causes the FederatedDomainChangeError sync error?
**A:** A synchronized user's UPN suffix is changed from one federated domain to another federated domain (e.g., bob@contoso.com to bob@fabrikam.com), so the UPN doesn't update.

**Q:** An on-premises user cannot soft-match a cloud object that has an admin role assigned. How do you fix it?
**A:** Remove the cloud account from all admin roles, hard-delete the quarantined object, let the next sync soft-match the users, then restore the role memberships.

### Microsoft Entra Connect Health

**Q:** Which license is required to use Microsoft Entra Connect Health?
**A:** Microsoft Entra ID Premium P1.

**Q:** Which role is required to register the Microsoft Entra Connect Health agent?
**A:** Global administrator in Microsoft Entra ID — by default, the global admin account has permissions to register the agent.

**Q:** Which firewall ports must the Health agent be able to use?
**A:** TCP 443 and TCP 5671. The latest agent version requires only port 443.

**Q:** Which Health agent is installed automatically?
**A:** The Microsoft Entra Connect Health agent for Sync — installed automatically with the latest version of Microsoft Entra Connect.

**Q:** Where should you NOT install the Health agent for AD FS?
**A:** On the Microsoft Entra Connect (Sync) server — the AD FS server must be a different machine with a unique host name.

### Managing Microsoft Entra Connect Health

**Q:** What happens when you delete a server from the Microsoft Entra Connect Health service?
**A:** Data collection and monitoring stop for that server; the agent is not uninstalled and collected data isn't deleted (Azure retention policy applies). To re-monitor, uninstall and reinstall the agent.

**Q:** Which built-in Azure RBAC roles does Microsoft Entra Connect Health support?
**A:** Owner (manage access, view all information, change settings), Contributor (view all information, change settings), Reader (view all information only). Other roles have no impact.

**Q:** Which RBAC role is required to use the Diagnose and Remediate feature for duplicated attribute sync errors?
**A:** Contributor permission from Azure RBAC.
