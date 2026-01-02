# KB — AD Account Lockout (identify source + fix)

## Summary
Repeated AD lockouts are commonly caused by a device/service using outdated cached credentials.

## Applies to
- Windows 10/11 domain-joined devices
- Active Directory user accounts

## Steps
1. Confirm lockout + bad password attempts in AD (LockedOut, BadPwdCount, LastBadPasswordAttempt).
2. Unlock the account after verifying user identity.
3. Identify the lockout source:
   - Domain Controller → Event Viewer → Windows Logs → Security → Event ID 4740
   - Note "Caller Computer Name"
4. On the source system/device:
   - Remove stale entries in Credential Manager
   - Reconnect mapped drives with updated credentials
   - Update Outlook/mobile email password
   - Check Scheduled Tasks/services running under the user context
5. Monitor for repeat lockouts.

## Notes / Gotchas
- Phones/tablets with mail profiles are a frequent cause after password changes.
- Persistent mapped drives can silently re-authenticate with old credentials.

## Verification
- User can sign in successfully.
- No new Event ID 4740 entries for the user after cleanup.
