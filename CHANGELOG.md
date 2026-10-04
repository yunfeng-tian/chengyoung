# Changelog

本项目的版本发布记录（面向用户）。每个发布版本从最新往下；「新增 / 改进与修复 / 安全」为面向用户的要点。破坏性变更（Breaking Changes）如有，会在对应版本顶部标注。

## [1.11.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.11.0) — 2026-10-05

### 新增
- **系统信息升级为完整构建与环境档案**（设置 → 应用 → 关于与帮助 → 「复制系统信息」）：在版本、平台、语言之外，新增 OS 版本号、WebView2 运行时版本、构建提交与构建日期——信息密度对照 VS Code「帮助 → 关于」，便于支持排障
- **诊断报告版本自检**：生成诊断包时，应用版本与平台信息统一由应用外壳提供，本地推理引擎的版本单列一行——两者不一致时在报告里一眼可见

### 修复
- **修正安装包内本地推理引擎的版本号**：1.10.0 安装包中应用版本显示正确，但内部推理引擎二进制停留在 1.9.1（版本号提升未触发其重新编译）。功能本身没有差异，但诊断报告会显示旧版本。发布链路同步加固：发布构建现在会自动重编引擎并校验其内嵌版本号，杜绝同类问题再次发生

## [1.10.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.10.0) — 2026-10-04

> 本条目同时涵盖中间的内部修订版本（其内容已全部包含在本版安装包中）。

### 新增
- **通知中心**：底部状态栏新增铃铛入口——通知即时弹出并自动留存，支持未读计数、单条清除与「全部清除」；留存仅保存在内存中，不写入任何文件
- **扩展页新增「会话级扩展」状态卡**：浏览器控制的接入状态（已接入 / 失败原因）与管理入口一目了然，与启动提示的统计口径一致

### 改进与修复
- **提示气泡全面重设计**：视觉融入亮 / 暗主题、宽度随内容自适应、整体位置下移，不再遮挡页面右上角的按钮
- **恢复历史会话时浏览器工具自动恢复可用**：此前恢复旧会话会出现「9/10 个扩展加载失败」且浏览器工具不可用，现在恢复时会自动续接最新端点
- **云端模型上下文窗口标注修正**：DeepSeek（1M 上下文）等 37 处内置标注按 2026-10 信息核对更新，上下文占用条与压缩提示与模型实际能力一致
- 内部推理进程改用专用名称，避免与官方 Goose 同装时产生进程混淆
- 发布流程加固：内置 16 语言文案质量门禁（防止个别语言回退英文）与版本 tag 幂等

## [1.9.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.9.0) — 2026-10-03

### 新增
- **右栏「工作文件」重设计**：会话产生的文件变更置顶展示，目录浏览增强；视图标签改为图标 + 悬停提示
- **Diff 支持非 UTF-8 文件**：GBK 等编码的文件在工作区 diff 中不再报「加载 Diff 失败」
- **文件编辑编码保真**：Agent 编辑文件时自动检测并保持原编码回写，不再把非 UTF-8 文件写成乱码
- Hub 启动页优化：问候与引导合并为一句、起手指令卡片按日轮换（两天换一批）、目录切换集中在输入框
- 附属页面头部排版收紧

### 改进与修复
- 内核同步上游 1.53.0：提示词分类器 fail-safe、会话 HTML 导出、模型目录等上游改进

### 安全
- MCP streamable HTTP 传输的 SSRF 防护修复（上游 #11501）

## [1.8.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.8.0) — 2026-10-02

### 新增
- **关闭时最小化到托盘**：关闭窗口不再退出，托盘菜单支持打开 / 设置 / 检查更新 / 退出
- **会话「复制并分享」**：右键一键复制整个会话的 Markdown 文本
- 主窗口隐藏预热：界面就绪后再显示，消除启动白闪

### 改进与修复
- 启动首帧闪烁（白闪 / 灰闪）与主题切换时的标题栏残留修复
- 主窗口顶部边界与标题栏细节修复

## [1.7.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.7.0) — 2026-10-01

### 新增
- **内嵌浏览器面板化**：第三方网页在主窗口内的隔离面板中打开，随主窗口联动收缩；独立配置目录 / 零注入 / 无法获取任何应用权限
- **Agent 浏览器控制增强**：支持输入并提交表单（默认关闭、按站点授权、每次操作确认、敏感字段页面端拦截、输入内容绝不写入任何日志）
- 顶栏新增「切换辅助栏」按钮
- **页面可见输入框枚举**：Agent 可列出页面上真实可见的输入框（只给选择器，不含已填内容），提升表单交互准确性

### 修复
- 本地推理在「核显 + 独显」混合架构设备上的 Vulkan 稳定性问题
- 配置网络代理时本机服务的连接异常

## [1.4.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-27 – 2026-09-29

### 新增
- **Agent 浏览器控制**：读取页面文本 / 点击元素 / 后退前进刷新（**默认关闭**、按站点授权）
- 内嵌浏览器导航即时同步；`goose://extension` 深链安装前格式校验

### 改进与修复
- 浏览器控制隐私文案合规；`target="_blank"` 新窗口处理提示
- 快捷键（菜单 role 键）、CSP、信任分层等安全加固
- 页面文本绝不写入任何日志

## [1.3.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-27 – 2026-09-28

### 新增
- **内嵌浏览器**：右栏控制面 + 独立窗口（地址栏 / 打开 / 后退前进 / 刷新 / 用系统浏览器打开）
- 浏览器状态即时轮询

### 安全
- 信任分层、零注入机制与独立 Profile 隔离（安全架构前置）

## [1.2.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-26 – 2026-09-27

### 新增
- **会话产物三视图**：结果（MCP Apps）/ 文档（md / diff / html 预览）/ 媒体（图片预览）

### 改进与修复
- 特定场景下的事件响应修复、CSP 修正
- 构建流程安全加固与稳定性提升

## [1.1.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-24 – 2026-09-26

### 新增
- 自绘菜单栏、辅助侧边栏（右侧工作区）、命令面板
- 快捷键体系（命令面板 / 返回）、导航回程
- 顶栏会话历史搜索

### 修复
- 更新链路（安装前停推理进程、下载进度、失败重试、安装不再弹框）

## [1.0.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-23

### 🎉 首次发布
- **ChengYoung**（Tauri 版）：品牌标识、16 种界面语言
- **本地推理**（llama.cpp / GGUF，Vulkan GPU 加速），模型应用内从 Hugging Face 获取（不随包）
- 模型引导、提示词中文对照、应用内帮助与合规入口、Hub 启动页
- NSIS 安装包 + 应用内自动更新（签名校验）

### 修复
- 更新链路（下载进度显示、安装卡死、失败自动重试）

---

## English

Release history for users, newest first. Categories: New / Improvements & fixes / Security. Breaking changes (if any) are flagged at the top of the affected version.

## [1.11.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.11.0) — 2026-10-05

### New
- **System info upgraded to a full build & environment report** (Settings → App → About & help → Copy system info): in addition to version, platform and locale, it now includes the OS version, the WebView2 Runtime version, and the build commit & date — on par with VS Code's Help → About for support troubleshooting
- **Diagnostics version self-check**: the diagnostic report now takes the app version and platform from the app shell, and lists the local inference engine version on its own line — any mismatch between the two is immediately visible in the report

### Fixed
- **Corrected the local inference engine version inside the installer**: in 1.10.0 the app version was correct, but the engine binary was still 1.9.1 (bumping the version did not trigger its recompilation). There was no functional difference, but the diagnostic report showed the old version. The release pipeline was hardened accordingly: release builds now rebuild the engine automatically and assert its embedded version, so this class of issue cannot recur

## [1.10.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.10.0) — 2026-10-04

> This entry also covers the intermediate internal revisions (all of their changes are included in this release's installer).

### New
- **Notification center**: a bell entry in the status bar — notifications pop up instantly and are archived for later review, with an unread counter, per-item dismiss and "clear all"; the archive lives in memory only and is never written to any file
- **"Session-level extensions" card on the Extensions page**: the connection state of browser control (connected / failure reason) and its manage entry at a glance, consistent with the startup toast

### Improvements & fixes
- **Redesigned toast notifications**: visuals match the light / dark themes, width adapts to content, and the position moved down so page controls in the top-right corner are no longer covered
- **Browser tools are available again after restoring an old session**: restoring a session used to show "9/10 extensions loaded" with browser tools unavailable; restoring now reconnects to the latest endpoint automatically
- **Cloud model context-window labels corrected**: 37 built-in labels (DeepSeek 1M context and more) verified against 2026-10 information, so the context bar and compaction hints match the model's real capability
- The internal inference process now uses a dedicated name, avoiding process confusion when the official Goose is installed alongside
- Release pipeline hardening: a built-in 16-locale copy-quality gate and version-tag idempotency

## [1.9.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.9.0) — 2026-10-03

### New
- **Redesigned "Workspace files" panel**: file changes produced by sessions are pinned at the top, with enhanced directory browsing; panel view tabs are now icons with tooltips
- **Diff view for non-UTF-8 files**: files in encodings such as GBK no longer fail with "Failed to load diff" in the workspace diff
- **Encoding-preserving file edits**: the Agent detects and preserves the original file encoding when editing, instead of rewriting non-UTF-8 files into mojibake
- Hub home page polish: merged greeting line, starter cards rotating by day (a new pair every two days), working-directory switching inside the input box
- Tighter headers on secondary pages

### Improvements & fixes
- Core synced to upstream 1.53.0: prompt-classifier fail-safe, session HTML export, model catalog and other upstream improvements

### Security
- SSRF protection fix in the MCP streamable-HTTP transport (upstream #11501)

## [1.8.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.8.0) — 2026-10-02

### New
- **Minimize to tray on close**: closing the window keeps the app in the tray, with a tray menu (open / settings / check for updates / exit)
- **Copy & share session**: copy a whole session as Markdown with one click from the context menu
- Hidden-until-ready main window: shown once the UI is ready, eliminating the startup flash

### Improvements & fixes
- Fixed first-frame flicker (white / gray flash) and title-bar leftovers on theme switch
- Main window top boundary and title-bar polish

## [1.7.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.7.0) — 2026-10-01

### New
- **Embedded browser as a panel**: third-party pages open in an isolated panel inside the main window and move / minimize with it; separate profile / zero injection / no access to app capabilities
- **Agent browser control extended**: type into and submit forms (off by default, per-origin authorization, confirmation on every action, sensitive fields intercepted on the page, input content never written to any logs)
- New "Toggle auxiliary panel" button in the top bar
- **Visible input-field enumeration**: the Agent can list the input fields really visible on the page (selectors only, never the values already entered), improving form interactions

### Fixes
- Local inference stability on hybrid iGPU/dGPU setups (Vulkan)
- Connectivity to local services when a network proxy is configured

## [1.4.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-27 – 2026-09-29

### New
- **Agent browser control**: read page text / click elements / go back and forward / reload (off by default, per-origin authorization)
- Instant browser-state sync; format validation before installing `goose://extension` deep links

### Improvements & fixes
- Privacy copy compliance; one-time notice when a `target="_blank"` link is blocked
- Keyboard shortcuts, CSP, trust layering and other security hardening
- Page text is never written to any logs

## [1.3.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-27 – 2026-09-28

### New
- **Embedded browser**: control surface in the right panel + a separate window (address bar / open / back-forward / reload / open in system browser)
- Instant browser-state polling

### Security
- Trust layering, zero-injection and isolated profile (security architecture groundwork)

## [1.2.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-26 – 2026-09-27

### New
- **Session artifact views**: results (MCP Apps) / documents (md, diff, html preview) / media (image preview)

### Improvements & fixes
- Event-response fixes in specific scenarios; CSP correction
- Build pipeline security hardening and stability improvements


## [1.1.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-24 – 2026-09-26

### New
- Custom menu bar, auxiliary sidebar (right workspace), command palette
- Keyboard shortcut system (command palette / back), navigation back handling
- Session-history search in the top bar

### Fixes
- Update pipeline (stop inference process before install, download progress, retry on failure, no more install dialogs)

## [1.0.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-23

### 🎉 Initial release
- **ChengYoung** (Tauri build): brand identity, 16 UI languages
- **Local inference** (llama.cpp / GGUF, Vulkan GPU acceleration), models fetched in-app from Hugging Face (not bundled)
- Model onboarding, Chinese prompt reference, in-app help & compliance entry, Hub home page
- NSIS installer + in-app auto-update (signature-verified)

### Fixes
- Update pipeline (download progress, stuck install, automatic retry)

