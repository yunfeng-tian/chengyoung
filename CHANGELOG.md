# Changelog

本项目的版本发布记录（面向用户）。每个发布版本从最新往下；「新增 / 修复 / 安全」为面向用户的要点。破坏性变更（Breaking Changes）如有，会在对应版本顶部标注。

Release notes for this project (user-facing). Versions are listed from newest to oldest; "Added / Fixed / Security" are the user-facing highlights. Breaking changes, if any, are noted at the top of the affected version.

## [1.25.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.25.0) — 2026-10-24

> **模型选择更聪明：置顶、目录合并与自动刷新**：模型下拉新增 ⭐ 置顶——手动输入的模型 ID 自动置顶并优先展示，目录刷新也不再清除它们；模型清单改为「在线目录 + 本地保留」合并语义；清单过期时自动后台刷新，选择器不再被刷新卡住。另有一批下拉体验修复与火山引擎 Coding Plan 配置引导。

### 新增

- **模型置顶（⭐）**：
  - 模型下拉的每一行都有星标：点击即可置顶 / 取消置顶，置顶项按置顶顺序排在「置顶」分组中；
  - 手动输入的模型 ID 在确认后自动置顶——即使它不在提供商的在线目录里，也会一直出现在下拉中（合并语义保证刷新后仍可用）；
  - 搜索时置顶组跟随过滤，与「全部」组使用同一套匹配规则；
  - 置顶跨窗口同步，写失败自动回滚，不影响当前选择。
- **火山引擎 Coding Plan 引导**：在「连接提供商」向导选择 `volcengine-coding-plan` 模板时会提示保持模型 ID 为 `ark-code-latest`（聚合入口，由火山引擎路由到实际模型，模型更新无需逐个改名）；快速开始文档与官网说明同步补充。

### 变更

- **模型清单合并语义**：提供商的在线模型清单刷新后，不再整体替换本地清单——在线目录在前、配置里保留的模型（含已下线条目）作为「本地保留」区追加在后；别名（如聚合入口 ID）不再因刷新而消失，从配置中删除的模型也不会复活。
- **清单过期自动刷新**：打开模型选择器时先用现有清单立即渲染（不再等待）；清单过期或从未同步时在后台自动刷新并替换为最新结果——刷新失败只记录警告，不影响当前使用。
- **刷新失败退避**：从未成功同步的提供商不再随每次会话创建反复请求在线清单（6 小时失败退避）；手动刷新与保存配置等显式动作始终立即刷新。
- **内置模型过滤**：在线清单落库前自动排除嵌入 / 重排 / 语音类模型（ID 含 embed / rerank / tts / asr），它们对对话模型选择没有意义；手动配置与置顶的模型不受影响。

### 修复

- **下拉分组头样式**：修复「置顶 / 全部」分组头在 unstyled 下拉中丢失样式的问题。
- **搜索与后台刷新不再互相打断**：后台刷新完成时若正在搜索，列表保持按搜索词过滤，不再回弹到全量。
- **后台刷新失败不再清空列表**：刷新后重拉结果为空时保留已渲染的清单。

### 移除

- 本次发布没有移除既有功能。

### 安全

- 本版本不采集、不上传任何使用数据的承诺不变；模型清单的在线刷新仅向您配置的提供商 API 发起请求，收藏数据只保存在本机设置中（详见应用内「隐私详情」）。

> **Smarter model switching: pins, merged catalogs and auto-refresh** — the model dropdown gains ⭐ pins: hand-typed model IDs are pinned automatically and survive catalog refreshes; the model list now merges the online catalog with locally kept entries; a stale list refreshes in the background instead of blocking the picker. Also includes dropdown UX fixes and Volcengine Coding Plan setup guidance.

### Added

- **Model pins (⭐)**: every row in the model dropdown has a star — click to pin/unpin; pinned models are grouped first in pin order. Hand-typed model IDs are pinned on submit and keep appearing in the dropdown even when absent from the provider's online catalog (merge semantics keep them usable across refreshes). The pinned group follows search filtering using the same matching rules as the "All" group, and pins sync across windows with rollback on write failure, without affecting the current selection.
- **Volcengine Coding Plan guidance**: the provider wizard now hints to keep `ark-code-latest` as the model ID for the `volcengine-coding-plan` template (an aggregated entry point routed by Volcengine); quickstart docs updated accordingly.

### Changed

- **Merged model catalog semantics**: refreshing a provider's online model list no longer replaces the local list — the online catalog comes first, models kept in your configuration (including offline ones) are appended as a locally-kept section; aliases survive refreshes and models removed from the configuration are not revived.
- **Stale-while-revalidate catalog refresh**: opening the model picker renders the existing list immediately; a stale or never-synced list refreshes in the background and swaps in the fresh result — refresh failures only log a warning without affecting current usage.
- **Refresh backoff**: providers that never synced successfully no longer re-request their online list on every session creation (6-hour backoff); explicit actions like manual refresh always refresh immediately.
- **Built-in model filter**: embedding / reranking / speech models (IDs containing embed / rerank / tts / asr) are excluded from refreshed online lists; manually configured and pinned models are unaffected.

### Fixed

- **Dropdown group heading styles**: the "Pinned / All" group headings no longer lose their styling in the unstyled dropdown.
- **Search and background refresh no longer fight**: when a background refresh lands mid-search, the list stays filtered by the current query instead of snapping back to the full catalog.
- **A failed background refresh no longer empties the list**: an empty re-fetch keeps the already-rendered list.

### Removed

- Nothing was removed in this release.

### Security

- The no-collection, no-telemetry promise is unchanged; online catalog refreshes only talk to the provider APIs you configured, and pin data stays in local settings (see in-app "Privacy details").

## [1.24.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.24.0) — 2026-10-23

> **截屏体验全面升级 + 本地推理性能修复**：截屏选区新增变暗遮罩与实时尺寸标注、支持方向键微调与双击确认；多屏场景下覆盖窗跟随触发来源、截屏进行中再按热键即可在当前屏幕重新开始。本地推理修复了模型被错误分配到 CPU 或集成显卡的问题——独立显卡全量接管后，推理速度最高提升数倍，偶发空回复的问题也已根治；默认推荐模型（Gemma-4-E4B）的工具调用可靠性已通过全量场景验证。

### 新增

- **截屏选区体验**：
  - 选区外变暗遮罩：一眼看清截取范围（此前选区内外亮度相同，只能靠细边框辨认）；
  - 选区尺寸实时标注：拖动与调整手柄时实时显示 宽 × 高；
  - 方向键微调：±1 像素，按住 Shift 为 ±10 像素；
  - 双击选区即可确认截图（与主流截图工具的操作习惯一致）。
- **截屏换屏重开**：截屏进行中再次按 Ctrl+Alt+A，会取消当前选区并以光标所在屏幕重新开始——多屏下选错屏幕时，不必再回到原屏幕取消后重来。

### 变更

- 本版本没有对既有行为的调整；截屏换屏重开（见「新增」）是全新的交互，不改变任何既有快捷键的行为。

### 修复

- **截屏覆盖窗跟随触发来源**：修复双屏扩展模式下覆盖窗固定开在主屏、导致另一侧屏幕无法截屏的问题——快捷键发起时出现在光标所在屏幕，输入框「+」菜单发起时出现在发起窗口所在屏幕。
- **本地推理性能：模型不再全部落在 CPU 上运行**：修复本地模型在部分机器上全部层都在 CPU 运行的问题——修复后模型完整驻留独立显卡，推理速度提升可达数倍。
- **本地推理：混合显卡场景下模型不再错分到集成显卡**：双显卡（独立 + 集成）机器上，模型层此前可能被拆分到集成显卡导致速度骤降，现在固定驻留性能更强的独立显卡。
- **本地推理：偶发空回复已根治**：本地模型偶尔返回空内容的问题已修复。

### 移除

- 本次发布没有移除既有功能。

### 安全

- 本版本不采集、不上传任何使用数据的承诺不变；截屏功能的截图内容仍只在本机内存中临时存在，不写入磁盘、不经过网络上传（详见应用内「隐私详情」）。

> **Screenshot UX overhaul + local inference performance fixes** — the selection overlay now dims the area outside the selection and shows a live W×H size label, with arrow-key nudging and double-click to confirm; on multi-monitor setups the capture window follows the triggering source, and pressing the hotkey again mid-session restarts the capture on the screen under your cursor. Local inference no longer misplaces the model on the CPU or the integrated GPU — with the discrete GPU fully utilized, inference is up to several times faster, and the occasional empty-reply issue is permanently fixed. The default recommended model (Gemma-4-E4B) has passed full tool-calling validation.

### Added

- **Screenshot selection experience**:
  - Dimmed overlay outside the selection: the captured area is obvious at a glance (previously both areas had identical brightness, distinguishable only by a thin border);
  - Live size label: shows width × height while dragging or adjusting the handles;
  - Arrow-key nudging: ±1 pixel, hold Shift for ±10 pixels;
  - Double-click inside the selection to confirm (consistent with mainstream screenshot tools).
- **Restart capture on another screen**: pressing Ctrl+Alt+A again while a capture session is active cancels the current selection and restarts on the screen under your cursor — no need to go back to the original screen to cancel first.

### Changed

- No changes to existing behavior in this release; the mid-session restart (see "Added") is an entirely new interaction and does not alter any existing shortcut behavior.

### Fixed

- **Capture window follows the triggering source**: fixed an issue where, on extended dual-monitor setups, the capture window always opened on the primary screen and left the other screen unusable — it now appears on the screen where the hotkey was pressed, or on the screen of the originating window when started from the "+" menu.
- **Local inference performance: the model no longer runs entirely on the CPU**: fixed an issue where, on some machines, all model layers were placed on the CPU — the model now resides fully on the discrete GPU, with inference speed improved several times.
- **Local inference: hybrid-GPU machines no longer spill onto the integrated GPU**: on dual-GPU (discrete + integrated) machines, model layers could previously be split onto the integrated GPU and slow inference to a crawl; they now stay on the faster discrete GPU.
- **Local inference: occasional empty replies permanently fixed**: local models sometimes returned empty content; this has been fixed.

### Removed

- Nothing was removed in this release.

### Security

- Our commitment not to collect or upload any usage data is unchanged; screenshot content from the capture feature still exists only in local memory — never written to disk, never sent over the network (see in-app "Privacy details").

## [1.23.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.23.0) — 2026-10-21

> **新增「截屏提问」**：在任何应用里按 Ctrl+Alt+A，框选屏幕任意区域，截图直接进入输入框——配合「提取文字 / 翻译 / 解题」三个一键指令，把看到的内容立刻交给 AI 处理。截图全程只在本机内存中处理，不保存文件、不上传。

### 新增

- **截屏提问**：
  - 全局快捷键 Ctrl+Alt+A：无需切回应用，任何界面下直接发起截屏（可在「设置 → 快捷键」中改键或禁用，改键即时生效）；
  - 全屏快照上直接框选：八方向手柄精调、选区内拖动移位、工具条跟随选区位置；
  - 三个一键指令：**提取文字 / 翻译 / 解题**——确认选区后自动填入对应指令，截图与问题一起发送；
  - 输入框「+」菜单同步提供「截屏提问」入口；
  - 隐私：截图只在内存中临时处理，不保存到磁盘、不上传任何内容，应用关闭即消失。
- **拖拽文件到输入框**：把文件直接拖进对话窗口即可添加（此前只能通过「+」菜单选择）。
- **图片数量上限提示**：单条消息最多 10 张图片，超出时不再静默丢弃，而是明确提示。
- 上述界面文案覆盖全部 16 种语言。

### 变更

- **会话启动等待更有反馈**：首次对话的启动等待超过 1.5 秒时，会显示「正在准备模型」的第二阶段提示，不再出现假死现象；启动等待画面为半透明毛玻璃，首页隐约可见；
- **侧栏分组更易辨认**：当前页面所在的侧栏分组，组头会显示高亮竖条。

### 修复

- **通知中心空态过高**：没有通知时，通知面板不再多占一块空白（此前会挡住输入框右侧）。

### 移除

- 本次发布没有移除既有功能。

### 安全

- 本版本不采集、不上传任何使用数据的承诺不变；「截屏提问」的截图内容只在本机内存中临时存在，不写入磁盘、不经过网络上传（详见应用内「隐私详情」）。

> **New: Screenshot Ask** — press Ctrl+Alt+A in any app, select any region of your screen, and the screenshot goes straight into the chat input — combined with three one-tap prompts (**Extract text / Translate / Solve**) to hand what you see to the AI instantly. Screenshots are processed in memory only: nothing is saved to disk, nothing is uploaded.

### Added

- **Screenshot Ask**:
  - global shortcut Ctrl+Alt+A: start a capture from anywhere without switching apps (remappable or can be disabled in Settings → Shortcuts, effective immediately);
  - select directly on a full-screen snapshot: 8-way adjustment handles, drag-to-move inside the selection, and a toolbar that follows the selection;
  - three one-tap prompts: **Extract text / Translate / Solve** — confirming the selection fills in the matching instruction automatically and sends both the screenshot and your question;
  - a "Screenshot Ask" entry in the input box "+" menu;
  - privacy: screenshots live in memory only — never written to disk, never uploaded, gone when the app closes.
- **Drag files into the input box**: drop files straight onto the conversation window to attach them (previously only via the "+" menu).
- **Image limit feedback**: when the 10-images-per-message limit is reached, you now get a clear notice instead of silently dropping them.
- All of the above copy ships in all 16 languages.

### Changed

- **Better feedback while a session starts**: if the first conversation takes longer than 1.5 seconds to start, a second-stage hint appears ("preparing the model") so it no longer appears frozen; the waiting overlay is now a translucent frosted glass with the home page faintly visible;
- **Sidebar groups easier to tell apart**: the group containing the current page shows a highlighted bar on its header.

### Fixed

- **Notification panel empty state**: the panel no longer reserves an oversized blank area when there are no notifications (it previously could cover the right side of the input box).

### Removed

- Nothing was removed in this release.

### Security

- The commitment of no telemetry and no data collection is unchanged; Screenshot Ask content exists temporarily in memory only — never written to disk, never sent over the network (see in-app "Privacy details").

## [1.22.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.22.0) — 2026-10-20

> **新增「用量与费用」页**：所有对话的 Token 与费用统计在一处看全——本月用了多少、哪个模型花得最多、每天的趋势如何。入口在侧栏「更多」分组，也可以直接点击对话底部的费用条跳转（自动定位到该对话）。所有统计只保存在本机。

### 新增

- **用量与费用页**：
  - 总费用与 Token 总量，支持本月 / 近 30 天 / 近 90 天 / 全部时间四档切换；
  - 按模型统计表：每个模型的费用、输入 / 输出 / 缓存 Token 与请求次数；
  - 趋势图：按日堆叠柱状图，区分「提供商实付」与「按目录价估算」两段；
  - 排行视图：按费用 / 按 Token 两个口径，可按模型或按日期切换；
  - 计价状态一目了然：每笔统计带「提供商实付 / 按目录价估算 / 自托管（不计费）/ 暂不可计价 / 未归属」徽标，费用数字旁边会说明口径（例如估算包含系统提示、工具定义与历史记录的输入 Token）；
  - 从对话底部的费用条点击直达，并自动筛选到该会话；窗口较窄时自动切换为适合的紧凑布局。
- 上述界面文案覆盖全部 16 种语言。

### 变更

- **统计更耐久**：费用与模型信息在对话发生时即以价格快照记录——之后模型价格调整不会改写历史统计；
- **删除对话保留统计**：删除对话不再连带删除其用量记录（历史统计完整保留，仅解除与该对话的关联）；
- **消息 Token 数更易查看**：消息下方的 Token 数徽标改为常驻低对比度显示（此前仅悬停才出现），悬停展开的明细浮层适配深色主题；
- **下拉风格统一**：时间范围下拉框与全局组件风格一致（含深色模式的下拉弹层）。

### 修复

- **应用内更新偶发安装失败**：修复更新安装时偶发的「Error opening file for writing」——安装等待时间加长，并在拉起安装器前清理残留进程；
- **新对话输入框残留**：点击「新对话」返回首页时，输入框不再残留上一个问题的全文。

### 移除

- 本次发布没有移除既有功能。

### 安全

- 本版本不采集、不上传任何使用数据的承诺不变；「用量与费用」页的全部数据保存在本机设备上（详见应用内「隐私详情」）。

> **New: the usage & cost page** — all of your conversations' token and cost statistics in one place: how much this month, which model costs the most, and how usage trends day by day. Find it in the "More" group of the sidebar, or click the cost bar at the bottom of a conversation to jump there (scoped to that conversation). All statistics are stored locally.

### Added

- **Usage & cost page**:
  - total cost and token usage with four time ranges: this month / last 30 days / last 90 days / all time;
  - per-model table: cost, input / output / cache tokens, and request count for each model;
  - trend chart: daily stacked bars separating "billed by the provider" from "estimated at list price";
  - ranking view: switch between cost and tokens, grouped by model or by day;
  - pricing status at a glance: every figure carries a badge — "billed by provider / estimated at list price / self-hosted (no charge) / not yet priceable / unattributed" — and the cost figure explains its basis (e.g. estimates include input tokens from the system prompt, tool definitions, and conversation history);
  - click the cost bar at the bottom of a conversation to jump in with that session pre-selected; the layout adapts automatically when the window is narrow.
- All of the above copy ships in all 16 languages.

### Changed

- **Statistics are more durable**: cost and model info are recorded as a price snapshot at conversation time — later price changes never rewrite historical statistics;
- **Deleting a conversation keeps its statistics**: deleting a conversation no longer deletes its usage records (history is fully preserved; only the link to that conversation is removed);
- **Message token counts easier to see**: the token badge under a message is now always visible at low contrast (previously it only appeared on hover), and the hover detail popover now matches the dark theme;
- **Consistent dropdown style**: the time-range dropdown now matches the global component style (including its dark-mode popover).

### Fixed

- **Occasional in-app update install failures**: fixed the intermittent "Error opening file for writing" during update installation — the delay before launching the installer has been increased, and leftover processes are cleaned up first;
- **Leftover text in the input box on new chat**: returning to the home screen via "New chat" no longer leaves the previous question's full text in the input box.

### Removed

- Nothing was removed in this release.

### Security

- The promise of no collection and no upload of any usage data is unchanged; everything on the usage & cost page is stored on your device (see in-app "Privacy details").

## [1.21.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.21.0) — 2026-10-18

> **行为变化**：当你关闭 ChengYoung 时，此前已在后台下载完成的更新会在退出过程中自动安装（静默、不重启应用）——下次启动即是新版本；不希望自动安装的，请在更新下载完成前不要关闭应用，或在设置中关闭自动下载。另外，长上下文模型的自动压缩触发点现在会明确显示在上下文条上（例如「上限 225k」），并且可以直接点击修改该上限。

### 新增

- **错误卡片与一键重试**：请求失败（断网、服务商故障等）不再以普通文字混入对话，而是显示一张红色错误卡片——说明失败原因（按错误类型给出中文提示）、保留技术详情，并提供「重试」按钮：重试会**从失败点继续**（已完成的工具结果保留、不会重复执行任何操作），也可选择「取消」保留错误记录。
- **长上下文「回到底部」按钮**：浏览超长对话时，离开底部即显示圆形按钮，一键回到最新消息。
- **上下文条显示压缩触发点**：对超大上下文模型（如 1M 窗口），自动压缩的触发点按服务商缓存折扣规则可能早于你设置的比例——现在上下文条会显示实际生效的触发点（如「上限 225k」），并支持点击修改该上限。
- **新对话发送过渡更柔和**：发送首条消息后的等待画面改为半透明毛玻璃效果，并分步显示进度，不再出现假死现象。
- **网关调试端点**（面向开发者）：设置 `CY_GATEWAY_DEBUG=1` 启动时，网关提供 `POST /debug/level` 运行时调整调试级别；审计数据新增 90 天自动清理。

### 变更

- **隐私面板文案全面修订**：更正了插件联网的表述（插件的 MCP 服务器与 hooks 和任何本机程序一样可以访问网络）、区分「消息发模型 / 工具调用发扩展」两条通道、补充更新检查的常规连接信息与浏览器本地缓存的说明——所有表述与实际行为一一对应。
- **费用提示口径更清晰**：悬停费用时，总额标注为「提供商实付」（含缓存折扣），token 数单独列出，不再并列一笔按目录价估算、与实付对不上的金额。
- **通知点击更可靠**：任务完成通知点击后可靠地还原并聚焦主窗口（此前最小化时可能无响应）。
- **压缩过程文案中文化**：长对话压缩时的状态提示与相关授权文案改为随界面语言显示。

### 移除

- 本次发布没有移除既有功能。

### 修复

- **界面品牌遗漏**：修复长对话压缩与部分 OAuth 授权流程中仍显示上游项目名的问题。
- **求助会话恢复后无法解锁工具**：跨重启恢复的只读诊断会话，「只读」横标现在会正确显示（此前不显示导致无法切回修复模式）。
- **诊断会话徽标跨重启恢复**后正确驱动模式开关（同上，属同一条修复链）。

### 安全

- **遥测边界技术化**：启动模型进程时显式清除继承自系统的 `OTEL_*` / `LANGFUSE_*` / 决策器实验类环境变量——「不含任何遥测」从环境约定升级为技术保证。
- **构建链加固**：模型进程产物在打包时强制通过字节级禁串检查（不含任何上报域名）；上游若引入外部决策器实验会被构建门禁直接拦截。
- 本版本不采集、不上传任何使用数据的承诺不变（详见应用内「隐私详情」）。

> **Behavior change**: when you close ChengYoung, an update that has already been downloaded in the background is now installed silently during exit (no relaunch) — the next launch runs the new version. To avoid auto-install, don't close the app while an update is downloading, or turn off automatic downloads in Settings. Long-context models now show the effective auto-compaction trigger on the context bar (e.g. "cap 225k"), clickable to edit that cap.

### Added

- **Error card with one-click retry**: failed requests (network, provider outages) now show a red error card instead of blending into the conversation — with a plain-language explanation of the failure, technical details preserved, and a **Retry** button that **resumes from the point of failure** (completed tool results are kept and nothing is re-executed); you can also dismiss and keep the error as history.
- **Back-to-bottom button**: when scrolling away from the bottom of a long conversation, a round button appears for one-click return to the latest message.
- **Compaction trigger on the context bar**: for very large-context models (e.g. 1M), the effective auto-compaction trigger may be earlier than your configured ratio due to provider cache discounts — the context bar now shows the effective trigger (e.g. "cap 225k") and lets you edit that cap.
- **Softer new-chat transition**: the waiting screen after sending the first message now uses a translucent frosted glass effect with step-by-step progress, instead of a frozen-looking pause.
- **Gateway debug endpoint** (developer-facing): with `CY_GATEWAY_DEBUG=1`, the gateway serves `POST /debug/level` for runtime debug-level changes; audit data now auto-purges after 90 days.

### Changed

- **Privacy panel copy fully revised**: the plugin networking wording is corrected (a plugin's MCP servers and hooks can reach the network like any other program), the "messages to the model" and "tool calls to extensions" channels are separated, and details about the routine connection metadata of update checks and about browser local caching are added — every statement now maps to actual behavior.
- **Clearer cost tooltip**: the total is now labeled "as billed by the provider" (including cache discounts), with token counts listed separately — no longer side by side with a list-price estimate that doesn't add up to the billed total.
- **More reliable notification focus**: clicking the task-completion toast now reliably restores and focuses the main window (previously unresponsive when minimized).
- **Localized compaction copy**: the status text during long-conversation compaction and related authorization copy now follow the interface language.

### Removed

- Nothing was removed in this release.

### Fixed

- **Upstream branding leaks**: fixed lingering upstream project names during long-conversation compaction and parts of the OAuth flow.
- **Help sessions unlockable after restart**: the read-only banner on a restored diagnostic session now appears correctly, so you can switch back to fix mode.
- **Mode badge on restored diagnostic sessions**: the badge now correctly drives the mode switch after a restart (same fix chain as above).

### Security

- **Telemetry boundary made technical**: inherited `OTEL_*` / `LANGFUSE_*` / decision-provider experiment variables are explicitly stripped when the model process starts — "no telemetry" is now a technical guarantee rather than an environment convention.
- **Build chain hardened**: the model-process artifact must pass a byte-level forbidden-string scan at packaging (verifying no reporting domains); an upstream decision-provider experiment would be blocked by the build gate.
- The promise of no collection and no upload of any usage data is unchanged (see in-app "Privacy details").

## [1.20.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.20.0) — 2026-10-17

> **行为变化**：听写设置不再提供「使用对话模型直接转写音频」的选项（该方式需要支持音频输入的模型，普通文本模型会转写失败）——已配置该方式的会话会自动切回「禁用」，请改用本地模型或云端转写服务。另外，输入框工具区已重新布局：添加文件 / 图片的入口移至左下角「+」菜单，麦克风按钮移至发送按钮旁。

### 新增

- **会话模式一键切换**：在输入框中即可在 自主 / 手动 / 智能 / 仅聊天 四种模式间切换，仅对当前会话生效；新会话的默认模式仍在设置中选择。
- **全局右键菜单**：在对话区右键即可复制所选文字、复制整条消息、朗读，或把选中内容作为引用加入输入框；空白处右键不再弹出浏览器默认菜单。
- **听写转写语言设置**：本地听写新增转写语言选择（此前固定按英文识别），支持中文、英语、日语、韩语等 15 种语言。
- **「+」添加菜单**：输入框左下角新增添加入口，上传图片（自动压缩、可预览）与上传文件一步完成。
- **历史消息窗口**：超长会话默认只加载最近 100 条消息，打开速度显著提升；更早的历史可一键展开（每次 100 条），Ctrl+F 全历史搜索功能保留不变，且不再出现加载动画闪烁。
- **消息操作行统一**：所有回复都提供复制入口（此前工具调用类回复没有），并新增「复制为 Markdown」（仅纯文本，贴入 GitHub / 编辑器更干净）。
- **发送即刻响应**：新对话发送消息后立即进入对话页（会话在后台创建），失败时保留草稿可一键重试。
- **等待状态可操作**：「正在等待你的确认」提示条现在会显示等待数量与类型，点击即可定位到对应的审批卡片。

### 变更

- **输入框工具区重新布局**：添加文件 / 图片的入口移至左下角「+」菜单；麦克风按钮移至发送按钮旁；工具排与输入框边缘的间距整体收紧。
- **听写转写失败提示更友好**：使用了不支持音频的模型时，会给出更换听写方式的指引，而不是显示技术错误详情。
- **浏览器面板工具条合并为单行**：导航与页面动作排在同一行，且在浏览器未打开时隐藏不可用的导航按钮（此前为一排置灰按钮）。

### 移除

- 本次发布没有移除既有功能；听写「使用对话模型直接转写」选项的调整见顶部行为变化。

### 修复

- **更新安装不再弹出向导**：点击「安装并重启」后，更新在后台完成并自动重启——期间仅显示一个会自动关闭的进度条，不再弹出需要逐步点击的安装向导窗口。
- **上下文压缩更干净**：修复压缩超长对话时，内部的压缩进度信息可能进入模型上下文并影响回答质量的问题。
- **上传文件无响应**：修复了部分环境下选择文件后没有任何反馈的问题（此前会连续弹出两个文件选择器）。

### 安全

- 本次发布未包含已知安全公告；随常规更新一并加固。

> **Behavior change**: dictation settings no longer offer the option to "transcribe audio directly with the conversation model" (it requires a model that supports audio input — plain-text models fail to transcribe). Sessions previously configured this way automatically revert to "Disabled"; please switch to a local model or a cloud transcription service instead. The input box toolbar is also relaid out: the image and file upload entries now live in the "+" menu at the bottom-left, and the microphone button sits next to the send button.

### Added

- **One-click session mode switch**: switch between Autonomous / Manual / Smart / Chat-only right from the input box, applying to the current session only; the default for new sessions is still chosen in Settings.
- **Global context menu**: right-click in the conversation to copy selected text, copy the whole message, read it aloud, or quote it into the input box; right-clicking empty space no longer shows the browser's default menu.
- **Dictation transcription language**: local dictation gains a transcription-language setting (previously fixed to English recognition), covering Chinese, English, Japanese, Korean and 11 more languages.
- **"+" Add menu**: a new add menu at the bottom-left of the input box — upload images (auto-compressed with preview) or files in one step.
- **History window for long sessions**: very long sessions now load only the latest 100 messages by default (dramatically faster to open); older history expands 100 at a time, Ctrl+F full-history search is preserved, and the flickering loading animation is gone.
- **Unified message actions**: every reply now has a copy entry (tool-call replies previously had none), plus a new **Copy as Markdown** (plain text only — cleaner when pasting into GitHub or editors).
- **Instant send on new chats**: sending from a new chat enters the conversation immediately (the session is created in the background); on failure your draft is kept for one-click retry.
- **Actionable waiting state**: the "waiting for your confirmation" bar now shows how many items are waiting and of which type, and clicking it scrolls to the corresponding approval card.

### Changed

- **Input toolbar relayout**: the image and file upload entries move into the new "+" menu at the bottom-left; the microphone button now sits next to the send button; overall spacing around the toolbar row is tighter.
- **Friendlier dictation errors**: transcribing with a model that does not support audio now shows guidance on switching dictation providers instead of raw technical details.
- **Browser panel toolbar merged into a single row**: navigation and page actions share one row, and unavailable navigation buttons are hidden while no browser window is open (previously a row of greyed-out buttons).

### Removed

- No existing features were removed in this release; see the behavior change at the top regarding the dictation option adjustment.

### Fixed

- **No more setup wizard during updates**: clicking "Install & Restart" now finishes the update in the background and restarts the app automatically — only a self-closing progress bar is shown, instead of the full setup wizard.
- **Cleaner context compaction**: fixed an issue where internal compaction progress details could leak into the model's context during long-conversation compaction and affect response quality.
- **File upload not responding**: fixed an issue where selecting a file gave no feedback in some environments (two pickers were shown back to back).

### Security

- No known security advisories are included in this release; routine updates and hardening apply as usual.

## [1.19.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.19.0) — 2026-10-15

> **行为变化（Breaking）**：本次同步上游后，执行引擎全面切换至状态机（State Machine）——旧执行循环（agent loop）已移除，状态机成为唯一执行路径；扩展改为**首次使用时才启动**（新对话不再等待全部扩展就绪，启动显著提速）。绝大多数对话行为不变；如遇边缘行为差异请反馈。

### 新增

- **会话导出为交互式 HTML**：将会话导出为单文件 HTML 交互式查看器，便于离线归档与分享。
- **消息引用**：在对话中选中文字，即可一键「加入消息」作为引用块继续提问。
- **上下文压缩可视化编辑**：上下文压缩阈值（默认 80%，自动压缩上限 225k token）现在可以在压缩提示条内直接查看与编辑，无需手改配置。
- **新声明式模型提供商**：新增 AIgateway 与 Z.ai Coding Plan 等声明式提供商定义。
- **首次配置询问运行模式**：首次运行引导配置时会询问会话的运行模式。
- **分布式追踪上下文传播**（面向集成开发者）：ACP 提示的 W3C 追踪上下文现在会传播到 MCP 工具调用。

### 变更

- **状态机成为唯一执行引擎**：旧执行循环已移除，所有会话统一由状态机驱动（含空响应重试、工具调用余量核算、子代理前台化等新能力）。
- **扩展按需启动**：扩展管理器重构为按会话租约（lease）管理，扩展在其首次被会话使用时才启动进程——新会话创建不再等待全部扩展就绪。
- **提供商会话路由统一**：会话的提供商路由现在统一由单一 ProviderManager 承载。
- **调度能力模块化**：调度（Scheduler）、平台应用、聊天召回、ACP HTTP 等能力改为按 feature 装配。
- **工具链升级**：Rust 1.99.0。

### 移除

- **Nostr 会话分享**：随上游一并移除（本产品从未在界面开放该入口）。
- **MCP sampling**：模型侧采样（`create_message`）随上游重构移除。
- **后台子代理与编排器**：后台子代理、orchestrator 与旧执行循环一并移除（Doctor 收敛为会话内能力）。

### 修复

- **侧栏会话列表新增加载状态**：会话列表加载中会显示「正在加载…」而非空白（并修复快速切换时的竞态）。
- **压缩计费口径修复**：长上下文价格分档正确应用于成本估算；Bedrock 不再对非上下文类校验错误重试；缓存写入 token 从 Responses API 用量读取。
- **提供商令牌刷新**：GitHub Copilot 令牌被撤销后列出模型时会自动刷新（且只刷新一次）。
- **自适应思考**：Claude Sonnet 与 Claude Haiku 5.5 的自适应思考正确生效。
- **协议版本声明**：initialize 时上报已实现的协议版本。
- **GOOSE_MODE 校验**：跨 ACP 提供商拒绝非法的运行模式值。
- **调度器工具先完成在途调用**：达到最大轮次上限前会先等待在途工具完成。

### 安全

- 本次同步未包含已知安全公告；上游依赖与子进程处理随常规更新一并加固。

### Added

- **Interactive HTML session export**: export a session as a single-file interactive HTML viewer for offline archiving and sharing.
- **Message quoting**: select text in the conversation and add it to your next message as a blockquote with one click.
- **Visual context-compaction editing**: the auto-compaction threshold (default 80%, capped at 225k tokens) can now be viewed and edited directly on the compaction alert bar — no manual config edits.
- **New declarative model providers**: AIgateway, Z.ai Coding Plan and more declarative provider definitions.
- **First-run mode prompt**: the first-run configure flow now asks for the session mode.
- **Distributed trace context propagation** (for integrators): W3C trace context from ACP prompts now propagates to MCP tool calls.

### Changed

- **State Machine is now the only execution engine**: the legacy agent loop has been removed; every session is driven by the state machine (with empty-response retries, remaining-tool-call accounting, foregrounded subagents).
- **Extensions start on first use**: the extension manager was rebuilt around per-session leases — an extension process starts the first time a session requests it, so new sessions no longer wait for every extension to come up.
- **Unified provider routing**: all session provider routing now goes through a single ProviderManager.
- **Modular scheduling**: scheduler, platform-apps, chat-recall and ACP HTTP capabilities are now assembled via features.
- **Toolchain**: Rust 1.99.0.

### Removed

- **Nostr session sharing**: removed along with upstream (never exposed in this product's UI).
- **MCP sampling** (`create_message`): removed with the upstream refactor.
- **Background subagents & orchestrator**: background subagents, the orchestrator and the legacy agent loop are gone (Doctor is now session-only).

### Fixed

- **Sidebar session list loading state**: the list now shows a loading indicator instead of blank space (and a race on rapid refreshes is fixed).
- **Cost accounting fixes**: long-context price tiers apply to estimates; Bedrock no longer retries non-context validation errors; cache write tokens are read from Responses API usage.
- **Provider token refresh**: revoked GitHub Copilot tokens refresh when listing models (refreshing only once).
- **Adaptive thinking**: works correctly for Claude Sonnet and Claude Haiku 5.5.
- **Protocol version stamping**: the implemented protocol version is reported in `initialize`.
- **GOOSE_MODE validation**: malformed mode values are rejected across ACP providers.
- **Pending tools finish before max turns**: in-flight tool calls complete before the turn limit is enforced.

### Security

- No known security advisories in this sync; dependency and subprocess hardening landed with the regular update.

## [1.18.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.18.0) — 2026-10-13

### 新增

- **会话回放**：会话历史列表与顶栏会话名菜单新增「查看回放」——全屏只读时间线按发生顺序还原一场对话：你的消息、AI 回复、工具调用与结果（子代理调用带专属徽标）、思考过程与系统事件；支持关键词过滤（检索范围含工具结果正文）与分页加载（每次 200 条）。
- **双阶段求助诊断**：从错误通知的「问问 ChengYoung」发起的求助会话先进入**只读诊断**——模型不执行任何命令，只收集信息并给出分析建议（输入区顶部显示诊断徽标）；你确认后点「开始修复」，会话切换为修复模式并自动发出开始指令，模型给出工具操作供你逐条确认执行；切换失败会明确提示且徽标保留。
- **错误求助自带完整上下文**：「问问 ChengYoung」现在会把错误标题、描述、堆栈与恢复建议整理为结构化求助草稿（路径、密钥类敏感信息自动脱敏），填入输入框供你补充场景后发送；不再仅发送一个英文模板句子。
- **网关指标新增引擎维度**（实验性功能，默认关闭）：本机网关开启后，`/metrics` 端点新增 `engine_` 前缀指标（引擎启动次数、自动重启次数、最近一次启动耗时）。

### 修复

- **中文与繁中界面补全「本地目录安装插件」的来源占位文案**（此前为英文原文）。
- **会话卡片布局优化**：悬停操作按钮合并进信息底行，卡片更紧凑、不再预留空行。

### Added

- **Session replay**: new "View replay" actions in the session history list and the top-bar session menu open a full-screen, read-only timeline that reconstructs a conversation in order — your messages, AI replies, tool calls with results (sub-agent calls with a badge), thinking blocks and system events — with keyword filtering (search covers tool result text) and paginated loading (200 events per page).
- **Two-phase diagnostic help**: help sessions started from the "Ask ChengYoung" action on error notifications now begin in a **read-only diagnosis** phase — the model runs no commands and only gathers information and suggestions (a diagnostic badge shows above the input). Once you confirm, "Start fixing" switches the session to fix mode and automatically sends the kickoff instruction; the model then proposes tool actions for your per-item approval. A failed switch shows a clear notice and keeps the badge.
- **Error help now carries full context**: "Ask ChengYoung" builds a structured help draft — error title, description, sanitized stack trace and recovery hints (paths and secret-like values are redacted automatically) — into the input box for you to review and send; no more just a single English template sentence.
- **Engine metrics on the gateway** (experimental, off by default): once the local gateway is enabled, `/metrics` gains `engine_`-prefixed gauges (engine starts, automatic restarts, last start duration).

### Fixed

- The "install from local directory" source placeholder in the plugin dialog is now properly translated in Simplified and Traditional Chinese (previously shown in English).
- Session card layout: hover actions merged into the info row — cards are more compact with no reserved empty row.

## [1.17.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.17.0) — 2026-10-12

### 新增
- **内置帮助中心（用户指南）**：对话页右上角的 ChengYoung Logo 现在打开全屏「用户指南」——16 篇离线文档随包分发（快速开始、安装与更新、本地模型、工作流、扩展、技能、调度、会话管理、上下文控制、插件、代码库分析、权限与安全、隐私、常见问题、使用技巧、限流与配额），目录树导航；可收起为悬浮图标随时回到对话，再次点击恢复到原阅读位置；非中英文界面自动切换至中文或英文文档并给出提示
- **凭据保护升级**：应用保存的凭据（如云端模型 API Key）迁移到独立的系统密钥链命名空间，与同机安装的官方原版应用互不共享；升级时旧凭据自动迁移、无需任何操作；系统密钥链不可用时不再静默降级为明文文件，而是明确报错（安全行为收紧）

### 修复
- **扩展（MCP）配置校验更严格**：添加扩展时，若 stdio 类型的命令字段中填入了网址，或 Streamable HTTP 类型的端点不是合法的 http(s) 地址，系统会立即在表单内提示并阻止保存（此前会先保存成功，连接时才暴露失败）

### Added
- **Built-in help center (user guide)**: the ChengYoung logo at the top-right of the chat page now opens a full-screen "User Guide" — 16 offline docs bundled with the app (quick start, install & update, local models, workflows, extensions, skills, scheduler, session management, context control, plugins, codebase analysis, permissions & safety, privacy, FAQ, tips, rate limits & quotas) with a table-of-contents tree; minimize it to a floating badge to jump back to the conversation and restore your reading position later; non-Chinese/English locales automatically fall back to the Chinese or English docs with a hint
- **Stronger credential protection**: credentials saved by the app (such as cloud model API keys) now live in a dedicated system keychain namespace, never shared with the official upstream app installed on the same machine; legacy credentials migrate automatically on upgrade with zero action needed; when the system keychain is unavailable the app now fails with a clear error instead of silently falling back to a plaintext file (security hardening)

### Fixed
- **Stricter extension (MCP) config validation**: when adding an extension, if a URL is entered in the command field of a stdio extension, or the endpoint of a Streamable HTTP extension is not a valid http(s) URL, the app flags it inline and blocks saving (previously it saved first and only failed when connecting)

## [1.16.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.16.0) — 2026-10-10

### 新增
- **从本地目录安装插件**：除 git 仓库外，现在可以直接选择电脑上的插件文件夹进行安装（适合企业内网与离线分发场景）。本地目录安装同样走「下载 → 启用前审阅」两步确认；「自动更新」仍仅对 git 来源开放（本地目录没有可拉取的远端）

### 修复
- **插件披露更完整**：插件提供的命令（commands）现在也会列在安装确认对话框与插件详情中（此前只列技能、MCP 服务器与钩子）；卸载提示同步更新

### Added
- **Install plugins from a local directory**: alongside git repositories, you can now point the installer at a folder on your computer (useful for offline and intranet distribution). Local installs go through the same two-step "download → review before enabling" flow; per-plugin auto-update remains git-only, since a local directory has nothing to pull from

### Fixed
- **More complete plugin disclosure**: a plugin's commands are now listed in the install review dialog and the plugin details (previously only skills, MCP servers, and hooks were shown); the uninstall prompt is updated accordingly

## [1.15.0](https://github.com/yunfeng-tian/chengyoung/releases/tag/v1.15.0) — 2026-10-10

### 新增
- **插件系统（首批）**：新增「设置 → 插件」，可以从 git 仓库安装插件，为 ChengYoung 扩展技能、命令与生命周期钩子。安装分两步：先下载，**启用前先审阅**——清单会列出插件提供的组件，钩子明确标注「会在本机执行命令」；安装后默认停用、由你逐个启用；「自动更新」默认关闭、需逐插件手动开启；卸载会同时清理配置与文件、不留残留。隐私说明同步更新：插件只在你安装或更新时从其来源下载，此外无任何联网行为
- **本地模型自动调用显卡**：启动本地模型时按显存大小自动决定放入显卡的层数——放得下就整体进显卡（更快），放不下就自动把多出的层留在内存，避免「显存不够还硬塞」导致的加载失败
- **双显卡笔记本的本地推理更稳**：核显 + 独显并存的机器上，此前本地模型可能因显存被分配到核显而启动失败；现在启动时固定使用独立显卡
- **更新失败可见**：应用内检查或下载更新失败时，失败原因会常驻显示在设置里（此前只弹一次提示，错过就无处可看）；托盘菜单新增「检查更新」入口

### 修复
- **移除已失效的「分享到 Nostr」入口**：该能力已退役，旧入口点了也不会成功；从 Nostr 链接导入会话不受影响

### Added
- **Plugin system (first batch)**: a new Settings → Plugins page lets you install plugins from git repositories to extend ChengYoung with skills, commands, and lifecycle hooks. Installation is two-step: download first, then **review before enabling** — the dialog lists the plugin's components and clearly marks hooks that run commands on this machine. Plugins are disabled by default and enabled individually; per-plugin auto-update is off by default and must be turned on manually; uninstalling removes both config entries and files with nothing left behind. The privacy notice is updated accordingly: plugins are downloaded from their source only when you install or update them, with no other network activity
- **Local models use your GPU automatically**: when starting a local model, the number of layers offloaded to the GPU is now chosen from your VRAM size — the model goes entirely to the GPU when it fits (faster), otherwise extra layers stay in memory, avoiding failures from over-offloading
- **More reliable local inference on dual-GPU laptops**: on machines with both an integrated and a discrete GPU, local models could previously fail to start when memory was allocated to the integrated GPU; startup now pins the discrete GPU
- **Update failures are visible**: when an in-app update check or download fails, the reason now stays visible in Settings (it previously flashed once and was gone); the tray menu gains a "Check for updates" entry

### Fixed
- **Removed the defunct "Share to Nostr" entry**: the capability was retired and the old entry never succeeded; importing sessions from Nostr links is unaffected

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

