# Role Reference, Cross-Vendor Review, and Abstract Tools

## Role Reference Table

Prefer roles from left to right; move right only when quotas run out or a role is unsuitable (roles are not bound to specific model names or quota numbers).

| Role | Best Suited For | Limits and Notes |
| --- | --- | --- |
| Orchestrator | Breaking down goals, partitioning file scopes, writing specs, final acceptance, talking with the user | Does not run batches or poll continuously; delegates routine monitoring to the lower-tier watcher |
| Integrator | Integrating branches, merging into main, committing, running releases (the only role allowed to touch main and releases) | Avoid opening too many concurrently; does not write large features from scratch |
| Builder (primary / fallback) | Writing code, fixing bugs, organizing delivery checklists in an isolated worktree/branch | May only touch files listed in the spec; sandboxed builders without command permissions only edit files, leaving builds/tests/commits to a builder with command execution or the integrator |
| Reviewer (first-pass) | Reviewing diffs, checking against specs, verifying fixes | Reads only materials, never the author's conclusions; do not assign text summarization; enforce cross-vendor review |
| Second-pass reviewer (second-pass / critical third-pass) | Double-checking; reserve critical reviews for migration scripts, pre-release checks, and data-write logic | Must be from a different vendor than the first-pass reviewer; use only when re-review or high-risk changes require it |
| Cold reviewer | Pre-start spec cold review, broad file search | Do not provide conversation history or orchestrator opinions during cold review; verify found paths actually exist using a script |
| Test author | Generating acceptance test cases from the spec and original requirements | Does not read source code and does not share a conversation with the code author; if lacking file-write permissions, outputs test text only |
| Cheap model / local model / public free pool | Cheap model: broad file search, non-sensitive minor edits; local model: private long-file summaries, keeping data on-machine; public free pool: public material translation and minor formatting | Private code and logs are strictly forbidden in public free pools; pause background batches when high-priority interactive tasks use local resources |
| Long-context research model | Long-form technical research, parallel design trade-off queries, initial doc drafts | Prone to fabricating projects or URLs; verify every item in outputs and test cases individually |

## Cross-Vendor Review Principles

- **Author and reviewer from different vendors**: The model that writes the code and the model that reviews it must come from different model families (for example, code written by the primary builder model is reviewed by a reviewer model from another vendor; code written by a fallback model is reviewed by a third or different-vendor model).
- **Parallel design trade-off queries**: For major architecture or design trade-offs, query two different-vendor models in parallel with the same prompt and compare results (provide complete materials upfront; do not let them scan the entire project).

## Sandboxed Tool Packaging Rules

If a builder or reviewer model runs inside a restricted sandbox (where reading files outside the workspace or running commands fails the entire turn):

- Copy the files to be read into a temporary working directory before running.
- Start file-reading prompts with: "Do not execute any commands; use only file-reading and directory-listing tools."
- Start web-search prompts with: "Do not execute any commands and do not read local files; use only web tools."

## Abstract Tools and Zero-Dependency Fallbacks

- **Task board**: Holds todos, shared-file registrations, discovered facts, and open "proposals". When this tool is unavailable, maintain `TASKS.md` at the project root.
- **Lower-tier watcher (`watch-task`)**: Watches lower-tier work in the background and wakes the dispatcher only on completion, stall, or timeout. When this tool is unavailable, check progress or output-file changes periodically.
- **Heavy-job queue lock (`heavy-lock`)**: Protects rebuilds, full acceptance tests, releases, and large local model evaluations so only one runs machine-wide at a time while others queue. When this tool is unavailable, run only one heavy job at a time and start the next only after completion.
- **Review header assembler**: Prepends review attitude, rules, and reply format to review prompts. When this tool is unavailable, copy `templates/review-header.md` directly to the top of the prompt.
