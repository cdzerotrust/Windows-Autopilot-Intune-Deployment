# Windows 11 Autopilot & Microsoft Intune Deployment

### Enterprise Endpoint Provisioning | Microsoft Entra ID | Identity & Access Management

**Organization:** Sentinel Ridge Cybersecurity Labs  
**Change Ticket:** CHG0001002  
**Project Status:** Successfully Implemented  
**Implementation Date:** November 15, 2026  
**Environment:** Microsoft Entra ID | Microsoft Intune | Windows Autopilot | Windows 11 Enterprise

---

## 📌 Project Overview

This project documents the deployment of a Windows 11 Enterprise workstation using Windows Autopilot, Microsoft Entra ID, and Microsoft Intune.

The objective was to transition an existing Windows workstation from a traditional on-premises Active Directory environment to a cloud-managed endpoint.

The deployment included hardware registration, Autopilot profile configuration, identity-based enrollment controls, operating system installation, Microsoft Entra join, and Microsoft Intune enrollment.

**Final Result:** The workstation was successfully joined to Microsoft Entra ID, enrolled in Microsoft Intune, and reported as compliant.

---

## 🎯 Business Objectives

- Implement standardized Windows endpoint provisioning using Windows Autopilot.
- Establish cloud-based device identity through Microsoft Entra ID.
- Enable centralized endpoint management through Microsoft Intune.
- Configure identity-based automatic MDM enrollment.
- Apply least-privilege principles to device enrollment.
- Validate Windows device activation, organizational join, and compliance.
- Document implementation activities through formal change management.

---

## 🏢 Lab Environment

| Component | Configuration |
|---|---|
| Organization | Sentinel Ridge Cybersecurity Labs |
| Endpoint | HP EliteDesk 800 G3 DM 65W |
| Original Computer Name | SIMAUTH0262 |
| Post-Deployment Name | DESKTOP-HE23LV1 |
| Original AD Domain | lab.local |
| Cloud Identity Provider | Microsoft Entra ID |
| Endpoint Management | Microsoft Intune |
| Provisioning Platform | Windows Autopilot |
| Operating System | Windows 11 Enterprise |
| Deployment Type | User-driven |
| Device Join Type | Microsoft Entra joined |
| User Privileges | Standard user |
| Enrollment Licensing | Microsoft 365 E5 |
| Change Record | CHG0001002 |

---

## 🏗️ Deployment Architecture

```text
              Sentinel Ridge Cybersecurity Labs
                            |
                     Microsoft Entra ID
                            |
               +------------+------------+
               |                         |
       Device Security Group     MDM Enrollment User Group
               |                         |
       Autopilot Profile          Licensed Cloud User
               |                         |
               +------------+------------+
                            |
                     Windows Autopilot
                            |
                  Windows 11 Installation
                            |
                    Microsoft Entra Join
                            |
                    Microsoft Intune
                            |
                 Device Compliance Reporting
```

---

## ⚙️ Implementation Summary

### Phase 1 — Windows Autopilot Registration

Collected the HP EliteDesk hardware information using the Microsoft Windows Autopilot PowerShell script.

Generated a hardware information CSV and imported it into Microsoft Intune.

**Result:** Device successfully registered with Windows Autopilot.

### Phase 2 — Device Group Configuration

Created the Microsoft Entra security group:

`SRC-Autopilot-Windows11-Devices`

Added the Autopilot-associated device identity to the group.

**Result:** Device targeting configured successfully.

### Phase 3 — Autopilot Deployment Profile

Created the deployment profile:

`SRC WIN11 Autopilot EntraJoin`

**Configuration:**

- Deployment mode: User-driven
- Join type: Microsoft Entra joined
- User account type: Standard
- Pre-provisioning: Disabled
- Device naming template: Not configured

Assigned the deployment profile to the Autopilot device security group.

**Verification:** Autopilot profile status displayed **Assigned**.

### Phase 4 — Automatic MDM Enrollment

Created a dedicated user security group:

`SRC-Intune-MDM-Enrollment-Users`

Added the licensed cloud-only enrollment user.

Configured Microsoft Intune automatic enrollment with:

- MDM user scope: **Some**
- Target group: `SRC-Intune-MDM-Enrollment-Users`

**Result:** Automatic enrollment configured for the authorized lab user.

### Phase 5 — Enrollment Prerequisite Validation

Verified the following:

- Active Microsoft 365 E5 license.
- Windows MDM enrollment allowed.
- Microsoft Entra device join permitted.
- Autopilot profile assigned.
- Intended enrollment user included in the MDM enrollment group.

**Result:** Cloud-side enrollment prerequisites validated.

### Phase 6 — Windows 11 Deployment

Installed Windows 11 on replacement storage.

Completed organizational sign-in using the licensed Microsoft Entra account and Microsoft Authenticator.

A temporary Windows OOBE error, `OOBESETTINGSELECTOR`, was resolved by retrying the setup step.

**Result:** Windows 11 Enterprise desktop successfully reached.

### Phase 7 — Post-Deployment Verification

Validated Microsoft Entra join using:

```powershell
dsregcmd /status
```

**Verified results:**

```text
AzureAdJoined    : YES
DomainJoined     : NO
DeviceAuthStatus : SUCCESS
TpmProtected     : YES
```

Reviewed the cloud device inventory and confirmed:

| Validation | Result |
|---|---|
| Microsoft Entra Joined | Yes |
| Device Enabled | Yes |
| MDM Authority | Microsoft Intune |
| Device Owner | Authorized enrollment user |
| Device Compliance | Yes |
| Windows Activation | Active |
| Windows Edition | Windows 11 Enterprise |

**Result:** Cloud identity, endpoint management, and reported compliance successfully validated.

---

## 🔎 Troubleshooting and Lessons Learned

| Issue | Resolution |
|---|---|
| PowerShell execution policy blocked Autopilot script | Used a process-scoped execution policy adjustment |
| USB storage was write-protected | Cleared the USB disk's read-only attribute |
| Autopilot profile initially showed Not assigned | Verified group targeting and allowed assignment processing |
| Organizational sign-in initially failed | Corrected the account entry |
| Windows OOBE displayed `OOBESETTINGSELECTOR` | Retried the setup step successfully |
| Autopilot-associated device object initially appeared disabled | Reviewed its predeployment identity state without deleting or manually enabling the object |

### Key Lessons

1. **Autopilot registration is not the same as Intune enrollment.**
2. **Device targeting and user enrollment permissions are separate controls.**
3. **Licensing, enrollment restrictions, and join permissions must all be validated.**
4. **Microsoft Entra join does not independently prove Intune enrollment.**
5. **Successful deployment requires post-implementation evidence.**

---

## 🛡️ Security Considerations

- Used a dedicated security group for Autopilot device targeting.
- Restricted automatic MDM enrollment to an authorized user group.
- Configured the deployment profile for standard user privileges.
- Used Microsoft Entra organizational authentication.
- Completed Microsoft Authenticator verification.
- Verified TPM-protected device identity.
- Confirmed Microsoft Intune management and reported device compliance.

**Hardware support note:** The HP EliteDesk is an older workstation. Successful Windows 11 installation does not independently establish official Microsoft hardware support. Update and compatibility behavior should be monitored.

---

## 📸 Deployment Evidence

Screenshots will be organized in the `screenshots/` directory and referenced in the project documentation.

Planned evidence includes:

1. Windows Autopilot device registration.
2. Autopilot deployment profile assignment.
3. Microsoft Entra device security group.
4. Automatic MDM enrollment configuration.
5. Enrollment user licensing.
6. Windows enrollment restrictions.
7. Microsoft Entra device join settings.
8. Windows 11 Enterprise activation.
9. `dsregcmd /status` verification.
10. Microsoft Intune enrollment and compliance verification.

**Privacy:** Screenshots should be reviewed and redacted before public publication.

---

## 📋 Change Management

**Change ID:** CHG0001002  
**Change Type:** Normal Change  
**Category:** Endpoint Management / Identity & Access Management  
**Implementation Outcome:** Successful  
**Rollback Required:** No

The formal change record documents the business justification, scope, implementation activities, risks, troubleshooting, validation, and closure recommendation.

**Change Ticket:** `CHG0001002-Change-Ticket.md` *(to be added)*

---

## 🧠 Skills Demonstrated

- Windows Autopilot device registration and provisioning
- Microsoft Entra ID device management
- Microsoft Intune MDM enrollment
- Microsoft Entra security groups
- Identity-based enrollment controls
- Microsoft 365 licensing validation
- Windows 11 Enterprise deployment
- PowerShell administration
- Windows troubleshooting
- Endpoint compliance verification
- IT change management and technical documentation

---

## 🏁 Project Outcome

Successfully deployed a Windows 11 Enterprise workstation using Microsoft Entra ID and Microsoft Intune.

The final device inventory confirmed:

**Microsoft Entra Joined: Yes**  
**MDM: Microsoft Intune**  
**Compliant: Yes**

The project demonstrates an end-to-end cloud-managed Windows endpoint deployment, including configuration, troubleshooting, validation, and change-management documentation.

---

**Sentinel Ridge Cybersecurity Labs**  
*Hands-On Identity, Endpoint Security & Cloud Administration*
