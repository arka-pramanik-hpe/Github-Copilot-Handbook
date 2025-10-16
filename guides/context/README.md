# Context superpowers: `#` references & `@` mentions

Copilot delivers precise, grounded answers when you feed it the right context. This guide breaks down every `#` reference and `@` mention available inside GitHub Copilot Chat so you can orchestrate the perfect response.

### Quick navigation

- [1. The Add context palette](#1-the-add-context-palette)
- [2. Blueprint: understanding a repository fast](#2-blueprint-understanding-a-repository-fast)
- [3. Inspecting external context](#3-inspecting-external-context)
- [4. Pairing context with agent modes](#4-pairing-context-with-agent-modes)

## 1. The Add context palette

Click **Add context** in Copilot Chat (or run **Copilot: Add Chat Context** from the command palette) to open the palette, then stack multiple references before sending your prompt. After Copilot responds, open the **Used n references** dropdown in the chat reply to confirm exactly which files and artifacts the assistant relied on.

<img src="../../images/add-context-panel.png" alt="Add context palette" width="1080" />

### `#` palette: repositories, tools, and automations

The `#` picker now bundles file references **and** Copilot agent tooling. Stack multiple entries to give Copilot precise scope plus the ability to run helper commands.

#### Core workspace references

| Command | What it provides | Ideal use |
| --- | --- | --- |
| `#repository` | Live view of the repository tree. | Architecture summaries, onboarding. |
| `#codebase` | Snapshot of the most relevant project files (Copilot-curated). | Rapid orientation when `#repository` feels noisy. |
| `#file` / `#folder` | Contents of a specific file or directory. | Targeted reviews, localized refactors. |
| `#branch` | Diffs between the current branch and base. | Pre-merge checks, planning rebases. |
| `#pull-request` / `#activePullRequest` | PR description, comments, and diff. | Review workflows, changelog drafting. |
| `#issue` | Issue title, body, and acceptance criteria. | Building features from backlog items. |
| `#url` | Fetches content from a public webpage or doc. | Vendor API integrations, spec-driven changes. |
| `#changes` | The working tree diff (git status). | Contextualizing uncommitted work for progress updates. |
| `#todos` | Copilot TODO list state from Edit mode. | Reporting progress or reprioritizing sub-tasks. |

> **Official note:** GitHub Copilot now auto-infers nearby symbols and variables in addition to whatever you pin via `#` references. Use explicit `#file` or `#codebase` entries when you need to force a less obvious file into scope.

#### File & content utilities

| Command | What it does | Best for |
| --- | --- | --- |
| `#readFile` | Injects the contents of a file without switching editors. | Quick reviews when the file isn’t open. |
| `#readNotebookCellOutput` | Shares the output from a specific notebook cell. | Debugging data science experiments. |
| `#search` / `#textSearch` | Performs a regex or plain-text search across the repo. | Locating usages or TODOs. |
| `#searchResults` | Replays the last search results from VS Code. | Following up on a search you just ran. |
| `#listDirectory` | Displays directory contents. | Auditing generated files or build artifacts. |
| `#usages` | Shows references for a symbol. | Refactor safety checks. |

#### Inline chat variables (editor shortcuts)

These keywords expand in the prompt box without opening the context palette—perfect for quick inline follow-ups.

| Variable | What it injects | Great for |
| --- | --- | --- |
| `#block` | The current code block or logical region. | Asking "what does this block do?" without selecting text. |
| `#class` | The surrounding class definition. | Reviewing class responsibilities or refactoring hooks. |
| `#comment` | The active comment node. | Rewriting docstrings or TODO notes. |
| `#file` | The entire active file. | Fast alternative to adding the file via the context palette. |
| `#function` | The enclosing function or method. | Requesting optimizations or tests for a single routine. |
| `#line` | The line where your cursor sits. | Quick explanations of compiler warnings or logs. |
| `#path` | The file path relative to the workspace. | Clarifying where changes should land. |
| `#project` | Project-wide metadata surfaced by Copilot. | High-level questions about build targets or structure. |
| `#selection` | The currently highlighted text. | When you've already made a manual selection. |
| `#sym` | The symbol under your cursor. | Inspecting usages or API docs for the current symbol. |

> **Official note:** Type `#` inside the chat box to see the live list for your IDE. Availability varies by environment, so rely on the picker to confirm what's supported.

#### Notebook & data workflows

| Command | What it does | Prerequisites |
| --- | --- | --- |
| `#configureNotebook` | Helps set up the kernel/kernel env for a notebook. | Jupyter notebook open in VS Code. |
| `#newJupyterNotebook` | Generates a fresh notebook scaffold. | Wanting a templated analysis start. |
| `#runCell` | Executes the highlighted notebook cell and shares output. | Selected cell in notebook editor. |
| `#runNotebooks` | Runs an entire notebook or suite of notebooks. | Batch-running data pipelines. |
| `#installNotebookPackages` | Installs Python packages into the notebook kernel. | Need extra libs mid-analysis. |
| `#listNotebookPackages` | Lists installed packages in the notebook environment. | Dependency audits. |
| `#dbclient-executeQuery` / `#dbclient-getDatabases` / `#dbclient-getTables` | Surfaces database metadata and query results via the Database Client extension. | SQL exploration and schema understanding. |

#### Python environment helpers

| Command | Role |
| --- | --- |
| `#configurePythonEnvironment` | Detects virtualenv/conda and configures VS Code to use it. |
| `#getPythonEnvironmentInfo` | Shares interpreter details and installed package versions. |
| `#getPythonExecutableCommand` | Provides the exact CLI (e.g., `poetry run python`) to execute scripts. |
| `#installPythonPackage` | Installs a package into the active Python environment. |

#### Task, terminal, and automation controls

| Command | What happens |
| --- | --- |
| `#runCommands` | Executes arbitrary shell commands through VS Code tasks. |
| `#runInTerminal` | Replays the last integrated-terminal command and output. |
| `#terminalLastCommand` | Shares the most recent terminal command text. |
| `#terminalSelection` | Captures currently selected terminal output. |
| `#runTask` / `#runTasks` | Starts a named VS Code task (single) or enumerates available tasks. |
| `#createAndRunTask` | Builds a temporary VS Code task definition and executes it. |
| `#getTaskOutput` | Fetches output from a specific VS Code task. |
| `#getTerminalOutput` | Streams the latest terminal buffer. |
| `#runVscodeCommand` | Invokes any VS Code command by ID. |
| `#openSimpleBrowser` | Opens a URL in VS Code’s simple browser and shares the rendered content. |

#### Project setup & scaffolding

| Command | Purpose |
| --- | --- |
| `#newWorkspace` | Spins up a new GitHub Copilot-compatible workspace scaffold. |
| `#getProjectSetupInfo` | Suggests tasks.json/launch.json and package scripts for the project type. |
| `#installExtension` | Installs a VS Code extension (marketplace). |
| `#extensions` | Lists currently installed extensions. |
| `#createDirectory` / `#createFile` | Creates folders or files directly from chat. |
| `#newJupyterNotebook` | (Also in notebook section) quickly add notebook files. |

#### GitHub & review helpers

| Command | Description |
| --- | --- |
| `#githubRepo` | Fetches another GitHub repository’s files for reference. |
| `#openPullRequest` | Opens a browser window to a PR and returns summary details. |
| `#copilotCodingAgent` | Hands the rest of the task to the async Copilot Coding Agent, creating a PR. |

#### Editing & diagnostics

| Command | Description |
| --- | --- |
| `#edit` / `#editFiles` | Launch Copilot Edit mode on the current or specified files. |
| `#editNotebook` | Opens notebook-aware editing capabilities. |
| `#problems` | Shares VS Code problem panel entries (lint/errors). |
| `#testFailure` | Surfaces the last unit-test failure details. |
| `#vscodeAPI` | Opens documentation excerpts for VS Code APIs (extension authors). |
| `#fetch` | Retrieves web resources (JSON/text) for follow-up parsing. |

> 🔁 Mix and match: `Explain #changes and craft fixes using #runTask npm:test plus #terminalLastCommand to double-check logs.`

### `@` mentions (runtime signals)

<img src="../../images/at-mentions.png" alt="@ mention palette" width="1080" />

| Mention | What it attaches | Requirements | Sample follow-up |
| --- | --- | --- | --- |
| `@terminal` | Most recent terminal command and output. | Run a command in the integrated terminal first. | `@terminal /explain why the npm build failed.` |
| `@workspace` | Names and excerpts of open editors plus unsaved changes. | Keep relevant files open in VS Code. | `@workspace /explain the relationship between these tabs.` |
| `@vscode` | VS Code context (settings, extensions, active branch). | Signed into VS Code with Copilot installed. | `@vscode /search for the eslint setting that ignores dist.` |
| `@codespaces` | Runtime info from the active Codespace: devcontainer JSON, forwarded ports, tasks. | Using GitHub Codespaces. | `@codespaces how is Docker configured for this repo?` |
| `@remote-ssh` | SSH target details and logs from Remote SSH sessions. | Connected via VS Code Remote SSH. | `@remote-ssh list the services running on the remote.` |
| `@github` | Repository metadata, issues, and pull requests linked to the current branch. | GitHub authentication with Copilot. | `@github summarize open issues labeled bug.` |
| `@githubpr` | Active pull request conversation, diffs, and review state. | Branch associated with a PR. | `@githubpr draft reviewer comments focusing on the API changes.` |
| `@dbclient` | SQL database connection definitions plus last query results from the Database Client extension. | VS Code Database Client (or Azure Databases) extension connected. | `@dbclient suggest indexes for the slowest queries.` |
| `@selection` | Currently highlighted code snippet. | Select text before opening chat. | `@selection rewrite this block to be asynchronous.` |
| `@notebook` | Current notebook kernel, variables, and selected cells. | Open Jupyter/interactive notebook. | `@notebook explain the dataframe transformations in cell 12.` |

**Tip:** Mentions auto-refresh—rerun the same `@terminal` after executing a command again to pass the new output.

> ℹ️ The [official Copilot docs](https://docs.github.com/en/copilot/how-tos/chat-with-copilot/chat-in-ide#using-keywords-in-your-prompt) note that chat participants can be auto-inferred (public preview). Still, typing `@` lets you pick the exact participant—including any Copilot Extensions installed from the GitHub or VS Code marketplaces.

**GitHub skills via `@github`**

- Mention `@github` to tap GitHub-specific skills (repo search, web lookups, dependency insights).
- Ask `@github What skills are available?` to see the current list, or combine with chat variables: `@github #web What is the latest Node.js LTS?`

#### Slash actions layered on mentions

Many mentions expose quick actions. Type the mention, press space, then choose an action from the palette.

| Mention + action | What it does |
| --- | --- |
| `@terminal /explain` | Summarises the attached terminal output and proposes fixes. |
| `@terminal /fix` | Generates a script or command sequence likely to resolve the error shown. |
| `@vscode /search` | Runs a full-text search across the workspace and reports results in chat. |
| `@vscode /startDebugging` | Inspects debug configurations and suggests how to launch the right one. |
| `@workspace /explain` | Walks through each open file, pointing out key code paths and TODOs. |
| `@workspace /fix` | Suggests edits across the open files to resolve the described issue. |
| `@workspace /new` | Drafts a brand-new file consistent with project conventions. |
| `@workspace /newNotebook` | Creates a notebook scaffold with recommended cells. |
| `@workspace /setupTests` | Generates boilerplate configs and scripts to enable testing frameworks. |
| `@workspace /tests` | Finds relevant tests and recommends how to run or extend them. |
| `/list` | Shows all available mentions and slash actions—perfect refresher mid-session. |
| `/clear` | Resets the current chat context without closing the tab. |
| `/save` | Pins the current conversation so you can revisit the transcript later. |

> 🧠 Combine slash actions: start with `/list` to rediscover options, then pivot to `@workspace /fix` or `@terminal /explain` to execute the next step instantly.

> 📚 Need the canonical list? Type `/` in chat or consult the [GitHub Copilot Chat cheat sheet](https://docs.github.com/en/copilot/how-tos/chat-with-copilot/chat-in-ide#using-keywords-in-your-prompt) for the latest slash commands and participants.

## 2. Blueprint: understanding a repository fast

1. Start with `Give me a high-level overview of this repo. Call out major packages and test suites.`
2. Add `# repository` so Copilot can see the structure.
3. Follow up: `Zoom in on #src/services. Summarize responsibilities and surface any TODO comments.`
4. Keep drilling: `List key exports inside #src/services/payment and highlight risky dependencies.`

> **Need ready-made prompts?** Check the [Prompt engineering playbook](../prompts/README.md) for curated templates you can copy and adapt.

## 3. Inspecting external context

Use `# url` to ground Copilot in vendor docs:

```
Integrate Plaid Link with our React app.
Reference #url:https://plaid.com/docs/link/web/ 
Generate a checklist of steps and draft a reusable hook.
```

Or point at issues to turn specs into code:

```
Implement the error banner requested in #issue:123.
Suggest which files under #src/components need updates.
```

## 4. Pairing context with agent modes

- **Ask mode:** pile up context first, then brainstorm or spec out changes.
- **Edit mode:** keep context focused (1–2 files) so Copilot can generate clean diffs.
- **Agent mode:** bundle every artifact the agent will need—requirements (`#issue`), code (`#folder`), validation commands (`@terminal`)—before you hand off the longer task.

Learn more about agent modes in [Advanced workflows](../advanced/README.md).

