# FixZtream — Windows Troubleshooting Kit

A Chrome/Edge extension with four one-click jobs and a live activity log:

| # | Job | What runs |

| 01 | Restart update services | `Restart-Service wuauserv, BITS -Force`, then confirms both are Running |
| 02 | Reset the update cache | Stops wuauserv + BITS (and Delivery Optimization), renames `C:\Windows\SoftwareDistribution` → `SoftwareDistribution.Old`, starts services again. Hold-to-confirm. |
| 03 | Chinese (Simplified, Mainland China) | `Get-WindowsCapability -Online` → `Add-WindowsCapability` for zh-CN: Language pack (Basic), Handwriting, Fonts — OCR / Speech / TTS optional |
| 04 | Chinese (Traditional, Taiwan) | Same for zh-TW |

## Why there are two parts

Chrome extensions cannot run PowerShell themselves. The `host` folder is a small
**native messaging host**: Chrome talks to it, and it asks for admin rights via UAC
each time a job runs. It only accepts these four jobs — there is no way to send it
an arbitrary command.

## Install (once)

1. Unzip somewhere permanent, e.g. `C:\Tools\FixZtream`.
2. **Host:** right-click `host\install.ps1` → **Run with PowerShell** → accept the admin prompt.
   It copies the host to `C:\Program Files\FixZtream` and registers it for Chrome and Edge.
3. **Extension:** open `chrome://extensions` (or `edge://extensions`), turn on **Developer mode**,
   click **Load unpacked**, pick the `extension` folder.
   The ID will be `nchdnbfdbhbcbhlicpngchnlhimjplhp` (fixed by the key in `manifest.json`).
4. Pin FixZtream to the toolbar, open it — the pill should read **Host ready**.

## Using it

- Click **Run / Install**, approve the UAC prompt. The popup may close when UAC appears —
  the job keeps running in the background, the toolbar badge shows `…`, and a desktop
  notification arrives when it's done.
- Reopen the popup to watch output stream live, or click the panel icon to pin it to the side panel.
- Keys `1`–`4` trigger jobs (job 2 still needs a hold).
- Activity log: click any entry for its full output; filter, copy, or export as `.txt`.
- A second, permanent log is written to `%ProgramData%\FixZtream\activity.log`.

## Notes

- Language installs download from Windows Update and can take several minutes each.
  On WSUS-managed PCs they may fail with `0x800f0954`; the log shows the policy to enable.
- Installing the capabilities does not add the keyboard to your input list — add it under
  Settings → Time & language → Language & region.
- Uninstall: `host\uninstall.ps1` (add `-RemoveLogs` to delete the ProgramData log too),
  then remove the extension.

## Publishing

Everything for the Chrome Web Store is in `store/`: the upload zip (no dev key),
screenshots and promo tiles, `STORE_LISTING.md` with all text to paste,
`privacy-policy.html`, and `FixZtream-Host-1.0.0.zip` for your GitHub release.
