# 安全说明 / Security

ChengYoung 是运行在你自己电脑上的桌面 AI 智能体（Agent）。与纯聊天的语言模型不同，Agent 能代表你在本机执行代码、读写文件、调用工具，因此具有**不同于传统软件的安全风险模型**（例如提示注入攻击）。请在使用前了解并采取以下预防措施。

## 安全提示 / Precautions

- **在隔离环境运行高风险任务**：建议使用专用虚拟机或沙箱容器（如 Docker Desktop / WSL2），限制权限，降低本机系统被攻击或关键资源被误访问的风险。
- **审查 Agent 生成的代码与测试**，确认无误后再执行。
- **不要向 Agent 提供敏感或机密信息**，避免信息泄露。
- **对会产生重大影响的操作要求人工确认**（应用内已内置权限确认与工作流变更二次确认）。
- **只使用你审查过的 MCP 扩展服务器**。
- 提示注入风险：Agent 可能被诱导执行外部内容中嵌入的**恶意**指令，即使这些指令与你的原始任务冲突。请通过上述措施控制风险。

## 漏洞报告 / Reporting a vulnerability

如果你发现 ChengYoung 的安全漏洞，请**私密**报告，**不要公开披露**：

- **邮件 / Email**：**support@chengyoung.com**（建议使用 TLS 加密传输）
- 请在邮件中描述：受影响版本、漏洞类型、复现步骤（可选：应用内「设置 → 关于与帮助」复制诊断信息，**发送前请确认不含敏感个人数据**）。

我们承诺在收到报告后 **72 小时内**进行初步响应与评估，并在修复后与报告者协商公开披露时机（协调披露 / Coordinated Disclosure）。

## 平台 / Platform

- Windows 10 / 11（x64）
- 相关说明见仓库 [README](README.md) 与 [隐私说明](PRIVACY.public.md)。

---

## English

This is a desktop AI Agent running on your own machine. Unlike chat-only models, an Agent can run code and take actions on your behalf, which introduces a **distinct security risk model** (e.g., prompt injection attacks). Please review the precautions below.

### Precautions

- **Run high-risk tasks in an isolated environment**: use a dedicated VM or a sandbox container (e.g., Docker Desktop / WSL2) with limited privileges, to reduce the risk of local attacks or unintended access to critical resources.
- **Always review Agent-generated code and tests** before running them.
- **Do not provide sensitive or confidential information** to the Agent, to avoid information leakage.
- **Require human confirmation for significant changes** (the app already asks before sensitive actions and workflow changes).
- **Only use MCP extension servers you have reviewed and trust.**
- Prompt injection: the Agent may be tricked into executing **malicious** instructions embedded in external content, even when they conflict with your intended task. Mitigate this risk using the precautions above.

### Reporting a vulnerability

If you find a security vulnerability in ChengYoung, please report it **privately** — do not disclose it publicly.

- **Email**: **support@chengyoung.com** (please use TLS-encrypted transport)
- Please include the affected version, vulnerability type, and reproduction steps (optionally, copy the diagnostic report from *Settings → About & help*; **please verify it contains no sensitive personal data before sending**).

We commit to an initial response and evaluation **within 72 hours** of receipt, and will coordinate public disclosure timing with the reporter (coordinated disclosure).

### Platform

- Windows 10 / 11 (x64)
- See the repository [README](README.md) and [Privacy Policy](PRIVACY.public.md) for details.

