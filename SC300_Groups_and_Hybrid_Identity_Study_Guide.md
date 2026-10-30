# SC-300 Study Notes - Microsoft Group Types, Hybrid Groups, and Sync Technologies

## 1. Microsoft 365 Group Types Comparison

| Feature | Microsoft 365 Group | Security Group | Mail-Enabled Security Group | Distribution List |
|----------|-------------------|----------------|----------------------------|-------------------|
| Primary Purpose | Collaboration | Access control | Access control + email distribution | Email distribution |
| Email Address | Yes | No | Yes | Yes |
| Shared Mailbox | Yes | No | No | No |
| Shared Calendar | Yes | No | No | No |
| SharePoint Site | Yes | No | No | No |
| Planner / Teams Integration | Yes | No | No | No |
| Can Assign Permissions | Yes (limited workload scenarios) | Yes | Yes | No |
| Can Assign Licenses | Yes | Yes | Yes | No |
| Dynamic Membership | Supported | Supported | Not supported | Not supported |
| External Members | Supported | Supported | Limited | Limited |
| Created in Entra Admin Center | Yes | Yes | No | No |
| Managed in Exchange Admin Center | Yes | Partial | Yes | Yes |

## Exam Tip

Only two native group types can be directly created in Microsoft Entra ID:

- Microsoft 365 Groups
- Security Groups

Mail-Enabled Security Groups and Distribution Lists are Exchange-based objects and are typically created and managed through Exchange Online or the Microsoft 365 Admin Center.

---

## 2. Entra ID vs Microsoft 365 Admin Center

### Microsoft Entra Admin Center

Can create:

- Security Groups
- Microsoft 365 Groups

Supports:

- Assigned Membership
- Dynamic User Membership
- Dynamic Device Membership
- Role-Assignable Groups

### Microsoft 365 Admin Center / Exchange Online

Can manage:

- Microsoft 365 Groups
- Security Groups
- Mail-Enabled Security Groups
- Distribution Lists

Also provides:

- Mail settings
- Exchange recipient management
- Email routing and moderation

---

## 3. Groups Synchronized from Active Directory

### AD Security Group → Entra ID

Supported and commonly synchronized.

Can be:

- Global
- Universal
- Domain Local (with limitations)

Used for:

- RBAC
- Application access
- Conditional Access assignments
- License assignment

### AD Distribution Group → Entra ID

Synchronizes as a mail-enabled distribution object.

Used only for:

- Email distribution

Cannot:

- Assign licenses
- Assign Azure/Entra permissions

### AD Mail-Enabled Security Group → Entra ID

Synchronizes as a mail-enabled security group.

Can:

- Be used for permissions
- Receive email

Commonly used in Exchange Hybrid environments.

### AD Dynamic Distribution Groups

Do NOT synchronize to Entra ID.

Reason:

- Membership calculation occurs inside Exchange.
- Not represented as an Entra security object.

---

## 4. Cloud-Managed vs Synced Groups

| Capability | Cloud Group | Synced from AD |
|------------|------------|----------------|
| Editable in Entra ID | Yes | No (source of authority = AD) |
| Membership Editable in Cloud | Yes | No |
| Dynamic Membership | Yes | No |
| Writeback Required | No | N/A |
| Source of Authority | Entra ID | Active Directory |

### Exam Tip

If a group is synchronized from Active Directory:

- Membership is managed on-premises.
- Most properties become read-only in Entra ID.
- Changes must be made in Active Directory.

---

## 5. Group Scope Types from Active Directory

### Global Groups

Used to group users from the same domain.

Can be members of:

- Universal groups
- Domain local groups

### Domain Local Groups

Used to assign permissions to resources in the local domain.

Can contain:

- Users
- Global groups
- Universal groups

### Universal Groups

Used across multiple domains.

Recommended for multi-domain forests.

### AGDLP Model (Exam Favorite)

Accounts → Global Groups → Domain Local Groups → Permissions

### AGUDLP Model

Accounts → Global → Universal → Domain Local → Permissions

---

## 6. Microsoft Entra Connect Sync vs Microsoft Entra Cloud Sync

| Feature | Entra Connect Sync | Entra Cloud Sync |
|----------|------------------|------------------|
| Sync Engine Location | On-premises server | Cloud service + lightweight agent |
| SQL Requirement | Yes | No |
| Installation Complexity | Higher | Lower |
| Management | Local server | Entra portal |
| Multiple Active Agents | No | Yes |
| High Availability | Staging Server | Multiple agents |
| Sync Configuration Stored | Local server | Cloud |
| Device Synchronization | Full support | Supported for Hybrid Join scenarios |
| Group Writeback | Legacy support | Current strategic direction |
| Custom Rules | Advanced customization | Simpler model |
| Resource Consumption | Higher | Lower |
| Recommended by Microsoft | Legacy hybrid solution | Strategic future direction |

---

## 7. Entra Cloud Sync Advantages

### Architecture

- Cloud-managed configuration
- Lightweight provisioning agents
- No SQL database
- Easier maintenance

### High Availability

- Multiple active agents
- Automatic failover
- No single synchronization server dependency

### Administrative Benefits

- Managed from Entra portal
- Easier upgrades
- Lower operational cost

---

## 8. Entra Connect Sync Advantages

### Advanced Hybrid Scenarios

Supports:

- Complex attribute transformations
- Advanced synchronization rules
- Some legacy hybrid configurations
- Mature customization options

### Common Exam Scenario

If a question mentions:

- complex synchronization rules
- heavy customization
- advanced filtering
- legacy hybrid identity

The answer is often:

**Microsoft Entra Connect Sync**

---

## 9. SC-300 Exam Memorization List

### Remember

1. Only Security Groups and Microsoft 365 Groups are native Entra group types.
2. Distribution Lists cannot be used for permissions.
3. Mail-Enabled Security Groups support both email and permissions.
4. Microsoft 365 Groups provide collaboration resources.
5. Synced groups are managed in Active Directory.
6. Dynamic membership exists only for Entra Security Groups and Microsoft 365 Groups.
7. Cloud Sync is Microsoft's strategic direction for hybrid identity synchronization.
8. Cloud Sync supports multiple active agents.
9. Entra Connect Sync requires a synchronization server.
10. Source of Authority determines where objects must be managed.
