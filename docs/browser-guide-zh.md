# OpenClaw Browser 工具完全指南

[**English**](browser-guide.md) | [**简体中文**](browser-guide-zh.md)

---

## 目录

- [概述](#概述)
- [工作原理](#工作原理)
- [浏览器模式对比](#浏览器模式对比)
- [Ubuntu/Linux 安装](#ubuntulinux-安装)
- [配置说明](#配置说明)
- [配置文件详解](#配置文件详解)
- [CLI 命令参考](#cli-命令参考)
- [Snapshot 与 Refs](#snapshot-与-refs)
- [高级功能](#高级功能)
- [远程浏览器](#远程浏览器)
- [Chrome 扩展模式](#chrome-扩展模式)
- [安全最佳实践](#安全最佳实践)
- [故障排查](#故障排查)

---

## 概述

OpenClaw Browser 工具允许 AI Agent 控制一个专用的浏览器实例，实现：

-  访问需要 JavaScript 的现代网站
-  模拟真实用户操作（点击、输入、滚动）
-  截图、快照、PDF 导出
-  处理登录表单和验证码
-  绕过反爬虫检测

### 核心特性

| 特性 | 说明 |
|------|------|
| **隔离运行** | 独立的浏览器配置文件，不影响个人浏览 |
| **多配置文件** | 支持多个命名配置（openclaw、work、remote 等） |
| **确定性控制** | 精确的标签页管理（列出/打开/聚焦/关闭） |
| **Agent 操作** | 点击、输入、拖拽、选择等 |
| **快照功能** | AI 优化的页面结构分析 |
| **多浏览器** | Chrome、Brave、Edge、Chromium |

---

## 工作原理

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
│  (托管浏览器)     │                 │  (扩展中继)      │
│  Port: 18800    │                 │  Port: 18792    │
│  Color: Orange  │                 │  Color: Blue    │
└─────────────────┘                 └─────────────────┘
```

### 技术架构

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

## 浏览器模式对比

### openclaw 模式 vs chrome 模式

| 特性 | **openclaw** (托管) | **chrome** (扩展中继) |
|------|---------------------|----------------------|
| **隔离性** | [PASS] 完全隔离 | [FAIL] 共享你的浏览器 |
| **配置文件** | 独立用户目录 | 使用现有配置 |
| **扩展要求** | 无需扩展 | 需要安装扩展 |
| **启动方式** | 自动启动 | 手动附加扩展 |
| **适用场景** | Agent 自动化 | 快速测试、调试 |
| **默认端口** | 18800 | 18792 |
| **推荐程度** |  |  |

### 选择建议

```
你的需求
    │
    ├──▶ Agent 需要独立浏览器？ ──▶ openclaw 模式
    │
    ├──▶ 想在现有浏览器测试？   ──▶ chrome 模式
    │
    └──▶ 需要远程控制？         ──▶ remote CDP
```

---

## Ubuntu/Linux 安装

### 步骤 1: 安装 Chrome 浏览器

#### 下载 Chrome

```bash
# 下载 Chrome .deb 包
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

#### 安装 Chrome

```bash
# 使用 apt 安装
sudo apt install ./google-chrome-stable_current_amd64.deb

# 或者使用 dpkg
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt-get install -f  # 修复依赖问题
```

#### 验证安装

```bash
# 检查 Chrome 版本
google-chrome --version

# 预期输出类似：
# Google Chrome 131.0.6778.86
```

### 步骤 2: 安装依赖（可选但推荐）

```bash
# 安装 Playwright 和浏览器依赖
npx playwright install chromium

# 或使用 OpenClaw 内置方式
docker compose run --rm openclaw-cli \
  node /app/node_modules/playwright-core/cli.js install chromium
```

### 步骤 3: 配置 OpenClaw

编辑配置文件：

```bash
nano ~/.openclaw/openclaw.json
```

---

## 配置说明

### 最小配置（Ubuntu 推荐）

```json
{
  "browser": {
    "enabled": true,
    "headless": true,
    "noSandbox": true
  }
}
```

### 完整配置示例

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

## 配置文件详解

### 关键配置项

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `enabled` | boolean | true | 启用浏览器工具 |
| `headless` | boolean | false | 无头模式（不显示窗口） |
| `noSandbox` | boolean | false | Linux 服务器必须设为 true |
| `defaultProfile` | string | "chrome" | 默认使用的配置文件 |
| `executablePath` | string | auto-detect | 浏览器可执行文件路径 |
| `color` | string | "#FF4500" | 浏览器 UI 主题色 |
| `attachOnly` | boolean | false | 仅连接已运行的浏览器 |

### 各平台可执行文件路径

#### Linux

```json
{
  "browser": {
    "executablePath": "/usr/bin/google-chrome"
  }
}
```

其他路径选项：
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

### Profile 配置详解

```json
{
  "profiles": {
    "openclaw": {
      "cdpPort": 18800,           // CDP 端口
      "color": "#FF4500",          // 主题色
      "userDataDir": "/path/to/dir" // 用户数据目录（可选）
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

## CLI 命令参考

### 基础命令

所有命令支持 `--browser-profile <name>` 指定配置文件。

```bash
# 查看浏览器状态
openclaw browser status

# 启动浏览器
openclaw browser start

# 停止浏览器
openclaw browser stop

# 使用指定配置文件
openclaw browser --browser-profile openclaw status
```

### 标签页管理

```bash
# 列出所有标签页
openclaw browser tabs

# 查看当前标签页
openclaw browser tab

# 打开新标签页
openclaw browser tab new

# 打开 URL
openclaw browser open https://example.com

# 聚焦标签页（通过 ID）
openclaw browser focus abcd1234

# 切换到第 N 个标签页
openclaw browser tab select 2

# 关闭标签页
openclaw browser close abcd1234
```

### 页面操作

```bash
# 导航到 URL
openclaw browser navigate https://example.com

# 调整窗口大小
openclaw browser resize 1280 720

# 刷新页面
openclaw browser reload

# 后退/前进
openclaw browser back
openclaw browser forward
```

### 交互操作

```bash
# 点击元素
openclaw browser click 12              # 使用数字 ref
openclaw browser click e12             # 使用角色 ref
openclaw browser click 12 --double     # 双击

# 输入文本
openclaw browser type 23 "hello"
openclaw browser type 23 "hello" --submit  # 输入后提交

# 按键
openclaw browser press Enter
openclaw browser press "Ctrl+A"

# 悬停
openclaw browser hover 44

# 拖拽
openclaw browser drag 10 11

# 选择下拉框
openclaw browser select 9 OptionA OptionB

# 滚动到视图
openclaw browser scrollintoview e12
```

### 表单操作

```bash
# 填写表单
openclaw browser fill --fields '[{"ref":"1","type":"text","value":"Ada"}]'

# 上传文件
openclaw browser upload /tmp/file.pdf
openclaw browser upload --ref e12 /tmp/file.pdf

# 处理对话框
openclaw browser dialog --accept
openclaw browser dialog --dismiss
```

### 截图与快照

```bash
# 截取当前视口
openclaw browser screenshot

# 全页截图
openclaw browser screenshot --full-page

# 截取指定元素
openclaw browser screenshot --ref 12

# 获取快照
openclaw browser snapshot

# ARIA 格式快照
openclaw browser snapshot --format aria

# 交互式快照（仅可交互元素）
openclaw browser snapshot --interactive

# 带标签的快照
openclaw browser snapshot --labels

# 限制深度
openclaw browser snapshot --depth 3

# 指定选择器
openclaw browser snapshot --selector "#main"

# Frame 内快照
openclaw browser snapshot --frame "iframe#main"
```

### 等待操作

```bash
# 等待文本出现
openclaw browser wait --text "Done"

# 等待选择器
openclaw browser wait "#main"

# 等待 URL
openclaw browser wait --url "**/dash"

# 等待加载状态
openclaw browser wait --load networkidle

# 等待 JavaScript 条件
openclaw browser wait --fn "window.ready===true"

# 组合等待
openclaw browser wait "#main" \
  --url "**/dash" \
  --load networkidle \
  --timeout-ms 15000
```

### 状态管理

```bash
# Cookies
openclaw browser cookies
openclaw browser cookies set session abc123 --url "https://example.com"
openclaw browser cookies clear

# Storage
openclaw browser storage local get
openclaw browser storage local set theme dark
openclaw browser storage session clear

# 离线模式
openclaw browser set offline on
openclaw browser set offline off

# 自定义请求头
openclaw browser set headers --json '{"X-Debug":"1"}'
openclaw browser set headers --clear

# 基本认证
openclaw browser set credentials user pass
openclaw browser set credentials --clear

# 地理位置
openclaw browser set geo 37.7749 -122.4194 --origin "https://example.com"
openclaw browser set geo --clear

# 媒体偏好
openclaw browser set media dark

# 时区和语言
openclaw browser set timezone America/New_York
openclaw browser set locale en-US

# 设备模拟
openclaw browser set device "iPhone 14"
```

### 调试功能

```bash
# 查看控制台日志
openclaw browser console --level error

# 查看错误
openclaw browser errors --clear

# 查看网络请求
openclaw browser requests --filter api --clear

# 导出 PDF
openclaw browser pdf

# 响应体
openclaw browser responsebody "**/api" --max-chars 5000

# 高亮元素
openclaw browser highlight e12

# 追踪记录
openclaw browser trace start
# ... 执行操作 ...
openclaw browser trace stop
```

### JSON 输出

所有命令支持 `--json` 参数用于脚本化：

```bash
openclaw browser status --json
openclaw browser snapshot --interactive --json
openclaw browser tabs --json
```

---

## Snapshot 与 Refs

### Snapshot 类型

OpenClaw 支持两种快照模式：

#### 1. AI Snapshot（默认）

```bash
openclaw browser snapshot
# 或显式指定
openclaw browser snapshot --format ai
```

**特点**：
- 返回带有数字引用的文本快照
- 引用格式：`[1]`, `[2]`, `[3]`...
- 操作命令：`click 1`, `type 2 "text"`

**输出示例**：
```
[1] button "Search"
[2] textbox "Username"
[3] textbox "Password"
[4] button "Login"
```

#### 2. Role Snapshot（交互式）

```bash
openclaw browser snapshot --interactive
```

**特点**：
- 返回角色为基础的列表
- 引用格式：`[ref=e12]`, `[ref=e23]`...
- 更稳定，适合复杂页面
- 操作命令：`click e12`, `type e23 "text"`

**输出示例**：
```
button[name="Search"] [ref=e12]
textbox[name="Username"] [ref=e23]
textbox[name="Password"] [ref=e34]
button[name="Login"] [ref=e45]
```

### Snapshot 选项

| 选项 | 说明 |
|------|------|
| `--interactive` | 仅输出可交互元素 |
| `--compact` | 紧凑格式 |
| `--depth N` | 限制深度 |
| `--selector "#main"` | 限制选择器范围 |
| `--frame "iframe#id"` | Frame 内快照 |
| `--labels` | 添加截图标签 |
| `--limit N` | 限制字符数 |

### Ref 使用规则

```bash
# 数字 ref（AI snapshot）
openclaw browser snapshot
openclaw browser click 12
openclaw browser type 23 "hello"

# 角色 ref（role snapshot）
openclaw browser snapshot --interactive
openclaw browser click e12
openclaw browser type e23 "hello"
```

> [WARN] **重要**：Refs 在页面导航后会变化。每次导航后重新获取快照。

---

## 高级功能

### 文件下载

```bash
# 下载文件
openclaw browser download e12 /tmp/report.pdf

# 等待下载完成
openclaw browser waitfordownload /tmp/report.pdf
```

### JavaScript 执行

```bash
# 执行 JavaScript
openclaw browser evaluate --fn '(el) => el.textContent' --ref 7

# 执行任意代码（需启用）
openclaw browser evaluate --fn 'document.title'
```

### 设备模拟

```bash
# 模拟 iPhone
openclaw browser set device "iPhone 14"

# 模拟 iPad
openclaw browser set device "iPad Pro"

# 自定义视口
openclaw browser set viewport 1280 720
```

### 地理位置模拟

```bash
# 设置位置（旧金山）
openclaw browser set geo 37.7749 -122.4194

# 设置位置并指定源
openclaw browser set geo 35.6762 139.6503 --origin "https://example.com"

# 清除位置
openclaw browser set geo --clear
```

---

## 远程浏览器

### Browserless（托管服务）

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

### 远程 CDP

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

### 带认证的远程 CDP

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

## Chrome 扩展模式

### 安装扩展

```bash
# 1. 生成扩展文件
openclaw browser extension install

# 2. 打开 Chrome 扩展页面
# chrome://extensions

# 3. 启用"开发者模式"

# 4. 点击"加载已解压的扩展程序"

# 5. 选择命令输出的目录
openclaw browser extension path
```

### 使用扩展模式

```bash
# 1. 启动 OpenClaw
openclaw gateway start

# 2. 在要控制的标签页上点击扩展图标

# 3. 使用 chrome 配置文件
openclaw browser --browser-profile chrome tabs
```

### 创建自定义扩展配置文件

```bash
openclaw browser create-profile \
  --name my-chrome \
  --driver extension \
  --cdp-url http://127.0.0.1:18792 \
  --color "#00AA00"
```

---

## 安全最佳实践

### 隔离保证

| 安全措施 | 说明 |
|----------|------|
| **独立用户目录** | 不接触个人浏览器配置 |
| **专用端口** | 避免与开发工作流冲突（避开 9222） |
| **确定性标签控制** | 通过 targetId 精确控制，非"最后标签" |

### 安全配置建议

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

### 安全检查清单

- [ ] 浏览器配置文件不包含敏感登录信息
- [ ] Gateway 仅监听 loopback 地址
- [ ] 远程 CDP 使用 HTTPS 和短令牌
- [ ] 禁用不必要的 `evaluateEnabled`
- [ ] 限制沙箱会话的浏览器访问
- [ ] 定期清理浏览器数据

---

## 故障排查

### Linux 常见问题

#### 问题：浏览器无法启动

**解决方案**：

```json
{
  "browser": {
    "noSandbox": true,
    "headless": true
  }
}
```

#### 问题：缺少依赖

**解决方案**：

```bash
# 安装 Chrome 依赖
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

### 常见错误

#### "Browser disabled"

**原因**：浏览器工具未启用

**解决**：
```json
{
  "browser": {
    "enabled": true
  }
}
```

#### "Playwright is not available"

**原因**：未安装 Playwright

**解决**：
```bash
npx playwright install chromium
```

#### "not visible" / "strict mode violation"

**解决**：
```bash
# 重新获取快照
openclaw browser snapshot --interactive

# 高亮元素查看位置
openclaw browser highlight e12
```

---

## 快速参考

### 常用工作流程

#### 搜索并截图

```bash
openclaw browser open https://google.com
openclaw browser snapshot --interactive
openclaw browser type e12 "OpenClaw"
openclaw browser press Enter
openclaw browser wait --load networkidle
openclaw browser screenshot --full-page
```

#### 表单填写

```bash
openclaw browser snapshot --interactive
openclaw browser type e12 "myemail@example.com"
openclaw browser type e23 "mypassword"
openclaw browser click e34
```

#### 调试流程

```bash
# 开始追踪
openclaw browser trace start

# 执行操作
openclaw browser navigate https://example.com
openclaw browser snapshot
openclaw browser click e12

# 停止追踪
openclaw browser trace stop
```

---

<div align="center">

**需要帮助？** [Discord 社区](https://discord.gg/openclaw) | [GitHub Issues](https://github.com/openclaw/openclaw/issues)

[ 返回顶部](#openclaw-browser-工具完全指南)

</div>
