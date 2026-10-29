# Module 03: Describe guest access and Business to Business accounts - Flashcards

<details><summary>Source lessons</summary>

- Introduction
- Describe guest access and Business to Business accounts
- Manage external collaboration
- Exercise - configure external collaboration
- Invite external users - individually and in bulk
- Exercise - add guest users to directory
- Exercise - invite guest users bulk
- Demo - manage guest users in Microsoft Entra ID
- Manage external user accounts in Microsoft Entra ID
- Manage external users in Microsoft 365 workloads
- Exercise - explore dynamic groups
- Implement and manage Microsoft Entra Verified ID
- Configure identity providers
- Implement cross-tenant access controls
- Summary and resources

</details>

### B2B collaboration and guest access

**Q:** What is Microsoft Entra B2B collaboration?
**A:** A feature of Microsoft Entra External Identities that lets you invite external users (guests) to access your apps and resources with their own credentials, while you keep control of corporate data.

**Q:** How can you identify a B2B collaboration user object in the directory?
**A:** UserType is typically "Guest" and the user principal name contains the #EXT# extension.

### External collaboration settings

**Q:** Who can invite guest users by default?
**A:** All users in the tenant, including guests, can invite guest users — even without an admin role.

**Q:** Which role can be assigned to delegate guest invitations to non-admin users?
**A:** The Guest Inviter role.

**Q:** What can guest users see in the directory by default?
**A:** They cannot list users, groups, or other directory resources, but they can see membership of non-hidden groups.

**Q:** Which guest user access setting is the most restrictive?
**A:** "Guest user access is restricted to properties and memberships of their own directory objects" — guests can only view their own profile.

**Q:** What is the portal path to configure external collaboration settings?
**A:** Microsoft Entra admin center > Identity > External Identities > External collaboration settings.

**Q:** Can you create both an allowlist and a blocklist for collaboration restrictions?
**A:** No — you can create only one type of list, and only one policy per organization.

**Q:** What is the maximum size of the collaboration restrictions policy?
**A:** 25 KB (25,000 characters), including the allow/blocklist and any other configured parameters.

### Inviting external users

**Q:** Do B2B invitations expire?
**A:** No — an invitation of a user does not expire.

**Q:** What must a guest user do after being invited to access resources?
**A:** Redeem the invitation, via the redemption URL in the invitation email or a direct link to the shared app.

**Q:** How do you invite many guest users at once?
**A:** Identity > Users > All users > Bulk operations > Bulk invite; download the CSV template, fill it in, upload, then verify.

**Q:** Which rows of the bulk invite CSV template must never be removed or modified?
**A:** The first two rows: the version number row and the column headings row.

**Q:** What must you do with the examples row in the bulk CSV template?
**A:** Remove it and replace it with your own entries.

**Q:** Which email addresses are not supported for guest invitations?
**A:** Group email addresses; plus (+) addressing is also not supported — omit the + and any text after it up to the @.

**Q:** What are the required values in the bulk invite CSV?
**A:** Email address to invite [inviteeEmail] and the redirection URL the user is forwarded to after accepting the invitation.

### Managing external user accounts

**Q:** What does the UserType property indicate?
**A:** The user's relationship to the host tenant: Member (employee) or Guest (external collaborator). It has no relation to sign-in method or directory role.

**Q:** Can a B2B collaboration user have UserType = Member?
**A:** Yes — change it in the Microsoft Entra user properties, e.g., when the partner belongs to a larger parent organization.

**Q:** What does an Identities value of "mail" mean for a guest user?
**A:** The user signed up using Microsoft Entra Email one-time passcode (OTP).

**Q:** What does an Identities value of google.com or facebook.com mean?
**A:** The user signed up via self-service with a Gmail or Facebook account.

**Q:** What is recommended for granting B2B/guest users privileged roles?
**A:** Follow least privilege and use Privileged Identity Management (PIM) to grant access.

### Microsoft 365 external collaboration

**Q:** Which Microsoft 365 external collaboration activity is disabled by default?
**A:** Shared channel in Teams (it uses an existing Microsoft 365 external account).

**Q:** Which Microsoft 365 external collaboration settings are enabled by default?
**A:** Authenticated file/folder sharing, site sharing, team sharing, external chat and meetings, anonymous meeting join, and unauthenticated file/folder sharing.

### Dynamic groups

**Q:** Which license is required to create and use dynamic groups?
**A:** Microsoft Entra ID Premium P1 or P2.

**Q:** Which dynamic membership rule includes all users, both members and guests?
**A:** user.objectId -ne null (Membership type: Dynamic User).

### Microsoft Entra Verified ID

**Q:** What is Microsoft Entra Verified ID?
**A:** A decentralized identity service to issue and verify verifiable credentials; verifiers get a free REST API to request and accept credentials in apps.

**Q:** What are the prerequisites to deploy Microsoft Entra Verified ID?
**A:** An Azure tenant with a subscription, a Microsoft Entra ID premium license, signed in as global administrator, and a configured Azure Key Vault.

**Q:** What is the purpose of the Domain setting in Verified ID setup?
**A:** It binds your DID to your domain; wallets use it to validate the DID and display a verified symbol.

### Identity providers

**Q:** What is the current name for direct federation?
**A:** SAML/WS-Fed identity provider (IdP) federation.

**Q:** Which protocols must a partner IdP support for SAML/WS-Fed IdP federation?
**A:** SAML 2.0 or WS-Federation (WS-Fed).

**Q:** Does setting up SAML/WS-Fed federation change authentication for guests who already redeemed invitations?
**A:** No — they keep their previous authentication method; only new guest users use the federated IdP.

**Q:** What happens to guest users if you delete a SAML/WS-Fed federation?
**A:** They can no longer sign in; you can update their authentication method by resetting their redemption status.

**Q:** Which NameID format is required in the SAML 2.0 token?
**A:** urn:oasis:names:tc:SAML:2.0:nameid-format:persistent

**Q:** Which WS-Fed identity providers have been tested for compatibility with Microsoft Entra ID?
**A:** AD FS and Shibboleth.

**Q:** Google federation is designed for which users, and what should you use for G Suite domains?
**A:** Gmail users (gmail.com/googlemail.com); for G Suite domains use SAML/WS-Fed (direct) federation.

**Q:** A Google guest user gets a "header too long" error. What is the fix?
**A:** Clear cookies or open a private/incognito window and try to sign in again.

**Q:** Can users redeem invitations with Facebook accounts?
**A:** No — Facebook works only for self-service sign-up user flows, not for invitation redemption.

**Q:** What must be enabled before adding Facebook as an identity provider?
**A:** Self-service sign-up for the tenant; then set up a user flow and select Facebook as a sign-in option.

**Q:** What happens if you delete Google federation?
**A:** Google guests who already redeemed their invitation cannot sign in; restore access by deleting them from the directory and reinviting them.

### Cross-tenant access controls

**Q:** What are the default cross-tenant access settings?
**A:** B2B collaboration with other Microsoft Entra organizations is enabled; B2B direct connect is blocked.

**Q:** Inbound vs outbound access settings — what does each control?
**A:** Inbound controls external users accessing your resources; outbound controls your users accessing external organizations' resources.

**Q:** What do inbound trust settings allow?
**A:** Your Conditional Access policies can trust the external organization's MFA, compliant device, or hybrid Microsoft Entra joined device.

**Q:** What is B2B direct connect and where does it work today?
**A:** A mutual trust between two Microsoft Entra organizations for seamless SSO; currently works with Teams shared channels.

**Q:** What is required to establish B2B direct connect?
**A:** Both organizations must mutually enable B2B direct connect in their cross-tenant access settings.

**Q:** Which settings connect your tenant to Microsoft Azure Government or Microsoft Azure China?
**A:** Microsoft cloud settings in cross-tenant access settings.
