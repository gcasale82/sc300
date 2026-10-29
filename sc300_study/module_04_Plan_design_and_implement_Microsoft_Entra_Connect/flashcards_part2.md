# Module 04: Plan, design, and implement Microsoft Entra Connect - Flashcards (part 2)

<details><summary>Source lessons</summary>

- Manage Microsoft Entra Health
- Summary and resources

</details>

### Microsoft Entra Connect Health – Alerts and Email Notifications

**Q:** Are Microsoft Entra Connect Health email notifications enabled by default?
**A:** Yes. Email notifications are enabled by default and are sent when an alert is generated and when it's resolved.

**Q:** Where in the portal do you configure email notifications for a Connect Health service?
**A:** Open the Alerts dialog for the service, select Notification Settings in the action bar, set the switch to ON, then select Save.

**Q:** How can you make all global administrators receive Connect Health alert emails?
**A:** In Notification Settings, select the check box to have all global administrators receive email notifications.

**Q:** How do you add extra recipients to Connect Health alert emails?
**A:** Enter the addresses in the Additional Email Recipients box; to remove one, select the entry and select Delete. Save to apply.

**Q:** When do notification setting changes take effect in Connect Health?
**A:** Only after you select Save. Changes take effect only after saving.

**Q:** How does Connect Health report back-end sync processing errors by email?
**A:** Instead of one message per error, it sends a daily digest of all errors to the tenant's administrative contact email address(es).

---

### Microsoft Entra Connect Health – Deleting Servers and Service Instances

**Q:** Which license is required for the steps to delete a server or service instance from Connect Health?
**A:** A Microsoft Entra ID premium license is required for the deletion steps.

**Q:** What happens to monitoring when you delete a server from Connect Health?
**A:** Data collection stops and the server is removed from monitoring — no new alerts, monitoring, or usage analytics data for that server.

**Q:** Does deleting a server in Connect Health uninstall the Health Agent?
**A:** No. The agent is not uninstalled; if it remains installed, you may see Health Agent-related errors on the server.

**Q:** What happens to data already collected from a deleted server?
**A:** It is not deleted immediately; it is deleted in accordance with the Azure data retention policy.

**Q:** What must you do to start monitoring the same server again after deleting it from Connect Health?
**A:** Uninstall and reinstall the Health Agent on that server.

**Q:** What must you do to resume monitoring after deleting an entire service instance?
**A:** Uninstall and reinstall the Health Agent on all the servers that were part of that service instance.

**Q:** How do you delete a server monitored by Connect Health for AD FS or Entra Connect (Sync)?
**A:** Open the Server screen from the Server List, select Delete, type the server name in the confirmation box, then select Delete.

**Q:** How do you delete a domain controller monitored by Connect Health for Microsoft Entra Domain Services?
**A:** Open the Domain Controllers dashboard, select the DC, select Delete Selected, confirm, then select Delete.

**Q:** How do you delete a service instance from Connect Health?
**A:** Open the Service screen from the Service List by service identifier (farm name), select Delete, type the service name (e.g., sts.contoso.com), then select Delete.

---

### Microsoft Entra Connect Health – Access with Azure RBAC

**Q:** Which built-in Azure RBAC roles does Microsoft Entra Connect Health support?
**A:** Owner, Contributor, and Reader. Other roles (e.g., User Access Administrator, DevTest Labs Users) have no impact on access.

**Q:** You need a user to view alerts and change settings (e.g., email notifications) but NOT manage access. Which role?
**A:** Contributor — can view all information and change settings, but not manage access.

**Q:** What can an Owner do in Connect Health that a Contributor cannot?
**A:** Manage access — e.g., assign a role to a user or group. Owners can also view all information and change settings.

**Q:** What can a Reader do in Connect Health?
**A:** Only view all information (e.g., view alerts) from the portal.

**Q:** Who is assigned the Owner role in Connect Health by default, and can this be changed?
**A:** Microsoft Entra global administrators are Owners by default; this assignment can't be changed.

**Q:** What are the two access scopes for managing Connect Health access?
**A:** All service instances (recommended in most cases) and an individual service instance.

**Q:** A user has access at both the directory level and a service instance level. Is permission granted?
**A:** Yes. Permission is granted if the user has access at either the directory or the service instance level.

**Q:** Is the Invite Users feature supported within Microsoft Entra Connect Health?
**A:** No. The Invite Users feature isn't supported.

**Q:** You must grant a user Connect Health access across all monitored role types. What scope and path?
**A:** All service instances scope: open the main Connect Health screen, then Configure > Users > Add, select the role, and add the user or group.

---

### Diagnosing and Remediating Duplicated Attribute Sync Errors

**Q:** Which sync error names indicate duplicated attribute conflicts?
**A:** QuarantinedAttributeValueMustBeUnique and AttributeValueMustBeUnique.

**Q:** Which attribute conflicts are commonly seen with these sync errors?
**A:** UserPrincipalName or Proxy Addresses conflicts; conflicting objects are quarantined in Microsoft Entra ID.

**Q:** Which duplicated attributes does the Connect Health diagnose feature support?
**A:** UserPrincipalName and ProxyAddresses (both error types); SipProxyAddress and OnPremiseSecurityIdentifier (AttributeValueMustBeUnique only).

**Q:** Which Azure RBAC permission is required to use the diagnose feature for sync errors?
**A:** Contributor permission from Azure RBAC.

**Q:** Is any upgrade or configuration required to enable the Connect Health diagnosis feature?
**A:** No. No upgrade or configuration is required.

**Q:** What is the "orphaned object" scenario?
**A:** The on-premises source object was deleted but the deletion never synced to Microsoft Entra ID; the existing user loses the Source Anchor, so a restored/recreated object can't match it.

**Q:** A cloud-only user exists in Microsoft Entra ID and a newly synced duplicate appears. Why can't they be matched?
**A:** The objects have different Source Anchors and there is no direct way to remap the Source Anchor.

**Q:** What does the Diagnose status "Not Started" mean?
**A:** The diagnosis process hasn't been visited; there may be a potential way to fix the sync error directly from the portal.

**Q:** What does the Diagnose status "Manual Fix Required" mean?
**A:** The error doesn't fit portal fix criteria (e.g., conflicting objects aren't users) or diagnostics found no portal fix — on-premises fixes are still an option.

**Q:** What does the Diagnose status "Pending Sync" mean?
**A:** A fix was applied and the portal is waiting for the next sync cycle to clear the error.

**Q:** Which button applies the portal-side fix for an identified orphaned object case?
**A:** Apply Fix — the status changes to Pending sync and the error is removed after the next sync cycle.

**Q:** Diagnostics end with a Close button instead of a fix. What does this mean?
**A:** No quick fix is available from the portal; use the shown on-premises solution. The error status switches to Manual fix required.

**Q:** Two users both have Joe.J@contoso.com as UPN and are quarantined. How can you fix it?
**A:** Update the conflicting source object from the on-premises side; the sync error resolves after the next sync.

---

### Module Summary – Hybrid Identity with Microsoft Entra ID

**Q:** Which sign-in/authentication options does the module cover for Microsoft Entra Connect?
**A:** Password hash synchronization (PHS), pass-through authentication (PTA), Seamless single sign-on (Seamless SSO), and federation (excluding manual AD FS deployments).

**Q:** Beyond hybrid deployment, which management areas does the module cover?
**A:** Managing Microsoft Entra Connect, managing PHS/PTA/Seamless SSO/federation, troubleshooting synchronization errors, and implementing/managing Microsoft Entra Connect Health.
