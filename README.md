<p align="center">
  <img src="assets/01-hero.png" alt="Antigravity Pulse: live tok/s and context capacity beside Turbo" width="100%">
</p>

<h1 align="center">Antigravity Pulse</h1>

<p align="center"><b>Native composer-bar telemetry for Google Antigravity 2.0.</b><br>Live <code>tok/s</code>, a context-capacity capsule, prompt cache hit ratio and the 5-hour quota countdown, right beside <code>Turbo</code>.</p>

<p align="center">
  <a href="https://github.com/akelabs/antigravity-pulse/releases/latest"><img alt="Release" src="https://img.shields.io/github/v/release/akelabs/antigravity-pulse?style=flat-square&label=release&labelColor=0B0B0F&color=3B82F6"></a>
  <img alt="Platform" src="https://img.shields.io/badge/platform-Windows%20%7C%20Web-0B0B0F?style=flat-square&labelColor=0B0B0F&color=5E9E8E">
  <img alt="Node.js 18+" src="https://img.shields.io/badge/Node.js-18%2B-0B0B0F?style=flat-square&labelColor=0B0B0F&color=C6A265">
  <img alt="Zero dependencies" src="https://img.shields.io/badge/dependencies-0-0B0B0F?style=flat-square&labelColor=0B0B0F&color=C26C77">
</p>

<p align="center"><a href="#quick-start"><b>Quick Start</b></a> · <a href="#why-antigravity-pulse"><b>Features</b></a> · <a href="#privacy--security"><b>Privacy</b></a> · <a href="https://github.com/akelabs/antigravity-pulse/releases/latest"><b>Download</b></a><br><sub><a href="README.md">English</a> · <a href="README.zh-CN.md">简体中文</a></sub></p>

<p align="center">
  <img src="assets/demo-preview.gif" alt="Antigravity Pulse demo" width="88%">
</p>

---

## Why Antigravity Pulse

Existing context monitors sit in the legacy bottom status bar as plain text and fail when switching between multiple chat panes, Remote Agent cloud workspaces, or the browser web client.

**Antigravity Pulse (`AGY-Pulse`)** lives directly inside `.agy-composer-footer` with native typography, spacing, and elevation:

- **Native Composer-Bar Layout**: `⚡ XX tok/s` ➔ `Morandi Runway Capsule (31k/256k, 12.1%)` ➔ `Turbo`.
- **Official `Turbo` Popover Aesthetics**: Zero emojis, inherited UI sans-serif with `tabular-nums`, `500/400` font weights, `6px` radius, Tailwind `shadow-md`, and instant Light/Dark theme adaptation.
- **Per-Pane Isolation**: Reads React Fiber state per composer instance so side-by-side chat panes track their own independent context and model limits (`256k` for Gemini / Claude, `80k` for GPT-OSS, with background shadow/image sub-models filtered out).
- **True End-to-End `tok/s` & Hybrid Delta Estimation**: Measures full output tokens (thinking + response) over real generation wall time, plus character-level live delta estimation between snapshots and `>5,000` token auto-compaction detection (Undo-immune).
- **5-Hour Quota Pool & Reset Countdown**: Accurately decodes Protobuf `Timestamp` objects and shared `gemini` / `premium` 5h quota pools (`Resets in 2h 40m`).
- **3-Habitat Engine**: Works identically across **Desktop Local Projects**, **Desktop Remote Agent (Cloud Workspaces)**, and **Browser Web**.
- **Near-Compaction Warning**: When context usage reaches `>= 85%`, the popover shows a quiet warning that automatic compaction is about to kick in.
- **1-Click `EN` ⇄ `中文` Toggle**: Follows your system language on first run, and a click on the capsule switches popover language anytime (your choice is remembered).

<table>
  <tr>
    <td width="50%" align="center"><img src="assets/02-popover.png" alt="Every number, one hover away"><br><sub>Every number, one hover away</sub></td>
    <td width="50%" align="center"><img src="assets/03-capsule-states.png" alt="Know before it compacts"><br><sub>Know before it compacts</sub></td>
  </tr>
</table>

---

## Quick Start

### 1. Desktop Client (Windows)

**One-line setup (recommended)**: open PowerShell and paste this line. It downloads `setup.ps1`, which checks for [Node.js](https://nodejs.org/) 18+ (and offers to install the LTS version with `winget` if it is missing), fetches the latest `install-context-capsule.js` and `context-capsule-payload.js` into `%USERPROFILE%\.gemini\tools\`, then patches `app.asar` in ~20ms and restarts Antigravity cleanly:

```powershell
$s = "$env:TEMP\agy-pulse-setup.ps1"; Invoke-WebRequest "https://github.com/akelabs/antigravity-pulse/releases/latest/download/setup.ps1" -OutFile $s -UseBasicParsing; powershell -NoProfile -ExecutionPolicy Bypass -File $s
```

> Antigravity restarts during setup, so save your work first. If you are connected through remote desktop, the connection will drop.

Prefer clicking? Download `setup.ps1`, `setup.cmd`, `install-context-capsule.js` and `context-capsule-payload.js` from **[Releases](../../releases)** into one folder and double-click `setup.cmd` (add `-Offline` to `setup.ps1` to use those local files without downloading).

Check status in `< 5ms` (useful after silent Antigravity client updates). Run the same line with `-Action check` at the end:

```powershell
$s = "$env:TEMP\agy-pulse-setup.ps1"; Invoke-WebRequest "https://github.com/akelabs/antigravity-pulse/releases/latest/download/setup.ps1" -OutFile $s -UseBasicParsing; powershell -NoProfile -ExecutionPolicy Bypass -File $s -Action check
```

<details>
<summary>Manual setup (if you'd rather not run the script)</summary>

1. Install [Node.js](https://nodejs.org/) LTS (18 or newer), or run `winget install OpenJS.NodeJS.LTS`. Open a new PowerShell window and check with `node --version`.
2. Create `%USERPROFILE%\.gemini\tools\` and put `install-context-capsule.js` and `context-capsule-payload.js` from **[Releases](../../releases)** in it.
3. Run:

```powershell
node "$HOME\.gemini\tools\install-context-capsule.js"            # install or update
node "$HOME\.gemini\tools\install-context-capsule.js" --check    # status
```

</details>

### 2. Browser Web Client (Chrome / Edge / Tampermonkey)

Every desktop setup run also writes the browser version to `%USERPROFILE%\.gemini\tools\antigravity-pulse-web\`. Only need the web version? Run the setup line with `-Action web` on Windows, or on macOS / Linux run `bash setup.sh` from **[Releases](../../releases)** (needs Node.js 18+; it writes to `~/.gemini/tools/antigravity-pulse-web/`).

- **Chrome / Edge Extension (Recommended)**: open `chrome://extensions/` (or `edge://extensions/`), enable **Developer mode**, click **Load unpacked**, and select the `antigravity-pulse-web` folder.
- **Tampermonkey**: Install `antigravity-pulse.user.js` from that folder into Tampermonkey. The userscript runs on Google-hosted Antigravity pages only; for a web client served from `localhost` / `127.0.0.1`, use the extension.

> **Zero Noise Guarantee**: On non-Antigravity pages (Gmail, Google Search, Docs, YouTube, or unrelated `localhost` apps), the script exits immediately without hooking `fetch` or running background timers.

### 3. Uninstall

Remove the hook and restore the original `preload.js` (Antigravity restarts):

```powershell
$s = "$env:TEMP\agy-pulse-setup.ps1"; Invoke-WebRequest "https://github.com/akelabs/antigravity-pulse/releases/latest/download/setup.ps1" -OutFile $s -UseBasicParsing; powershell -NoProfile -ExecutionPolicy Bypass -File $s -Action uninstall
```

Or, with the files already in place: `node "$HOME\.gemini\tools\install-context-capsule.js" --uninstall`.

In the browser, remove the extension from `chrome://extensions/` (or `edge://extensions/`), or delete the script in Tampermonkey.

---

## Privacy & Security

- **100% Local & Same-Origin**: Reads telemetry strictly from your own active Antigravity session in memory. Zero external network requests, zero analytics, zero telemetry servers.
- **Surgical `~20ms` Patch**: Updates only `dist/preload.js` inside `app.asar` in memory while keeping all other 760+ application files untouched.

---

## License

Copyright © 2026 Antigravity Pulse Authors. Release files are provided free for personal and developer use.

Antigravity Pulse is an independent community project and is not affiliated with or endorsed by Google.
