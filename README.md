# Outpost Beacon

A macOS menu bar app that monitors the health of key infrastructure services at a glance.

<p align="center">
  <img src="screenshots/dashboard.png" width="420" alt="Status Dashboard">
</p>

## Features

- **Menu bar status icon** — changes color and shape to reflect overall service health
- **Status dashboard** — floating panel shows all monitored services with component-level detail
- **Dashboard pinning** — pin the dashboard to keep it visible when clicking outside
- **Service drill-down** — click any service to see components, active incidents, and recent event history
- **Downtime duration** — non-operational services show how long they've been in their current state
- **Global keyboard shortcut** — configurable hotkey to toggle the dashboard
- **Launch at Login** — optional automatic startup when you log in
- **Background polling** — refreshes service status on a configurable interval, with optional per-script intervals
- **macOS notifications** — opt-in alerts when a service degrades or recovers
- **Response time tracking** — measures and displays service response time
- **Uptime history** — opt-in graphical timeline showing historical uptime and response times per service
- **Uptime report export** — export uptime data as CSV or PDF
- **Custom status checks** — write JavaScript scripts to monitor any service using the built-in API
- **Built-in script editor** — create and debug check scripts with syntax highlighting, templates, and a run console
- **Dark mode support** — adapts to system appearance

<p align="center">
  <img src="screenshots/general.png" width="420" alt="Settings — General">
  &nbsp;&nbsp;
  <img src="screenshots/scripts.png" width="420" alt="Settings — Scripts">
</p>

<p align="center">
  <img src="screenshots/script-editor.png" width="600" alt="Script Editor">
</p>

<p align="center">
  <img src="screenshots/new-script.png" width="420" alt="New Script Wizard">
  &nbsp;&nbsp;
  <img src="screenshots/uptime-report.png" width="420" alt="Uptime History">
</p>

## Requirements

- macOS 14.0 (Sonoma) or later

## Installation

Outpost Beacon is available on the [Mac App Store](https://apps.apple.com/app/outpost-beacon/id6772313616).

## Custom Status Checks

Outpost Beacon runs JavaScript check scripts using Apple's built-in JavaScriptCore engine — no external runtime required. Scripts live in:

```
~/Library/Application Support/Outpost Beacon/checks/
```

### Script Metadata

| Comment Directive | Required | Description |
|-------------------|----------|-------------|
| `// OUTPOST_NAME = "My Service"` | No | Display name (defaults to filename without extension) |
| `// OUTPOST_URL = "https://..."` | No | Status page URL for the service |
| `// OUTPOST_INTERVAL = "60"` | No | Custom polling interval in seconds (minimum 30) |
| `// OUTPOST_ICON = "cloud.fill"` | No | SF Symbol name for the service icon in the dashboard. Use the icon picker in the script editor or any [SF Symbol name](https://developer.apple.com/sf-symbols/). |
| `// OUTPOST_COLOR = "teal"` | No | Icon color. Options: red, blue, green, orange, purple, teal, pink, indigo, mint, yellow, brown, cyan, gray. |

### Available Functions

| Function | Description |
|----------|-------------|
| `fetch(url)` | HTTP GET → parsed JSON object |
| `fetch(url, {insecure: true})` | HTTP GET accepting self-signed certificates |
| `fetchResponse(url)` | HTTP GET → `{ status, body }` |
| `fetchResponse(url, {insecure: true})` | Same, accepting self-signed certificates |
| `fetchText(url)` | HTTP GET → raw response string |
| `fetchText(url, {insecure: true})` | Same, accepting self-signed certificates |
| `fetchAll([url1, url2, ...])` | Concurrent HTTP GET → array of parsed JSON |
| `output(obj)` | Set the script's result (required, call once) |
| `statuspageCheck(url)` | One-liner for any Statuspage.io service |
| `tcpCheck(host, port, options?)` | TCP connect check → `{ success, latencyMs, error }` |
| `stripHtml(text)` | Remove HTML tags and decode entities |
| `log(message)` | Debug logging |

### Example: Statuspage.io Service

```javascript
// OUTPOST_NAME = "GitHub"
// OUTPOST_URL = "https://www.githubstatus.com"

statuspageCheck("https://www.githubstatus.com/api/v2/summary.json");
```

### Example: HTTP Health Check

```javascript
// OUTPOST_NAME = "My API"
// OUTPOST_URL = "https://api.example.com"

try {
    fetch("https://api.example.com/health");
    output({ status: "operational" });
} catch (e) {
    output({ status: "major_outage" });
}
```

### Example: TCP Port Check

```javascript
// OUTPOST_NAME = "Database"


var result = tcpCheck("db.example.com", 5432, { timeout: 3 });
output({
    status: result.success ? "operational" : "major_outage",
    responseTimeMs: result.latencyMs
});
```

### Example: Self-Signed Certificate

Pass `{insecure: true}` to accept self-signed or untrusted certificates — useful for internal services, NAS devices, and development servers:

```javascript
// OUTPOST_NAME = "My NAS"
// OUTPOST_URL = "https://nas.local:8443"

var data = fetch("https://nas.local:8443/api/health", {insecure: true});
output({ status: data.ok ? "operational" : "major_outage" });
```

For more examples and the full scripting reference, see the [Scripting Guide](docs/scripting.md).

## Support

- **Support page** — [Outpost Beacon Support](https://mcherry.github.io/Outpost-Beacon/support.html)
- **Bug reports & feature requests** — [open an issue](https://github.com/mcherry/Outpost-Beacon/issues)
- **Email** — info@inditech.org

## Privacy

Outpost Beacon does not collect, transmit, or share any personal data. See the full [Privacy Policy](https://mcherry.github.io/Outpost-Beacon/privacy.html).

## License

Copyright © 2025 Mike Cherry. All rights reserved.
