<div align="center">

<img src="assets/logo.png" alt="Awesome OpenClaw" width="480" />

# Awesome OpenClaw

[![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)
[![Stars](https://img.shields.io/github/stars/openclaw-tools/awesome-openclaw?style=social)](https://github.com/openclaw-tools/awesome-openclaw)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/openclaw-tools/awesome-openclaw)

[**English**](README.md) | [**简体中文**](README_zh.md)

---

**OpenClaw 资源、指南、技能插件和最佳实践的精选合集。**

[OpenClaw](https://openclaw.ai/) 是一个运行在你本地机器上、可与你日常使用的聊天应用程序集成的开源 AI 代理平台。

[快速入门](#快速入门) • [安装指南](#安装指南) • [技能插件](#技能插件) • [安全实践](#安全实践) • [社区资源](#社区资源)

</div>

---

## 目录

- [项目简介](#项目简介)
- [快速入门](#快速入门)
  - [什么是 OpenClaw？](#什么是-openclaw)
  - [核心特性](#核心特性)
  - [支持的渠道](#支持的渠道)
- [安装指南](#安装指南)
  - [系统要求](#系统要求)
  - [安装方法](#安装方法)
  - [配置说明](#配置说明)
- [渠道与集成](#渠道与集成)
- [技能与插件](#技能与插件)
- [模型与提供商](#模型与提供商)
- [安全最佳实践](#安全最佳实践)
- [教程与指南](#教程与指南)
- [故障排查](#故障排查)
- [社区资源](#社区资源)
- [贡献指南](#贡献指南)

---

## 项目简介

> **"去角质！去角质！"** — 一只太空龙虾，大概

OpenClaw 是一个**开源 AI 代理网关**，它运行在你自己的机器上，并与你喜爱的聊天应用程序无缝集成。与 SaaS AI 助手不同，OpenClaw 让你**完全掌控**你的数据、基础设施和 API 密钥。

### OpenClaw 的独特之处

| 特性 | OpenClaw | SaaS 替代方案 |
|------|----------|---------------|
| **数据隐私** | 你的机器，你的数据 | 数据存储在外部服务器 |
| **API 成本** | 直接向提供商付费 | 订阅费 + 加价 |
| **可扩展性** | 开源，可自托管 | 封闭生态系统 |
| **支持渠道** | WhatsApp、Telegram、Discord 等 | 选择有限 |
| **自定义技能** | 完整的插件系统 | 功能受限 |

---

## 快速入门

### 什么是 OpenClaw？

OpenClaw（前身为 Clawd/Moltbot）是一个 **AI 代理网关**，它将 Claude/GPT 和其他大语言模型与消息平台连接起来。它可以在本地或你自己的基础设施上运行，并连接到你日常使用的聊天应用。

### 核心特性

-  **多渠道支持**：WhatsApp、Telegram、Discord、Slack、Teams、Twitch、Google Chat、iMessage 等
-  **多模型提供商**：Anthropic Claude、OpenAI GPT、Google Gemini、KIMI、小米等
-  **插件系统**：创建和共享自定义技能
-  **可自托管**：你的基础设施，你的规则，你的数据
-  **Web 聊天**：内置网页界面，支持图片
-  **34+ 项安全增强**：针对提示注入和常见漏洞的加固防护

### 支持的渠道

| 渠道 | 状态 | 说明 |
|------|------|------|
| **WhatsApp** | [PASS] 稳定 | 最受欢迎的选择 |
| **Telegram** | [PASS] 稳定 | 完整功能支持 |
| **Discord** | [PASS] 稳定 | 支持斜杠命令 |
| **Slack** | [PASS] 稳定 | 企业级就绪 |
| **Microsoft Teams** | [PASS] 稳定 | 企业集成 |
| **Twitch** | [PASS] 稳定 | 最新版本新增 |
| **Google Chat** | [PASS] 稳定 | 最新版本新增 |
| **iMessage** | [PASS] 稳定 | 仅限 macOS |
| **Mattermost** | [PASS] 稳定 | 自托管聊天 |

---

## 安装指南

### 系统要求

- **操作系统**：Linux、macOS 或 Windows（推荐 WSL2）
- **内存**：最低 4GB，推荐 8GB+
- **磁盘空间**：500MB 可用空间
- **网络**：稳定的网络连接，用于 LLM API 调用

### 安装方法

#### 方法一：快速安装（推荐）

```bash
# 克隆仓库
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# 运行安装脚本
./install.sh
```

#### 方法二：Docker 安装

```bash
docker pull ghcr.io/openclaw/openclaw:latest
docker run -d --name openclaw \
  -v ~/.openclaw:/app/.openclaw \
  -p 8080:8080 \
  ghcr.io/openclaw/openclaw:latest
```

#### 方法三：手动安装

```bash
# 安装 Python 3.10+
python3 --version

# 安装依赖
pip install -r requirements.txt

# 复制并编辑配置文件
cp config.example.yaml config.yaml
nano config.yaml
```

### 配置说明

OpenClaw 使用 YAML 配置文件。关键设置：

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

## 渠道与集成

### WhatsApp 配置

<details>
<summary><b>点击展开 WhatsApp 配置指南</b></summary>

1. **获取 WhatsApp Business API 访问权限**
   - 在 [Meta 开发者平台](https://developers.facebook.com/) 注册
   - 创建 WhatsApp Business 应用
   - 获取你的电话号码 ID 和访问令牌

2. **配置 OpenClaw**
   ```yaml
   channels:
     whatsapp:
       enabled: true
       phone_number_id: "your-phone-number-id"
       access_token: "your-access-token"
       webhook_verify_token: "random-verify-token"
   ```

3. **设置 Webhook**
   - 你的 webhook URL：`https://your-domain.com/webhook/whatsapp`
   - 订阅消息 webhook

4. **发送测试消息**
   ```
   你好！今天有什么可以帮你的吗？
   ```

</details>

### Telegram 配置

<details>
<summary><b>点击展开 Telegram 配置指南</b></summary>

1. **创建 Telegram 机器人**
   - 在 Telegram 上搜索 [@BotFather](https://t.me/botfather)
   - 发送 `/newbot`
   - 按提示为你的机器人命名
   - 保存 API 令牌

2. **配置 OpenClaw**
   ```yaml
   channels:
     telegram:
       enabled: true
       bot_token: "your-bot-token-here"
   ```

3. **启动机器人**
   ```bash
   python -m openclaw.cli --channel telegram
   ```

4. **测试机器人**
   - 在 Telegram 中打开你的机器人
   - 发送 `/start` 开始对话

</details>

### Discord 配置

<details>
<summary><b>点击展开 Discord 配置指南</b></summary>

1. **创建 Discord 应用**
   - 访问 [Discord 开发者门户](https://discord.com/developers/applications)
   - 创建新应用
   - 启用机器人功能

2. **配置机器人权限**
   - 所需权限：
     - 读取消息/查看频道
     - 发送消息
     - 嵌入链接
     - 附加文件
     - 添加反应

3. **配置 OpenClaw**
   ```yaml
   channels:
     discord:
       enabled: true
       bot_token: "your-discord-bot-token"
       command_prefix: "!"
   ```

4. **邀请机器人到服务器**
   - 在开发者门户使用 OAuth2 URL 生成器
   - 使用所需权限邀请机器人

</details>

---

## 技能与插件

### 什么是技能？

技能是扩展 OpenClaw 功能的模块化插件。它们可以：
- 调用外部服务的 API
- 处理和分析数据
- 自动化重复任务
- 与第三方工具集成

### 热门社区技能

####  邮件与日历

| 技能 | 描述 | 仓库 |
|------|------|------|
| **Gmail 集成** | 读取、搜索和发送邮件 | `openclaw-skill-gmail` |
| **日历管理** | 创建事件、检查空闲时间 | `openclaw-skill-calendar` |
| **Outlook 桥接** | Microsoft Outlook 集成 | `openclaw-skill-outlook` |

####  文件与云存储

| 技能 | 描述 | 仓库 |
|------|------|------|
| **Google Drive** | 搜索、上传、下载文件 | `openclaw-skill-gdrive` |
| **Dropbox 处理** | Dropbox 文件操作 | `openclaw-skill-dropbox` |
| **Notion 写入** | 创建和编辑 Notion 页面 | `openclaw-skill-notion` |

####  网络与研究

| 技能 | 描述 | 仓库 |
|------|------|------|
| **网络搜索** | Google/Bing/DuckDuckGo 搜索 | `openclaw-skill-search` |
| **网页摘要** | 总结网络文章 | `openclaw-skill-summary` |
| **维基百科查询** | 快速获取维基百科事实 | `openclaw-skill-wiki` |

####  电子商务

| 技能 | 描述 | 仓库 |
|------|------|------|
| **亚马逊搜索** | 商品搜索和比较 | `openclaw-skill-amazon` |
| **价格追踪** | 监控价格变化 | `openclaw-skill-prices` |

####  娱乐

| 技能 | 描述 | 仓库 |
|------|------|------|
| **Spotify 控制** | 控制音乐播放 | `openclaw-skill-spotify` |
| **YouTube 处理** | 搜索和管理视频 | `openclaw-skill-youtube` |
| **游戏统计** | 获取游戏统计数据 | `openclaw-skill-gaming` |

### 安装技能

```bash
# 使用 CLI
openclaw skills install <技能名称>

# 手动安装
git clone https://github.com/openclaw/skill-<名称>.git
cp -r skill-<名称> ~/.openclaw/skills/
```

### 创建自定义技能

<details>
<summary><b>点击展开技能创建指南</b></summary>

1. **创建技能目录**
   ```bash
   mkdir ~/.openclaw/skills/my-skill
   cd ~/.openclaw/skills/my-skill
   ```

2. **创建 skill.yaml**
   ```yaml
   name: "my-skill"
   version: "1.0.0"
   description: "我的自定义 OpenClaw 技能"
   author: "你的名字"
   triggers:
     - "我的命令"
     - "做某事"
   ```

3. **创建 main.py**
   ```python
   from openclaw import Skill

   class MySkill(Skill):
       def handle(self, message, context):
           return "你好，这是我的技能！"
   ```

4. **注册技能**
   ```bash
   openclaw skills register my-skill
   ```

5. **测试技能**
   ```
   你: 我的命令
   OpenClaw: 你好，这是我的技能！
   ```

</details>

---

## 模型与提供商

OpenClaw 支持多个 LLM 提供商：

### 支持的提供商

| 提供商 | 模型 | 状态 |
|--------|------|------|
| **Anthropic** | Claude 3.5 Sonnet、Claude 3 Opus/Haiku | [PASS] 原生支持 |
| **OpenAI** | GPT-4o、GPT-4-turbo、GPT-3.5 | [PASS] 原生支持 |
| **Google** | Gemini 1.5 Pro/Ultra | [PASS] 原生支持 |
| **KIMI** | KIMI 2.5 | [PASS] v2.0 新增 |
| **小米** | MiMo-V2-Flash | [PASS] v2.0 新增 |
| **OpenRouter** | 100+ 模型通过 API | [PASS] 支持 |
| **本地模型** | Ollama、LM Studio | [PASS] 支持 |

### 模型配置

```yaml
api:
  primary: "anthropic"  # 默认提供商
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

### 模型选择建议

| 使用场景 | 推荐模型 | 原因 |
|----------|----------|------|
| **复杂推理** | Claude 3.5 Sonnet | 最适合分析任务 |
| **代码生成** | GPT-4o | 强大的编码能力 |
| **快速响应** | Claude 3 Haiku | 快速且经济 |
| **中文语言** | KIMI 2.5 | 针对中文优化 |
| **成本敏感** | GPT-3.5 / Haiku | 成本最低 |

---

## 安全最佳实践

> [WARN] **重要提示**：提示注入仍然是业界未解决的问题。请始终使用强大的模型并遵循安全指南。

### 关键安全原则

1. **永远不要在聊天中暴露 API 密钥**
   - 使用环境变量存储密钥
   - 永远不要在响应中打印密钥
   - 定期轮换密钥

2. **实施访问控制**
   ```yaml
   security:
     allowed_users:
       - "verified@email.com"
       - "+1234567890"
     blocked_users:
       - "spam@example.com"
   ```

3. **启用速率限制**
   ```yaml
   security:
     rate_limit:
       requests_per_minute: 60
       cost_limit_per_hour: 10.0
   ```

4. **清理所有输入**
   - 在处理前验证用户输入
   - 转义特殊字符
   - 检查命令注入模式

5. **使用 HTTPS/Webhook 验证**
   ```yaml
   channels:
     whatsapp:
       webhook_verify_token: "random-secure-token"
   ```

### 安全检查清单

- [ ] API 密钥存储在环境变量中
- [ ] 启用 Webhook 验证
- [ ] 配置访问控制列表
- [ ] 启用速率限制
- [ ] 实施输入清理
- [ ] 启用 HTTPS/TLS
- [ ] 定期安全审计
- [ ] 保持依赖项更新

### 官方安全资源

- [OpenClaw 安全最佳实践](https://docs.openclaw.ai/security)
- [安全模型文档](https://docs.openclaw.ai/security/models)
- [报告漏洞](https://github.com/openclaw/openclaw/security/advisories)

---

## 教程与指南

### 入门教程

| 教程 | 时长 | 描述 |
|------|------|------|
| [OpenClaw 入门](https://docs.openclaw.ai/tutorials/getting-started) | 15 分钟 | 你的第一个 OpenClaw 设置 |
| [WhatsApp 集成指南](https://docs.openclaw.ai/tutorials/whatsapp) | 20 分钟 | 连接 WhatsApp |
| [基础技能创建](https://docs.openclaw.ai/tutorials/first-skill) | 30 分钟 | 构建你的第一个技能 |
| [Web Tools 完全指南](docs/web-tools-guide-zh.md) | 20 分钟 | 网络搜索与网页抓取配置 🆕 |
| [Browser 工具完全指南](docs/browser-guide-zh.md) | 25 分钟 | 浏览器自动化与控制 🆕 |

### 高级指南

| 指南 | 时长 | 描述 |
|------|------|------|
| [生产环境部署](https://docs.openclaw.ai/guides/production) | 45 分钟 | 部署到 VPS |
| [自定义渠道开发](https://docs.openclaw.ai/guides/custom-channels) | 60 分钟 | 创建新渠道 |
| [多模型路由](https://docs.openclaw.ai/guides/model-routing) | 30 分钟 | 智能模型选择 |
| [技能测试与调试](https://docs.openclaw.ai/guides/testing) | 40 分钟 | 有效测试技能 |

### 视频教程

| 标题 | 创建者 | 时长 |
|------|--------|------|
| OpenClaw 概览 | OpenClaw 团队 | 12:30 |
| WhatsApp 设置教程 | Tech Tutorials | 18:45 |
| 构建自定义技能 | Code with Me | 32:10 |

---

## 故障排查

### 常见问题

#### 问题：Webhook 未收到消息

**解决方案：**
```bash
# 检查端口是否开放
netstat -tuln | grep 8080

# 使用 ngrok 进行测试
ngrok http 8080

# 在平台控制台验证 webhook URL
```

#### 问题：API 速率限制

**解决方案：**
```yaml
# 添加重试配置
api:
  retry:
    max_attempts: 3
    backoff: "exponential"
```

#### 问题：内存使用过高

**解决方案：**
```bash
# 减少上下文窗口
api:
  anthropic:
    max_tokens: 4096  # 而不是 8192
```

#### 问题：技能无法加载

**解决方案：**
```bash
# 检查技能日志
openclaw logs --skill <技能名称>

# 验证 skill.yaml 语法
openclaw skills validate <技能名称>

# 重新安装技能
openclaw skills reinstall <技能名称>
```

### 获取帮助

-  [文档](https://docs.openclaw.ai/)
-  [Discord 社区](https://discord.gg/openclaw)
-  [GitHub Issues](https://github.com/openclaw/openclaw/issues)
-  [搜索现有问题](https://github.com/openclaw/openclaw/issues?q=is%3Aissue)

---

## 社区资源

### 官方渠道

| 平台 | 链接 | 用途 |
|------|------|------|
| **Discord** | [加入 Discord](https://discord.gg/openclaw) | 与社区聊天 |
| **GitHub** | [github.com/openclaw](https://github.com/openclaw) | 源代码和问题 |
| **官网** | [openclaw.ai](https://openclaw.ai/) | 官方网站 |
| **文档** | [docs.openclaw.ai](https://docs.openclaw.ai/) | 文档 |
| **微博** | [@openclawai](https://x.com/openclawai) | 更新和新闻 |

### 社区项目

- [OpenClaw Hub](https://hub.openclaw.ai/) - 社区技能注册表
- [OpenClaw Configs](https://github.com/openclaw/awesome-configs) - 共享配置
- [OpenClaw Templates](https://github.com/openclaw/templates) - 启动模板

### 贡献

我们欢迎贡献！请参阅 [CONTRIBUTING.md](CONTRIBUTING.md) 了解指南。

#### 可以贡献的领域

-  文档改进
-  错误修复
-  新功能
-  翻译
-  测试和 QA
-  社区支持

---

## 开发路线图

### v2.1（即将推出）
- [ ] 增强的移动体验
- [ ] 语音消息支持
- [ ] 更多模型提供商
- [ ] 改进的安全加固

### 未来计划
- [ ] 桌面应用程序
- [ ] 移动应用
- [ ] 企业功能
- [ ] 云托管选项

---

## 许可证

本仓库采用 [MIT 许可证](LICENSE)。

OpenClaw 本身采用 Apache 2.0 许可证发布。

---

## 致谢

- **原作者**：Peter C.（又称"龙虾人"）
- **核心团队**：OpenClaw 维护者
- **贡献者**：所有使这个项目成为可能的 [clawtributors](https://github.com/openclaw/openclaw/graphs/contributors)
- **社区**：Discord 上令人惊叹的 Claw Crew

---

---

##  打赏 Buy Me A Coffee

如果本项目对您有帮助，欢迎请作者喝杯咖啡 

### 微信打赏

<img src="https://raw.githubusercontent.com/geekjourneyx/awesome-developer-go-sail/main/docs/assets/wechat-reward-code.jpg" alt="微信打赏码" width="200" />

---

##  作者

- **作者**: geekjourneyx
- **X (Twitter)**: https://x.com/seekjourney
- **公众号**: 极客杰尼

关注公众号，获取更多 AI 编程、AI 工具与 AI 出海建站的实战分享：

<p align="center">
<img src="https://raw.githubusercontent.com/geekjourneyx/awesome-developer-go-sail/main/docs/assets/qrcode.jpg" alt="公众号：极客杰尼" width="180" />
</p>

---

<div align="center">

**"龙虾已蜕变为最终形态。"** — Peter, 2025

**Made with  for OpenClaw**

[ 返回顶部](#-awesome-openclaw)

</div>
