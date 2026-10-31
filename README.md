<div align="center">

<img src="assets/readme/hero.png" width="880" alt="Now Bar in the Windows taskbar with music, Claude Code, scores, timers, Galaxy Buds, weather, and battery indicators">

# Now Bar

**Samsung One UI-inspired Now Bar for the Windows 11 and Windows 10 taskbar.**

A compact pill in your taskbar shows what's happening right now: your music, timers, live scores, Claude Code activity, and more. Click the pill to expand a detailed card.

**[Download / Build](#installation)** &nbsp;·&nbsp;
[Features](#features) &nbsp;·&nbsp;
[Installation](#installation) &nbsp;·&nbsp;
[Build](#build)

<sub>English is the default language for new installations in this fork. English and Turkish are supported in the app.</sub>

</div>

<br>

> **Fork notice:** This is an independently maintained fork of [nixesty/now-bar](https://github.com/nixesty/now-bar). The Download link above points to the original project's releases, not a separately built release of this fork. Source changes in this fork are **not** included in the upstream installer.

## Features

<img src="assets/readme/pills.png" width="880" alt="Taskbar pills for music, Claude Code permissions, live scores, timers, weather, battery, Galaxy Buds, screen time, and more">

- **Music:** Works with apps that create a Windows media session, including Spotify, YouTube Music, and browsers. The pill adapts to the album artwork's colors. The card shows album art, a progress bar, synchronized lyrics when available, and audio-output selection.
- **Weekly music:** See your most-played songs and artists over the past seven days, accessible from the Digital Wellbeing card or system-tray menu.
- **Timer and stopwatch:** Start from the picker (`Win+Alt+N`) or system-tray menu. An alarm sounds and a card opens when the timer ends.
- **Live sports:** Follow football and basketball teams with scores, match time, and scoring events.
- **Weather:** Shows the time and current weather when idle, with alerts for rain, snow, storms, strong winds, extreme temperatures, frost, and poor air quality. Windows location can be used if no city is selected.
- **Galaxy Buds:** See the battery levels of both earbuds and the case, and manage noise controls. Select which pair to display when multiple devices are connected.
- **Claude Code:** See current activity and task progress. Permission requests display **Allow / Deny** buttons in the card (see [Live activities](#live-activities)).
- **Ask Claude:** Press `Win+Alt+C` or Ctrl-click the pill to ask a question and stream a response from a locally installed and authenticated Claude Code CLI (run `claude` and then `/login` once).
- **Games:** Monitor Steam and Epic Games downloads, updates, pauses, and playtime.
- **Digital Wellbeing:** Review daily screen time, application usage, and personal limits.
- **Shelf:** Drop files onto the pill and retrieve them later. Newly captured screenshots can be collected automatically. Actions include **Share**, **Copy text**, **Resize** (JPEG up to 1920 pixels on the longest side), and **ZIP**.
- **File operations:** Show Explorer copy, move, and delete progress, including percentage and time remaining, even if the Explorer window is minimized.
- **While you were away:** After your PC has been locked for at least one minute, review completed Claude tasks, goals, weather alerts, completed downloads, timers, and app notifications received while you were away.
- **System status:** Volume, charging, low battery, microphone/camera use, and Bluetooth headset battery level.

When multiple activities are active, thin indicators appear beside the foreground pill. Scroll over the pill to switch between them. The pill hides during fullscreen games, videos, or presentations; urgent permission requests and weather alerts may instead appear in a small floating island.

## Cards

<img src="assets/readme/music.png" width="880" alt="Music card with playback controls, progress bar, lyrics, and the taskbar music pill">

Cards expand from the pill and collapse when the pointer leaves. The music card includes a **Lyrics** button. Lyrics come from `lrclib.net`, with NetEase Cloud Music and Kugou as fallbacks. When synchronized lyrics are available, they follow playback. Lyrics for altered-speed tracks may be adjusted from the original version when no exact match is found.

<img src="assets/readme/day.png" width="880" alt="Daily briefing, timer, hourly weather, and Digital Wellbeing cards">

Click the clock while idle to open a daily briefing with the weather, sports, most recent music, and screen time.

<img src="assets/readme/live.png" width="880" alt="Live sports, Claude Code permission, Galaxy Buds, and file shelf cards">

## Settings

<img src="assets/readme/settings.png" width="880" alt="Settings window showing navigation, pill and card previews, and appearance settings">

Open **Settings** from the system-tray menu. Changes are reflected immediately. The search box helps locate settings, teams, and fullscreen exceptions. Use `Ctrl+F` to search and `Ctrl+K` to open the action menu.

The application supports **English and Turkish**. Change the language in **Settings → General → Language**. New installations of this fork default to **English**; an existing saved language preference is retained.

## Installation

<img src="assets/readme/setup.png" width="480" align="right" alt="Now Bar installer window">

There is currently **no installer published for this fork**. The [original developer's release](https://github.com/nixesty/now-bar/releases/latest) provides `nowbar-setup.exe`, but does not include this fork's changes. To run this fork specifically, [build it from source](#build).

The upstream installer does not require administrator privileges and installs by default to `%LOCALAPPDATA%\Programs\NowBar`. Its options include:

- **Start with Windows**
- **Connect to Claude Code**, if installed
- **Enable app notifications** (off by default; authorizations are your responsibility)
- **Create desktop shortcut**
- **Open Settings when finished**

A Start Menu shortcut is always created. The installed application appears in **Windows Settings → Apps** and can be uninstalled there. Uninstalling removes shortcuts, autostart registration, and Claude Code hooks; removal of saved preferences is optional.

<br clear="right">

Re-running an installer can repair or update an installation. Upstream silent commands are:

```powershell
nowbar-setup.exe --quiet
"%LOCALAPPDATA%\Programs\NowBar\nowbar-setup.exe" --uninstall --quiet
```

## Controls

| Action | Shortcut or interaction |
|---|---|
| Open or close a card | Click the pill; it also closes when the pointer leaves |
| Play or pause media | Middle-click the pill |
| Switch activities | Scroll over the pill or card |
| Timer picker | `Win+Alt+N` (customizable under General) |
| Ask Claude | `Win+Alt+C` or Ctrl-click the pill |
| Select shelf files | Click a file; click again to deselect |
| System-tray menu | Daily briefing, Digital Wellbeing, weekly music, Ask Claude, timer, stopwatch, autostart, Settings, and Exit |

## Live activities

Other local programs or scripts can publish activities to the pill. If Now Bar is not running, the command exits without output:

```powershell
nowbar.exe push --id backup --title "Backup" --text "Documents" --progress 40
nowbar.exe push --id backup --title "Backup" --state done
```

The `--state` argument accepts `run` (default), `wait` (temporarily prioritized), `done` (shown briefly), and `clear` (remove immediately). Activities expire after one hour without updates. This feature can be disabled in **Settings → System → Live Activities**.

**Local application API:** Other applications may create notifications and live activities using a local HTTP API, with their own icons, colors, and up to two buttons. It is disabled by default and reachable only from this computer (typically `127.0.0.1:20034`). The first request from an app requires user approval. See [DOCS.md](DOCS.md) for the developer interface.

```powershell
curl -H "Content-Type: application/json" -d "{\"app\":\"Backup Tool\",\"title\":\"Backup completed\"}" http://127.0.0.1:20034/v1/notify
```

**Claude Code:** The setup option can install hooks, or they can be managed manually:

```powershell
nowbar.exe claude-hooks install
nowbar.exe claude-hooks remove
```

The hook installer backs up Claude settings before changing them and does not alter unrelated hooks. The pill shows Claude's current task and elapsed time; the card may show the task checklist, recent steps, and response summary. Permission prompts can be answered from the card only when the complete request fits within its supported display; longer commands, plans, and complex prompts must be handled in Claude's own interface. A card approval request times out after 60 seconds. Hooks are refreshed when the application's executable location changes.

## Build

Build on **Windows** with the **Rust MSVC toolchain**. The Windows SDK (`rc.exe`) is needed to embed custom executable icons; without it, the executable may use the default Windows icon.

```powershell
cargo b
```

`cargo b` is configured in `.cargo\config.toml` as a quick-profile build for both `nowbar` and `nowbar-settings`. It favors faster iteration over full link-time optimization.

Output is written to `target\quick\`. Keep the project in a short filesystem path to avoid Windows/MSVC path-length issues. **Keep `nowbar.exe` and `nowbar-settings.exe` in the same directory**, as the application looks for the settings executable beside itself.

**Release packaging:** The Windows PowerShell script creates `dist\nowbar-setup.exe`, `nowbar.exe`, `nowbar-settings.exe`, and `SHA256SUMS.txt`:

```powershell
powershell -ExecutionPolicy Bypass -File tools\make-setup.ps1 -Jobs 6
```

Release builds use the `target\dist\` directory. `-Jobs` sets build parallelism; `-Clean` removes release build artifacts. Verify executable hashes before distributing.

### Fonts

The pill and cards look for optional `OneUISans*.ttf` files under a nearby `fonts\` directory, then `Inter*.ttf`, and otherwise use Windows Segoe UI Variable. The Settings UI bundles Inter under its own SIL Open Font License.

## Local files

- Settings: `%APPDATA%\NowBar\settings.json`
- Logs: `%LOCALAPPDATA%\NowBar\log.txt`
- Team crests: `%LOCALAPPDATA%\NowBar\crests\`
- Weekly music history: `%LOCALAPPDATA%\NowBar\music.json` (last seven days)
- Shelf exports: stored beside their originals where possible, otherwise under `%LOCALAPPDATA%\NowBar\Raf\`

## Updates

The app checks GitHub for updates shortly after launch and then daily. When an update is available, the tray menu and About page offer to download the two executables, verify them against `SHA256SUMS.txt`, replace the existing copies, and restart.

**Important for this fork:** The original updater may point to upstream releases. Do not assume its update feed provides binaries built from this fork. Disable automatic update checks in Settings if you need to avoid replacing a custom build until fork-specific update settings are configured.

## Privacy and network access

External services used by optional features (no separate API key required):

- **Lyrics:** Track artist, title, and duration may be sent to `lrclib.net` and, if necessary, `music.163.com` or `lyrics.kugou.com`. Disable lyrics in Media settings.
- **Weather:** Selected city coordinates are sent to Open-Meteo. With **Use my location**, Windows location services provide coordinates; approximate coordinates may be sent to OpenStreetMap Nominatim to resolve a place name.
- **Sports:** ESPN and EuroLeague/EuroCup services provide fixtures, scores, and logos.
- **Updates:** GitHub's API is used to check for releases.
- **Ask Claude:** Questions are passed to the local Claude Code CLI, which may contact Anthropic using your authenticated session. The app does not separately send those questions to its own service.

Weekly music analysis stays local. **Copy text** uses Windows text recognition, and **Share** opens the Windows share interface.

When application notifications are enabled, a local service listens on loopback addresses only (typically `127.0.0.1:20034` and `[::1]`). If the default port is unavailable, a dynamically assigned port is recorded under `%LOCALAPPDATA%\NowBar\api.json`. The service is not opened when notifications are disabled. See [DOCS.md](DOCS.md).

Screenshot monitoring can observe the ShareX screenshot folder and Windows Pictures/Screenshots folder. Claude activity and permission prompts may be visible on-screen, so take this into account when screen sharing.

## Known limitations

- Requires **Windows 11 or Windows 10 version 1809 or newer**. Windows 10 lacks certain backdrop effects; taskbar layout support has been tested less extensively.
- Galaxy Buds integration has been tested with Buds2. Other supported-family devices may work through the same protocol; original Galaxy Buds, Buds+, and Buds Live are not supported.
- Paused media disappears from the pill after a period of inactivity unless configured otherwise.
- Settings previews use sample content rather than current scores, timers, or battery measurements.
- The pill appears only on the primary display's taskbar.
- Restarting Windows Explorer should restore the pill, but any open card will close.

## Development

```powershell
cargo t
cargo lint
```

The repository uses Rust tooling and a Slint-based Settings UI. The `.cargo\config.toml` aliases run tests and linting using the quick profile. Additional developer tools include:

- `NOWBAR_SETTINGS_SHOTS=<folder> nowbar-settings.exe` for automated settings screenshots.
- `pillsheet`, `cardsheet`, `matchsheet`, `settingsheet` and `readmesheet` for preview assets.
- `tools\readme-images.py` to regenerate README artwork.
- `nowbar.exe --demo` for a sample timer.
- `tools\make-setup.ps1` for release packaging.

A release should increment the version in `Cargo.toml` and publish the files from `dist\` with matching checksums.

## License and credits

Now Bar is licensed under the **[GNU General Public License version 3](LICENSE) (GPL-3.0)**. You may use, modify, and redistribute the source, provided the relevant GPL requirements are met. Original copyright: **© 2026 Kaan**.

Third-party components have their own ownership or licenses:

- Galaxy Buds noise-control icons originate from Samsung's SystemUI.
- The Claude symbol belongs to Anthropic.
- Inter font in the Settings UI is available under the SIL Open Font License (`ui\fonts\Inter-OFL.txt`).

---

<sub>Not affiliated with Samsung. This is an independent project inspired by One UI; Galaxy Buds icons belong to Samsung. Fork maintained by BaselGhanem.</sub>
