---
name: multi-agent-project-en
description: Workflow specification for multi-agent collaborative project development. Use when cross-file edits, new feature development, bug fixes, architecture changes, data migration, or release processes require orchestrator breakdown, integrator coordination, and cross-vendor review.
---

# Multi-Agent Project Collaboration Workflow

This specification defines role boundaries, pre-start specification gates, supervision and handoff, cross-vendor review, and enforcement mechanisms when multiple AI agents collaborate on project development.

## Three-Tier Division of Labor

- **User (top tier)**: Provides goals, makes decisions, and performs final acceptance; consulted only when a final call or hands-on action (entering a password, closing an app, viewing the screen) is needed.
- **Orchestrator**: Breaks down goals, splits file scopes, and writes specifications before work starts; reads changes for final acceptance upon delivery; halts work if the direction is wrong.
- **Integrator**: Integrates branches, merges into the main branch, commits, and runs releases (the only tier allowed to touch the main branch and releases).
- **Builder / Reviewer**: Routed by work type to build in an isolated worktree/branch or perform independent reviews, without touching the main branch.

For each role's purpose, tool restrictions, zero-dependency fallbacks, and cross-vendor review principles, see `references/roles-and-tools.md`.

## Agreeing on Completion Criteria and Pre-Start Specification Gate

Classify the work before starting (when unsure, treat as Medium; when asked to "just do it", condense the spec into a scope list written by the orchestrator):

| Level | Criteria | Execution Steps |
| --- | --- | --- |
| Small | Single-file, clearly specified minor fixes (typos, minor UI bugs, test failures with clear causes) | Skip the spec gate and dispatch directly |
| Medium | Cross-file changes, adding or altering existing behavior, touching data writes/deletes/configs/quotas, or ambiguous requirements | Write a one-page spec -> 1 round of cold review -> ask the user only if open items remain -> start work |
| Large | New features, architecture changes, data migration, release processes, reinstall scripts | Challenge premises first -> propose at least two options (must include a minimal-change option) for selection -> write spec -> up to 3 rounds of cold review -> start work after approval |

- **Hard gate**: For Medium and Large work, do not open working branches, dispatch builders, or build scaffolding before the spec is reviewed and open items are answered.
- **Do not agree by default**: When asked whether to expand scope, or when a proposed approach is not worthwhile and its complexity outweighs its benefit, state the assessment directly and explicitly recommend against it or suggest trimming it.
- **Agree on completion criteria upfront**: The spec must state inputs/scenarios and their corresponding concrete outcomes, behaviors that must not change, and completion criteria, and be handed to both the builder and the test author.

See `templates/spec.md` for the spec format, and `references/delivery-and-review.md` for spec cold review and user-question rules.

## Dispatching with Scope and Supervision

- **Scope and single owner**: Assign each task to one person only; the dispatch brief specifies the exact list of files allowed to be modified and a reasonable time limit; do not swap assignees mid-task; the upper tier halts the work if the direction is wrong or the issue is already resolved.
- **Monitor outputs, not status**: After dispatching, run a background watcher for the lower tier (or check progress periodically if that tool is unavailable), observing actual side effects (new commits, modified files, progress files, delivery files) rather than trusting status labels like "working"; stay quiet and do not interrupt if there are no changes and the timeout has not elapsed.
- **Red line against disabling checks**: Never disable any check or change it to "pass even on failure"; relaxing thresholds requires verification evidence and upper-tier review.

For stuck detection, premise verification on consecutive failures, and interruption/handoff rules, see `references/supervision.md`.

## Shared Files and Resource Contention Prevention

- When dispatching, the upper tier first partitions the files each node owns; before touching a shared file, register it on the task board (or in `TASKS.md` if that tool is unavailable) and release it when done.
- Heavy jobs (rebuilds, full acceptance tests, releases, large local model evaluations) must use the heavy-job queue lock (or run one at a time if that tool is unavailable) to avoid concurrent collisions.
- Local model resources yield according to interactive priority; pause background batch processing while high-priority interactive tasks are running.

## Delivery and Acceptance

- Builders commit only in their own worktree/branch; deliveries must include a file list, suggested commit message, how it was tested, untested items, and test side effects.
- Upon receipt, the upper tier must actually read the changes (focusing on potential data loss) and re-run at least one verification by sampling; if issues are found, return to the original conversation to fix rather than patching it yourself.

For full delivery rules, receipt checklists, test-authoring rules, and release processes, see `references/delivery-and-review.md`.

## Review (Cross-Vendor Review)

- **Cross-vendor review**: The author of the code and the reviewer must use models from different vendors; files under review must not be modified while a review is in progress.
- Provide only the diff, spec, and files to the reviewer, never the author's conclusions; always prepend `templates/review-header.md` to the review prompt.

For assembling review prompts and additional rules for second-opinion reviews, see `templates/review-prompt.md`.

## Testing, Permission Safety, and Automation

- **Test isolation and irreversible confirmation**: Always run tests using temporary directories and test configs without touching production data, keychains, or personal settings; irreversible operations such as deleting, overwriting, or changing settings require confirmation from the user in person; private code and logs must not enter public free pools or unapproved external channels.
- **Escalate twice-repeated mistakes into mandatory checks**: When the same class of mistake occurs a second time, lock it in at the highest effective layer in order of architecture, types/lint/hooks/permission settings, acceptance tests, and documentation; see `references/repeat-mistakes.md`.
- **Automation entry gate and quota principles**: For the two-question check before auto-starting/auto-releasing, budget caps, and rule-writing principles, see `references/automation-gate.md`.
