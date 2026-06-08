# Changelog

All notable changes to Outpost Beacon are documented here.

## [1.2] — Unreleased

### Added
- **Response Time Alerts** — set a per-service response time threshold and get a macOS notification when a service gets too slow; require several consecutive breaches to ignore transient spikes, with a built-in cooldown to prevent alert spam
- **Webhook & Automation** — trigger a webhook, Slack message, or Discord message when a service changes status or crosses a response time threshold, with configurable trigger conditions (any change, degradation, recovery, or outage) and a Test button to verify your endpoint
- **Self-Signed Certificate Support for Webhooks** — opt in to accept self-signed certificates on webhook automations pointed at internal services
- **Script Validation** — the script editor now flags common mistakes inline — missing `output()`, malformed URLs, intervals below the minimum, network calls without error handling, and more — with hover details and one-click fixes for several of them
- **Auto-Completion** — the editor suggests API functions and metadata directives as you type; press Tab or Return to accept
- **Inline API Documentation** — hover any built-in function in the editor to see its signature, parameters, return value, and an example
- **Network Request Log** — the debug console now lists every HTTP and TCP request a script makes during a run, with method, URL, status code, and timing

### Security
- Additional input validation and hardening across script handling, data export, and outbound requests

## [1.1] — 2026-06-05

### Added
- **Launch at Login** — new toggle in Settings → General to start Outpost Beacon automatically when you log in
- **Dashboard Pinning** — pin button in the dashboard header keeps the panel visible when clicking outside
- **Downtime Duration** — services in a non-operational state show how long they've been degraded (e.g. "Degraded for 12 min")
- **Duplicate Script** — new button in Settings → Scripts to clone an existing check script with a modified name
- **Comment Toggle** — Cmd+/ toggles line comments in the script editor
- **Line Operations** — Cmd+Shift+K deletes the current line, Cmd+Shift+D duplicates it, Option+Up/Down moves it
- **Auto-Close Brackets** — typing `(`, `{`, `[`, `"`, `'`, or `` ` `` auto-inserts the closing pair; backspace deletes empty pairs
- **Word Wrap Toggle** — toolbar button in the script editor to switch between word wrap and horizontal scrolling
- **Bracket Matching** — matching brackets highlight in blue when the cursor is adjacent; unmatched brackets highlight in red
- **Self-Signed Certificate Support** — pass `{insecure: true}` to `fetch()`, `fetchResponse()`, or `fetchText()` to accept self-signed certificates
- **Cmd+W Close** — all windows (Settings, Uptime History, Script Editor) now close with Cmd+W
- **Custom Service Icons** — set a custom SF Symbol icon and color for any service via `OUTPOST_ICON` and `OUTPOST_COLOR` metadata directives, or use the icon picker in the script editor toolbar
- **Context Menu Shortcuts** — right-click in the script editor to access Toggle Comment, Delete Line, Duplicate Line, and Move Line operations
- **Keyboard Shortcut Reference** — keyboard icon button in the script editor toolbar shows a quick reference of all available shortcuts
- **Response Time Hover** — hover over the response time chart in Uptime History to see exact values with a smoothly tracking indicator

### Security
- Improved input validation and resource limits for script execution

### Fixed
- Line numbers no longer overflow into the debug console header when scrolling
- Fixed focus ring appearing on dashboard pin and refresh buttons

## [1.0] Build 2 — 2026-06-03

### Fixed
- Improved input validation and error handling across network requests, database operations, and script loading
- Resolved potential stability issues under edge-case conditions

### Changed
- Removed in-app update checker (updates are now handled through the Mac App Store)
- About tab now links to the support page

## [1.0] — 2026-05-22

### Added
- Initial release
- Menu bar status icon with color-coded health indicators
- Floating status dashboard with service drill-down
- Configurable global keyboard shortcut
- Background polling with configurable intervals
- Per-script custom polling intervals
- macOS notification alerts for status changes
- Response time tracking and display
- Uptime history with graphical timeline charts
- Uptime report export (CSV and PDF)
- JavaScript-based custom status checks via JavaScriptCore
- Built-in script editor with syntax highlighting
- New script wizard with templates (Statuspage.io, HTTP, TCP, Simple, Blank)
- Script debug console with run/test support
- Sleep/wake awareness with automatic refresh
- Default check scripts for GitHub, AWS, Azure DevOps, PagerDuty, Slack, Zendesk, OpenAI, and Claude
- Automatic update checking
- Dark mode support
