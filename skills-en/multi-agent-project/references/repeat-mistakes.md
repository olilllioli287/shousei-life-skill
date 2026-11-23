# Escalating Twice-Repeated Mistakes into Mandatory Checks

## Escalation Tiers (Highest to Lowest)

When the same class of mistake occurs a second time, adding another line of rules is not enough. Find the highest tier that can block it, in order:

1. **Make it impossible in architecture**: One owner per state, one way to do one thing, delete old patterns that get copied.
2. **Block with types, lint, hooks, or permission settings**: Put what should be used instead directly in the error message.
3. **Test behavior with acceptance tests**: Use tests to verify behavior does not regress.
4. **Write docs or rules last**: Reserve only for matters requiring judgment (where failing to follow does not break anything).

## Verification, Cleanup, and Periodic Scanning

- **Verify the check works**: Any newly added check must prove it catches that past real mistake (running it against that past version must fail).
- **Rules-to-enforcement mapping table**: When corrected by the user, fix the current instance first, then check the mapping table; if a rule already exists on the table without an enforcement mechanism, it is a repeat offense—escalate it to a higher tier in the same change.
- **Clean up redundant text rules**: Once a rule is blocked by architecture, types, lint, hooks, or permission settings, delete the original prose rule and keep only its row in the mapping table to save context and quota; ask the user before deleting rules in project rule files or memory. Rules that rely purely on judgment and cannot be structured go in a prominent place with one real failure example.
- **Periodic scanning**: Once a week, have a broad file-reading model or reviewer model read recent commits, reverts, review rejections, memory, and user corrections to list "mistake categories appearing twice or more + evidence + recommended tier to block at"; the orchestrator picks high-frequency ones to open tasks on the task board (or writes them into `TASKS.md` if that tool is unavailable).
