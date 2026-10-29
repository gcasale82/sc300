# Module 04: Plan, design, and implement Microsoft Entra Connect - Flashcards (part 1)

<details><summary>Source lessons</summary>

- Introduction
- Plan, design, and implement Microsoft Entra Connect
- Implement manage password hash synchronization (PHS)
- Implement manage pass-through authentication (PTA)
- Explore pass-through authentication and seamless single sign-on (SSO)
- Implement and manage federation
- Trouble-shoot synchronization errors
- Implement Microsoft Entra Connect Health

</details>

### Introduction to Hybrid Identity

**Q:** What is hybrid identity?
**A:** An identity solution spanning on-premises and cloud environments, providing unified authentication and authorization to resources regardless of location.

**Q:** You need to implement a hybrid identity solution. Which two tools are central to this?
**A:** Microsoft Entra ID and Microsoft Entra Connect.

**Q:** Which synchronization-related tasks are covered in the hybrid identity module?
**A:** Plan/design/implement Microsoft Entra Connect, manage PHS and PTA, seamless SSO, federation (excluding manual AD FS deployments), troubleshoot sync errors, and implement Microsoft Entra Connect Health.

### Microsoft Entra Connect: Planning and Design

**Q:** What are the five capabilities of Microsoft Entra Connect?
**A:** Synchronization, password hash synchronization (PHS), pass-through authentication (PTA), federation integration, and health monitoring.

**Q:** Which two sign-in methods fall under "cloud authentication" in a hybrid identity design?
**A:** Password hash synchronization (PHS) and pass-through authentication (PTA).

**Q:** How often does the password hash synchronization process run as part of the Microsoft Entra Connect sync process?
**A:** Every 2 minutes (frequency cannot be modified).

**Q:** You need to enforce on-premises account states, password policies, and sign-in hours immediately at sign-in time. Which authentication method should you use?
**A:** Pass-through authentication (PTA).

**Q:** How many PTA agents are recommended for high availability?
**A:** Three agents (one on the Microsoft Entra Connect server plus two extra).

**Q:** Can PTA agents be deployed in a perimeter network?
**A:** No — agents need access to on-premises domain controllers and outbound internet access, so perimeter network deployment is not supported.

**Q:** What is the backup authentication method for PTA when agents can't validate credentials, and is failover automatic?
**A:** Password hash synchronization; failover is NOT automatic — you must manually switch the sign-in method using Microsoft Entra Connect.

**Q:** Which authentication method is required for smartcard authentication, third-party MFA servers, or sign-in using sAMAccountName (DOMAIN\username)?
**A:** Federated authentication (e.g., AD FS).

**Q:** Why should you enable password hash synchronization regardless of the chosen authentication method?
**A:** For high availability/disaster recovery, on-premises outage survival, and Identity Protection (leaked credentials detection requires Entra ID Premium P2).

**Q:** What is the recommended way to protect against Microsoft Entra Connect server failure?
**A:** Deploy a second Microsoft Entra Connect server in staging mode as a standby.

**Q:** What does a Microsoft Entra Connect server in staging mode do?
**A:** It reads data from all connected directories and runs the normal sync cycle, but writes nothing to connected directories.

**Q:** What is the sourceAnchor attribute also called, and what is its definition?
**A:** immutableId — an attribute immutable during the lifetime of an object that uniquely identifies an object as the same on-premises and in Microsoft Entra ID.

**Q:** Which attribute should be used as sourceAnchor for a single forest, and which is used with express settings?
**A:** objectGuid.

**Q:** What are the length and character rules for the sourceAnchor value?
**A:** Fewer than 60 characters; special characters are not allowed; must be globally unique; string, integer, or binary; not case-sensitive; shouldn't be based on the user's name.

**Q:** Which attribute does Microsoft Entra ID use to authenticate users, and what must its suffix match?
**A:** userPrincipalName (UPN); the suffix must match a verified custom domain in Microsoft Entra ID.

**Q:** The UPN suffix contoso.com is not a verified domain in Microsoft Entra ID. What happens when users sync?
**A:** The tool replaces the UPN suffix with the default domain, e.g., contoso.onmicrosoft.com.

**Q:** You run a non-routable domain like contoso.local. Which installation type does Microsoft Entra Connect suggest, and why?
**A:** Custom settings — so you can specify the attribute to use as UPN for sign-in to Azure.

**Q:** What is the most common supported topology for Microsoft Entra Connect?
**A:** Single on-premises forest (one or multiple domains) with a single Microsoft Entra tenant — the only topology supported by express installation.

**Q:** How many Microsoft Entra Connect sync servers are needed per Microsoft Entra tenant?
**A:** One — there is a 1:1 relationship between a sync server and a tenant.

**Q:** What are the four staging areas/components Microsoft Entra Connect uses for synchronization?
**A:** Connector Space (CS), Metaverse (MV), sync rules, and run profiles.

**Q:** How many Metaverses (MV) exist in a Microsoft Entra Connect deployment?
**A:** Only one.

**Q:** What is Microsoft Entra Connect cloud sync, and what agent does it use?
**A:** A synchronization solution for users, groups, and contacts to Microsoft Entra ID using the cloud provisioning agent, with sync configuration managed in the cloud.

**Q:** What is the maximum group member size supported by Microsoft Entra Connect cloud sync?
**A:** 50,000 members (use only the OU scoping filter for large groups).

**Q:** Which scenario is cloud sync designed for regarding disconnected forests?
**A:** Synchronizing to a Microsoft Entra tenant from multi-forest disconnected AD environments, e.g., mergers and acquisitions.

### Password Hash Synchronization (PHS)

**Q:** What exactly does Microsoft Entra Connect synchronize for PHS?
**A:** A hash of the hash of the user's on-premises AD password to Microsoft Entra ID.

**Q:** Can you revert a password hash to plain text?
**A:** No — a hash is the result of a one-way mathematical function and cannot be reverted.

**Q:** During the first PHS enablement, can you define a subset of users to synchronize?
**A:** No — the initial sync covers all in-scope users; selective password hash sync can be set up only after initial synchronization.

**Q:** Which installation option automatically enables PHS?
**A:** Express Settings during Microsoft Entra Connect installation.

**Q:** On a FIPS-locked-down server, how do you enable MD5 for PHS?
**A:** Edit miiserver.exe.config in %programfiles%\Azure AD Sync\Bin and add `<enforceFIPSPolicy enabled="false"/>` under the runtime node.

**Q:** Which federation product other than AD FS can be configured with Microsoft Entra Connect, and what is its minimum version?
**A:** PingFederate, version 8.4 or later.

### Pass-Through Authentication (PTA)

**Q:** How does PTA validate user passwords?
**A:** A software agent on on-premises servers validates passwords directly against on-premises Active Directory — validation doesn't happen in the cloud.

**Q:** Through which tool do you enable pass-through authentication?
**A:** Microsoft Entra Connect (custom installation path, or the "Change user sign-in" task on an existing install).

**Q:** Is PTA a tenant-level or per-user feature?
**A:** Tenant-level — enabling it affects sign-in for users across all managed domains in the tenant.

**Q:** When switching from AD FS to PTA, how long should you wait before shutting down the AD FS infrastructure?
**A:** At least 12 hours, to ensure users can keep signing in to Exchange ActiveSync during the transition.

### Seamless Single Sign-On (Seamless SSO)

**Q:** Which sign-in methods can be combined with seamless SSO, and which cannot?
**A:** Works with PHS and PTA; NOT applicable to Active Directory Federation Services (ADFS).

**Q:** Which computer account in Active Directory represents Microsoft Entra ID for seamless SSO Kerberos tickets?
**A:** AZUREADSSOACC.

**Q:** In browser-based seamless SSO, what HTTP response does Microsoft Entra ID use to challenge the browser for a Kerberos ticket?
**A:** A 401 Unauthorized response, via JavaScript in the background.

**Q:** In seamless SSO, what does Microsoft Entra ID use to decrypt the Kerberos ticket?
**A:** The previously shared key (the AZUREADSSOACC computer account's secret).

**Q:** What token does Microsoft Entra ID issue to a native client app after validating the Kerberos ticket in seamless SSO?
**A:** A SAML token, which the app submits to the Microsoft Entra ID OAuth2 token endpoint to receive access, refresh, and ID tokens.

### Federation

**Q:** What is the minimum Windows Server version for an AD FS farm used with Microsoft Entra Connect federation?
**A:** Windows Server 2012 R2 or later.

**Q:** Where is the Web Application Proxy (WAP) server deployed in a federation topology?
**A:** In the perimeter network, facing the extranet, to support authentication requests from the extranet.

**Q:** Do AD FS servers need to be domain-joined before federation setup? What about WAP servers?
**A:** AD FS servers must be joined to Active Directory; domain join is not required for Web Application Proxy servers.

**Q:** Which two types of service accounts can the AD FS service use?
**A:** Group managed service account or domain user account.

**Q:** How many domains can you federate during the initial Microsoft Entra Connect installation?
**A:** Only a single domain; more domains can be added later by running Microsoft Entra Connect again.

**Q:** What is AlternateID sign-in used for in federation?
**A:** Allowing users to sign in with an attribute other than UPN (e.g., mail) when the on-premises UPN uses a non-routable domain or can't be changed.

**Q:** What is device writeback used for?
**A:** To enable device-based conditional access for AD FS-protected devices by synchronizing devices registered in Azure back to on-premises Active Directory.

### Troubleshooting Synchronization Errors

**Q:** Where is the Synchronization Errors report available?
**A:** In the Azure portal as part of Microsoft Entra Connect Health for sync.

**Q:** What are the three operation types Microsoft Entra Connect performs on directories?
**A:** Import, Synchronization, and Export.

**Q:** What is a "Hard Match" in Microsoft Entra ID?
**A:** Matching an incoming object's sourceAnchor attribute to the immutableId attribute of objects in Microsoft Entra ID.

**Q:** What is a "Soft Match" and which attributes does it use?
**A:** A fallback match when hard match fails, using ProxyAddresses and UserPrincipalName attributes to match objects already present in Microsoft Entra ID.

**Q:** What causes an InvalidSoftMatch error?
**A:** Hard match finds no object, but soft match finds an object that has a different immutableId value than the incoming object's sourceAnchor.

**Q:** Which attributes must have unique values in the Microsoft Entra directory schema?
**A:** ProxyAddresses, UserPrincipalName, onPremisesSecurityIdentifier, and ObjectId.

**Q:** What does the Microsoft Entra Attribute Duplicate Attribute Resiliency feature do?
**A:** It reduces sync errors by handling duplicated ProxyAddresses and UserPrincipalName more resiliently, allowing new objects to be provisioned — but it doesn't fix the duplicate data itself.

**Q:** What causes an ObjectTypeMismatch error?
**A:** Two objects of different types (User, Group, Contact) have the same value for attributes used in soft match, e.g., the same ProxyAddresses value.

**Q:** What causes an AttributeValueMustBeUnique error?
**A:** Microsoft Entra Connect tries to add/update an object with a ProxyAddresses or UserPrincipalName value already assigned to another object in Microsoft Entra ID.

**Q:** What causes an IdentityDataValidationFailed error?
**A:** The UserPrincipalName attribute contains invalid/unsupported characters or doesn't follow the required format.

**Q:** What causes a FederatedDomainChangeError?
**A:** A synchronized user's UPN suffix is changed from one federated domain to another federated domain (e.g., bob@contoso.com to bob@fabrikam.com).

**Q:** Which attributes typically trigger a LargeObject (ExceededAllowedLength) sync error?
**A:** userCertificate, userSMIMECertificate, thumbnailPhoto, and proxyAddresses.

**Q:** What is the hard limit for certificates in the userCertificate and userSMIMECertificate attributes?
**A:** 15 certificates.

**Q:** What causes an "Existing Admin Role Conflict" sync error?
**A:** An on-premises user object with administrative permissions and the same UPN as an existing Microsoft Entra object — soft-matching is not allowed with cloud objects holding admin roles.

**Q:** How do you fix an Existing Admin Role Conflict?
**A:** Remove the cloud account from all admin roles, hard delete the quarantined cloud object, let the next sync cycle soft-match, then restore the role memberships.

**Q:** How often are sync error reports in Microsoft Entra Connect Health for sync updated?
**A:** Every 30 minutes, including errors from the latest synchronization attempt.

### Microsoft Entra Connect Health

**Q:** What does Microsoft Entra Connect Health provide?
**A:** Monitoring of on-premises identity infrastructure — alerts, performance monitoring, and usage analytics via the Microsoft Entra Connect Health portal.

**Q:** Which license is required to use Microsoft Entra Connect Health?
**A:** Microsoft Entra ID Premium P1.

**Q:** Which administrative role is required to install and configure the Microsoft Entra Connect Health agent?
**A:** Global administrator in Microsoft Entra ID.

**Q:** Which firewall ports must be open for the Microsoft Entra Connect Health agent?
**A:** TCP 443 and TCP 5671 — the latest agent version requires only port 443.

**Q:** Which PowerShell version is required for the Health agent, and what must be disabled (FIPS)?
**A:** PowerShell 4.0 or newer; FIPS (Federal Information Processing Standard) must be disabled.

**Q:** Should the AD FS Health agent be installed on the Sync server?
**A:** No — the AD FS server should be different from the Sync server; don't install the AD FS agent on the Sync server.

**Q:** How is the Microsoft Entra Connect Health agent for Sync installed?
**A:** It is installed automatically with the latest version of Microsoft Entra Connect.

**Q:** Which services verify the Health agent installation for AD FS and for Sync?
**A:** AD FS: Diagnostics, Insights, and Monitoring Services. Sync: Sync Insights Service and Sync Monitoring Service.
