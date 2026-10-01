# Extensions

Extensions are Model Context Protocol (MCP) servers. ChengYoung starts them as a local process or connects to a remote endpoint, then hands the tools, prompts and resources they publish to the model.

## What an extension adds

| Component | What the model gets |
| --- | --- |
| Tools | Callable actions, such as running a command, querying an API or editing a file |
| Prompts | Ready-made instructions you can reuse in a chat |
| Resources | Files or records the model can read on request |

Extensions are not skills and not apps: a skill is a reusable workflow written for ChengYoung, and an app is a small user interface generated for a task. Extensions are how the session reaches software that already exists.

## Add an extension

1. Open **Settings → Extensions**.
2. Select **Add custom extension**.
3. Choose the transport:
   - `stdio` — ChengYoung runs a command on this computer, for example `npx -y @modelcontextprotocol/server-filesystem /path`.
   - `streamable_http` — ChengYoung connects to an endpoint you host or trust.
   - `builtin` — an extension that ships with the application.
4. Fill in environment variables, headers or a timeout when the server needs them.
5. Save, then enable the extension in the list.

A `stdio` server runs locally, so the first start may need network access to fetch its package; later starts use the local cache.

## Manage the list

- Turn an extension off in the list to keep it out of new sessions; its configuration stays.
- Open the same entry to change the command, the endpoint or the environment variables.
- Delete an extension you no longer use.

## Where servers come from

Server implementations are published by their own authors: community projects, vendors and internal teams. Copy the command or the endpoint from the documentation of a server you trust, and check its source and license before enabling it — an extension can read files, run commands and call APIs on your behalf.

## Permissions

ChengYoung asks for approval when a tool call needs to do something sensitive. Read the approval dialog the first time you use an extension, and keep only the extensions you are actively using enabled.
