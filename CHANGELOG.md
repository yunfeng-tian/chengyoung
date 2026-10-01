# Changelog

本项目的版本发布记录（面向用户）。每个发布版本从最新往下；「新增 / 改进与修复 / 安全」为面向用户的要点。破坏性变更（Breaking Changes）如有，会在对应版本顶部标注。

## [1.7.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.7.0) — 2026-10-01

### 新增
- **内嵌浏览器面板化**：第三方网页在主窗口内的隔离面板中打开，随主窗口联动收缩；独立配置目录 / 零注入 / 无法获取任何应用权限
- **Agent 浏览器控制增强**：支持输入并提交表单（默认全关、逐域名授权、每次操作确认、敏感字段页面端拦截、输入内容绝不写入任何日志）
- 顶栏新增「切换辅助栏」按钮
- **页面可见输入框枚举**：Agent 可列出页面上真实可见的输入框（只给选择器，不含已填内容），提升表单交互准确性

### 修复
- 本地推理在「核显 + 独显」混合架构设备上的 Vulkan 稳定性问题
- 配置网络代理时本机服务的连接异常

## [1.4.x Series](https://github.com/yunfeng-tian/chengyoung/releases) — 2026-09-27 – 2026-09-29

### 新增
- **Agent 浏览器控制**：读取页面文本 / 点击元素 / 后退前进刷新（**默认全关**、逐域名授权）
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

