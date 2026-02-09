<div align="center">

<img src="assets/logo.png" alt="Awesome OpenClaw" width="480" />

# Awesome OpenClaw

[![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)
[![Stars](https://img.shields.io/github/stars/geekjourneyx/awesome-openclaw?style=social)](https://github.com/geekjourneyx/awesome-openclaw)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

[**English**](README.md) | [**简体中文**](README_zh.md)

---

**OpenClaw 资源、指南和最佳实践精选合集。**

[OpenClaw](https://openclaw.ai/) 是一个自托管网关，将您喜爱的聊天应用连接到 AI 智能体。

[概述](#概述) • [安装](#安装) • [渠道](#渠道) • [工具](#工具) • [提供商](#提供商) • [指南](#指南) • [资源](#资源)

</div>

---

## 目录

- [概述](#概述)
- [安装](#安装)
- [配置](#配置)
- [渠道](#渠道)
- [工具](#工具)
- [模型提供商](#模型提供商)
- [指南](#指南)
- [官方资源](#官方资源)
- [社区](#社区)
- [许可证](#许可证)

---

## 概述

> **"去角质！去角质！"** — 一只太空龙虾，大概

OpenClaw 是一个**自托管网关**，将您喜爱的聊天应用 — WhatsApp、Telegram、Discord、iMessage 等 — 连接到 AI 智能体。您在自己的机器（或服务器）上运行单个 Gateway 进程，它成为您的消息应用与随时可用的 AI 助手之间的桥梁。

### 核心特性

| 特性 | 描述 |
|------|------|
| **自托管** | 运行在您的硬件上，您的规则，您的数据 |
| **多渠道** | 一个 Gateway 同时支持 WhatsApp、Telegram、Discord 等 |
| **智能体原生** | 为编码智能体构建，支持工具使用、会话和记忆 |
| **开源** | MIT 许可证，社区驱动 |
| **一流工具** | 浏览器、画布、节点、定时任务、网络搜索等 |

### 系统要求

- **Node.js 22+**（安装脚本会在缺少时自动安装）
- **操作系统**：macOS、Linux 或 Windows（Windows 推荐 WSL2）
- **API 密钥**：Anthropic Claude 或其他支持的提供商

---

## 安装

### 快速安装（推荐）

安装脚本一步完成 Node 检测、安装和入门配置。

**macOS / Linux / WSL2：**

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows (PowerShell)：**

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

### 其他安装方式

**npm / pnpm：**

```bash
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

**Docker：**

参见官方文档中的 [Docker 安装指南](https://docs.openclaw.ai/install#docker)。

**从源码构建：**

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm ui:build
pnpm build
pnpm link --global
openclaw onboard --install-daemon
```

### 验证安装

```bash
openclaw doctor    # 检查配置问题
openclaw status    # Gateway 状态
openclaw dashboard # 打开浏览器 UI
```

---

## 配置

配置文件位于 `~/.openclaw/openclaw.json`。

### 最小配置

如果您不做任何配置，OpenClaw 会使用合理的默认值。以下是最小配置示例：

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-opus-4-6"
      }
    }
  }
}
```

### 访问控制示例

```json
{
  "channels": {
    "whatsapp": {
      "allowFrom": ["+8613800138000"],
      "groups": {
        "*": { "requireMention": true }
      }
    }
  },
  "messages": {
    "groupChat": {
      "mentionPatterns": ["@openclaw"]
    }
  }
}
```

完整的配置选项请参阅[官方文档](https://docs.openclaw.ai/)。

---

## 渠道

OpenClaw 通过单个 Gateway 支持多个消息平台：

| 渠道 | 描述 | 文档 |
|------|------|------|
| **WhatsApp** | 最受欢迎的选择 | [WhatsApp 设置](https://docs.openclaw.ai/channels/whatsapp) |
| **Telegram** | 完整功能支持 | [Telegram 设置](https://docs.openclaw.ai/channels/telegram) |
| **Discord** | 支持斜杠命令 | [Discord 设置](https://docs.openclaw.ai/channels/discord) |
| **Slack** | 企业级就绪 | [Slack 设置](https://docs.openclaw.ai/channels/slack) |
| **Signal** | 注重隐私 | [Signal 设置](https://docs.openclaw.ai/channels/signal) |
| **iMessage** | 仅限 macOS | [iMessage 设置](https://docs.openclaw.ai/channels/imessage) |
| **WebChat** | 内置网页 UI | [WebChat 设置](https://docs.openclaw.ai/channels/webchat) |

查看所有[支持的渠道](https://docs.openclaw.ai/channels)。

---

## 工具

OpenClaw 为浏览器、画布、节点和定时任务提供**一流智能体工具**：

| 工具 | 描述 | 文档 |
|------|------|------|
| **browser** | 控制专用浏览器实例 | [浏览器工具](https://docs.openclaw.ai/tools#browser) |
| **canvas** | 驱动节点画布 (A2UI) | [画布工具](https://docs.openclaw.ai/tools#canvas) |
| **nodes** | 发现和定位配对节点 | [节点工具](https://docs.openclaw.ai/tools#nodes) |
| **cron** | 管理 Gateway 定时任务和唤醒 | [定时任务](https://docs.openclaw.ai/tools#cron) |
| **exec** | 在工作区运行 Shell 命令 | [执行工具](https://docs.openclaw.ai/tools#exec) |
| **web_search** | 使用 Brave Search API 搜索网络 | [网络搜索](https://docs.openclaw.ai/tools#web_search) |
| **web_fetch** | 获取并提取 URL 的可读内容 | [网络获取](https://docs.openclaw.ai/tools#web_fetch) |
| **message** | 跨渠道发送消息 | [消息工具](https://docs.openclaw.ai/tools#message) |
| **sessions** | 列出、检查和发送到会话 | [会话工具](https://docs.openclaw.ai/tools#sessions_list) |

查看[完整工具文档](https://docs.openclaw.ai/tools)。

### 工具配置文件

针对不同用例预配置的工具允许列表：

| 配置文件 | 包含工具 | 最适合 |
|---------|----------|--------|
| `minimal` | 仅 session_status | 受限环境 |
| `coding` | 文件系统、运行时、会话、内存、图像 | 开发工作 |
| `messaging` | 消息传递、会话操作 | 聊天专注的智能体 |
| `full` | 无限制（默认） | 完整智能体能力 |

---

## 模型提供商

OpenClaw 支持多个 LLM 提供商。通过提供商验证，然后设置默认模型。

### 支持的提供商

| 提供商 | 模型 | 状态 |
|--------|------|------|
| **Anthropic** | Claude Opus 4.6、Sonnet、Haiku | 原生支持 |
| **OpenAI** | GPT-4.1、GPT-4o、GPT-3.5 | 原生支持 |
| **Google** | Gemini 2.5 Pro/Ultra | 原生支持 |
| **Venice AI** | Llama 3.3、Claude Opus | 推荐用于隐私保护 |
| **Qwen** | Qwen 模型 | OAuth 支持 |
| **OpenRouter** | 100+ 模型通过 API | 支持 |
| **Vercel AI Gateway** | 多个模型 | 支持 |
| **Cloudflare AI Gateway** | 多个模型 | 支持 |
| **Ollama** | 本地模型 | 支持 |

查看所有[提供商文档](https://docs.openclaw.ai/providers)。

### 模型配置

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-opus-4-6"
      }
    }
  }
}
```

---

## 指南

### 实战指南

| 指南 | 描述 |
|------|------|
| [Web Tools 完全指南](docs/web-tools-guide-zh.md) | 网络搜索和网页抓取配置 |
| [Browser 工具完全指南](docs/browser-guide-zh.md) | 浏览器自动化与控制 |

### 官方指南

| 指南 | 描述 | 链接 |
|------|------|------|
| **快速入门** | 快速入门指南 | [docs.openclaw.ai](https://docs.openclaw.ai/) |
| **架构** | Gateway、客户端、节点概述 | [架构](https://docs.openclaw.ai/concepts/architecture) |
| **安装** | 详细安装说明 | [安装](https://docs.openclaw.ai/install) |
| **平台** | 平台特定设置 | [平台](https://docs.openclaw.ai/platforms) |
| **Gateway & 运维** | Gateway 运维 | [Gateway](https://docs.openclaw.ai/gateway) |
| **CLI 参考** | 命令行界面 | [CLI](https://docs.openclaw.ai/cli) |
| **帮助** | 一般帮助 | [帮助](https://docs.openclaw.ai/help) |

---

## 官方资源

| 资源 | URL | 描述 |
|------|-----|------|
| **官方网站** | https://openclaw.ai/ | 主站 |
| **文档** | https://docs.openclaw.ai/ | 官方文档 |
| **GitHub** | https://github.com/openclaw/openclaw | 源代码 |
| **ClawHub** | https://clawhub.ai/ | 技能注册表 |
| **技能目录** | https://github.com/openclaw/clawhub | 技能仓库 |
| **Awesome 技能** | https://github.com/VoltAgent/awesome-openclaw-skills | 社区技能 |
| **Discord** | https://discord.gg/openclaw | 社区聊天 |

---

## 社区

- **Discord**: [加入 OpenClaw Discord](https://discord.gg/openclaw)
- **GitHub**: [OpenClaw 仓库](https://github.com/openclaw/openclaw)
- **ClawHub**: [查找和分享技能](https://clawhub.ai/)

---

## 许可证

本仓库采用 [MIT 许可证](LICENSE)。

OpenClaw 本身采用 Apache 2.0 许可证发布。

---

## 作者

- **作者**: geekjourneyx
- **X (Twitter)**: https://x.com/seekjourney
- **微信公众号**: 极客杰尼

<p align="center">
<img src="https://raw.githubusercontent.com/geekjourneyx/awesome-developer-go-sail/main/docs/assets/qrcode.jpg" alt="微信公众号: 极客杰尼" width="180" />
</p>

---

<div align="center">

**用心为 OpenClaw 而作**

[返回顶部](#awesome-openclaw)

</div>
