# CHG0001002 — Windows 11 Autopilot Deployment and Microsoft Intune Enrollment

**Organization:** Sentinel Ridge Cybersecurity Labs  
**Change ID:** CHG0001002  
**Change Type:** Normal Change  
**Category:** Endpoint Management / Identity and Access Management  
**Configuration Item:** HP EliteDesk 800 G3 DM 65W  
**Environment:** Microsoft Entra ID / Microsoft Intune / Windows Autopilot  
**Implementation Status:** Successfully Implemented  
**Validation Status:** Passed  
**Rollback Status:** Not Required  
**Change Closure:** Pending final documentation and evidence review

---

## 1. Change Summary

Implement Windows Autopilot provisioning and Microsoft Intune management for an existing HP EliteDesk workstation.

The change transitions the endpoint from its previous on-premises Active Directory configuration to a cloud-managed Windows 11 Enterprise environment.

The implementation includes:

- Windows Autopilot hardware registration.
- Microsoft Entra security group configuration.
- Autopilot deployment profile creation and assignment.
- Microsoft Intune automatic MDM enrollment configuration.
- Enrollment licensing and prerequisite validation.
- Windows 11 installation and organizational sign-in.
- Microsoft Entra join verification.
- Microsoft Intune enrollment and compliance validation.

**Implementation result:** The workstation was successfully joined to Microsoft Entra ID, managed by Microsoft Intune, and reported as compliant.

---

## 2. Business Justification

Traditional domain-based workstation deployment requires substantial manual configuration and dependency on on-premises infrastructure.

Windows Autopilot and Microsoft Intune provide a foundation for standardized, cloud-managed endpoint provisioning.

### Expected Benefits

1. Reduce manual endpoint provisioning activities.
2. Centralize endpoint management.
3. Establish cloud-based device identity.
4. Support identity-driven enrollment controls.
5. Improve deployment consistency and auditability.
6. Enable visibility into device compliance.
7. Reduce reliance on traditional domain-join workflows.

This implementation serves as a controlled proof of concept for Sentinel Ridge Cybersecurity Labs.

---

## 3. Scope of Change

### In Scope

- One HP EliteDesk 800 G3 DM 65W workstation.
- Autopilot hardware identity registration.
- Microsoft Entra security groups.
- Autopilot deployment profile.
- Microsoft Intune automatic enrollment configuration.
- Windows 11 Enterprise installation.
- Microsoft Entra device join.
- Endpoint management and compliance verification.
- Change documentation and evidence collection.

### Out of Scope

- Production-wide Autopilot rollout.
- Enterprise application deployment.
- Custom Windows security baseline deployment.
- Conditional Access policy changes.
- On-premises Microsoft Entra Connect remediation.
- Migration of production user data.
- Enterprise-wide endpoint compliance policy redesign.

---

## 4. Configuration Items and Dependencies

| Item | Configuration |
|---|---|
| Physical Workstation | HP EliteDesk 800 G3 DM 65W |
| Original Computer Name | SIMAUTH0262 |
| New Computer Name | DESKTOP-HE23LV1 |
| Original AD Domain | lab.local |
| Target Device Identity | Microsoft Entra joined |
| Target Management Platform | Microsoft Intune |
| Operating System | Windows 11 Enterprise |
| Autopilot Profile | SRC WIN11 Autopilot EntraJoin |
| Autopilot Device Group | SRC-Autopilot-Windows11-Devices |
| MDM Enrollment User Group | SRC-Intune-MDM-Enrollment-Users |
| Enrollment License | Microsoft 365 E5 |
| Deployment Mode | User-driven |
| Assigned User Privilege | Standard user |

**Dependency:** The authorized enrollment account must have the required Microsoft Intune entitlement and be within the configured MDM user scope.

---

## 5. Risk Assessment

| Risk | Potential Impact | Mitigation |
|---|---|---|
| Operating system installation failure | Endpoint unavailable | Preserve original storage for rollback |
| Incorrect Autopilot targeting | Deployment profile not applied | Validate device group membership and profile assignment |
| Missing MDM entitlement | Automatic enrollment failure | Verify assigned license and service availability |
| Enrollment restriction conflict | Device enrollment blocked | Validate Windows enrollment restrictions |
| Incorrect organizational identity | Sign-in or enrollment failure | Use intended licensed cloud-only account |
| Hardware compatibility limitations | Driver, update, or stability issues | Validate device functionality and monitor support limitations |
| Incomplete cloud enrollment | Unmanaged endpoint | Verify Entra join, Intune management, and compliance separately |
| Sensitive data in public evidence | Information disclosure | Redact identifiers and authentication details before publication |

### Hardware Compatibility Consideration

The HP EliteDesk 800 G3 is an older workstation. Successful Windows 11 installation and activation do not independently confirm official Windows 11 hardware support.

The installed Windows build should also be reviewed for its servicing and support status before the device is considered suitable for production use.

---

## 6. Pre-Implementation Validation

The following prerequisites were checked during the implementation:

| Prerequisite | Result |
|---|---|
| Autopilot hardware information collected | Passed |
| Hardware identity imported into Autopilot | Passed |
| Device security group configured | Passed |
| Autopilot deployment profile created | Passed |
| Autopilot deployment profile assigned | Passed |
| Enrollment user group configured | Passed |
| Microsoft Intune MDM user scope configured | Passed |
| Enrollment user license verified | Passed |
| Windows MDM enrollment permitted | Passed |
| Microsoft Entra device join permitted | Passed |
| Original workstation storage retained | Passed |

---

## 7. Implementation Plan and Execution Record

### Step 1 — Collect Autopilot Hardware Information

**Source workstation:** SIMAUTH0262

Opened PowerShell and installed the Windows Autopilot hardware information collection script.

```powershell
Install-Script -Name Get-WindowsAutopilotInfo -Scope CurrentUser
```

Applied a process-scoped execution policy adjustment:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned
```

Generated the Autopilot CSV:

```powershell
Get-WindowsAutopilotInfo -OutputFile "$env:USERPROFILE\Desktop\SIMAUTH0262-Autopilot.csv"
```

Imported the resulting hardware information into Windows Autopilot.

**Outcome:** Hardware registration completed successfully.

### Step 2 — Configure Autopilot Device Targeting

Created the Microsoft Entra security group:

`SRC-Autopilot-Windows11-Devices`

Added the registered Autopilot device identity.

**Outcome:** Device group targeting established.

### Step 3 — Create and Assign the Autopilot Profile

Created the Windows Autopilot deployment profile:

`SRC WIN11 Autopilot EntraJoin`

| Setting | Value |
|---|---|
| Deployment Mode | User-driven |
| Join Type | Microsoft Entra joined |
| User Account Type | Standard |
| Pre-provisioning | No |
| Device Naming Template | Not configured |
| Default OS Language | Operating system default |
| Automatic Keyboard Configuration | Yes |

Configured the profile to hide selected setup screens, including privacy, licensing, and account-change options.

Assigned the profile to:

`SRC-Autopilot-Windows11-Devices`

**Outcome:** Autopilot deployment profile status displayed **Assigned**.

### Step 4 — Configure Automatic MDM Enrollment

Created the Microsoft Entra security group:

`SRC-Intune-MDM-Enrollment-Users`

Added the designated cloud-only enrollment user.

Configured automatic enrollment:

| Setting | Value |
|---|---|
| MDM Provider | Microsoft Intune |
| MDM User Scope | Some |
| Target Group | SRC-Intune-MDM-Enrollment-Users |
| WIP User Scope | None |

**Outcome:** Automatic MDM enrollment configured for the authorized user group.

### Step 5 — Validate Licensing and Enrollment Controls

Confirmed the designated enrollment user had an active Microsoft 365 E5 license that included Microsoft Intune Plan 1.

Validated the following controls:

- Windows MDM enrollment: Allowed.
- Personally owned Windows enrollment: Allowed under the existing default restriction.
- Microsoft Entra device join: Permitted.
- Microsoft Entra maximum devices per user: 50.

**Outcome:** Enrollment prerequisites validated.

**Security observation:** Allowing personally owned Windows enrollment is broader than a corporate-only enrollment model. A production deployment should evaluate whether more restrictive enrollment controls are appropriate.

### Step 6 — Install Windows 11

Preserved the original workstation hard drive as a rollback option.

Installed Windows 11 on replacement SSD storage.

Completed organizational setup using the intended licensed cloud-only Microsoft Entra account.

Microsoft Authenticator was used during the sign-in process.

**Issue encountered:** Windows setup displayed `OOBESETTINGSELECTOR`.

**Resolution:** Retried the affected setup step and successfully continued.

**Outcome:** Windows 11 Enterprise desktop successfully reached.

### Step 7 — Validate Windows Activation

Reviewed Windows activation settings.

**Observed results:**

- Windows edition: Windows 11 Enterprise.
- Subscription activation: Active.
- Digital license activation: Active.

**Outcome:** Activation verification passed.

### Step 8 — Validate Microsoft Entra Join

Executed:

```powershell
dsregcmd /status
```

**Observed results:**

```text
AzureAdJoined    : YES
EnterpriseJoined : NO
DomainJoined     : NO
DeviceAuthStatus : SUCCESS
TpmProtected     : YES
```

**Device name:** `DESKTOP-HE23LV1`

**Outcome:** Microsoft Entra join and device authentication verification passed.

### Step 9 — Validate Microsoft Intune Enrollment

Reviewed the endpoint in Microsoft Entra device inventory.

**Observed results:**

| Property | Verified Value |
|---|---|
| Device | DESKTOP-HE23LV1 |
| Enabled | Yes |
| Operating System | Windows |
| Join Type | Microsoft Entra joined |
| MDM | Microsoft Intune |
| Security Settings Management | Microsoft Intune |
| Device Owner | Authorized enrollment user |
| Compliant | Yes |

**Outcome:** Microsoft Intune management and reported device compliance confirmed.

---

## 8. Issues and Troubleshooting Record

| Issue | Investigation / Resolution | Outcome |
|---|---|---|
| PowerShell execution policy prevented script execution | Applied process-scoped RemoteSigned policy | Resolved |
| USB storage was write-protected | Cleared read-only disk attribute using DiskPart | Resolved |
| Autopilot profile initially not assigned | Verified device targeting and assignment processing | Resolved |
| Organizational sign-in difficulty | Corrected enrollment account selection | Resolved |
| Windows OOBE `OOBESETTINGSELECTOR` error | Retried the setup step | Resolved |
| Initial Autopilot-associated Entra object appeared disabled with no MDM | Distinguished predeployment registration state from the final enrolled device record | Final device validated |

### Related but Separate Issue

**PRB0001001 — Microsoft Entra Connect synchronization**

An existing Entra Connect synchronization issue was tracked separately and was not included in this endpoint deployment change.

The Autopilot deployment used a cloud-only enrollment identity and did not require resolution of that synchronization problem.

---

## 9. Post-Implementation Validation

| Test ID | Validation Activity | Expected Result | Actual Result | Status |
|---|---|---|---|---|
| VAL-01 | Autopilot registration | Device registered | Registration completed | Pass |
| VAL-02 | Autopilot profile assignment | Profile assigned | Assigned | Pass |
| VAL-03 | MDM enrollment eligibility | Authorized user eligible | Prerequisites validated | Pass |
| VAL-04 | Windows installation | Desktop accessible | Windows 11 installed | Pass |
| VAL-05 | Windows activation | Active | Active | Pass |
| VAL-06 | Microsoft Entra join | AzureAdJoined = YES | YES | Pass |
| VAL-07 | Device authentication | DeviceAuthStatus = SUCCESS | SUCCESS | Pass |
| VAL-08 | Microsoft Intune management | MDM = Microsoft Intune | Microsoft Intune | Pass |
| VAL-09 | Device compliance | Compliant = Yes | Yes | Pass |

**Overall validation result: PASS**

### Validation Limitations

The available evidence confirms successful device registration, organizational join, Microsoft Intune management, and reported compliance.

It does not independently establish:

- Production hardware support eligibility.
- Deployment of a custom security baseline.
- Enterprise application deployment.
- Compliance against an independently defined security framework.
- Completion of a broader production pilot.

---

## 10. Rollback Plan

### Rollback Trigger Conditions

Rollback would be considered if:

- Windows installation could not be completed.
- Organizational sign-in failed persistently.
- The device could not join Microsoft Entra ID.
- Required Microsoft Intune management could not be established.
- Critical hardware functionality was unavailable.
- Deployment caused unacceptable operational impact.

### Rollback Procedure

1. Stop deployment activity and document the failure.
2. Shut down the workstation.
3. Remove the replacement SSD if necessary.
4. Reinstall the preserved original hard drive.
5. Boot the original operating system.
6. Verify local functionality and previous domain configuration.
7. Review any cloud device objects created during the unsuccessful deployment.
8. Remove or retire cloud objects only after confirming their purpose and dependencies.
9. Record rollback results in CHG0001002.

**Rollback readiness:** Original storage preserved.

**Rollback execution:** Not required; implementation succeeded.

---

## 11. Evidence Register

The following evidence artifacts are planned for publication in the repository's `screenshots/` directory.

| Evidence ID | Description | Planned Filename |
|---|---|---|
| EVD-01 | Windows Autopilot device registration | `01-autopilot-device-registration.png` |
| EVD-02 | Autopilot device security group | `02-autopilot-device-group.png` |
| EVD-03 | Autopilot deployment profile assignment | `03-autopilot-profile-assigned.png` |
| EVD-04 | Automatic MDM enrollment scope | `04-mdm-enrollment-scope.png` |
| EVD-05 | Enrollment user licensing | `05-user-license-verification.png` |
| EVD-06 | Windows enrollment restrictions | `06-enrollment-restrictions.png` |
| EVD-07 | Microsoft Entra device join configuration | `07-entra-device-join.png` |
| EVD-08 | Windows 11 Enterprise activation | `08-windows-11-activation.png` |
| EVD-09 | `dsregcmd /status` join verification | `09-dsregcmd-join-verification.png` |
| EVD-10 | Intune management and compliance verification | `10-intune-compliance-verification.png` |

**Publication note:** The evidence register identifies planned files. Screenshots must be uploaded and verified before being considered published evidence.

**Privacy control:** Redact personal information, tenant identifiers, device identifiers, authentication details, and other unnecessary sensitive information before uploading screenshots to this public repository.

---

## 12. Implementation Outcome

The change achieved its principal technical objectives:

- Windows Autopilot hardware registration completed.
- Device targeting and deployment profile assignment established.
- Automatic Microsoft Intune enrollment configured.
- Windows 11 Enterprise installed and activated.
- Microsoft Entra join verified.
- Device authentication confirmed.
- Microsoft Intune management confirmed.
- Device compliance reported as **Yes**.

**Technical implementation status: SUCCESSFUL**

---

## 13. Change Closure Recommendation

**Recommended disposition:** Close as **Successfully Implemented** after final documentation and evidence review.

### Remaining Administrative Actions

- Upload redacted screenshots.
- Verify screenshot references and evidence filenames.
- Confirm the repository contains no sensitive information.
- Review temporary troubleshooting configurations for cleanup.
- Complete final change record review.

**Final assessment:** CHG0001002 successfully demonstrates cloud-managed Windows endpoint provisioning using Windows Autopilot, Microsoft Entra ID, and Microsoft Intune.

---

**Sentinel Ridge Cybersecurity Labs**  
**Change Record:** CHG0001002  
**Implementation:** Successful  
**Documentation:** Pending final evidence publication
