# 隐私说明 / Privacy

本文与应用内「设置 → App → 隐私详情」同源，是 ChengYoung 隐私政策的**权威对外来源**（另一份为随包帮助文档）。遇到问题或需要反馈，见 [Support](SUPPORT.md)。

## 隐私说明（中文）

ChengYoung 在你的电脑上运行。**本版本不采集、不上传使用数据，也不生成设备标识。**

**我们不会收到你的会话、代码、文件或工具参数。本版本不含任何遥测。**

### 保留在你本机的内容，以及数据的去向

- 本地 ChengYoung 数据目录中的会话、设置与日志
- 下载的模型及其配置，保存在你的磁盘上
- 诊断信息只有在**你自己导出并发送**时才会离开你的电脑
- 扩展与技能只从**你选择的来源**安装
- 插件带来的 MCP 服务器与 hooks 会在你的机器上运行，和任何本机程序一样可以访问网络

### 联网时机

只有在你主动触发时才会联网：

1. 下载本地模型（Hugging Face）
2. 检查 / 下载更新（GitHub Releases）—— 仅发送当前版本号，以及任何网络请求都会带有的常规连接信息
3. 你自行配置的模型提供商 API —— 你发给模型的消息只会发送给你配置的提供商；工具调用会发给你启用的扩展、MCP 服务器及其操作的网站
4. 你安装的插件带来的 MCP 服务器与 hooks —— 和任何本机程序一样可以访问网络

### 内嵌浏览器

- 浏览器控制**默认关闭**。只有你开启该功能并授权某个站点后，Agent 才能读取该页文本或点击元素。Agent 读到的内容会进入模型上下文（本地模型留在本机，云模型会发送给对应的云提供商），并保存在你的本地会话记录中，但不会以明文写入应用日志。
- 写入输入框与提交表单由**另外的开关**控制：每次操作前都会先征求你的同意，且 Agent 写入的文本不会记入应用日志。
- 内嵌浏览器里打开的网页**直连该网站**：我们不代理、不改写，也不记录页面内容或请求；浏览器会像任何浏览器一样在本机保留缓存与 Cookie。

### 数据目录

会话与设置保存在本机 `%APPDATA%\ChengYoung\` 目录（1.13.0 起从上游 `Block\goose` 路径完成了一次性自动迁移；旧版本升级时旧数据会完整复制到新目录，会话历史和配置保持不变）。

---

## Privacy Policy (English)

ChengYoung runs on your computer. **This version does not collect or upload usage data, and it does not create a device identifier.**

**We never receive your conversations, code, files, or tool arguments. This version has no telemetry.**

### What stays on your device, and where data goes

- Conversations, settings, and logs in your local ChengYoung data folder
- Downloaded models and their configuration, kept on your disk
- Diagnostics leave your computer only if you export and send them yourself
- Extensions and skills are installed only from sources you choose
- A plugin's MCP servers and hooks run on your machine and can reach the network like any other program you run

### When the app goes online

The app only goes online when you:

1. Download local models (Hugging Face)
2. Check for or download updates (GitHub Releases) — only your current version number is sent, plus the connection details every web request carries
3. Call the model providers you configured — messages you send to the model go only to that provider; tool calls go to the extensions, MCP servers and websites they act on
4. Use MCP servers and hooks installed by your plugins — they can reach the network like any other program on your machine

### Embedded browser

- Browser control is **off by default**. Only after you turn on the feature and authorize a site can the Agent read that page's text or click elements. What the Agent reads enters the model context (a local model keeps it on your computer, a cloud model sends it to the corresponding cloud provider), and it is saved in your local conversation history but never written to application logs in plain text.
- Typing into fields and submitting forms are controlled by **separate switches**: each operation asks for your consent first, and text the Agent types is never written to application logs.
- Pages you open in the embedded browser load straight from that site. We do not proxy or rewrite them, and we do not keep a record of page content or requests; the browser keeps its own local cache and cookies, like any browser.

### Data directory

Sessions and settings live on your machine in the `%APPDATA%\ChengYoung\` directory (migrated once from the upstream `Block\goose` path in v1.13.0; when upgrading from an older version, your existing data is fully copied to the new folder — conversations and settings are preserved).