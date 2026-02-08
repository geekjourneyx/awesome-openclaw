# OpenClaw Browser Tool Complete Guide

[**English**](browser-guide.md) | [**简体中文**](browser-guide-zh.md)

---

## Table of Contents

- [Overview](#overview)
- [How It Works](#how-it-works)
- [Browser Modes Comparison](#browser-modes-comparison)
- [Ubuntu/Linux Installation](#ubuntulinux-installation)
- [Configuration](#configuration)
- [Configuration File Details](#configuration-file-details)
- [CLI Command Reference](#cli-command-reference)
- [Snapshots and Refs](#snapshots-and-refs)
- [Advanced Features](#advanced-features)
- [Remote Browser](#remote-browser)
- [Chrome Extension Mode](#chrome-extension-mode)
- [Security Best Practices](#security-best-practices)
- [Troubleshooting](#troubleshooting)

---

## Overview

The OpenClaw Browser tool allows AI Agents to control a dedicated browser instance, enabling:

-  Access to modern websites that require JavaScript
-  Simulate real user actions (click, type, scroll)
-  Screenshots, snapshots, PDF exports
-  Handle login forms and CAPTCHAs
-  Bypass anti-bot detection

### Core Features

| Feature | Description |
|---------|-------------|
| **Isolated Execution** | Separate browser profile, doesn't affect personal browsing |
| **Multi-profile Support** | Multiple named profiles (openclaw, work, remote, etc.) |
| **Deterministic Control** | Precise tab management (list/open/focus/close) |
| **Agent Actions** | Click, type, drag, select, etc. |
| **Snapshot Feature** | AI-optimized page structure analysis |
| **Multi-browser** | Chrome, Brave, Edge, Chromium |

---

## How It Works

```
┌─────────────────────────────────────────────────────────────────┐
│                         OpenClaw Gateway                         │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │              Browser Control Service                       │  │
│  │                   (Loopback Only)                          │  │
│  └───────────────────────┬───────────────────────────────────┘  │
│                          │ CDP Protocol                         │
└──────────────────────────┼──────────────────────────────────────┘
                           │
         ┌─────────────────┴─────────────────┐
         │                                   │
         ▼                                   ▼
┌─────────────────┐                 ┌─────────────────┐
│  openclaw Profile│                 │  Chrome Profile  │
│  (Managed)       │                 │  (Extension Relay)│
│  Port: 18800    │                 │  Port: 18792    │
│  Color: Orange  │                 │  Color: Blue    │
└─────────────────┘                 └─────────────────┘
```

### Technical Architecture

```
Agent Request
      │
      ▼
┌─────────────────┐
│  Browser Tool   │
└────────┬────────┘
         │
         ▼
┌─────────────────────────────┐
│   HTTP Control Server       │
│   (localhost:18791)         │
└────────┬────────────────────┘
         │
         ▼
┌─────────────────────────────┐
│   CDP (Chrome DevTools)     │
│   Protocol Communication    │
└────────┬────────────────────┘
         │
         ▼
┌─────────────────────────────┐
│      Playwright Layer       │
│   (Advanced Actions)        │
└────────┬────────────────────┘
         │
         ▼
┌─────────────────────────────┐
│   Chromium-based Browser    │
│   (Chrome/Brave/Edge)       │
└─────────────────────────────┘
```

---

## Browser Modes Comparison

### openclaw mode vs chrome mode

| Feature | **openclaw** (Managed) | **chrome** (Extension Relay) |
|---------|----------------------|------------------------------|
| **Isolation** | [PASS] Fully isolated | [FAIL] Shares your browser |
| **Profile** | Separate user directory | Uses existing profile |
| **Extension Required** | Not needed | Must install extension |
| **Launch Method** | Automatic | Manual attachment |
| **Best For** | Agent automation | Quick testing, debugging |
| **Default Port** | 18800 | 18792 |
| **Recommendation** |  |  |

### Selection Guide

```
Your Needs
    │
    ├──▶ Agent needs isolated browser? ──▶ openclaw mode
    │
    ├──▶ Test in existing browser?      ──▶ chrome mode
    │
    └──▶ Need remote control?           ──▶ remote CDP
```

---

## Ubuntu/Linux Installation

### Step 1: Install Chrome Browser

#### Download Chrome

```bash
# Download Chrome .deb package
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

#### Install Chrome

```bash
# Install using apt
sudo apt install ./google-chrome-stable_current_amd64.deb

# Or use dpkg
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt-get install -f  # Fix dependencies
```

#### Verify Installation

```bash
# Check Chrome version
google-chrome --version

# Expected output:
# Google Chrome 131.0.6778.86
```

### Step 2: Install Dependencies (Optional but Recommended)

```bash
# Install Playwright and browser dependencies
npx playwright install chromium

# Or use OpenClaw built-in method
docker compose run --rm openclaw-cli \
  node /app/node_modules/playwright-core/cli.js install chromium
```

### Step 3: Configure OpenClaw

Edit configuration file:

```bash
nano ~/.openclaw/openclaw.json
```

---

## Configuration

### Minimal Configuration (Ubuntu Recommended)

```json
{
  "browser": {
    "enabled": true,
    "headless": true,
    "noSandbox": true
  }
}
```

### Full Configuration Example

```json
{
  "browser": {
    "enabled": true,
    "headless": false,
    "noSandbox": false,
    "defaultProfile": "openclaw",
    "executablePath": "/usr/bin/google-chrome",
    "color": "#FF4500",
    "remoteCdpTimeoutMs": 1500,
    "remoteCdpHandshakeTimeoutMs": 3000,
    "attachOnly": false,
    "profiles": {
      "openclaw": {
        "cdpPort": 18800,
        "color": "#FF4500"
      },
      "work": {
        "cdpPort": 18801,
        "color": "#0066CC"
      },
      "browserless": {
        "cdpUrl": "https://production-sfo.browserless.io?token=YOUR_TOKEN",
        "color": "#00AA00"
      }
    }
  }
}
```

---

## Configuration File Details

### Key Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `enabled` | boolean | true | Enable browser tool |
| `headless` | boolean | false | Run without UI window |
| `noSandbox` | boolean | false | Required for Linux servers |
| `defaultProfile` | string | "chrome" | Default browser profile |
| `executablePath` | string | auto-detect | Browser executable path |
| `color` | string | "#FF4500" | Browser UI theme color |
| `attachOnly` | boolean | false | Only attach to running browser |

### Executable Paths by Platform

#### Linux

```json
{
  "browser": {
    "executablePath": "/usr/bin/google-chrome"
  }
}
```

Other path options:
- `/usr/bin/google-chrome-stable`
- `/usr/bin/brave-browser`
- `/usr/bin/microsoft-edge`
- `/usr/bin/chromium`

#### macOS

```json
{
  "browser": {
    "executablePath": "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
  }
}
```

#### Windows

```json
{
  "browser": {
    "executablePath": "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe"
  }
}
```

### Profile Configuration

```json
{
  "profiles": {
    "openclaw": {
      "cdpPort": 18800,           // CDP port
      "color": "#FF4500",          // Theme color
      "userDataDir": "/path/to/dir" // User data directory (optional)
    },
    "work": {
      "cdpPort": 18801,
      "color": "#0066CC"
    },
    "remote": {
      "cdpUrl": "http://10.0.0.42:9222",
      "color": "#00AA00"
    }
  }
}
```

---

## CLI Command Reference

### Basic Commands

All commands support `--browser-profile <name>` to specify a profile.

```bash
# Check browser status
openclaw browser status

# Start browser
openclaw browser start

# Stop browser
openclaw browser stop

# Use specific profile
openclaw browser --browser-profile openclaw status
```

### Tab Management

```bash
# List all tabs
openclaw browser tabs

# View current tab
openclaw browser tab

# Open new tab
openclaw browser tab new

# Open URL
openclaw browser open https://example.com

# Focus tab (by ID)
openclaw browser focus abcd1234

# Switch to Nth tab
openclaw browser tab select 2

# Close tab
openclaw browser close abcd1234
```

### Page Operations

```bash
# Navigate to URL
openclaw browser navigate https://example.com

# Resize window
openclaw browser resize 1280 720

# Reload page
openclaw browser reload

# Back/Forward
openclaw browser back
openclaw browser forward
```

### Interactive Actions

```bash
# Click element
openclaw browser click 12              # Using numeric ref
openclaw browser click e12             # Using role ref
openclaw browser click 12 --double     # Double click

# Type text
openclaw browser type 23 "hello"
openclaw browser type 23 "hello" --submit  # Type and submit

# Press keys
openclaw browser press Enter
openclaw browser press "Ctrl+A"

# Hover
openclaw browser hover 44

# Drag
openclaw browser drag 10 11

# Select dropdown
openclaw browser select 9 OptionA OptionB

# Scroll into view
openclaw browser scrollintoview e12
```

### Form Operations

```bash
# Fill form
openclaw browser fill --fields '[{"ref":"1","type":"text","value":"Ada"}]'

# Upload file
openclaw browser upload /tmp/file.pdf
openclaw browser upload --ref e12 /tmp/file.pdf

# Handle dialogs
openclaw browser dialog --accept
openclaw browser dialog --dismiss
```

### Screenshots and Snapshots

```bash
# Capture viewport
openclaw browser screenshot

# Full page screenshot
openclaw browser screenshot --full-page

# Capture specific element
openclaw browser screenshot --ref 12

# Get snapshot
openclaw browser snapshot

# ARIA format snapshot
openclaw browser snapshot --format aria

# Interactive snapshot (interactive elements only)
openclaw browser snapshot --interactive

# Snapshot with labels
openclaw browser snapshot --labels

# Limit depth
openclaw browser snapshot --depth 3

# Specify selector
openclaw browser snapshot --selector "#main"

# Snapshot within frame
openclaw browser snapshot --frame "iframe#main"
```

### Wait Operations

```bash
# Wait for text
openclaw browser wait --text "Done"

# Wait for selector
openclaw browser wait "#main"

# Wait for URL
openclaw browser wait --url "**/dash"

# Wait for load state
openclaw browser wait --load networkidle

# Wait for JavaScript condition
openclaw browser wait --fn "window.ready===true"

# Combined wait
openclaw browser wait "#main" \
  --url "**/dash" \
  --load networkidle \
  --timeout-ms 15000
```

### State Management

```bash
# Cookies
openclaw browser cookies
openclaw browser cookies set session abc123 --url "https://example.com"
openclaw browser cookies clear

# Storage
openclaw browser storage local get
openclaw browser storage local set theme dark
openclaw browser storage session clear

# Offline mode
openclaw browser set offline on
openclaw browser set offline off

# Custom headers
openclaw browser set headers --json '{"X-Debug":"1"}'
openclaw browser set headers --clear

# Basic auth
openclaw browser set credentials user pass
openclaw browser set credentials --clear

# Geolocation
openclaw browser set geo 37.7749 -122.4194 --origin "https://example.com"
openclaw browser set geo --clear

# Media preference
openclaw browser set media dark

# Timezone and locale
openclaw browser set timezone America/New_York
openclaw browser set locale en-US

# Device emulation
openclaw browser set device "iPhone 14"
```

### Debugging

```bash
# View console logs
openclaw browser console --level error

# View errors
openclaw browser errors --clear

# View network requests
openclaw browser requests --filter api --clear

# Export PDF
openclaw browser pdf

# Response body
openclaw browser responsebody "**/api" --max-chars 5000

# Highlight element
openclaw browser highlight e12

# Tracing
openclaw browser trace start
# ... perform actions ...
openclaw browser trace stop
```

### JSON Output

All commands support `--json` for scripting:

```bash
openclaw browser status --json
openclaw browser snapshot --interactive --json
openclaw browser tabs --json
```

---

## Snapshots and Refs

### Snapshot Types

OpenClaw supports two snapshot modes:

#### 1. AI Snapshot (Default)

```bash
openclaw browser snapshot
# Or explicitly
openclaw browser snapshot --format ai
```

**Features**:
- Returns text snapshot with numeric refs
- Ref format: `[1]`, `[2]`, `[3]`...
- Action commands: `click 1`, `type 2 "text"`

**Output Example**:
```
[1] button "Search"
[2] textbox "Username"
[3] textbox "Password"
[4] button "Login"
```

#### 2. Role Snapshot (Interactive)

```bash
openclaw browser snapshot --interactive
```

**Features**:
- Returns role-based list
- Ref format: `[ref=e12]`, `[ref=e23]`...
- More stable for complex pages
- Action commands: `click e12`, `type e23 "text"`

**Output Example**:
```
button[name="Search"] [ref=e12]
textbox[name="Username"] [ref=e23]
textbox[name="Password"] [ref=e34]
button[name="Login"] [ref=e45]
```

### Snapshot Options

| Option | Description |
|--------|-------------|
| `--interactive` | Output only interactive elements |
| `--compact` | Compact format |
| `--depth N` | Limit depth |
| `--selector "#main"` | Limit to selector |
| `--frame "iframe#id"` | Snapshot within frame |
| `--labels` | Add screenshot labels |
| `--limit N` | Limit character count |

### Ref Usage Rules

```bash
# Numeric ref (AI snapshot)
openclaw browser snapshot
openclaw browser click 12
openclaw browser type 23 "hello"

# Role ref (role snapshot)
openclaw browser snapshot --interactive
openclaw browser click e12
openclaw browser type e23 "hello"
```

> [WARN] **Important**: Refs change after page navigation. Always get a fresh snapshot after navigation.

---

## Advanced Features

### File Downloads

```bash
# Download file
openclaw browser download e12 /tmp/report.pdf

# Wait for download completion
openclaw browser waitfordownload /tmp/report.pdf
```

### JavaScript Execution

```bash
# Execute JavaScript
openclaw browser evaluate --fn '(el) => el.textContent' --ref 7

# Execute arbitrary code (requires enablement)
openclaw browser evaluate --fn 'document.title'
```

### Device Emulation

```bash
# Emulate iPhone
openclaw browser set device "iPhone 14"

# Emulate iPad
openclaw browser set device "iPad Pro"

# Custom viewport
openclaw browser set viewport 1280 720
```

### Geolocation Spoofing

```bash
# Set location (San Francisco)
openclaw browser set geo 37.7749 -122.4194

# Set location with origin
openclaw browser set geo 35.6762 139.6503 --origin "https://example.com"

# Clear location
openclaw browser set geo --clear
```

---

## Remote Browser

### Browserless (Hosted Service)

```json
{
  "browser": {
    "enabled": true,
    "defaultProfile": "browserless",
    "profiles": {
      "browserless": {
        "cdpUrl": "https://production-sfo.browserless.io?token=YOUR_TOKEN",
        "color": "#00AA00"
      }
    }
  }
}
```

### Remote CDP

```json
{
  "browser": {
    "profiles": {
      "remote": {
        "cdpUrl": "http://10.0.0.42:9222",
        "color": "#00AA00"
      }
    }
  }
}
```

### Authenticated Remote CDP

```json
{
  "browser": {
    "profiles": {
      "remote": {
        "cdpUrl": "https://user:pass@remote.example.com",
        "color": "#00AA00"
      }
    }
  }
}
```

---

## Chrome Extension Mode

### Install Extension

```bash
# 1. Generate extension files
openclaw browser extension install

# 2. Open Chrome extensions page
# chrome://extensions

# 3. Enable "Developer mode"

# 4. Click "Load unpacked"

# 5. Select the directory from command output
openclaw browser extension path
```

### Using Extension Mode

```bash
# 1. Start OpenClaw
openclaw gateway start

# 2. Click extension icon on the tab you want to control

# 3. Use chrome profile
openclaw browser --browser-profile chrome tabs
```

### Create Custom Extension Profile

```bash
openclaw browser create-profile \
  --name my-chrome \
  --driver extension \
  --cdp-url http://127.0.0.1:18792 \
  --color "#00AA00"
```

---

## Security Best Practices

### Isolation Guarantees

| Security Measure | Description |
|------------------|-------------|
| **Separate User Dir** | Doesn't touch personal browser profile |
| **Dedicated Ports** | Avoids conflicts with dev workflows (avoids 9222) |
| **Deterministic Tab Control** | Control tabs by targetId, not "last tab" |

### Security Configuration Recommendations

```json
{
  "browser": {
    "enabled": true,
    "headless": true,
    "noSandbox": true,
    "evaluateEnabled": false
  }
}
```

### Security Checklist

- [ ] Browser profile doesn't contain sensitive login data
- [ ] Gateway listens only on loopback
- [ ] Remote CDP uses HTTPS and short-lived tokens
- [ ] Disable unnecessary `evaluateEnabled`
- [ ] Restrict browser access in sandboxed sessions
- [ ] Regularly clear browser data

---

## Troubleshooting

### Linux Common Issues

#### Problem: Browser won't start

**Solution**:

```json
{
  "browser": {
    "noSandbox": true,
    "headless": true
  }
}
```

#### Problem: Missing dependencies

**Solution**:

```bash
# Install Chrome dependencies
sudo apt-get install \
  libnss3 \
  libatk1.0-0 \
  libatk-bridge2.0-0 \
  libcups2 \
  libdrm2 \
  libxkbcommon0 \
  libxcomposite1 \
  libxdamage1 \
  libxfixes3 \
  libxrandr2 \
  libgbm1 \
  libasound2
```

### Common Errors

#### "Browser disabled"

**Cause**: Browser tool not enabled

**Solution**:
```json
{
  "browser": {
    "enabled": true
  }
}
```

#### "Playwright is not available"

**Cause**: Playwright not installed

**Solution**:
```bash
npx playwright install chromium
```

#### "not visible" / "strict mode violation"

**Solution**:
```bash
# Get fresh snapshot
openclaw browser snapshot --interactive

# Highlight element to see location
openclaw browser highlight e12
```

---

## Quick Reference

### Common Workflows

#### Search and Screenshot

```bash
openclaw browser open https://google.com
openclaw browser snapshot --interactive
openclaw browser type e12 "OpenClaw"
openclaw browser press Enter
openclaw browser wait --load networkidle
openclaw browser screenshot --full-page
```

#### Fill Form

```bash
openclaw browser snapshot --interactive
openclaw browser type e12 "myemail@example.com"
openclaw browser type e23 "mypassword"
openclaw browser click e34
```

#### Debug Workflow

```bash
# Start tracing
openclaw browser trace start

# Perform actions
openclaw browser navigate https://example.com
openclaw browser snapshot
openclaw browser click e12

# Stop tracing
openclaw browser trace stop
```

---

<div align="center">

**Need help?** [Discord Community](https://discord.gg/openclaw) | [GitHub Issues](https://github.com/openclaw/openclaw/issues)

[ Back to Top](#openclaw-browser-tool-complete-guide)

</div>
