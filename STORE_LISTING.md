# FixZtream — Chrome Web Store listing (v2.0.0)

Item ID: `bbpgdeogddgnmliillenanankjgojcfm` · Dashboard: https://chrome.google.com/webstore/devconsole/

---

## Publishing v2.0.0

Do the GitHub part **first**: the new jobs need the new helper, and reviewers will download it.

1. **GitHub → Releases → your `Fixztream` release → Edit.** Delete the old
   `FixZtreamHost_CompanionHelper.zip` asset and upload the new one with the **same name**.
   The in-app "Download helper" button points at that exact name, so it keeps working.
2. **GitHub → repo:** replace `privacy-policy.html` with the new one.
3. **Dashboard → FixZtream → Package → Upload new package:** `upload/FixZtream-2.0.0-webstore.zip`.
4. **Store listing tab:** paste the new summary/description; replace the screenshots with
   `images/screenshot-1…5` and the small promo tile with `images/promo-small-440x280.png`.
5. **Privacy practices tab:** replace the single purpose (below). Permissions are unchanged.
6. **Test instructions tab:** paste the new text (below). Save draft → **Submit for review**.

Existing users: the extension updates by itself (no new permissions). Their old helper keeps
running the original jobs; the popup shows **"Helper update available"** with a download
button until they run the new `install.ps1`.

---

## 1. Store listing tab

**Summary** (from manifest):
Windows repair, network, device and Windows Update fixes plus site-data cleanup — one click each, fully logged.

**Description:**

```
FixZtream turns everyday Windows and browser troubleshooting into one-click jobs — with live output and a log of every run.

REPAIR
• Create a System Restore point before you start
• System File Checker (sfc /scannow) with a plain-English result
• DISM RestoreHealth and component-store cleanup
• Clear temp files (older than 24 hours; files in use are skipped)
• Microsoft Defender quick scan — updates definitions first
• Force a Group Policy refresh

WINDOWS UPDATE
• Restart wuauserv and BITS
• Reset the update cache (SoftwareDistribution → .Old), press-and-hold to confirm
• Install Chinese (Simplified / Traditional) language pack, handwriting, fonts, OCR, speech

NETWORK
• Flush DNS and ARP cache
• Renew IP address
• Reset Winsock and TCP/IP (press-and-hold; restart required)

DEVICES
• Fix stuck printing — restarts the spooler and clears the queue
• Fix no sound — restarts the Windows Audio services
• Fix camera — restarts the Camera Frame Server
• Rescan hardware and list devices that report a problem
• Battery report and disk health (SMART, wear, free space) — no admin prompt
• Check drive C: for errors (online chkdsk scan, no restart)

BROWSER
• Clear site data for the current site — or all sites — choosing exactly what to remove: cache, cookies, local storage, IndexedDB, service workers and more, with a live storage breakdown
• Shortcuts to Chrome's own fix-it pages: DNS host cache, socket pools, reset, updates, extensions

TOOLS
• Open Device Manager, Disk Management, Services, Event Viewer, Task Manager, Network Connections, Programs & Features and other Windows consoles straight from the browser

ALSO
• Live status: Windows Update services, update-cache size, free disk space, uptime, "restart pending"
• Ctrl+K searches every job, tool and shortcut; keys 1–6 switch tabs
• Activity log with full output — filter, copy or export as .txt
• Side panel support, light and dark themes

REQUIREMENTS — PLEASE READ
The Windows jobs need Windows 10 or 11 and a small companion helper, because browsers cannot run PowerShell on their own. Use the "Download helper" button in FixZtream (or https://github.com/MarkGenerosa/Fixztream/releases), then right-click install.ps1 → Run with PowerShell.

System jobs ask for administrator approval through the standard UAC prompt every time. The helper only accepts FixZtream's fixed list of jobs — it cannot run any other command. The extension sends nothing over the network and collects no data.

Need the full toolkit (BitLocker, debloat, software manager and more)? See Workztream: https://workztream.com

Source code: https://github.com/MarkGenerosa/Fixztream
```

**Screenshots:** `images/screenshot-1-repair.png` … `screenshot-5-search.png` (remove the old four).
**Small promo tile:** `images/promo-small-440x280.png` · **Marquee (optional):** `images/promo-marquee-1400x560.png`

---

## 2. Privacy practices tab

**Single purpose** (replace):

```
Windows and browser troubleshooting: run a fixed set of repair, network, device and Windows Update tasks through a locally installed helper, clear a website's stored browsing data, and show a log of the results.
```

**Permissions:** unchanged from v1.1.0 — no edits needed.
*(If the dashboard asks again, the nativeMessaging justification now reads: "Sends the selected task or Windows tool name to the FixZtream helper on the user's PC, which runs the corresponding fixed PowerShell command and returns its output. Browsers cannot run system commands any other way.")*

**Remote code:** No. **Data usage:** none — leave every box unticked, keep the three certifications.

---

## 3. Test instructions tab (replace)

```
FixZtream has a browser part (works anywhere) and Windows jobs (need Windows 10/11 + the companion helper).

A) BROWSER TAB — no setup
1. Open any website, click the FixZtream icon, go to the Browser tab (or press 5).
2. Open "Clear site data", pick data types, press "Clear data for <site>". The run appears in the Activity log.
3. Chrome shortcuts open Chrome's own pages (DNS cache, reset, etc.) in a new tab.
4. Press Ctrl+K to search all actions.

B) WINDOWS JOBS — need the helper
1. Click "Download helper" in the popup (or get FixZtreamHost_CompanionHelper.zip from https://github.com/MarkGenerosa/Fixztream/releases) and extract it.
2. Right-click install.ps1 → Run with PowerShell → approve the admin prompt. It installs host.bat, host.ps1 and worker.ps1 to C:\Program Files\FixZtream and registers "com.fixztream.host" for this extension.
3. Reopen the popup — the pill reads "Host ready".
4. Safe tests:
   • Devices → Disk health (no admin prompt) → results stream into the log.
   • Network → Flush DNS & ARP cache → approve UAC → logged.
   • Tools → Device Manager opens.
   • Declining UAC is logged as cancelled; nothing changes.

Notes:
• SFC, DISM and Defender scans can take 5–30 minutes; progress streams into the popup.
• "Reset the update cache" and "Reset Winsock & TCP/IP" need a press-and-hold.
• The helper accepts only FixZtream's fixed job and tool names; it never executes text received from the extension.
• Uninstall the helper with uninstall.ps1.

No account or login is needed.
```

---

## Future updates

Bump `"version"` in `manifest.json`, zip the **contents** of `upload/extension` (manifest at the zip root, no `"key"`),
and upload under **Package → Upload new package**. If a job needs new helper code, also bump `$Version` in
`host.ps1`, set `FB_HELPER_MIN` / `since` in `actions.js`, and replace the GitHub release asset (same file name).
