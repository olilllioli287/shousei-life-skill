# Delivery, Receipt Inspection, and Review Dispatch Rules

## Spec Review and Asking the User

- **Spec cold review**: Handed to a cold reviewer for a cold read with only the spec and relevant files, without conversation history or orchestrator opinions; prepend `templates/review-header.md` to the prompt and additionally ask: "Is the spec written too broadly, self-contradictory, or missing behaviors that must not change?" Run at most 3 rounds; if the same unresolved issue repeats for two consecutive rounds, stop and hand it to the user to decide. Agreement among multiple models counts only as a recommendation.
- **Asking the user**: Only the orchestrator may ask the user; integrators and builder nodes do not ask directly and instead organize open items into a list for the orchestrator. The orchestrator decides technical trade-offs first according to rules (do not relax protections; pick the simplest reliable option) and asks the user only what truly requires user sign-off. Ask one question at a time in plain language, stating what breaks or is lost if chosen wrong, pros and cons of each option, and the orchestrator's recommendation (explicitly marked as only a recommendation); leave unanswered items in "Proposals" on the task board (or write them into `TASKS.md` if that tool is unavailable) and do not dispatch work.

## Delivery and Sampled Re-Run

- Deliveries include: file list, suggested commit message, how it was tested, what was not tested, and test side effects.
- The upper tier must actually read the changes, focusing on whether data could be lost; if issues are found, return to the original conversation to fix rather than patching it yourself.
- **Sampled re-run**: Even when the lower tier attaches evidence, the upper tier does not need to re-run everything, but must personally re-run or verify at least one item per delivery (for example, checking the version number and signature after release, or spot-checking whether a reviewer's quote actually exists in the source file).
- Statements in reports must distinguish between "measured / inferred / guessed"; checks that the lower tier can run itself must not be pushed onto the upper tier or the user. Evidence proves only the specific cases tested, not that the entire task is done.

## Receipt Checklist (Orchestrator / Integrator)

1. **Item-by-item spec verdict**: Done (with `filename:line`) / Partial / Not done / Changed / Unverifiable (with manual verification steps). "Modified related code" does not count as done.
2. **Actual acceptance tests run**: How many cases actually ran (vs. expected); 0 cases counts as failure, and exit codes are not evidence.
3. **Filtering review findings**: Discard (with reasons) findings that are purely stylistic, cannot point to a triggering call site or input, add unrequested defenses, merely suggest an alternative phrasing, or hypothesize risks that only occur with unmade future changes; for findings with reproduction commands, the receiving tier re-runs them and keeps them only if they actually fail; merge multiple items sharing the same root cause; findings independently spotted by multiple reviewers are most credible; in the summary, state how many items were filtered out per reviewer and why.
4. **Version and round cap**: The reviewed version equals the version to be committed; at most 3 rounds—if it does not converge, stop and report "not converged".
5. **Multi-reviewer consolidation**: Separate results from multiple reviewers into "found by all" and "found by one side only", and mark anyone who did not finish.

## Test-Authoring Rules (Acceptance Tests)

- **Separation of duties**: Separate test authoring from code writing; the test author reads only the spec and original requirements, does not read source code, and does not share a conversation with the code author.
- **State-change matrix**: When touching branches such as delete/archive/replace/restore, create a "type x action -> data that must remain" matrix of acceptance tests, one test per cell; any cell not covered by a test counts as a highest-severity issue.
- **Identifier changes**: When renaming or deleting identifiers, simultaneously check whether old acceptance tests asserting "old name does not exist" are still valid.
- **Actual bug-fix test**: Acceptance tests for bug fixes must include an actual run showing "fails before the fix".

## Review Dispatch Rules

- Files under review must not be modified while a review is in progress.
- Before dispatching a review, confirm the builder has delivered and stopped making changes; before collecting results, confirm every reviewer has reported back or stopped.

## Release Process

1. The builder finishes edits and commits in their own worktree/branch, then delivers to the integrator.
2. The integrator reads the changes and commits only the files on the delivery list (do not use `git add -A`).
3. Always run acceptance tests before releasing; if acceptance tests fail, do not tag or push.
4. The release script builds, releases, and installs from a clean copy of the tag.
5. Fixes that were reported and are awaited for immediate use are released as soon as fixed; minor changes are batched into 3-5 items or released at wrap-up.
