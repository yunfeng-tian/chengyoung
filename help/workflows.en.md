# Workflows

A workflow is a saved YAML file that describes a task: the instructions the model receives, which extensions the session may use, and how the run starts. Reach for one when a task repeats and you want the same shape of answer every time.

## Where workflows are stored

| Location | Scope |
| --- | --- |
| `recipes/` inside the configuration directory | Every session on this computer |
| `.goose/recipes/` in the working directory | Sessions that run in that directory |
| `.agents/recipes/` in the working directory or in the home directory | Shared convention for agents |

A workflow added through the interface is written to the configuration directory, so it is available in every session.

## Fields you will use most

```yaml
version: 1.0.0
title: Release check
description: Review a repository before a release
instructions: You are a careful release reviewer. Report findings as a short list.
prompt: Review this repository and list the risks you find.
extensions:
  - developer
settings:
  temperature: 0.2
  max_turns: 20
parameters:
  - key: target
    input_type: string
    requirement: required
    description: Version being verified
```

- `instructions` is the standing guidance for the run; `prompt` is the first message.
- `extensions` limits which extensions the session may use, so a workflow cannot reach tools it does not need.
- `settings` pins the provider, the model, `temperature` and `max_turns`.
- `parameters` turn the workflow into a small form: you fill the values in when you run it.
- `response` can require a JSON shape, and `sub_recipes` can call another workflow file.

## Create, run, reuse

1. Create a workflow from the chat input or from the workflows page.
2. Describe the goal, then edit the YAML directly when you need exact field names.
3. Run it from the workflow list, or start a chat with it and keep iterating.
4. Save the run as a session so you can find it later in the session history.

A workflow can also be carried in a link whose fragment holds the whole definition (`goose://recipe?config=...`), which is convenient for sharing inside a team.

## Workflows, skills and apps

| Concept | What it is |
| --- | --- |
| Workflow | A saved task definition you run on purpose |
| Skill | Reusable guidance ChengYoung adds to a session when the topic matches |
| App | A small interface generated for a specific task |
| Schedule | A workflow started by a timer instead of by hand |
