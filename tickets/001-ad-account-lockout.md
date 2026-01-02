# TKT-001: AD Account Lockout (repeated lockouts)

## Symptoms
- User reports repeated “account locked” prompts shortly after sign-in attempts.
- Lockout reoccurs even after unlock/password reset.
- Impact may include Outlook/Teams/VPN sign-in failures depending on the environment.

## Environment
- Device/OS: Windows 10/11
- Identity: Active Directory (on-prem)
- Common triggers: cached credentials, mapped drives, scheduled tasks/services, mobile mail clients

## Triage (what I checked first)
- Confirmed user identity and scope (can they sign in anywhere / which apps are impacted).
- Verified the account is currently locked and noted the approximate lockout time.
- Asked whether the user recently changed their password (common trigger for stale credentials).

## Diagnostics (commands/logs)
```powershell
# Requires RSAT (ActiveDirectory module) + permissions

# 1) Confirm lockout + recent bad password attempts
Get-ADUser -Identity <username> -Properties LockedOut,BadPwdCount,LastBadPasswordAttempt |
  Select-Object LockedOut, BadPwdCount, LastBadPasswordAttempt

# 2) Unlock (after confirming identity)
Unlock-ADAccount -Identity <username>

# 3) Identify lockout source
# Domain Controller: Event Viewer → Windows Logs → Security → Event ID 4740
# Key field: "Caller Computer Name" (source host)

# 4) Useful inventory (optional)
Search-ADAccount -LockedOut -UsersOnly | Select-Object Name,SamAccountName
