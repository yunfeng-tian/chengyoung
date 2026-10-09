<div align="center">

# ChengYoung

**本地优先的桌面 AI 智能体——开箱即用、数据不出本机**

<a href="https://www.apache.org/licenses/LICENSE-2.0"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg" alt="License: Apache 2.0"></a>
<a href="https://github.com/yunfeng-tian/chengyoung/releases/latest"><img src="https://img.shields.io/github/v/release/yunfeng-tian/chengyoung?label=latest%20release" alt="Latest release"></a>
<img src="https://img.shields.io/badge/platform-Windows%20x64-blue" alt="Platform: Windows x64">

</div>

> **本仓库仅发布构建产物**（Windows 安装包、校验值、更新清单），**不含源代码**。
> 源码不对外发布；本仓库发布的是可直接安装、可自动更新的成品安装包。

ChengYoung 是一款运行在本机的通用桌面 AI 智能体应用，可用于代码编写、信息检索、文档整理和日常自动化等任务。
它内置本地推理引擎（llama.cpp / GGUF），可完全离线运行；也可接入自行配置的云端模型服务。
**不需要注册账户，也不收集任何使用数据。**

---

## 亮点

| 亮点 | 说明 |
| --- | --- |
| **轻量定制外壳（Tauri 2）** | 采用基于 Tauri 2 的定制外壳，替代上游 Electron 桌面运行时：**安装包与外壳体积较 Electron 方案大幅减小**，并复用系统 WebView2；支持应用内**自动更新**（minisign 签名校验，签名不匹配将拒绝安装） |
| **完全本地 + 零遥测** | 本地推理引擎支持 **Vulkan GPU 加速**（并针对核显与独显混合架构设备进行了稳定性优化）；**不收集、不上传任何使用数据** |
| **安全内嵌浏览器** | 第三方网页在**隔离面板**（主窗口内的子 WebView）中打开：独立配置目录、零注入，且无法获取任何应用权限；Agent 浏览器控制**默认关闭**，按站点授权并逐次确认，页面端自动拦截敏感字段输入，输入内容绝不写入日志 |

## 下载与安装（Windows 10 / 11，x64）

1. 打开 [最新版本](https://github.com/yunfeng-tian/chengyoung/releases/latest)，下载 `ChengYoung_<版本号>_x64-setup.exe`
2. 双击安装（按当前用户安装，**不需要管理员权限**）
3. 首次启动后，在「设置 → 模型」里搜索并下载一个本地模型；也可以先配置云端模型提供商再开始使用

- **校验值**：同一 Release 页面的 `SHA256SUMS.txt`
- **应用内更新**：设置 → 关于 ChengYoung → 检查更新（应用默认定期自动检查并下载，**安装前会再次确认**；安装包经 minisign 签名校验，签名不匹配将拒绝安装）
- ⚠️ **手动双击安装包前，请先完全退出应用（包括托盘图标）**——应用运行时文件被占用，可能提示「无法写入」或要求重启

## 主要能力

| 能力 | 说明 |
| --- | --- |
| 本地模型 | 应用内搜索和下载 Hugging Face 上的 GGUF 模型，支持 GPU（Vulkan）加速；**模型不随安装包分发**，许可以各模型页为准（Qwen2.5 为 Apache-2.0，Gemma 系列遵循 Google Gemma Terms），下载前请确认是否符合你的使用场景 |
| 云端模型 | 自备 API Key 接入 Anthropic、OpenAI、Google、OpenRouter、Ollama 等提供商 |
| 会话 | 支持会话历史、重命名、置顶、导出、归档、一键复制为 Markdown 分享，以及从任意消息派生新会话 |
| 工作流（Recipe） | 将常用任务沉淀为可复用流程；可通过 `goose://recipe?...` 深链分享，运行前会显示流程内容并进行变更确认 |
| 技能、应用、调度、扩展 | 内置能力提供可视化管理；扩展采用 MCP 协议（可自行配置第三方 MCP 服务器），敏感操作会先请求许可 |
| **Agent 浏览器控制** | 读取页面文本、点击元素、输入并提交表单，**默认关闭**、按站点授权并逐次确认，页面端自动拦截敏感字段输入，输入内容绝不写入日志 |
| 工作区右侧面板（Workspace Panel） | 文件、媒体、运行结果；以及**内嵌浏览器**（第三方网页在**隔离面板**中打开，独立配置目录、零注入，无法访问应用与你的文件） |
| 提示词与提示文件 | 查看 / 覆盖 9 个内置提示模板，编辑项目与全局提示文件（`.cyhints`，兼容 `.goosehints`） |
| **通知中心** | 状态栏铃铛会留存所有通知：未读计数、单条清除与全部清除；留存仅在内存中，不写入任何文件 |
| 上下文与工作区感知 | 顶栏常驻上下文占用与自动压缩刻度；工作区未提交变更徽章（本地只读统计） |
| 系统集成 | 系统托盘、**关闭时最小化到托盘**、全局快捷键快速启动器、**命令面板**、`goose://` 深链、16 种界面语言 |
| 权限与安全 | 工具执行前的权限确认、工作目录绑定、工作流变更二次确认 |

## 隐私与联网

- **不收集、不上传任何使用数据**：不包含遥测、设备指纹或崩溃上报功能。
- 应用会在以下情况联网：
  1. 下载本地模型（Hugging Face，用户主动）
  2. 检查 / 下载更新（GitHub Releases；应用默认**定期自动检查并自动下载**，可在设置 → 关于 ChengYoung 中关闭）
  3. 调用你自行配置的模型提供商 API（用户主动）
- **自动检查更新补充**：检查请求只从 GitHub Releases 拉取版本清单，不上传任何本机数据；下载完成后会通知你，**安装前会再次确认**。
- 会话与设置保存在本机 `%APPDATA%\ChengYoung\` 目录（1.13.0 起首次启动会把旧版位于 `%APPDATA%\Block\goose\` 的数据自动复制到新目录，旧目录保留不动——与原版 Goose 并装互不影响；也可用环境变量 `GOOSE_PATH_ROOT` 自行指定数据根）。
- **自行验证**：在未执行上述操作时，可以用 Windows「资源监视器」或 Procmon 观察应用的网络活动——不应出现任何出站连接。

## 环境要求

- Windows 10 / 11（x64）
- 安装包约 65 MB（随版本略有变化，已含本地推理引擎）；本地模型按需下载，体积另计
- Windows 11 已内置 WebView2；Windows 10 首次安装时安装器会联网补装 WebView2 运行时
- 运行本地模型的内存参考：**最低可启动** 8 GB（仅小参数模型、短上下文）；**日常流畅** 16 GB 内存 / 8 GB 显存（7B 模型，4K–8K 上下文）；**长上下文** 16 GB 以上显存或 32 GB 内存（7B 模型，16K 以上上下文，或更大参数模型）。使用 GPU 加速需要支持 Vulkan 的显卡驱动。

## 校验下载

```powershell
# 把 <版本号> 换成实际下载的文件名中的版本号（例如 ChengYoung_1.14.0_x64-setup.exe）
certutil -hashfile ChengYoung_<版本号>_x64-setup.exe SHA256
```

将输出的哈希与同一 Release 的 `SHA256SUMS.txt` 逐字比对。应用内更新的安装包由更新器自动校验 minisign 签名（公钥指纹 `3651-C9C4-6E36`，即内置公钥 SHA-256 的前 12 位，可在 Release 页面核对），签名不匹配会被拒绝安装。

## 常见问题

- **自动检查更新会上传我的数据吗？** 不会。检查请求只从 GitHub Releases 拉取版本清单，不上传任何本机数据；检查和下载均可在设置中关闭。
- **同时安装原版 Goose 会冲突吗？** 不会。ChengYoung 1.13.0 起使用独立数据目录（`%APPDATA%\ChengYoung\`），首次启动会复制旧数据，原版 Goose 的目录保留不动。
- **旧会话会丢失吗？** 不会。旧数据会完整复制到新目录，会话历史和配置保持不变。

## 反馈

- 问题与建议：本仓库的 [Issues](https://github.com/yunfeng-tian/chengyoung/issues)
- 邮件支持：**support@chengyoung.com**
- 应用内「设置 → 关于 ChengYoung」可一键复制诊断信息（粘贴至 Issue 或邮件中，以便更快定位问题）

## 许可与来源

- 本发行版是基于开源项目 **Goose**（Agentic AI Foundation / Linux Foundation，Apache License 2.0）的**品牌化构建与独立发行版**；它**并非** Goose 官方发行版，与上游项目及其维护者**无隶属、背书或赞助关系**。
- **修改声明**（Apache-2.0 第 4 节）：本发行版对上游 Goose 做了以下层面的修改：(1) 以 Tauri 2 外壳替代 Electron 桌面运行时；(2) 新增浏览器隔离面板与 Agent 浏览器控制功能；(3) 调整默认配置与安全策略（更新签名校验、数据目录品牌化等）。未修改的部分与上游保持一致。
- 分发遵循 Apache License 2.0；**LICENSE**、**NOTICE** 与 **THIRD_PARTY_NOTICES**（穷举第三方许可清单）随安装包分发（安装后位于应用目录的 `resources/` 下，含上游署名与第三方组件声明）。
- 本仓库发布的是**编译产物，不提供源代码**；文中出现的 “Goose” 等名称仅用于说明来源，相关商标归其权利人所有。
- 本地模型由应用从 Hugging Face 获取，**不随本发行版分发**；模型的许可以其**模型页**为准（例如 Qwen2.5 为 Apache-2.0，Gemma 系列遵循 Google Gemma Terms）。


---

## English

<div align="center">

ChengYoung — a local-first desktop AI agent. Ready out of the box, and your data never leaves your machine.

</div>

> **This repository hosts release artifacts only** (Windows installer, checksums, update manifest), **no source code**. Source code is not publicly released; what is published here is a ready-to-install, auto-updatable build.

ChengYoung is a general-purpose **desktop** AI agent that runs on your own computer, capable of handling coding, information retrieval, document organization, and everyday automation. Local inference (llama.cpp / GGUF) is built in, so it works fully offline; you can also connect your configured cloud model providers. No account is required, and no usage data is collected.

### Highlights

| Area | What you get |
| --- | --- |
| **Lightweight custom Tauri 2 shell** | A custom Tauri 2 shell replaces the upstream Electron desktop runtime: **installer and shell are dramatically smaller than an Electron-based form**, reusing the OS WebView2; in-app **auto-update** (minisign-verified; rejected on signature mismatch) |
| **Fully local, zero telemetry** | Local inference with **Vulkan GPU acceleration** (stability-optimized for hybrid iGPU/dGPU setups); **no usage data is collected or uploaded** |
| **Secure embedded browser** | Third-party pages open in an **isolated panel** (a child webview in the main window): separate profile / zero injection / no access to app capabilities; Agent browser control is **off by default**, per-site authorization + per-action confirmation, sensitive field input is automatically intercepted on the page, and input content is never logged |

### Download and install (Windows 10 / 11, x64)

1. Open the [latest release](https://github.com/yunfeng-tian/chengyoung/releases/latest) and download `ChengYoung_<version>_x64-setup.exe`
2. Run the installer (per-user install, no administrator rights needed)
3. On first launch, download a local model under *Settings → Models*, or configure a cloud provider before you start

- Checksums: `SHA256SUMS.txt` in the same release
- In-app updates: *Settings → About ChengYoung → Check for updates* (the app periodically checks and downloads by default; **installation asks for confirmation**; installers are minisign-verified and rejected on signature mismatch)
- ⚠️ Fully quit the app (including the tray icon) before running an installer manually — a running app keeps the files locked, and you may see a “cannot write” error or be asked to restart.

### Key features

| Area | What you get |
| --- | --- |
| Local models | Search and download GGUF models from Hugging Face in-app, with GPU (Vulkan) acceleration; **models are not bundled**, and their licenses are governed by each model page (e.g. Qwen2.5 is Apache-2.0, the Gemma family follows the Google Gemma Terms) — please check before downloading whether a model fits your use case |
| Cloud models | Bring your own API key for Anthropic, OpenAI, Google, OpenRouter, Ollama and more |
| Sessions | History, rename, pin, export, archive, one-click **copy as Markdown** for sharing, and branch a new session from any message |
| Workflows (Recipes) | Turn recurring tasks into reusable flows and share them with `goose://recipe?...` links; the content is shown and changes are confirmed before running |
| Skills / Apps / Scheduler / Extensions | Built-in capabilities are managed visually in-app; extensions use the MCP protocol (third-party MCP servers supported), and sensitive actions require permission first |
| **Agent browser control** | Read page text, click elements, and type into or submit forms — **off by default**, per-site authorization + per-action confirmation, sensitive field input is automatically intercepted on the page, and input content is never logged |
| Workspace right panel | Files, media and run results — plus an **embedded browser** (third-party pages open in an **isolated panel** with their own profile and cannot reach the app or your files) |
| Prompts and hints | Inspect or override the 9 built-in prompt templates, and edit project or global hint files (`.cyhints`, with `.goosehints` supported for compatibility) |
| **Notification center** | A bell in the status bar archives every notification: unread counter, per-item dismiss and clear all; the archive lives in memory only and is never written to any file |
| Context & workspace awareness | Persistent context-usage bar with auto-compaction scale in the top bar; a badge for uncommitted workspace changes (read-only, computed locally) |
| System integration | Tray icon, **minimize to tray on close**, global-shortcut quick launcher, **command palette**, `goose://` deep links, 16 UI languages |
| Permissions and security | Explicit permission prompts before tool calls, bound working directories, and secondary confirmation for workflow changes |

### Privacy and network access

- **No usage data is collected or uploaded**: the app does not include telemetry, device fingerprinting, or crash reporting.
- The app goes online in the following cases:
  1. Download local models (Hugging Face; user-initiated)
  2. Check for or download updates (GitHub Releases; the app **periodically checks and downloads by default**, which can be turned off in Settings → About ChengYoung)
  3. Call the APIs of the model providers you configured (user-initiated)
- **About automatic checks**: the check request only fetches the version manifest from GitHub Releases and never uploads anything from your machine; once downloaded you are notified and **installation asks for confirmation**.
- Sessions and settings live on your machine in `%APPDATA%\ChengYoung\` (from 1.13.0, the first launch copies data from the legacy `%APPDATA%\Block\goose\` directory to the new one; the old directory is left untouched, so coexisting with the original Goose is safe. You can also override the data root with the `GOOSE_PATH_ROOT` environment variable).
- **Verify it yourself**: when none of the above is happening, watch the app's network activity with Windows Resource Monitor or Process Monitor — no outbound connections should appear.

### Requirements

- Windows 10 / 11 (x64)
- The installer is about 65 MB (slightly varies per release; the local inference engine is included); models are downloaded on demand, with separate download sizes
- Windows 11 ships with WebView2; on Windows 10 the installer fetches the WebView2 runtime if it is missing
- Memory reference for local models: **minimum to start** 8 GB (small models, short context); **comfortable** 16 GB RAM / 8 GB VRAM (7B models, 4K–8K context); **long context** 16 GB+ VRAM or 32 GB RAM (7B models, 16K+ context, or larger models). GPU acceleration requires a Vulkan-capable driver.

### Verify the download

```powershell
# Replace <version> with the version in the file name you downloaded (for example, ChengYoung_1.14.0_x64-setup.exe)
certutil -hashfile ChengYoung_<version>_x64-setup.exe SHA256
```

Compare the resulting hash with `SHA256SUMS.txt` from the same release. In-app update installers are automatically verified by the updater against the minisign signature (public-key fingerprint `3651-C9C4-6E36`, the first 12 hex characters of the SHA-256 of the built-in public key — check it on the Release page) and are rejected if it does not match.

### FAQ

- **Does the automatic update check upload my data?** No. The check only fetches the version manifest from GitHub Releases and never uploads anything from your machine; both the check and the download can be turned off in Settings.
- **Will it conflict with the original Goose if both are installed?** No. ChengYoung 1.13.0+ uses its own data directory (`%APPDATA%\ChengYoung\`); the first launch copies legacy data over and the original Goose's directory is left untouched.
- **Will my old sessions survive the upgrade?** Yes. Legacy data is copied in full to the new directory — session history and configuration are preserved as-is.

### Feedback

- Issues and suggestions in this repository: [yunfeng-tian/chengyoung/issues](https://github.com/yunfeng-tian/chengyoung/issues)
- Email support: **support@chengyoung.com**
- In the app, *Settings → About ChengYoung* lets you copy a diagnostic report with one click (paste it into your issue or email to help diagnose the problem faster).

### Licensing and provenance

- This is a **branded build and independent distribution** of the open-source **Goose** project (Agentic AI Foundation / Linux Foundation, Apache License 2.0). It is **not** an official Goose release and is **not affiliated with, endorsed by, or sponsored by** the Goose project or its maintainers.
- **Notice of modifications** (Apache-2.0 §4): this distribution modifies upstream Goose as follows: (1) the Electron desktop runtime is replaced by a Tauri 2 shell; (2) an isolated browser panel and Agent browser control are added; (3) default configuration and security policies are adjusted (update signature verification, branded data directory, etc.). Unmodified parts stay identical to upstream.
- Distributed under the Apache License 2.0; **LICENSE**, **NOTICE**, and **THIRD_PARTY_NOTICES** (the exhaustive third-party license list) ship with the installer (installed under the application's `resources/` directory) and include upstream attribution and third-party notices.
- Only prebuilt binaries are published here; **no source code is provided**. Names such as "Goose" appear for attribution only and remain the property of their respective owners.
- Local models are fetched from Hugging Face by the app and are **not redistributed** here; each model's license is governed by its model page (for example Qwen2.5 is Apache-2.0, while the Gemma family follows the Google Gemma Terms).



