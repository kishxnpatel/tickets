# KB — Fix Windows Update failures (DISM/SFC + reset cache)

## Summary
Use DISM and SFC to repair Windows image/system files. If failures persist, reset Windows Update cache folders.

## Applies to
- Windows 10/11 endpoints

## Steps
1. Confirm stable internet and sufficient free disk space.
2. Run:
   - DISM /Online /Cleanup-Image /RestoreHealth
   - sfc /scannow
3. If still failing, reset Windows Update components:
   - Stop services (wuauserv, BITS, CryptSvc, msiserver)
   - Rename SoftwareDistribution and catroot2
   - Start services
4. Re-run Windows Update and reboot.

## Notes / Gotchas
- Renaming folders is safer than deleting (easy rollback).
- Some updates require multiple restarts to complete.

## Verification
- Updates install successfully.
- No repeated failures in Windows Update history.
