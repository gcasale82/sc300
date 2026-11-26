

        MICROSOFT ENTRA ID ENTITLEMENT MANAGEMENT: CATALOG ROLES



1. CATALOG OWNER (Full Control)

   ├── Scope: Entire catalog, its resources, access packages, and role delegation.

   ├── Key Permissions:

   │   ├── Add or remove resources (Groups, Apps, SharePoint sites) in the catalog.

   │   ├── Delegate any of the 4 catalog roles to other users.

   │   ├── Create, edit, and delete access packages and policies.

   │   └── Manage user assignments, approvals, and access reviews.

   └── Exam Trigger: Required whenever a user needs to ADD NEW RESOURCES to a catalog.



2. ACCESS PACKAGE MANAGER (Package Builder)

   ├── Scope: Access packages and policies within a specific catalog.

   ├── Key Permissions:

   │   ├── Create, edit, and delete access packages using EXISTING catalog resources.

   │   ├── Create and modify assignment policies.

   │   └── Manage user assignments, approvals, and access reviews.

   ├── Restrictions: CANNOT add/remove resources from the catalog or delegate catalog roles.

   └── Exam Trigger: Required to build/manage packages without modifying catalog resources.



3. ACCESS PACKAGE ASSIGNMENT MANAGER (Assignment Operator)

   ├── Scope: User assignments and requests for existing access packages.

   ├── Key Permissions:

   │   ├── Manually assign or remove users from existing access packages.

   │   ├── Approve or deny access package requests.

   │   └── Manage access reviews for existing assignments.

   ├── Restrictions: CANNOT create/edit access packages, policies, or catalog resources.

   └── Exam Trigger: Least-privilege role for delegating day-to-day user onboarding/offboarding.



4. CATALOG READER (Auditor / Read-Only)

   ├── Scope: Read-only visibility across the catalog.

   ├── Key Permissions:

   │   └── View catalog resources, access packages, policies, assignments, and logs.

   ├── Restrictions: CANNOT create, modify, delete, or approve anything.

   └── Exam Trigger: Ideal for security auditors or compliance reviewers.





QUICK CHEAT SHEET (LEAST PRIVILEGE RULE)



* Add/Remove Resources to Catalog?  --> Catalog owner

* Create/Edit Access Packages?      --> Access package manager

* Assign/Remove Users to Packages?  --> Access package assignment manager

* View/Audit Only?                  --> Catalog reader
---


        MICROSOFT ENTRA ID: CREATING AN ACCESS PACKAGE (MIND MAP)





1. BASICS

   ├── Name & Description: Visible to users in the My Access portal.

   └── Catalog: Select the container holding the resources (Default: General).



2. RESOURCE ROLES (What the user gets)

   ├── Supported Resources:

   │   ├── Microsoft Entra Security Groups & M365 Groups (Roles: Member or Owner)

   │   ├── Microsoft Teams (Roles: Member or Owner)

   │   ├── Enterprise Applications (App-specific roles)

   │   └── SharePoint Online Sites (Site collection roles: Owner, Member, Visitor)

   └── Exam Note: Resources must be added to the Catalog FIRST (requires Catalog Owner).



3. REQUESTS (Who can request & Approval rules)

   ├── Who can request access?

   │   ├── For users in your directory:

   │   │   ├── Specific users and groups

   │   │   ├── All members (excluding guests)

   │   │   └── All users (including guests)

   │   ├── For users NOT in your directory (External/B2B):

   │   │   ├── Specific connected organizations

   │   │   ├── All configured connected organizations

   │   │   └── All users (any external directory / social identity)

   │   └── None (Administrator direct assignment only - no self-service requests)

   │

   ├── Request Settings:

   │   ├── Require requestor justification (Yes/No - mandatory text box for user)

   │   └── Enable new requests (Toggle to pause/resume requests without deleting policy)

   │

   └── Approval Settings:

       ├── Require approval? (Yes/No)

       ├── Require approver justification? (Yes/No - mandatory reason when approving/denying)

       ├── How many stages? (1, 2, or 3 sequential approval stages)

       ├── Approver Types:

       │   ├── Manager (Direct manager of requestor + optional fallback approver)

       │   └── Specific approvers (Internal users/groups, or External sponsors for guests)

       ├── Decision must be made in how many days? (Default: 14 days; auto-denies if expired)

       └── Alternate approvers: Escalate if primary approver doesn't respond in time.



4. REQUESTOR INFORMATION (Optional)

   └── Custom Questions: Add questions (Short text, Multiple choice, Long text) for requestors.



5. LIFECYCLE (Expiration, Extensions & Access Reviews)

   ├── Expiration (When does access end?):

   │   ├── On date: Hard cutoff on a specific calendar date.

   │   ├── Number of days: Relative duration (e.g., expires X days after assignment).

   │   └── Never: Permanent assignment (unless removed manually or via Access Review).

   │

   ├── Timeline Customization:

   │   └── Users can request specific timeline (Yes/No):

   │       ├── Allows user to pick custom Start and End dates during request.

   │       └── Exam Trap: Custom dates CANNOT exceed the maximum policy expiration!

   │

   ├── Extensions (Renewing access before expiration):

   │   ├── Allow users to extend access (Yes/No - user gets notified 14 days & 1 day prior).

   │   └── Require approval to grant extension (Yes/No):

   │       └── Exam Note: Uses the EXACT SAME approval stages/settings as initial access!

   │

   └── Access Reviews (Periodic recertification):

       ├── Require access review (Yes/No)

       ├── Review frequency: One-time, Weekly, Monthly, Quarterly, Semi-annually, Annually.

       ├── Reviewers: Self-review, Specific users/groups, or Manager.

       └── Action if reviewers don't respond: Remove access, Approve, or Take recommendations.



6. RULES & CUSTOM EXTENSIONS (Automation)

   ├── Attribute-Based Auto-Assignment Rules:

   │   └── Automatically assign/remove package based on user attributes (e.g., Department = Sales).

   │

   └── Custom Extensions (Azure Logic Apps integration):

       └── Trigger automated workflows at specific lifecycle events:

           ├── When request is created

           ├── When request is approved

           ├── When assignment is granted

           └── 14 days / 1 day before assignment expires / When assignment is removed





KEY SC-300 EXAM TRAPS TO REMEMBER



* Multiple Policies: One Access Package = One set of Resources, but can have MULTIPLE Policies (different requestor groups & approvers).

* Custom Timeline Limit: User-requested dates can shorten access, NEVER extend it past the policy limit.

* Extension Approval: You cannot define a *different* approver for extensions; it mirrors the initial request's approval workflow.

* Max Approval Stages: Up to 3 sequential stages.



        MICROSOFT ENTRA ID: TERMS OF USE (ToU) - MIND MAP & EXAM TRAPS





1. OVERVIEW & FILE REQUIREMENTS

   ├── Purpose: Present legal/compliance disclaimers to users before granting access.

   ├── File Format: MUST be uploaded as a PDF file (supports multi-language PDFs).

   ├── Display Name: Shown to users during sign-in AND used to select the ToU in Conditional Access.

   └── Enforcement Mechanism: A ToU does NOTHING on its own; it MUST be linked to a Conditional Access (CA) policy.



2. CORE CONFIGURATION SETTINGS

   ├── Require users to expand the terms of use (On/Off):

   │   └── Forces the user to open/expand the PDF document before the "Accept" button becomes active.

   │

   ├── Require users to consent on every device (On/Off):

   │   ├── Behavior: Forces the user to accept the ToU on EACH individual device they sign in from.

   │   └── [EXAM TRAP #1 - Device Registration Requirement]:

   │       ├── To track consent per device, the device MUST be registered in Microsoft Entra ID!

   │       ├── Therefore, enabling this setting AUTOMATICALLY triggers device registration upon sign-in.

   │       └── Crucial Rule: You CANNOT exclude the "Microsoft Azure Device Registration Service" (or Intune enrollment) in your Conditional Access policy, or users will get locked out in an infinite loop!

   │

   └── Expire consents (On/Off):

       ├── Purpose: Forces users to periodically re-accept the Terms of Use.

       ├── Expire starting on: Sets the exact calendar date when the first expiration cycle begins.

       └── Frequency: How often the consent expires AFTER the starting date (Monthly, Quarterly, Semi-annually, Annually).

       └── [EXAM TRAP #2 - Expiration Calculation]:

           ├── Expiration is NOT calculated as "X days after the user accepted it."

           └── It is a fixed schedule anchored to the "Expire starting on" date + "Frequency" (e.g., if starting on Jan 1 with Monthly frequency, everyone's consent expires on the 1st of every month regardless of when they first accepted).



3. CONDITIONAL ACCESS (CA) POLICY ENFORCEMENT

   ├── Navigation: Protection > Conditional Access > Policies > Grant.

   ├── Grant Control Configuration:

   │   ├── Select "Grant access" -> Check the box for the specific [Terms of Use Name].

   │   └── [EXAM TRAP #3 - Multiple Controls Logic]:

   │       ├── If you select BOTH "Require multifactor authentication" AND "Terms of Use" in the same CA policy:

   │       └── Ensure "Require ALL the selected controls" is selected (if "Require ONE..." is selected, users can bypass the ToU simply by completing MFA!).

   │

   └── Targeting & Exclusions:

       ├── Can target specific Users/Groups, Cloud Apps, or User Actions.

       └── Remember: Leave "Microsoft Azure Device Registration Service" unblocked if "Consent on every device" is On.



4. EDITING & UPDATING AN EXISTING ToU

   ├── What can be edited directly?

   │   └── Display name, Language/PDF upload, "Require expand", "Expire consents" settings.

   │

   └── [EXAM TRAP #4 - Updating the PDF & Requiring Re-acceptance]:

       ├── Scenario: You upload a new version of the PDF document to an existing ToU.

       ├── Default Behavior: Uploading a new PDF does NOT automatically force users who already accepted the old version to re-accept!

       └── Solution to force immediate re-acceptance:

           └── When uploading the updated PDF, you MUST explicitly check the box:

               "Require users to re-accept the updated terms of use" (or "Require re-accept").





QUICK SC-300 EXAM CHECKLIST FOR TERMS OF USE



* File type?                     --> PDF only.

* How is it enforced?            --> Conditional Access Policy -> Grant -> Select ToU.

* Consent on every device = On?  --> Requires Device Registration (do NOT block it in CA!).

* MFA + ToU in same CA policy?   --> Must use "Require ALL selected controls".

* Expiration logic?              --> Anchored to "Expire starting on" + "Frequency" (not user sign-in date).

* Updated PDF + force re-sign?   --> Must select "Require users to re-accept" during upload.




  ENTITLEMENT MANAGEMENT: CONNECTED ORGS, REVIEWS & LIFECYCLE SETTINGS





1. CONNECTED ORGANIZATIONS (Identity Governance > Connected organizations)

   ├── Purpose: Define external partner directories/domains to collaborate with via Access Packages.

   ├── States:

   │   ├── Configured: Explicitly added partner organization (can be targeted in policies).

   │   └── Proposed: Automatically created when an external user from an unconfigured domain requests an Access Package (if policy allows "All users").

   ├── Identity Sources Supported:

   │   └── Microsoft Entra ID tenants, Direct Federation (SAML/WS-Fed), or Verified Domains.

   ├── Sponsors (Internal & External):

   │   ├── Internal Sponsors: Users in YOUR tenant who own the relationship with the partner.

   │   ├── External Sponsors: Users from the PARTNER tenant who vouch for/approve their own users.

   │   └── Exam Tip: Sponsors can be assigned as Approvers or Access Reviewers for external guests!

   └── Cross-Tenant Access Integration:

       └── Connected Organizations define WHO can request packages; Cross-Tenant Access Settings enforce the technical trust/block at the directory level.



2. ACCESS PACKAGE ACCESS REVIEWS (Periodic Recertification)

   ├── Purpose: Ensure users still require access to an Access Package over time.

   ├── Configuration Location: Configured inside the Access Package's Policy -> Lifecycle tab.

   ├── Key Settings:

   │   ├── Review Frequency: One-time, Weekly, Monthly, Quarterly, Semi-annually, Annually.

   │   ├── Duration: How many days the review remains open for reviewers to decide.

   │   ├── Reviewers:

   │   │   ├── Self-review (Users attest to their own need for access).

   │   │   ├── Specific reviewers (Designated individuals or groups).

   │   │   └── Manager (Direct manager of the assigned user + fallback reviewer).

   │   └── Auto-Apply Results / If Reviewers Don't Respond:

   │       ├── Remove access (Revokes assignment if denied or no response -> least privilege).

   │       ├── Approve access (Keeps assignment).

   │       └── Take recommendations (Uses Entra ID sign-in activity insights to decide).

   └── Exam Trap: Removing a user's assignment via an Access Review triggers the "Lifecycle of external users" rules if it was their LAST assignment!



3. LIFECYCLE OF EXTERNAL USERS (Entitlement Management > Control configurations)

   ├── Trigger Condition:

   │   └── Applies ONLY when an external (B2B Guest) user, who was invited/added through an Access Package request, loses their LAST active assignment to ANY Access Package.

   ├── Setting 1: Block external user from signing in to directory (Yes/No)

   │   └── Immediately disables the guest account (`accountEnabled = false`) once their last assignment ends, preventing login while keeping the user object temporarily.

   ├── Setting 2: Remove external user (Yes/No)

   │   └── Automatically deletes the guest user object from Microsoft Entra ID (moves to Deleted Users for 30 days before permanent deletion).

   ├── Setting 3: Number of days before removing external user from directory

   │   └── Grace period (e.g., 30 days) after losing the last assignment before the guest account is deleted (gives time to request a new package without recreating the object).

   └── Exam Trap:

       └── This ONLY affects guest users brought in via Entitlement Management (Access Packages). Guests invited manually via Entra ID Users blade are NOT automatically deleted by this setting!



4. MY ACCESS SETTINGS FOR END USERS (Entitlement Management > Control configurations)

   ├── Portal Purpose: End-user web portal (`myaccess.microsoft.com`) to request packages, approve requests, and complete access reviews.

   ├── Setting 1: Show insights for suggested access packages in My Access (Yes/No)

   │   └── Displays AI/peer-based recommendations to users based on what packages people with the same manager or group memberships have.

   ├── Setting 2: View access package assignments for direct reports (Yes/No)

   │   └── Allows managers to view and manage (add/remove) Access Package assignments for their direct reports directly inside the My Access portal.

   ├── Setting 3: Show approver details to members on pending access package requests (Yes/No)

   │   └── Privacy/Transparency toggle: Determines whether internal requestors ("members") can see the names/details of who is currently pending approval on their request.

   └── Setting 4: Restrict who users can delegate approvals and access reviews to (Preview)

       └── Allows admins to limit delegation options (e.g., preventing users from delegating their approval/review duties to unauthorized individuals or external guests).





QUICK SC-300 EXAM CHECKLIST



* Guest loses LAST access package?       --> Blocked from sign-in -> Deleted after X days (Control configurations).

* Manually invited guest loses package?  --> NOT auto-deleted by Entitlement Management lifecycle settings.

* Unknown partner requests a package?    --> Creates a "Proposed" Connected Organization.

* Manager wants to see team's packages?  --> Enable "View access package assignments for direct reports" in My Access settings.

* Hide who is approving a request?       --> Disable "Show approver details to members on pending access package requests".


