# Advanced workflows, debugging & automation

Push Copilot beyond Q&A with agent modes, structured checklists, and debugging rituals that keep you unblocked.

### Quick navigation

- [1. Copilot modes at a glance](#1-copilot-modes-at-a-glance)
- [2. Runbooks for common scenarios](#2-runbooks-for-common-scenarios)
- [3. Prompt patterns to keep handy](#3-prompt-patterns-to-keep-handy)
- [4. Automate your sessions](#4-automate-your-sessions)
- [5. Debugging power moves](#5-debugging-power-moves)
- [6. Mix & match with other guides](#6-mix--match-with-other-guides)

## 1. Copilot modes at a glance

<img src="../../images/agent-modes.png" alt="Ask, Edit, and Agent modes" width="1080" />

| Mode | Mindset | How to launch | Best for | Guardrails |
| --- | --- | --- | --- | --- |
| **Ask** | Conversational partner | Default chat composer | Clarifying requirements, documenting systems, reviewing diffs | Keep context rich with `#` & `@` references so answers stay grounded. |
| **Edit** | Task executor | Press `Esc` in chat or use the composer dropdown | Applying diffs, guided refactors, running TODO checklists | Scope prompts to specific files/folders; review every diff chunk. |
| **Agent** | Hands-off automation | Composer dropdown → **Agent** (beta) | Multi-step jobs, background work, generating PR-ready changes | Provide explicit acceptance criteria and read the summary before merging. |

> **Official note:** Inside VS Code and JetBrains, the chat mode picker falls under the Copilot **Edits** umbrella. Ask is the default dialog, while Edit and Agent tap Copilot Edits to orchestrate multi-file changes straight from chat.

### Ask mode — explore and align

- Treat Ask as your planning room: confirm the problem, discuss alternatives, draft docs, or analyze logs.
- Add the right context first (`#repository`, `#folder`, `@terminal`) so Copilot cites concrete code.
- Chain follow-up questions instead of repeating context—Copilot remembers within the conversation thread.
- Typical outcomes: implementation checklists, design diagrams, trade-off tables, release notes.

### Edit mode — apply targeted changes

- Flip from Ask with `Esc` (or the chat dropdown) once you have a plan.
- Prompts work best when they reference a concrete path: `Update #src/lib/formatter.ts to support ISO week numbers`.
- Copilot surfaces diffs you approve chunk by chunk; reject or tweak anything that feels off.
- Use the built-in TODO tracker to break large efforts into safe steps you can check off as you accept patches.
- Official flow (VS Code / JetBrains): open chat → pick **Edit**, curate the working set (`Add all open files` or search), submit your prompt, then **Apply** or **Discard** per file. Copilot logs a short description alongside each change so you can skim before accepting, and every prompt stays inside your premium allowance for the selected model multiplier.
- Best for surgical updates where you want to control file scope and limit LLM requests.

### Agent mode — delegate longer runs

- Agent mode lets Copilot drive a longer sequence: it can edit multiple files, run commands, and report back.
- Supply:
	- **Goal:** what “done” looks like.
	- **Constraints:** language, frameworks, files to avoid.
	- **Validation:** commands to run (`npm test`, `pytest`, etc.).
- Copilot then executes the plan, summarises the work, and provides a diff preview or optional PR through the Copilot Coding Agent.
- Stay in the loop: you can pause, ask for status, or stop the agent if it drifts.
- Ideal when: the task is larger than a single diff, you need background execution, or you want Copilot to shepherd a change while you multitask.
- Official flow: open chat → choose **Agent** → submit the end goal. Copilot streams edits and highlights any terminal commands it wants to run—approve or decline each run. Only the prompts you type count as premium requests (multiplied by the model’s factor); follow-up tool calls are on Copilot.
- Works across VS Code, JetBrains, Xcode, and Eclipse (preview) for complex, multi-step jobs requiring external tools or MCP integrations.

#### Agent mode vs. Copilot coding agent

- **Where it runs:** Agent mode edits your local workspace. Copilot coding agent executes in a GitHub Actions environment, opening branches like `copilot/feature-xyz` and submitting pull requests on your behalf.
- **When to choose coding agent:** Use it when you want asynchronous progress on GitHub—hand off an issue, let Copilot iterate in commits, then review the PR. It currently runs on Claude Sonnet 4.5 and is available to Copilot Pro, Pro+, Business, and Enterprise plans (subject to org policies).
- **Usage accounting:** Coding agent sessions consume GitHub Actions minutes and Copilot premium requests, while in-IDE agent mode only bills the prompts you send. Branch protections, reviewers, and workflow approvals still apply before merges.
- **Safety rails:** Coding agent cannot push outside its `copilot/` branches, respects repository access policies, and requires a maintainer to approve workflows before they run—mirroring the guardrails documented in [About GitHub Copilot coding agent](https://docs.github.com/en/copilot/concepts/agents/coding-agent/about-coding-agent).

## 2. Runbooks for common scenarios

### 📦 Ship a feature

1. **Ask:** `Draft an implementation plan for the checkout totals update. Identify impacted files.`
2. **Add context:** `#src/cart`, `#src/pricing`.
3. **Switch to Edit:** `Apply the plan now. Update the cart reducer and totals display.`
4. **Verify:** `@terminal` after running tests → `Summarize failures and suggest fixes.`

### 🐞 Crush a bug fast

1. Capture logs with `@terminal` (rerun failing command first).
2. Ask: `Explain why the tests failed. Point to the suspect code.`
3. Pin the suspect file with `# file` and switch to Edit.
4. Prompt: `Patch #src/utils/normalizer.ts so it handles null inputs.`
5. Accept the diff, rerun tests, repeat.

### 🔍 Deep code reviews

1. `# pull-request` to fetch the diff.
2. Ask: `Highlight risky changes and outdated TODOs.`
3. Follow up: `Generate inline review comments for the serializer changes.`
4. Optional: Switch to Edit and let Copilot draft suggested patches directly in your branch.

## 3. Prompt patterns to keep handy

The full prompt library now lives in the [Prompt engineering playbook](../prompts/README.md). Grab ready-made templates for design walkthroughs, regression triage, documentation, and more—all with recommended context references.

> **Quick picks:** When you need a refresher mid-session, jump to the “Quick-start templates” and “Specialized reviews & automations” sections in the playbook. They mirror the examples previously listed here and include new developer workflows like security sweeps, DevOps fixes, and incident follow-ups.

## 4. Automate your sessions

- **Inline TODOs:** When Copilot suggests a plan, click **Track with TODOs**. The checklist stays pinned in chat so you can mark progress.
- **Slash commands:** Type `/tests`, `/plan`, or `/fix` inside chat to trigger curated flows preloaded with best-practice prompts.
- **Saved prompts:** Pin your favorite prompts as code snippets in VS Code or reuse them in Copilot Chat history.
- **Install curated modes:** Explore [Awesome GitHub Copilot Customizations](https://github.com/github/awesome-copilot) to import community-built prompts, instructions, and chat modes directly into your IDE, then combine them with the strategies above.

## 5. Debugging power moves

| Technique | How to do it |
| --- | --- |
| Capture environment | `@codespaces` shares ports, tasks, environment variables. |
| View file diffs | `Compare the current file with main and point out risky hunks.` (Copilot auto-fetches diff.) |
| Sanity-check suggestions | Ask `Explain why this change is safe. Provide potential regressions.` before accepting large diffs. |
| Generate telemetry hooks | `Add logging around #src/server/payment.ts to surface Stripe errors.` |

## 6. Mix & match with other guides

- Refresh on [Context superpowers](../context/README.md) to keep prompts grounded.
- Switch models mid-stream with guidance from [Choosing the right Copilot model](../models/README.md).
- Layer in automated guardrails from [Code reviews in VS Code](../reviews/README.md) so Copilot flags risky diffs while you build.
- Share this playbook with teammates so everyone speaks the same Copilot language.
