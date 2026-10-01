# 隐私说明 / Privacy

本文与应用内「设置 → 关于与帮助 → 隐私详情」同源，是 ChengYoung 隐私政策的**权威对外来源**（另一份为随包帮助文档）。

## 隐私说明（中文）

ChengYoung 在你的电脑上运行。**本版本不采集、不上传使用数据，也不生成设备标识。**

**我们不会收到你的会话、代码、文件或工具参数。本版本不含任何遥测。**

### 保留在你本机的内容

- 本地 ChengYoung 数据目录中的会话、设置与日志
- 下载的模型及其配置，保存在你的磁盘上
- 诊断信息只有在**你自己导出并发送**时才会离开你的电脑
- 扩展与技能只从**你选择的来源**安装

### 联网时机

只有在你主动触发时才会联网：

1. 下载本地模型（Hugging Face）
2. 检查 / 下载更新（GitHub Releases）—— 仅发送当前版本号（不含设备信息或用户标识）
3. 你自行配置的模型提供商 API —— 你的消息只会发送给你配置的提供商

### 内嵌浏览器

- 浏览器控制**默认关闭**。只有在你打开并授权某个站点后，Agent 才能读取该页文本或点击元素；读到的内容进入模型上下文（本地模型留在本机，云端提供商接收）；页面文本绝不写入任何日志。
- 写入输入框与提交表单是**独立的权限控制**：每一次写入与提交都会先征求你的同意，Agent 写入的文本也绝不写入任何日志。
- 内嵌浏览器里打开的网页**直连该网站**：我们不代理、不改写、也不记录它们。

### 数据目录

会话与设置保存在本机 `%APPDATA%\Block\goose\` 目录（保留上游目录名以确保升级兼容性，即上游 Goose 项目的默认路径；可用环境变量 `GOOSE_PATH_ROOT` 覆盖）。

---

## Privacy Policy (English)

ChengYoung runs on your computer. **This version does not collect or upload usage data, and it does not create a device identifier.**

**We never receive your conversations, code, files, or tool arguments. This version has no telemetry.**

### What stays on your device

- Conversations, settings, and logs in your local ChengYoung data folder
- Downloaded models and their configuration, kept on your disk
- Diagnostics leave your computer only if you export and send them yourself
- Extensions and skills are installed only from sources you choose

### When the app goes online

The app only goes online when you:

1. Download local models (Hugging Face)
2. Check for or download updates (GitHub Releases) — only your current version number is sent (no device info or user identifiers)
3. Call the model providers you configured — your messages go only to that provider

### Embedded browser

- Browser control is **off by default**. Only after you turn it on and authorize a site can the Agent read that page's text or click elements; what it reads enters the model context (a local model keeps it on your computer, a cloud provider you configured receives it). Page text is never written to any logs.
- Typing into fields and submitting forms require **separate explicit permissions**: every write and submit asks for your consent first, and the text the Agent types is never written to any logs.
- Pages you open in the embedded browser load straight from that site. We do not proxy, rewrite, or record them.

### Data directory

Sessions and settings live on your machine in the `%APPDATA%\Block\goose\` directory (upstream directory name retained for upgrade compatibility; override it with the `GOOSE_PATH_ROOT` environment variable).
