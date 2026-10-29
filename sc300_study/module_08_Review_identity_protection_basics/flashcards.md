# Module 08: Review identity protection basics - Flashcards

<details><summary>Source lessons</summary>

- Introduction
- Review identity protection basics
- Implement and manage user risk policy
- Exercise enable sign-in risk policy
- Exercise configure Microsoft Entra multifactor authentication registration polic
- Monitor, investigate, and remediate elevated risky users
- Implement security for workload identities
- Explore Microsoft Defender for Identity
- Explore the Identity Risk Management Agent
- Summary and resources

</details>

### Identity Protection Basics

**Q:** Which license is required to use Identity Protection risk policies, risk notifications, and the MFA registration policy?
**A:** Microsoft Entra ID Premium P2.

**Q:** What are the three key tasks organizations can accomplish with Identity Protection?
**A:** Automate detection and remediation of identity-based risks; investigate risk data in the portal; export risk detection data to third-party utilities (e.g., a SIEM).

**Q:** How many signals does Microsoft analyze per day to identify and protect customers from threats?
**A:** 6.5 trillion signals per day.

**Q:** Which risk detection fires when multiple usernames are attacked using common passwords in a unified brute-force manner?
**A:** Password spray.

**Q:** A user signs in from a location atypical compared to their recent sign-ins. Which detection triggers?
**A:** Atypical travel.

**Q:** Which detection flags unusual token characteristics, such as an unusual token lifetime or a token replayed from an unfamiliar location?
**A:** Anomalous token.

**Q:** Which Identity Protection risk detections are discovered by Microsoft Defender for Cloud Apps (MDCA)?
**A:** New country, Activity from anonymous IP address, and Suspicious inbox forwarding.

**Q:** With Microsoft Entra ID P1, what does the Risky users report show?
**A:** Limited information: only users with medium and high risk, no details drawer, no risk history.

### Identity Protection Roles and Permissions

**Q:** Which role has full access to Identity Protection but cannot reset a user's password?
**A:** Security Administrator.

**Q:** Which role can view
