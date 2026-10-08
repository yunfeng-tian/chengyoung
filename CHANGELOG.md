# Changelog

本项目的版本发布记录（面向用户）。每个发布版本从最新往下；「新增 / 修复 / 安全」为面向用户的要点。破坏性变更（Breaking Changes）如有，会在对应版本顶部标注。

Release notes for this project (user-facing). Versions are listed from newest to oldest; "Added / Fixed / Security" are the user-facing highlights. Breaking changes, if any, are noted at the top of the affected version.

## [1.14.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.14.0) — 2026-10-06

### 新增
- **本地 OpenAI 兼容网关**：本机上的 IDE、脚本与第三方工具可以直接连接你已下载的本地模型（也可以把请求用**你自己的 API Key** 转发到你配置的云端提供商）。**默认不监听**——在「设置 → 网关」里配置别名并开启后才会启动；仅监听本机 `127.0.0.1`，所有请求都需要令牌（令牌明文只保存在系统凭据库），并且面板里常显数据去向说明
- **点击通知直接回到对话窗口**：点击「任务完成」这类系统通知，即可把 ChengYoung 窗口带回前台
- **标题栏新增置顶图钉**：一键把窗口置顶；同时开多个窗口时，每个窗口各记各的状态

### 修复
- **全新安装也会把数据放在 ChengYoung 自己的目录下**：此前只有从旧版本升级过的用户会迁移数据，全新安装的用户数据仍写在旧版目录里（与旧版应用并装会互相影响）；现在两种情况一致，都在 `%APPDATA%\ChengYoung\` 下
- **应用内「项目提示」帮助页的配置目录改成真实路径**：此前仍写着品牌化之前的旧目录
- **首次启动的数据说明改写为「你的数据，始终在你手里」**：四条改为「存储位置 / 无遥测 / 审计日志 / API Key」的分组写法，写明数据保存在本机 `%APPDATA%\ChengYoung` 目录且不经过我们的服务器、不会向任何外部地址上传遥测或行为数据、审计日志默认加密存储且仅你可读写（可随时关闭或彻底删除）、API Key 只存放在操作系统密钥链中且不以明文写盘
- **右栏文件树在每轮对话结束后自动刷新**：此前要手动刷新才能看到新文件
- **中文回复更一致**：强化了输出语言指令，减少回复里中英混排
- **网关在异常退出后能正常启动**：断电或进程被杀会留下半条审计记录，此前会让网关无法启动；现在只丢弃这条损坏的尾部记录并记入日志，其余记录完整保留（本机加密审计文件位于数据目录的 `gateway\` 下）
- **提示文件改用 ChengYoung 自己的名字 `.cyhints`**：此前设置 → 聊天的标题、配置弹窗与「使用 .goosehints」帮助页里都写着上游的 `.goosehints`，现在界面与帮助文档统一为 `.cyhints`。**旧项目里的 `.goosehints` 照旧会被读取**（不必改名、内容零丢失），应用新建或保存时写入 `.cyhints`
- **右栏浏览器工具条改为图标按钮**：打开 / 关闭 / 用系统浏览器打开 / 后退 / 前进 / 刷新只留图标，含义改为**鼠标悬停提示**显示（键盘聚焦同样显示）—— 这些文案在其它语言里可能长出一倍，此前会把按钮行折成两三行、把网页区域挤小
- **回答「怎么做」类问题时先说明、不抢跑**：本地小模型面对「怎么创建工作流」这类提问，会先用文字把方法讲清楚，再主动提议代为执行，而不是上来就运行命令演示
- **工作流目录指引统一为 `.agents/recipes/`**：模型介绍工作流的存放位置时，统一指向项目内的 `.agents/recipes/` 目录（各工具通用的约定）；老项目里 `.goose/recipes/` 下的工作流**照旧会被读取**，无需迁移

### 安全
- 网关仅绑定本机回环地址，校验 `Host` 头（防 DNS 重绑定），不返回任何 CORS 头；除健康检查外所有端点都要求令牌
- 审计日志**只记录元数据**（模型、Token 数、延迟），对话正文永不写入，且在本机加密保存；面板提供一键清空

### Added
- **Local OpenAI-compatible gateway**: IDEs, scripts and third-party tools on this computer can connect directly to the local models you already downloaded (or forward requests to the cloud provider you configured, using **your own API Key**). It does **not listen by default** — it starts only after you add an alias and turn it on in Settings → Gateway. It binds to `127.0.0.1` only, every request needs a token (the plaintext token is stored in the system keychain), and the panel always shows where data goes
- **Clicking a notification brings the chat window back to the front**
- **New pin button in the title bar**: keep the window always on top; with multiple windows open, each window tracks its own state

### Fixed
- **Fresh installs now keep their data under ChengYoung's own directory too**: previously only upgrades migrated it, while a brand-new installation still wrote into the legacy directory (so the two interfered); both paths now use `%APPDATA%\ChengYoung\`
- **The bundled "project hints" help page shows the real config directory**: it still named the pre-branding path
- **The first-launch data notice now reads "Your data stays in your hands"**: the four points are grouped as Storage / No telemetry / Audit logs / API keys — stating that data stays in your local `%APPDATA%\ChengYoung` folder and never passes through our servers, that no telemetry or behavioral data is uploaded to any external address, that audit logs are stored encrypted by default and readable only by you (turn them off or delete them at any time), and that API keys live only in your OS keychain and are never written to disk in plain text
- **The file tree in the right panel now refreshes automatically at the end of every turn** (it previously needed a manual refresh)
- **More consistent Chinese replies**: the output-language instruction was strengthened to reduce mixed-language answers
- **Gateway recovers after a crash or forced kill**: a power loss or killed process could leave a truncated audit record, which previously prevented the gateway from starting; the app now discards only that corrupted trailing record and logs it, while all other records stay intact (the local encrypted audit file is stored in the `gateway` subfolder of the data directory)
- **Project hints now use ChengYoung's own file name `.cyhints`**: the Settings → Chat page title, the configuration dialog, and the bundled help page used to name the upstream `.goosehints`; the UI and the help documents now say `.cyhints`. Existing `.goosehints` files are still read, so nothing needs renaming and no content is lost; the app writes `.cyhints` when it creates or saves the file
- **Browser toolbar uses icons only**: Open / Close / Open in system browser / Back / Forward / Refresh now show a hover tooltip (also on keyboard focus) instead of text — those labels can be twice as long in other languages, which used to wrap the button row onto two or three lines and shrink the page area
- **How-to questions get an explanation first**: when asked things like "how do I create a workflow?", local small models now answer in words first and offer to run it for you, instead of jumping straight into running demo commands
- **Workflow directory guidance unified on `.agents/recipes/`**: when describing where workflows live, the model now points to the project's `.agents/recipes/` directory (a tool-agnostic convention); workflows under `.goose/recipes/` in older projects **are still read** — nothing needs migrating

### Security
- The gateway binds to the loopback address only, validates the `Host` header (anti DNS-rebinding) and returns no CORS headers; every endpoint except the health check requires the bearer token
- Audit logs keep **metadata only** (model, token counts, latency) — conversation content is never written — and are stored encrypted on this computer, with a one-click clear button

## [1.13.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.13.0) — 2026-10-05

### 新增
- **自动检查与下载更新**：应用默认每 4 小时自动检查更新（首次检查在启动 5 分钟后），发现新版本会自动后台下载并在就绪时通知——安装仍需确认；两个开关（自动检查 / 自动下载）均可在设置 → 关于 ChengYoung 中关闭
- **数据目录品牌化**：数据目录迁移至 `%APPDATA%\ChengYoung\`，与原版 Goose 并装互不影响；首次启动自动把旧数据完整复制到新目录，会话历史与配置原样保留（旧目录保留不动，可自行删除）
- **README 说明增强**：新增修改声明（Apache-2.0 第 4 节）、零遥测自测方法、minisign 公钥指纹、内存分档建议与常见问题（FAQ）

### 修复
- **修复托盘「停止引擎」后托盘图标无响应的问题**：确认对话框此前在主线程上同步等待，导致托盘图标的左键与右键全部失去响应；现改为非阻塞弹窗，停止操作在确认后正常执行，无需重启应用

### Added
- **Automatic update checks and downloads**: the app now checks for updates every 4 hours by default (first check 5 minutes after launch), downloads new versions automatically in the background and notifies you when ready — installation still asks for confirmation. Both switches (automatic check / automatic download) can be turned off in Settings → About ChengYoung
- **Branded data directory**: the data directory moves to `%APPDATA%\ChengYoung\`, isolated from the original Goose; the first launch copies legacy data in full to the new directory — session history and configuration are preserved as-is (the old directory is left untouched and can be deleted manually)
- **README improvements**: notice of modifications (Apache-2.0 §4), zero-telemetry self-verification guide, minisign public-key fingerprint, tiered memory recommendations, and a FAQ

### Fixed
- **Fixed the tray icon becoming unresponsive after "Stop engine"**: the confirmation dialog previously blocked the main thread, freezing all tray interactions; the dialog is now non-blocking and the stop action runs normally after confirmation, with no restart required


## [1.12.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.12.0) — 2026-10-05

### 新增
- **引擎生命周期管理**：本地推理引擎意外退出后，应用会自动按退避策略重启（立即 / 2 秒 / 8 秒，最多 3 次）；长期稳定运行（超过 60 秒）后的崩溃不会继承历史失败。多次重启仍失败时才显示错误，并提供诊断与日志入口
- **托盘新增引擎控制**：托盘菜单显示引擎状态（运行中 / 已停止），并提供「停止引擎」与「启动引擎」入口。停止引擎只释放模型占用的显存——聊天记录保留、应用保持打开（需二次确认）；停止引擎 ≠ 退出应用
- **错误页按引擎状态区分**：引擎是从托盘主动停止时，错误页显示「启动引擎」而非「重启」；连续两次重启失败后收起重试按钮，只保留「打开日志文件夹」诊断出口

### Added
- **Engine lifecycle management**: after an unexpected exit of the local inference engine, the app now restarts it automatically with backoff (immediately / 2 s / 8 s, up to 3 attempts); a crash after a long stable run (over 60 s) does not inherit past failures. The error state is shown only after repeated restart attempts fail, with diagnostics and log access
- **Engine controls in the tray**: the tray menu now shows the engine state (running / stopped) and offers "Stop engine" / "Start engine". Stopping the engine only frees the model's GPU memory — chat history is kept and the app stays open (with a confirmation dialog). Stopping the engine is distinct from quitting the app
- **Error page distinguishes engine states**: when the engine was stopped from the tray, the page offers "Start engine" instead of "Restart"; after two consecutive failed restarts the retry button is replaced by an "Open logs folder" diagnostics exit


## [1.11.1](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.11.1) — 2026-10-05

### 修复
- **错误页的「重试」现在真正可用**：此前本地推理引擎被停止或意外退出后，错误页的重试只会反复检查一个已不存在的服务、永远失败；现在重试会让应用自动重启引擎并重新连接（约 3-5 秒）。重启本身失败时提供「打开日志目录」，便于附在反馈里排查
- 系统信息的 OS 一行现在带平台名（`Windows 10.0.26300`），不再只有裸版本号

### Fixed
- **The "Retry" button on the error page now actually works**: after the local inference engine was stopped or crashed, retrying only re-checked a dead endpoint and always failed; retrying now restarts the engine automatically and reconnects (about 3-5 seconds). If the restart itself fails, an "Open logs folder" action is offered for troubleshooting feedback
- The OS line in system info now includes the platform name (`Windows 10.0.26300`) instead of a bare version number

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

## [1.11.1](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.11.1) — 2026-10-05

### Fixed
- **The "Retry" button on the error page now actually works**: after the local inference engine was stopped or crashed, retrying only re-checked a dead endpoint and always failed; retrying now restarts the engine automatically and reconnects (about 3-5 seconds). If the restart itself fails, an "Open logs folder" action is offered for troubleshooting feedback
- The OS line in system info now includes the platform name (`Windows 10.0.26300`) instead of a bare version number

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

