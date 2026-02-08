<div align="center">

<img src="assets/logo.png" alt="Awesome OpenClaw" width="480" />

#  Awesome OpenClaw

[![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)
[![Stars](https://img.shields.io/github/stars/geekjourneyx/awesome-openclaw?style=social)](https://github.com/geekjourneyx/awesome-openclaw)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/geekjourneyx/awesome-openclaw)

[**English**](README.md) | [**简体中文**](README_zh.md)

---

**The ultimate curated collection of OpenClaw resources, guides, skills, and best practices.**

[OpenClaw](https://openclaw.ai/) is an open agent platform that runs on your machine and works from the chat apps you already use.

[Getting Started](#getting-started) • [Installation](#installation) • [Skills](#skills) • [Security](#security) • [Community](#community)

</div>

---

## Table of Contents

- [Quick Overview](#quick-overview)
- [Getting Started](#getting-started)
  - [What is OpenClaw?](#what-is-openclaw)
  - [Key Features](#key-features)
  - [Supported Channels](#supported-channels)
- [Installation](#installation)
  - [System Requirements](#system-requirements)
  - [Installation Methods](#installation-methods)
  - [Configuration](#configuration)
- [Channels & Integrations](#channels--integrations)
- [Skills & Plugins](#skills--plugins)
- [Models & Providers](#models--providers)
- [Security Best Practices](#security-best-practices)
- [Tutorials & Guides](#tutorials--guides)
- [Troubleshooting](#troubleshooting)
- [Community Resources](#community-resources)
- [Contributing](#contributing)

---

## Quick Overview

> **"EXFOLIATE! EXFOLIATE!"** — A space lobster, probably

OpenClaw is an **open agent platform** that runs on your machine and integrates with your favorite chat applications. Unlike SaaS AI assistants, OpenClaw gives you **full control** over your data, infrastructure, and API keys.

### What Makes OpenClaw Different?

| Feature | OpenClaw | SaaS Alternatives |
|---------|----------|-------------------|
| **Data Privacy** | Your machine, your data | Data on external servers |
| **API Costs** | You pay providers directly | Subscription fees + markup |
| **Extensibility** | Open source, self-hostable | Closed ecosystem |
| **Channels** | WhatsApp, Telegram, Discord, and more | Limited options |
| **Custom Skills** | Full plugin system | Restricted capabilities |

---

## Getting Started

### What is OpenClaw?

OpenClaw (formerly Clawd/Moltbot) is an **AI agent gateway** that bridges Claude/GPT and other LLMs with messaging platforms. It runs locally or on your own infrastructure and connects to chat apps you already use daily.

### Key Features

-  **Multi-Channel Support**: WhatsApp, Telegram, Discord, Slack, Teams, Twitch, Google Chat, iMessage, and more
-  **Multiple Model Providers**: Anthropic Claude, OpenAI GPT, Google Gemini, KIMI, Xiaomi, and others
-  **Plugin System**: Create and share custom skills
-  **Self-Hosted**: Your infrastructure, your rules, your data
-  **Web Chat**: Built-in web interface with image support
-  **34+ Security Enhancements**: Hardened against prompt injection and common vulnerabilities

### Supported Channels

| Channel | Status | Notes |
|---------|--------|-------|
| **WhatsApp** | [PASS] Stable | Most popular option |
| **Telegram** | [PASS] Stable | Full feature support |
| **Discord** | [PASS] Stable | Slash commands supported |
| **Slack** | [PASS] Stable | Enterprise ready |
| **Microsoft Teams** | [PASS] Stable | Business integration |
| **Twitch** | [PASS] Stable | New in latest release |
| **Google Chat** | [PASS] Stable | New in latest release |
| **iMessage** | [PASS] Stable | macOS only |
| **Mattermost** | [PASS] Stable | Self-hosted chat |

---

## Installation

### System Requirements

- **OS**: Linux, macOS, or Windows (WSL2 recommended)
- **RAM**: Minimum 4GB, 8GB+ recommended
- **Disk**: 500MB free space
- **Network**: Stable internet connection for LLM API calls

### Installation Methods

#### Method 1: Quick Start (Recommended)

```bash
# Clone the repository
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# Run the installation script
./install.sh
```

#### Method 2: Docker

```bash
docker pull ghcr.io/openclaw/openclaw:latest
docker run -d --name openclaw \
  -v ~/.openclaw:/app/.openclaw \
  -p 8080:8080 \
  ghcr.io/openclaw/openclaw:latest
```

#### Method 3: Manual Installation

```bash
# Install Python 3.10+
python3 --version

# Install dependencies
pip install -r requirements.txt

# Copy and edit configuration
cp config.example.yaml config.yaml
nano config.yaml
```

### Configuration

OpenClaw uses a YAML configuration file. Key settings:

```yaml
# config.yaml
api:
  anthropic:
    api_key: "your-anthropic-api-key"
    model: "claude-3-5-sonnet-20241022"

  openai:
    api_key: "your-openai-api-key"
    model: "gpt-4o"

channels:
  whatsapp:
    enabled: true
    phone_number: "your-whatsapp-number"

  telegram:
    enabled: true
    bot_token: "your-telegram-bot-token"

security:
  allowed_users:
    - "user1@example.com"
    - "+1234567890"
```

---

## Channels & Integrations

### WhatsApp Setup

<details>
<summary>Click to expand WhatsApp setup guide</summary>

1. **Get WhatsApp Business API Access**
   - Sign up at [Meta for Developers](https://developers.facebook.com/)
   - Create a WhatsApp Business App
   - Get your Phone Number ID and Access Token

2. **Configure OpenClaw**
   ```yaml
   channels:
     whatsapp:
       enabled: true
       phone_number_id: "your-phone-number-id"
       access_token: "your-access-token"
       webhook_verify_token: "random-verify-token"
   ```

3. **Set Up Webhook**
   - Your webhook URL: `https://your-domain.com/webhook/whatsapp`
   - Subscribe to messages webhook

4. **Send a Test Message**
   ```
   Hello! How can I help you today?
   ```

</details>

### Telegram Setup

<details>
<summary>Click to expand Telegram setup guide</summary>

1. **Create a Telegram Bot**
   - Message [@BotFather](https://t.me/botfather) on Telegram
   - Send `/newbot`
   - Follow the prompts to name your bot
   - Save the API token

2. **Configure OpenClaw**
   ```yaml
   channels:
     telegram:
       enabled: true
       bot_token: "your-bot-token-here"
   ```

3. **Start Your Bot**
   ```bash
   python -m openclaw.cli --channel telegram
   ```

4. **Test the Bot**
   - Open your bot in Telegram
   - Send `/start` to begin

</details>

### Discord Setup

<details>
<summary>Click to expand Discord setup guide</summary>

1. **Create Discord Application**
   - Go to [Discord Developer Portal](https://discord.com/developers/applications)
   - Create a new application
   - Enable bot functionality

2. **Configure Bot Permissions**
   - Required permissions:
     - Read Messages/View Channels
     - Send Messages
     - Embed Links
     - Attach Files
     - Add Reactions

3. **Configure OpenClaw**
   ```yaml
   channels:
     discord:
       enabled: true
       bot_token: "your-discord-bot-token"
       command_prefix: "!"
   ```

4. **Invite Bot to Server**
   - Use OAuth2 URL generator in Developer Portal
   - Invite bot with required permissions

</details>

---

## Skills & Plugins

### What are Skills?

Skills are modular plugins that extend OpenClaw's capabilities. They can:
- Make API calls to external services
- Process and analyze data
- Automate repetitive tasks
- Integrate with third-party tools

### Popular Community Skills

####  Email & Calendar

| Skill | Description | Repository |
|-------|-------------|------------|
| **Gmail Integration** | Read, search, and send emails | `openclaw-skill-gmail` |
| **Calendar Manager** | Create events, check availability | `openclaw-skill-calendar` |
| **Outlook Bridge** | Microsoft Outlook integration | `openclaw-skill-outlook` |

####  File & Cloud

| Skill | Description | Repository |
|-------|-------------|------------|
| **Google Drive** | Search, upload, download files | `openclaw-skill-gdrive` |
| **Dropbox Handler** | Dropbox file operations | `openclaw-skill-dropbox` |
| **Notion Writer** | Create and edit Notion pages | `openclaw-skill-notion` |

####  Web & Research

| Skill | Description | Repository |
|-------|-------------|------------|
| **Web Search** | Google/Bing/DuckDuckGo search | `openclaw-skill-search` |
| **URL Summarizer** | Summarize web articles | `openclaw-skill-summary` |
| **Wikipedia Lookup** | Quick Wikipedia facts | `openclaw-skill-wiki` |

####  E-Commerce

| Skill | Description | Repository |
|-------|-------------|------------|
| **Amazon Search** | Product search and comparison | `openclaw-skill-amazon` |
| **Price Tracker** | Monitor price changes | `openclaw-skill-prices` |

####  Entertainment

| Skill | Description | Repository |
|-------|-------------|------------|
| **Spotify Control** | Control music playback | `openclaw-skill-spotify` |
| **YouTube Handler** | Search and manage videos | `openclaw-skill-youtube` |
| **Game Stats** | Fetch game statistics | `openclaw-skill-gaming` |

### Installing Skills

```bash
# Using the CLI
openclaw skills install <skill-name>

# Manual installation
git clone https://github.com/openclaw/skill-<name>.git
cp -r skill-<name> ~/.openclaw/skills/
```

### Creating Your Own Skills

<details>
<summary>Click to expand skill creation guide</summary>

1. **Create Skill Directory**
   ```bash
   mkdir ~/.openclaw/skills/my-skill
   cd ~/.openclaw/skills/my-skill
   ```

2. **Create skill.yaml**
   ```yaml
   name: "my-skill"
   version: "1.0.0"
   description: "My custom OpenClaw skill"
   author: "Your Name"
   triggers:
     - "my command"
     - "do something"
   ```

3. **Create main.py**
   ```python
   from openclaw import Skill

   class MySkill(Skill):
       def handle(self, message, context):
           return "Hello from my skill!"
   ```

4. **Register the Skill**
   ```bash
   openclaw skills register my-skill
   ```

5. **Test Your Skill**
   ```
   You: my command
   OpenClaw: Hello from my skill!
   ```

</details>

---

## Models & Providers

OpenClaw supports multiple LLM providers:

### Supported Providers

| Provider | Models | Status |
|----------|--------|--------|
| **Anthropic** | Claude 3.5 Sonnet, Claude 3 Opus/Haiku | [PASS] Native Support |
| **OpenAI** | GPT-4o, GPT-4-turbo, GPT-3.5 | [PASS] Native Support |
| **Google** | Gemini 1.5 Pro/Ultra | [PASS] Native Support |
| **KIMI** | KIMI 2.5 | [PASS] New in v2.0 |
| **Xiaomi** | MiMo-V2-Flash | [PASS] New in v2.0 |
| **OpenRouter** | 100+ models via API | [PASS] Supported |
| **Local Models** | Ollama, LM Studio | [PASS] Supported |

### Model Configuration

```yaml
api:
  primary: "anthropic"  # Default provider
  fallback: "openai"

  anthropic:
    api_key: "${ANTHROPIC_API_KEY}"
    model: "claude-3-5-sonnet-20241022"
    max_tokens: 8192
    temperature: 0.7

  openai:
    api_key: "${OPENAI_API_KEY}"
    model: "gpt-4o"
    max_tokens: 4096
```

### Model Selection Tips

| Use Case | Recommended Model | Why |
|----------|-------------------|-----|
| **Complex Reasoning** | Claude 3.5 Sonnet | Best for analysis |
| **Code Generation** | GPT-4o | Strong coding ability |
| **Quick Responses** | Claude 3 Haiku | Fast & cost-effective |
| **Chinese Language** | KIMI 2.5 | Optimized for Chinese |
| **Cost Sensitive** | GPT-3.5 / Haiku | Lowest cost |

---

## Security Best Practices

> [WARN] **Important**: Prompt injection remains an industry-wide unsolved problem. Always use strong models and follow security guidelines.

### Critical Security Principles

1. **Never expose API keys in chat**
   - Use environment variables for secrets
   - Never print keys in responses
   - Rotate keys regularly

2. **Implement Access Control**
   ```yaml
   security:
     allowed_users:
       - "verified@email.com"
       - "+1234567890"
     blocked_users:
       - "spam@example.com"
   ```

3. **Enable Rate Limiting**
   ```yaml
   security:
     rate_limit:
       requests_per_minute: 60
       cost_limit_per_hour: 10.0
   ```

4. **Sanitize All Inputs**
   - Validate user input before processing
   - Escape special characters
   - Check for command injection patterns

5. **Use HTTPS/Webhook Verification**
   ```yaml
   channels:
     whatsapp:
       webhook_verify_token: "random-secure-token"
   ```

### Security Checklist

- [ ] API keys stored in environment variables
- [ ] Webhook verification enabled
- [ ] Access control list configured
- [ ] Rate limiting active
- [ ] Input sanitization implemented
- [ ] HTTPS/TLS enabled
- [ ] Regular security audits
- [ ] Dependencies kept updated

### Official Security Resources

- [OpenClaw Security Best Practices](https://docs.openclaw.ai/security)
- [Security Model Documentation](https://docs.openclaw.ai/security/models)
- [Report a Vulnerability](https://github.com/openclaw/openclaw/security/advisories)

---

## Tutorials & Guides

### Beginner Tutorials

| Tutorial | Duration | Description |
|----------|----------|-------------|
| [Getting Started with OpenClaw](https://docs.openclaw.ai/tutorials/getting-started) | 15 min | Your first OpenClaw setup |
| [WhatsApp Integration Guide](https://docs.openclaw.ai/tutorials/whatsapp) | 20 min | Connect WhatsApp |
| [Basic Skill Creation](https://docs.openclaw.ai/tutorials/first-skill) | 30 min | Build your first skill |
| [Web Tools Complete Guide](docs/web-tools-guide.md) | 20 min | Web search & fetch configuration 🆕 |
| [Browser Tool Complete Guide](docs/browser-guide.md) | 25 min | Browser automation & control 🆕 |

### Advanced Guides

| Guide | Duration | Description |
|-------|----------|-------------|
| [Production Deployment](https://docs.openclaw.ai/guides/production) | 45 min | Deploy to VPS |
| [Custom Channel Development](https://docs.openclaw.ai/guides/custom-channels) | 60 min | Create new channels |
| [Multi-Model Routing](https://docs.openclaw.ai/guides/model-routing) | 30 min | Smart model selection |
| [Skill Testing & Debugging](https://docs.openclaw.ai/guides/testing) | 40 min | Test skills effectively |

### Video Tutorials

| Title | Creator | Length |
|-------|---------|--------|
| OpenClaw Overview | OpenClaw Team | 12:30 |
| WhatsApp Setup Tutorial | Tech Tutorials | 18:45 |
| Building Custom Skills | Code with Me | 32:10 |

---

## Troubleshooting

### Common Issues

#### Issue: Webhook Not Receiving Messages

**Solution:**
```bash
# Check if port is open
netstat -tuln | grep 8080

# Use ngrok for testing
ngrok http 8080

# Verify webhook URL in platform console
```

#### Issue: API Rate Limiting

**Solution:**
```yaml
# Add retry configuration
api:
  retry:
    max_attempts: 3
    backoff: "exponential"
```

#### Issue: High Memory Usage

**Solution:**
```bash
# Reduce context window
api:
  anthropic:
    max_tokens: 4096  # instead of 8192
```

#### Issue: Skills Not Loading

**Solution:**
```bash
# Check skill logs
openclaw logs --skill <skill-name>

# Verify skill.yaml syntax
openclaw skills validate <skill-name>

# Reinstall the skill
openclaw skills reinstall <skill-name>
```

### Getting Help

-  [Documentation](https://docs.openclaw.ai/)
-  [Discord Community](https://discord.gg/openclaw)
-  [GitHub Issues](https://github.com/openclaw/openclaw/issues)
-  [Search Existing Issues](https://github.com/openclaw/openclaw/issues?q=is%3Aissue)

---

## Community Resources

### Official Channels

| Platform | Link | Purpose |
|----------|------|---------|
| **Discord** | [Join Discord](https://discord.gg/openclaw) | Chat with community |
| **GitHub** | [github.com/openclaw](https://github.com/openclaw) | Source code & issues |
| **Website** | [openclaw.ai](https://openclaw.ai/) | Official site |
| **Docs** | [docs.openclaw.ai](https://docs.openclaw.ai/) | Documentation |
| **ClawHub** | [clawhub.ai](https://clawhub.ai/) | Skill registry & downloads |
| **X/Twitter** | [@openclawai](https://x.com/openclawai) | Updates & news |

### Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

#### Areas to Contribute

-  Documentation improvements
-  Bug fixes
-  New features
-  Translations
-  Testing & QA
-  Community support

---

## Roadmap

### v2.1 (Upcoming)
- [ ] Enhanced mobile experience
- [ ] Voice message support
- [ ] More model providers
- [ ] Improved security hardening

### Future Plans
- [ ] Desktop application
- [ ] Mobile apps
- [ ] Enterprise features
- [ ] Cloud hosting options

---

## License

This repository is licensed under the [MIT License](LICENSE).

OpenClaw itself is released under the Apache 2.0 License.

---

## Acknowledgments

- **Original Creator**: Peter C. (aka "the lobster person")
- **Core Team**: The OpenClaw maintainers
- **Contributors**: All [clawtributors](https://github.com/openclaw/openclaw/graphs/contributors) who have made this project possible
- **Community**: The amazing Claw Crew on Discord

---

---

##  Buy Me A Coffee

If this project has helped you, please consider buying me a coffee 

### WeChat Pay

<img src="https://raw.githubusercontent.com/geekjourneyx/awesome-developer-go-sail/main/docs/assets/wechat-reward-code.jpg" alt="WeChat Pay QR Code" width="200" />

---

##  Author

- **Author**: geekjourneyx
- **X (Twitter)**: https://x.com/seekjourney
- **WeChat Official Account**: 极客杰尼

Follow for more practical sharing on AI programming, AI tools, and AI-powered global websites:

<p align="center">
<img src="https://raw.githubusercontent.com/geekjourneyx/awesome-developer-go-sail/main/docs/assets/qrcode.jpg" alt="WeChat Official Account: 极客杰尼" width="180" />
</p>

---

<div align="center">

**"The lobster has molted into its final form."** — Peter, 2025

**Made with  for OpenClaw**

[ Back to Top](#-awesome-openclaw)

</div>
