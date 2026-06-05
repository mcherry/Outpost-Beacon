# Changelog

All notable changes to Outpost Beacon are documented here.

## [1.1] — Unreleased

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
