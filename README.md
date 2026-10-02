# FixZtream — Windows Troubleshooting Kit (v2.0.0)

A Chrome/Edge extension with 21 one-click Windows jobs, browser site-data cleanup,
shortcuts to Windows consoles and Chrome's own fix-it pages, and a live activity log.

| Tab | What's in it |
|---|---|
| Repair | Restore point · SFC · DISM RestoreHealth · DISM component cleanup · temp cleanup · Defender quick scan · gpupdate |
| Update | Restart wuauserv + BITS · reset SoftwareDistribution · Chinese (Simplified / Traditional) language capabilities |
| Network | Flush DNS + ARP · renew IP · reset Winsock + TCP/IP |
| Devices | Print spooler fix · audio fix · camera fix · hardware rescan · battery report · disk health · chkdsk /scan |
| Browser | Clear site data (this site / all sites) · Chrome troubleshooting shortcuts |
| Tools | Device Manager, Disk Management, Services, Event Viewer and 13 more Windows consoles/settings |

Ctrl+K searches everything; keys 1–6 switch tabs.

## Two parts

- `extension/` — the browser extension (developer build, fixed ID via `"key"`).
- `host/` — the PowerShell helper (native messaging host). Chrome can't run PowerShell itself;
  the helper accepts only the fixed list of jobs and asks for admin (UAC) per system job.
  Battery report and disk health run as the user, without a UAC prompt.

The extension checks the helper's version: if a user's helper is older than a job needs,
the popup shows "Helper update available" and disables just those jobs.

## Install (developer)

1. Right-click `host\install.ps1` → **Run with PowerShell** (accept the admin prompt).
2. `chrome://extensions` → Developer mode → **Load unpacked** → pick `extension`.

## Publishing

Everything for the Chrome Web Store is in `store/` — see `store/STORE_LISTING.md`.

## Adding a job

1. `host/worker.ps1`: add the action to `ValidateSet`, write an `Invoke-…` function, add it to the `switch`.
2. `host/host.ps1`: add it to `$ElevatedActions` (or `$UserActions` for no-UAC jobs); bump `$Version`.
3. `extension/actions.js`: add it to `FB_ACTIONS` with `since:` set to the new helper version, and raise `FB_HELPER_MIN`.
4. Release the new helper zip under the same asset name, then publish the extension.
