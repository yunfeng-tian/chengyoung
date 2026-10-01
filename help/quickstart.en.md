# ChengYoung in 5 minutes

ChengYoung is a local-first AI agent that runs on your own computer. It reads and writes the folder you open, runs commands you approve, and works with either a model installed on this machine or a provider you already pay for.

Launching costs nothing: there is no account to create and no sign-up screen to pass.

Five minutes is enough to cover the four things that make it useful:

- Choosing the model behind the conversation
- Pointing the agent at a folder
- Handing it a first task
- Adding an extension when you need reach beyond that folder

## 1. Choose a model

The first launch asks how you want to run conversations. There are two paths, and you can switch later.

**On this machine, without an account** — pick *Use a Local Model*. ChengYoung suggests downloads that fit your hardware, shows the file size before anything is transferred, and reports progress while it downloads. When the download finishes, the model is ready to use. Prompts, files and answers stay on the device.

**Through a provider** — pick *Connect to a Provider*, choose the vendor you already use, then paste the API key from that vendor's console. The key is stored in your local configuration on this machine.

To change the model later, use the model selector at the bottom of the chat window, or open **Settings → Models** and choose *Switch models*. Downloads, disk usage and a Hugging Face search box live in **Settings → Local Inference**, which is also where you free up space by removing a model you no longer need.

Local inference is worth it for offline work and private material, but it is bounded by the hardware: a model larger than your free memory will be slow or refuse to load, and long conversations fill the context window sooner than a hosted model would.

Already have model files on disk? They are picked up from the Hugging Face cache (`~/.cache/huggingface/hub`) and from this app's `models` data folder by default, and you can register further folders in `model_paths.yaml` inside the configuration directory (`%APPDATA%\Block\goose\config\` on Windows, `~/.config/goose/` on Linux and macOS). Everything found this way shows up in the same model list.

## 2. Point it at a folder

ChengYoung works inside a working directory. The home screen shows which one is active: the folder name sits in the top row, and hovering it reveals the full path.

- To open another folder, click the directory name at the bottom of the chat input and choose **Choose directory…**, or type a path directly.
- The same menu holds your recent folders, the Git worktrees of the current repository, and the directory in use right now.
- Everything the agent reads or writes is resolved against that working directory unless you explicitly say otherwise.

Nothing is uploaded: the folder stays exactly where it is, and a task only touches what it needs.

## 3. Hand over a first task

Type in the chat input, or start from one of the four cards on the home screen — skim the folder and outline its structure, summarise the documents in it, review the code for issues, or draft a commit message from the current changes. The cards only fill the input box, so you can rewrite the wording before sending.

Worth knowing on the first run:

- ChengYoung reads files and runs commands through tools, so the answer arrives as a chain of steps you can follow, not a single block of text.
- Sensitive operations raise a permission dialog first. Read it, then approve or deny. **Settings → Chat** holds the approval mode if you would rather be asked less often.
- Drop files onto the input to attach them to your next message.
- A `.goosehints` file in a project — or a global one for every project — states your conventions and build commands once instead of in every prompt.
- You can stop a running turn at any time; the conversation keeps its history either way.

## 4. Add an extension when you need more reach

Extensions are Model Context Protocol (MCP) servers: a local process or a remote endpoint that hands the model extra tools. They cover what the folder alone cannot — an issue tracker, a database, a browser.

1. Open **Extensions** in the left navigation.
2. Choose **Add custom extension**.
3. Describe how to reach the server: a `stdio` command, a `streamable_http` address, or a built-in that ships with the app.
4. Save it, then switch it on in the list.

An extension acts with your authority, so keep only the ones you are using enabled. That screen's own guide covers the details.

## Where to go next

| Want to | Open |
| --- | --- |
| Reuse a stored setup for a recurring task | Workflows |
| Teach ChengYoung your conventions | Skills, or a `.goosehints` file |
| Run something on a schedule | Scheduler |
| Reopen an earlier conversation | Session History |
| Change language, theme, updates or privacy options | Settings → App |

That is the whole loop: pick a model, choose a folder, ask for something, and extend it only when a task needs more than the folder.
