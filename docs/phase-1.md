# Phase 1 — Active Directory & Windows Endpoint Security

## Objective

Build an enterprise-style Windows domain environment and apply centralized endpoint-security controls that can be verified from a domain-joined workstation.

## Environment

| System | Role | Notes |
|---|---|---|
| DC01 | Windows Server 2022 Domain Controller | AD DS, DNS, Group Policy |
| WIN11-01 | Windows 11 Pro Workstation | Domain joined to `metbo.test` |
| Kali Linux | Security testing VM | Reserved for Phase 2 |
| Ubuntu | Monitoring/SIEM VM | Reserved for later phase |
| VMware Workstation | Hypervisor | Hosts the isolated lab |

## Network

- Lab subnet: `192.168.200.0/24`
- Domain controller: `192.168.200.10`
- Domain: `metbo.test`
- DNS for domain members: `192.168.200.10`

## Implementation Summary

### 1. Domain Services

Windows Server 2022 was promoted to a domain controller and configured with AD DS and DNS. The new Active Directory forest/domain was created as `metbo.test`.

### 2. Active Directory Structure

Organizational Units were created to model an enterprise layout, including departments, workstations, servers, and administrative accounts. Users and security groups were created to practice identity administration and access control.

### 3. Windows 11 Domain Join

`WIN11-01` was configured to use the domain controller for DNS, validated with network and DNS tests, and joined to `metbo.test`.

### 4. Group Policy

A `Workstation-Security-Baseline` GPO was created and linked to the workstation OU. Policy application was validated on the endpoint.

### 5. Windows Firewall

Firewall profiles were enabled centrally with default inbound traffic blocked and default outbound traffic allowed.

### 6. Microsoft Defender

Microsoft Defender Antivirus, Real-Time Protection, and Behavior Monitoring were verified as enabled.

### 7. Advanced Audit Policy

The workstation was configured to record:

- Successful logons
- Failed logons
- Process creation

### 8. Event Investigation

Process auditing was validated using Windows Security Event ID **4688**, demonstrating visibility into process execution on the endpoint.

## Troubleshooting Notes

### GPO initially not visible

Group Policy application was validated using `gpupdate /force` and `gpresult /r`. This helped distinguish policy-link/configuration issues from endpoint refresh issues.

### AuditPol privilege error

Running `auditpol` from a standard-user shell returned a required-privilege error. The command succeeded after using an elevated administrative console, reinforcing the principle of least privilege.

### Process Creation showed No Auditing

The Advanced Audit Policy configuration was corrected in Group Policy and refreshed on the workstation. The final effective policy generated Event ID 4688 successfully.

### Security Event Log access denied

A standard user could not read the protected Security log. Event Viewer was reopened with administrative privileges, after which the Security events were accessible.

## Evidence

See the `screenshots/` directory for Phase 1 verification evidence.

## Outcome

Phase 1 produced a functioning Windows domain environment with centralized endpoint security, domain authentication, firewall hardening, antivirus protection, security auditing, and security-event investigation capabilities.
