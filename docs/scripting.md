# Scripting Guide

Watchtower uses JavaScript check scripts to monitor services. Scripts are executed via Apple's built-in JavaScriptCore engine — no external runtime needed.

## Script Location

Scripts live in:

```
~/Library/Application Support/Watchtower/checks/
```

On first launch, Watchtower copies default check scripts into this directory.

## Script Metadata

Add comment directives at the top of your script to configure how Watchtower handles it:

| Directive | Required | Description |
|-----------|----------|-------------|
| `// WATCHTOWER_NAME = "My Service"` | No | Display name (defaults to filename) |
| `// WATCHTOWER_URL = "https://..."` | No | Status page URL for the service |
| `// WATCHTOWER_INTERVAL = "60"` | No | Custom polling interval in seconds (minimum 30) |

## Available Functions

### HTTP Functions

| Function | Returns | Description |
|----------|---------|-------------|
| `fetch(url)` | JSON object | HTTP GET, parses response as JSON |
| `fetch(url, {encoding: "utf-16"})` | JSON object | HTTP GET with custom response encoding |
| `fetchResponse(url)` | `{status, body}` | HTTP GET with status code access |
| `fetchText(url)` | String | HTTP GET, returns raw text |
| `fetchAll([url1, url2, ...])` | Array | Concurrent HTTP GET, returns array of parsed JSON |

### Check Helpers

| Function | Returns | Description |
|----------|---------|-------------|
| `statuspageCheck(url)` | — | One-liner for Statuspage.io services (calls `output()` for you) |
| `statuspageCheck(url, {showcaseFilter: false})` | — | Same, but includes all components |
| `tcpCheck(host, port)` | `{success, latencyMs, error}` | TCP connection check |
| `tcpCheck(host, port, {timeout: 5})` | `{success, latencyMs, error}` | TCP check with custom timeout (seconds) |

### Utility Functions

| Function | Description |
|----------|-------------|
| `output(obj)` | Set the script's result **(required, call exactly once)** |
| `stripHtml(text)` | Remove HTML tags and decode entities |
| `log(message)` | Debug logging (visible in Console.app and the script editor console) |

## Status Values

Use these strings for `status`, component `status`, and incident `impact` fields:

| Value | Meaning |
|-------|---------|
| `operational` | All systems normal |
| `degraded` | Degraded performance |
| `partial_outage` | Partial outage |
| `major_outage` | Major outage |

## Output Schema

Only `status` is required. Everything else is optional:

```json
{
  "status": "operational",
  "responseTimeMs": 142,
  "components": [
    { "name": "API", "status": "operational", "description": "Optional detail" }
  ],
  "incidents": [
    {
      "title": "Elevated error rates",
      "status": "Investigating",
      "impact": "minor",
      "created_at": "2025-01-15T10:00:00Z",
      "is_active": true,
      "updates": [
        { "body": "Looking into it.", "status": "Investigating", "created_at": "2025-01-15T10:05:00Z" }
      ]
    }
  ]
}
```

## Examples

### Statuspage.io Service

```javascript
// WATCHTOWER_NAME = "GitHub"
// WATCHTOWER_URL = "https://www.githubstatus.com"

statuspageCheck("https://www.githubstatus.com/api/v2/summary.json");
```

### Simple Health Check

```javascript
// WATCHTOWER_NAME = "My API"
// WATCHTOWER_URL = "https://api.example.com"

try {
    fetch("https://api.example.com/health");
    output({ status: "operational" });
} catch (e) {
    output({ status: "major_outage" });
}
```

### HTTP Status Code Check

```javascript
// WATCHTOWER_NAME = "My API"
// WATCHTOWER_URL = "https://api.example.com"

var res = fetchResponse("https://api.example.com/health");

if (res.status === 503) {
    output({ status: "major_outage" });
} else if (res.status === 429 || res.status >= 500) {
    output({ status: "degraded" });
} else if (res.status === 200) {
    output({ status: "operational" });
} else {
    output({ status: "unknown" });
}
```

### TCP Port Check

```javascript
// WATCHTOWER_NAME = "Database Cluster"

var primary = tcpCheck("db-primary.example.com", 5432, { timeout: 3 });
var replica = tcpCheck("db-replica.example.com", 5432, { timeout: 3 });

var components = [
    {
        name: "Primary (db-primary:5432)",
        status: primary.success ? "operational" : "major_outage",
        description: primary.success ? "Latency: " + primary.latencyMs + "ms" : primary.error
    },
    {
        name: "Replica (db-replica:5432)",
        status: replica.success ? "operational" : "major_outage",
        description: replica.success ? "Latency: " + replica.latencyMs + "ms" : replica.error
    }
];

var status = "operational";
if (!primary.success) status = "major_outage";
else if (!replica.success) status = "partial_outage";

output({ status: status, components: components, responseTimeMs: primary.latencyMs });
```

### Custom API with Components

```javascript
// WATCHTOWER_NAME = "Internal Platform"
// WATCHTOWER_URL = "https://status.internal.example.com"

var data = fetch("https://status.internal.example.com/api/health");

var components = (data.services || []).map(function(svc) {
    return {
        name: svc.name,
        status: svc.healthy ? "operational" : "major_outage",
        description: svc.message || null
    };
});

var worst = "operational";
components.forEach(function(c) {
    if (c.status === "major_outage") worst = "major_outage";
    else if (c.status === "degraded" && worst === "operational") worst = "degraded";
});

output({ status: worst, components: components });
```

## Tips

- **Disable a check** — remove the `.js` file or rename its extension (e.g., `.js.disabled`)
- **Reorder checks** — rename files with numeric prefixes (`01-`, `02-`, etc.)
- **Debug a script** — use the built-in script editor's Run button, or use `log()` and check Console.app
- **Reload after edits** — go to Settings → Scripts → Reload Scripts, or restart the app
- **Script errors** — if a script throws or never calls `output()`, the service shows as "Unknown"
