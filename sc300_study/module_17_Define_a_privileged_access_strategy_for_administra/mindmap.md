# Module 17: Define a privileged access strategy for administrative users - Mind Map

<details><summary>Source lessons</summary>

- Introduction
- Define a privileged access strategy for administrative users
- Configure Privileged Identity Management for Azure resources
- Exercise configure Privileged Identity Management for Microsoft Entra roles
- Exercise assign Microsoft Entra roles in Privileged Identity Management
- Exercise assign Azure resource roles in Privileged Identity Management
- Plan and configure Privileged Access Groups
- Analyze Privileged Identity Management audit history and reports
- Create and manage emergency access accounts
- Summary and resources

</details>

```mermaid
mindmap
  root((Privileged Identity Management))
    PIM Overview
      Service in Microsoft Entra ID
      Requires Premium P2 license
      Just-in-time privileged access
      Time-bound assignments
      Approval-based activation
      Enforce MFA on activation
      Justification and notifications
      Access reviews
    Access Strategy
      Identify stakeholders
      Least privilege
        Reduce Global Administrators
        Assign granular built-in roles
        Access reviews for cleanup
      Roles to protect
        Global Administrator
        Security Administrator
        User Administrator
        Exchange Administrator
        Azure Owner role
        User Access Administrator
        Roles with guest users
      Group-based assignment
        Role-assignable groups
        Delegate to group owners
      Eligible versus permanent
        Zero standing administrators
        Two
