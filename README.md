# Tool Call Viewer

Web UI for browsing OpenClaw session tool call history.

![Screenshot](screenshot.png)

## ⚠️ Security Warning

**Do not expose this to the public internet.** Session logs contain your full command history, file paths, API responses, and potentially sensitive data. This tool is designed for **local network use only**.

## Features

- **Dynamic parsing** of JSONL session files
- **Filter by:** date range, tool type (multi-select), session, text search
- **Sort by:** date, tool name, session, model (with toggle direction)
- **Copy rows** as JSON (double-click or 📋 button)
- **Auto-refresh** — poll for new calls every 10 seconds
- **Network accessible** — bind to `0.0.0.0` for LAN access
- **Customizable** — set your agent name via CLI or env var

## Requirements

- [Node.js](https://nodejs.org/) v18 or later

### Installing Node.js

**macOS (Homebrew):**
```bash
brew install node
```

**macOS/Linux (nvm - recommended):**
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install --lts
```

**Ubuntu/Debian:**
```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**Windows:**
Download installer from [nodejs.org](https://nodejs.org/)

## Installation

```bash
git clone https://github.com/youruser/toolcallviewer.git
cd toolcallviewer
node server.js
```

No npm install needed — zero dependencies, just Node.js.

## Usage

```bash
# Default (port 3847, OpenClaw sessions)
node server.js

# Custom port
node server.js --port 8080

# Custom sessions directory
node server.js --sessions /path/to/sessions

# Custom agent name (shows in title)
node server.js --name "🔳 TARS"

# Or via environment variable
OPENCLAW_AGENT_NAME="🤖 Jarvis" node server.js

# Demo mode (fake data for screenshots)
node server.js --demo

# Show help
node server.js --help
```

Then open http://localhost:3847 (or your machine's IP for LAN access).

## CLI Options

| Option | Short | Description | Default |
|--------|-------|-------------|---------|
| `--port` | `-p` | Port to listen on | `3847` |
| `--sessions` | `-s` | Path to sessions directory | `~/.openclaw/agents/main/sessions` |
| `--name` | `-n` | Agent name for title | `OpenClaw` (or `OPENCLAW_AGENT_NAME` env) |
| `--demo` | | Run with fake demo data | |
| `--help` | `-h` | Show help message | |

## API Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /` | Web UI |
| `GET /api/tools` | All tool calls as JSON array |
| `GET /api/stats` | Aggregated stats by tool type |

## Running as a Service

### macOS (launchd)

Create `~/Library/LaunchAgents/com.toolcallviewer.plist`:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.toolcallviewer</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/bin/node</string>
        <string>/path/to/toolcallviewer/server.js</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
</dict>
</plist>
```

Then:
```bash
launchctl load ~/Library/LaunchAgents/com.toolcallviewer.plist
```

### Linux (systemd)

Create `~/.config/systemd/user/toolcallviewer.service`:
```ini
[Unit]
Description=Tool Call Viewer

[Service]
ExecStart=/usr/bin/node /path/to/toolcallviewer/server.js
Restart=always

[Install]
WantedBy=default.target
```

Then:
```bash
systemctl --user enable toolcallviewer
systemctl --user start toolcallviewer
```

## License

MIT
