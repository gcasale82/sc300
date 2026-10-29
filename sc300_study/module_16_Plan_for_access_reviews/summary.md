# Module 16: Plan for access reviews - Summary

<details><summary>Source lessons</summary>

- Introduction
- Plan for access reviews
- Create access reviews for groups and apps
- Create and configure access reviews programmatically
- Monitor access review findings
- Automate access review management tasks
- Configure recurring access reviews
- Explore the Access Review Agent in Microsoft Entra
- Summary and resources

</details>

```text
ACCESS REVIEWS · MICROSOFT ENTRA ID GOVERNANCE (SC-300)
│
├── 🎯 FOUNDATIONS & LICENSING
│   ├── Purpose ────────────> Periodic recertification of group, app & role access;
│   │                        denied users removed automatically, no manual chase
│   ├── Full licensing ─────> Microsoft Entra ID Governance or Microsoft Entra Suite
│   │                        ⚠️ Entra ID P2 alone unlocks only SOME capabilities
│   ├── P2 per user ────────> Required for each reviewer, self-reviewer, group or
│   │                        app owner performing a review
│   │                        ⭐ Global Admin / User Admin (setup or apply) need NO license
│   └── Governance goals ───> Right people, right access, right resources; auditable
│
├── 🏢 PLANNING & ADMIN ROLES
│   ├── Stakeholders ───────> IT, Security, Development, Business units, Governance
│   ├── Pilot ──────────────> Small scope, noncritical resources, auto-apply OFF,
│   │                        document removals, valid emails, watch audit logs
│   ├── Reviewable targets ─> Groups, SSO apps, Access Packages, Entra + Azure roles
│   │                        (PIM), custom data resources (preview)
│   ├── Create grp/app ─────> Global Admin, User Admin, Identity Governance Admin,
│   │                        Privileged Role Admin (Entra assignable groups only)
│   ├── Create Entra roles ─> Global Admin, Privileged Role Administrator
│   ├── Create Azure roles ─> Global Admin, User Access Administrator, Resource Owner
│   ├── Read results ───────> Global Reader, User Admin, IG Admin, Security Reader,
│   │                        Privileged Role Admin, group owner
│   ├── Reviewer choice ────> ⚠️ Fixed at creation — cannot change once review starts
│   ├── Personas ───────────> Resource owners, selected delegates, self-attest users
│   ├── Group owners ───────> ⭐ Best reviewers; Teams creator auto-becomes owner;
│   │                        synced groups can't have Entra owners → pick people
│   ├── On-prem synced ─────> ⚠️ Review possible but membership NOT changed in cloud
│   │                        (source of authority on-prem; results via CSV/Graph)
│   ├── CA exclusions ──────> Review membership of CA exception groups (e.g. sales)
│   ├── External users ─────> Dynamic groups by attributes; internal sponsor reviews
│   └── Communications ─────> Custom reviewer email; reminders mid-way or 1 day prior
│
├── ⚙️ CREATE A REVIEW (PORTAL)
│   ├── Portal path ────────> Entra admin center > ID Governance > Access reviews >
│   │                        New access review
│   ├── Required role ──────> Identity Governance Administrator or Global Administrator
│   ├── What to review ─────> Teams + Groups (finite set OR all M365 groups w/ guests)
│   │                        OR Applications ⚠️ apps have no owner-reviewer option
│   ├── Scope ──────────────> Guest users only OR Everyone
│   │                        ⚠️ "All M365 groups w/ guests" forces Guest users only
│   ├── Inactive users ─────> Group reviews can target inactivity up to 730 days
│   ├── Reviewer types ─────> Group owner(s), selected users/groups, self-review,
│   │                        managers of users
│   └── Fallback reviewer ──> Covers users with no manager / groups with no owner
│
├── 🤖 AUTOMATION & COMPLETION SETTINGS
│   ├── Recurrence ─────────> Weekly / Monthly / Quarterly / Semi-annually / Annually
│   │                        ⚠️ Monthly review: max duration 27 days (no overlap)
│   ├── Auto-apply results ─> Enable = denied users removed automatically at the end
│   ├── If no response ─────> No change / Remove access / Approve access /
│   │                        Take recommendations (applied to non-responders)
│   ├── Denied guests ──────> Remove from resource only OR block sign-in 30 days
│   │                        then delete (admin can re-enable within the 30 days)
│   ├── Guest action rule ──> ⚠️ Not configurable on Everyone-scope reviews (default:
│   │                        remove membership); also fixed for all-M365-guest reviews
│   ├── Guest reviews ──────> Per-partner reviews for compliance; guests get access
│   │                        via groups, Teams, apps/packages, or privileged roles
│   ├── Decision helpers ───> Show recommendations: last sign-in + peer outlier
│   └── Advanced ───────────> Justification required, email notifications, reminders,
│                            additional content for reviewer email
│
├── 📊 PERFORM & MONITOR (REVIEWER SIDE)
│   ├── Portal ─────────────> https://myaccess.microsoft.com > Access reviews
│   ├── Decisions ──────────> Approve / Deny / "Don't know" (keeps access, audited)
│   ├── Start email ────────> Sent by default at review start; unaccepted guest
│   │                        invites receive NO email
│   ├── Multi-reviewer ─────> ⚠️ The LAST submitted decision is the one recorded
│   ├── Recommendations ────> Deny if no sign-in for 30 days OR peer outlier in the
│   │                        reporting structure; accept one-by-one or in bulk
│   ├── Removal timing ─────> Denied users removed at period end or when admin stops
│   │                        the review (if Auto-apply is enabled)
│   ├── Status flow ────────> NotStarted > Initializing > Starting > InProgress >
│   │                        Completing > Auto-Reviewing > Auto-Reviewed > Applying >
│   │                        Applied
│   ├── Failed status ──────> ⚠️ Tenant deletion, license change, internal changes
│   └── Audit logs ─────────> Entra audit logs, category "Policy" (create/update/end/
│                            delete review, approve/deny/reset/apply decision);
│                            export to Log Analytics or Event Hubs for dashboards
│
├── 💻 PROGRAMMATIC (MICROSOFT GRAPH)
│   ├── Create permission ──> AccessReview.ReadWrite.All (delegated or application)
│   ├── Delegated caller ───> Must hold the Identity Governance Administrator role
│   ├── Read permission ────> AccessReview.Read.All (application/service principal)
│   ├── PowerShell cmdlet ──> New-MgIdentityGovernanceAccessReviewDefinition
│   └── Tip ────────────────> Prototype queries in Graph Explorer before scripting
│
├── 🔄 RECURRING REVIEWS (LIFECYCLE)
│   ├── Series ends ────────> Never, a specific end date, or after N occurrences
│   ├── Update scope ───────> ⭐ Apply edits to "Current" instance OR whole "Series"
│   └── Use case ───────────> Reviewer left the org → replace them across the Series
│
└── 🧠 ACCESS REVIEW AGENT (PREVIEW)
    ├── What it does ────────> Natural-language reviews inside Microsoft Teams with
    │                        insights + approve/deny recommendation & justification
    ├── Licensing ───────────> ID Governance / Suite + Security Copilot (min 1 SCU)
    ├── Admin roles ─────────> ALL of: Identity Governance Administrator + Lifecycle
    │                        Workflows Administrator + Security Copilot Contributor
    ├── Reviewer needs ──────> Teams + an assigned active review + Security Copilot
    │                        Contributor role
    ├── Scoring signals ─────> Inactivity, user-to-group affiliation, accountEnabled,
    │                        employeeLeaveDateTime, mover workflow in past 30 days,
    │                        prior review decisions, access-package request history
    ├── Enable path ─────────> Entra admin center > Agents > Access Review Agent >
    │                        Start agent; per review: Settings > Advanced > tick
    │                        "Access Review Agent (Preview)"
    └── Limitation ──────────> ⚠️ Once started, the agent CANNOT be stopped or paused
```
