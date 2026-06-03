# Changelog

All notable changes to Outpost Beacon are documented here.

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
