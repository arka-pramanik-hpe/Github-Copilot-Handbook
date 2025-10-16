# Copilot code reviews in VS Code

Turn GitHub Copilot into your first-pass reviewer without leaving Visual Studio Code. Use it to triage risky changes, surface suggested fixes, and keep your team's standards in view.

### Quick navigation

- [1. Know what you need](#1-know-what-you-need)
- [2. Run a quick selection review](#2-run-a-quick-selection-review)
- [3. Review all uncommitted changes](#3-review-all-uncommitted-changes)
- [4. Work through Copilot's feedback](#4-work-through-copilots-feedback)
- [5. Tune reviews with custom instructions](#5-tune-reviews-with-custom-instructions)
- [6. Mind the quotas and limits](#6-mind-the-quotas-and-limits)
- [7. Keep humans in the loop](#7-keep-humans-in-the-loop)

## 1. Know what you need

| Requirement | Why it matters |
| --- | --- |
| GitHub Copilot extension + sign-in | Copilot code review runs inside the same chat extension you use for prompts. |
| Supported plan | Reviewing **all uncommitted changes** consumes one premium request and needs Copilot **Pro, Pro+, Business, or Enterprise**. Reviewing a **selected range** is available to every Copilot seat in VS Code, including Free users ([docs](https://docs.github.com/en/copilot/concepts/agents/code-review#availability)). |
| GitHub Pull Requests & Issues extension (recommended) | Gives you the PR view and the dedicated **Copilot Code Review** button in Source Control. |
| Up-to-date workspace | Copilot reads the file contents currently open in VS Code, so fetch and rebase before you ask it for a review. |

> **Premium request accounting:** Copilot deducts one premium request each time it reviews your uncommitted changes. Selection reviews are free ([docs](https://docs.github.com/en/copilot/concepts/agents/code-review#code-review-monthly-quota)).

### Copilot's two review modes in VS Code

| Mode | Trigger | Cost | Custom instructions |
| --- | --- | --- | --- |
| **Selection review** | Highlight code → run **GitHub Copilot: Review and Comment** | Included for all plans | Repository-wide instructions only ([docs](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review?tool=vscode#customizing-copilots-reviews-with-custom-instructions-1)) |
| **Uncommitted changes** | Source Control view → Copilot Code Review button | Uses 1 premium request | Currently ignores custom instructions |

## 2. Run a quick selection review

Use this when you want a fast gut check on the block of code you just edited.

1. Highlight the lines you want Copilot to inspect.
2. Open the Command Palette (`⇧⌘P` on macOS, `Ctrl+Shift+P` on Windows/Linux).
3. Run **GitHub Copilot: Review and Comment** ([docs](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review?tool=vscode#reviewing-a-selection-of-code)).
4. Wait a few seconds; Copilot posts inline comments and populates the **Problems** panel with findings.

Selection reviews never hit your premium quota, so fire them off liberally as you iterate.

## 3. Review all uncommitted changes

Ready for a more thorough pass before you commit or open a PR? Ask Copilot to scan every staged and unstaged change.

1. Open the **Source Control** view in VS Code.
2. Hover over the **CHANGES** header and click the **Copilot Code Review – Uncommitted Changes** button ([docs](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review?tool=vscode#reviewing-all-uncommitted-changes)).
3. Copilot analyzes the entire diff. Reviews typically finish in under 30 seconds.
4. Results appear inline and in the **Problems** panel, grouped by file. Expand each entry to jump straight to the comment thread.

> **Heads-up:** This workflow spends one premium request from your monthly pool. Save it for meaningful checkpoints (pre-commit, before pushing, or just prior to requesting a human review).

## 4. Work through Copilot's feedback

- **Inline threads:** Each comment shows up beside the relevant code. Use the **Apply and Go To Next** button to accept a suggested patch or **Discard and Go To Next** to skip it ([docs](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review?tool=vscode#working-with-suggested-changes-provided-by-copilot-1)).
- **Problems panel:** Copilot mirrors its findings here so you can triage them like build or lint warnings.
- **Feedback loop:** Thumb comments up or down to teach Copilot what good feedback looks like for your repo ([docs](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review?tool=vscode#providing-feedback-on-copilots-reviews-1)).
- **Suggested edits aren't committed:** Every accepted change stays staged in your workspace; you still control git history.

## 5. Tune reviews with custom instructions

Keep Copilot aligned with team practices by committing repository instructions.

1. Add `.github/copilot-instructions.md` to outline review expectations (style guides, security checklists, languages to reply in).
2. Optionally add path-specific rule files under `.github/instructions/**/NAME.instructions.md` for framework- or folder-specific checks ([docs](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot)).
3. Today, VS Code honors only repository-wide instructions, and only when you run a **selection review** ([docs](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review?tool=vscode#customizing-copilots-reviews-with-custom-instructions-1)).
4. Re-run the review and watch Copilot cite the instructions you supplied.

Pair these instructions with the context layering tips in [Adding rich context with `#` and `@`](../context/README.md) so Copilot sees the right files and decisions.

## 6. Mind the quotas and limits

- **Premium request budget:** Each uncommitted review burns a premium request. Track usage in your billing portal and upgrade or add requests if you're bumping into the cap ([docs](https://docs.github.com/en/copilot/concepts/agents/code-review#code-review-monthly-quota)).
- **Model is fixed:** Copilot code review uses a dedicated model mix—you can't swap in GPT-5 or Claude directly ([docs](https://docs.github.com/en/copilot/concepts/agents/code-review#model-usage)).
- **Org policies apply:** If you're on a managed seat, admins can disable code review or limit where it runs. Check with them if the button is missing.

## 7. Keep humans in the loop

Copilot accelerates reviews but doesn't replace them.

- Treat Copilot as the first pass: let it flag obvious bugs, missing tests, or insecure patterns, then hand the branch to your teammate.
- Validate anything it suggests, especially migrations or logic changes. See [_Responsible use of GitHub Copilot code review_](https://docs.github.com/en/copilot/responsible-use-of-github-copilot-features/responsible-use-of-github-copilot-code-review) for guidance.
- Combine this guide with the workflow maps in [Advanced workflows, debugging & automation](../advanced/README.md) to run Copilot reviews alongside test plans and deployment checks.

When you're ready, roll Copilot's comments into your commits, rerun tests, and keep iterating—you'll have cleaner diffs and happier human reviewers.
