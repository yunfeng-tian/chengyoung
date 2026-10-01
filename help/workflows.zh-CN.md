# 工作流

工作流是一份保存下来的 YAML 文件，用来描述一件任务：模型收到的指令、这次会话可以使用的扩展，以及运行如何开始。当某件事会反复做、且你希望每次结果形状一致时，就适合写成工作流。

## 工作流保存在哪里

| 位置 | 生效范围 |
| --- | --- |
| 配置目录下的 `recipes/` | 本机所有会话 |
| 工作目录下的 `.goose/recipes/` | 在该目录中运行的会话 |
| 工作目录或用户主目录下的 `.agents/recipes/` | 面向智能体的通用约定 |

在界面里新建的工作流会写入配置目录，因此每个会话都能看到它。

## 常用字段

```yaml
version: 1.0.0
title: 发布前检查
description: 发布前对仓库做一次复核
instructions: 你是一名严谨的发布复核者，请用简短清单回报发现的问题。
prompt: 复核这个仓库，列出你发现的风险。
extensions:
  - developer
settings:
  temperature: 0.2
  max_turns: 20
parameters:
  - key: target
    input_type: string
    requirement: required
    description: 要核验的版本号
```

- `instructions` 是本次运行的长期指令；`prompt` 是第一条消息。
- `extensions` 限定这次会话可用的扩展，工作流因此不会触碰到它不需要的工具。
- `settings` 可固定提供方、模型、`temperature` 与 `max_turns`。
- `parameters` 把工作流变成一张小表单：运行时再填具体取值。
- `response` 可要求固定的 JSON 结构，`sub_recipes` 可调用另一份工作流文件。

## 创建、运行与复用

1. 在输入框或工作流页面新建工作流。
2. 先描述目标，需要精确字段名时直接编辑 YAML。
3. 在工作流列表里运行它，或带着它开启一段会话继续打磨。
4. 把这次运行保存为会话，之后可在会话历史里找回。

工作流还可以用链接携带（`goose://recipe?config=...`，整份定义在链接片段里），适合在团队内分享。

## 工作流、技能与应用

| 概念 | 说明 |
| --- | --- |
| 工作流 | 保存下来的任务定义，由你主动运行 |
| 技能 | 主题匹配时 ChengYoung 自动带入会话的可复用指引 |
| 应用 | 为特定任务生成的小界面 |
| 调度 | 由定时器而非手工触发的工作流 |
