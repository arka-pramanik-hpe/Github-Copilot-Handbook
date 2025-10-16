# Prompt engineering playbook

A shared library of high-signal prompts for GitHub Copilot Chat, inline chat, and agent workflows. Use it to jump-start reviews, refactors, and automations—and extend it with your own templates so the whole team benefits.

### Quick navigation

- [1. Orientation](#1-orientation)
- [2. Core principles](#2-core-principles)
- [3. Quick-start templates](#3-quick-start-templates)
- [4. Deep-dive recipes](#4-deep-dive-recipes)
- [5. Specialized reviews & automations](#5-specialized-reviews--automations)
- [6. Contribute new prompts](#6-contribute-new-prompts)
- [7. Related resources](#7-related-resources)

## 1. Orientation

- Pair this guide with [Context superpowers](../context/README.md) to layer the right `#` references and `@` mentions.
- Switch models on the fly with the [Model selection cheat sheet](../models/README.md) when you need more reasoning depth.
- Keep [Advanced workflows](../advanced/README.md) handy for orchestration tips (Edit mode, Agent mode, Copilot coding agent).

## 2. Core principles

| Principle | Why it matters | Try it |
| --- | --- | --- |
| Anchor prompts in source artifacts | Copilot is most accurate when it sees the real files, diffs, and logs. | Add `#file`, `#changes`, or `@terminal` before you ask for code or debugging help. |
| Declare intent and constraints | Be explicit about style, scope, tests, and forbidden changes. | `Update #src/router.ts to add OAuth routes. Keep legacy SAML untouched.` |
| Request outcomes, not steps | Focus on what “done” looks like; Copilot can suggest the how. | `Draft migration notes summarizing breaking changes in #folder:v2`. |
| Inspect the diff | Even solid prompts can drift—review every chunk before committing. | Use Edit mode or apply suggested changes manually. |
| Iterate | Chain concise follow-ups instead of rewriting the entire prompt each time. | `Great—now add edge-case tests to cover null IDs.` |

## 3. Quick-start templates

These prompts mirror the "quick wins" from the basics guide, with extra context notes for faster reuse.

| Goal | Template | Recommended context |
| --- | --- | --- |
| Summarize a component | `Summarize #src/components/Button.tsx in two bullet points. Include props and side effects.` | `#file` |
| Generate targeted tests | `Create Vitest cases that cover the new formatConfig function. Highlight Arrange/Act/Assert.` | `#selection` or inline chat |
| Explain a failure | `@terminal Explain why this npm build failed and list the files to inspect first.` | `@terminal` |
| Translate code | `Convert this class to Python 3.12 and explain API differences. Preserve async behavior.` | Inline selection |
| Plan a refactor | `Outline steps to migrate #folder:packages/ui to Tailwind. Call out risk areas and test strategy.` | `#folder` + Ask mode |
| Draft release notes | `Summarize #changes into release notes with highlights, fixes, and upgrade guidance.` | `#changes` |
| Suggest metrics | `Given #src/server/payments create observability metrics to monitor retries, failures, and latency.` | `#folder` |
| Review before commit | `Scan #changes for risky migrations, TODOs, or missing null checks. List actionable follow-ups.` | `#changes` |
| Bootstrap docs | `Draft README sections (intro, setup, usage, troubleshooting) for #folder:packages/worker.` | `#folder` |

### New developer-focused templates

| Scenario | Prompt | Add-ons |
| --- | --- | --- |
| Performance regression | `Inspect @terminal and #changes to hypothesize the performance regression in the checkout flow. Suggest profiling hooks.` | Add `#file` for hot paths |
| API contract drift | `Compare #file:src/api/v2/routes.ts with #file:docs/api.md and flag mismatches in payloads or status codes.` | Pin both files |
| Security sweep | `Run an auth/security sweep across #folder:services/auth. Identify weak cryptography, insecure defaults, and missing audit logs.` | Combine with `#search token` |
| Database migration review | `Review #file:db/migrations/202510160945_add_invoice_indexes.sql for downtime risks and back-out plan recommendations.` | Add `@terminal /explain` after running migration dry-run |
| DevOps fix | `Given @terminal output from the failed GitHub Actions job, propose fixes and indicate which workflow file needs edits.` | `#file:.github/workflows/deploy.yml` |
| Legacy cleanup | `Suggest a staged plan to replace the legacy XML parser in #folder:src/legacy. Include deprecation messaging.` | `#folder` + Ask mode |
| UI accessibility | `Evaluate #selection for accessibility. Note color contrast issues, missing ARIA, and keyboard traps. Provide code fixes.` | Inline selection |
| Data contract tests | `Generate contract tests for #folder:services/reports ensuring schemas match versioned expectations.` | Add schema files |
| Incident follow-up | `Digest @terminal crash logs and #file:postmortems/2025-10-15.md to propose action items for follow-up.` | `@terminal` + doc |

## 4. Deep-dive recipes

Adapted from the original context guide, these patterns help you interrogate entire codebases or external references.

### Repository reconnaissance

- `Based on the code in #repository, give me an overview of the architecture. Link to entry points and major dependencies.`
- `Analyze this repo and list application-level security mechanisms. Cite the files you reference.`
- `Explain the files in #folder:packages/api. Which ones bootstrap the service vs. configure routing?`
- `@github Which commits touched payment logic this week? Summarize intent and reviewers.`

### Visual walkthroughs

- `Describe this UI mockup image and outline the React components Copilot should scaffold.`
- `Explain this flowchart, map each step to existing services, and suggest telemetry hooks in #file:src/server/logs.ts.`
- `Compare the two attached screenshots and flag accessibility issues (contrast, labels, keyboard traps). Provide fixes.`

### Web and vendor integrations

- `@github #web Scan the OAuth 2.1 spec and summarize changes that affect #repository.`
- `Fetch #url:https://developer.paypal.com/api/orders/v2/ and show how to integrate capture requests into #file:src/payments/paypal.ts.`
- `Combine #issue:342 with #url:https://docs.aws.amazon.com/lambda/latest/dg/welcome.html to propose a deployment checklist tailored to this feature.`
- `Using @github Search, find discussions about "retry backoff" in our org and synthesize recommended defaults for #folder:infra/config.`

## 5. Specialized reviews & automations

Borrowed from the advanced playbook—use these when you need Copilot to reason across multiple systems.

| Goal | Prompt pattern | Notes |
| --- | --- | --- |
| Understand a design | `Summarize how state flows through #src/state. Include a Mermaid diagram.` | Follow with `Generate diagram code only` if needed. |
| Create resilient tests | `Generate Jest tests for #src/lib/math.ts covering success, failure, and null inputs.` | Ask for structured Arrange-Act-Assert steps. |
| Refactor carefully | `In Edit mode: Convert #src/hooks/useStore.ts to Zustand while preserving tests.` | Run tests (`npm test -- runInBand`) after applying diffs. |
| Debug regressions | `Given @terminal and #src/components/Header.tsx, hypothesize why login is broken and list fix options.` | Include reproduction steps in prompt. |
| Draft documentation | `Author migration notes for the move from #folder:v1 to #folder:v2. Highlight breaking changes and rollout plan.` | Ask for checklists or tables. |
| Threat model review | `Review #folder:services/payments for auth bypasses, injection risks, and missing rate limits. Prioritize fixes.` | Pair with `@terminal` logs if available. |
| PR review aid | `Given #pull-request focus on serialization changes. Suggest inline comments and highlight missing tests.` | Use inside PR review threads. |
| Automation | `Design a GitHub Actions workflow to lint and test monorepo packages touched in #changes.` | Ask Copilot to output YAML skeletons. |

## 6. Contribute new prompts

- Add a new Markdown file under `guides/prompts/` (for example, `guides/prompts/mobile.md`) or expand this playbook.
- Keep prompts short, actionable, and scoped to a single objective. Include recommended context so others can reproduce the results.
- Reference official docs or team conventions when possible. Use inline links for provenance.
- Submit a pull request describing why the prompt is useful and where it’s been tested.

## 7. Related resources

- [Awesome GitHub Copilot Customizations](https://github.com/github/awesome-copilot) – import community prompt packs, instructions, and chat modes.
- [Requests in GitHub Copilot](https://docs.github.com/en/copilot/concepts/billing/copilot-requests) – understand how premium request multipliers apply to prompt-heavy workflows.
- [Using GitHub Copilot code review](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review?tool=vscode) – pair these prompts with automated PR checks.
