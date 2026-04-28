# Incident Report - 01 (Splunk Investigation)

## Executive Summary
During log analysis in Splunk (index=main), a security incident was identified involving unauthorized user creation and remote command execution. 
The attacker successfully created a backdoor account "A1berto" on a target system using Windows Management Instrumentation (WMIC) via PowerShell. 
The activity indicates persistence and lateral movement attempts within the environment.

---

## Timeline
- Initial log ingestion completed (12256 events indexed)
- Suspicious process creation detected (Event ID 4688)
- PowerShell execution observed spawning WMIC
- Remote command execution initiated on WORKSTATION6
- Backdoor user "A1berto" created using `net user /add`
- Registry updated: HKLM\SAM\SAM\Domains\Account\Users\Names\A1berto
- Attacker attempted impersonation of legitimate user "Alberto"
- No successful login observed from backdoor account

---

## IOCs Found
- Backdoor Username: A1berto
- Legitimate User Targeted: Alberto
- Command Used:
  "C:\windows\System32\Wbem\WMIC.exe" /node:WORKSTATION6 process call create "net user /add A1berto paw0rd1"
- Event ID: 4688 (Process Creation)
- Registry Path:
  HKLM\SAM\SAM\Domains\Account\Users\Names\A1berto
- Host Involved: WORKSTATION6
- Parent Process: powershell.exe

---

## Recommended Actions
- Disable and remove unauthorized account "A1berto"
- Reset credentials for potentially affected accounts
- Monitor and restrict use of WMIC for remote execution
- Enable advanced PowerShell logging (Script Block Logging)
- Implement multi-factor authentication (MFA)
- Apply least privilege principle across systems
- Monitor Event IDs 4688 and 4720 for suspicious activity
- Deploy endpoint detection and response (EDR) tools
