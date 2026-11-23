# Supervision, Consecutive-Failure Verification, and Interruption Handoff

## Supervision and Detecting Stalls from Outputs

- After dispatching work, run a lower-tier watcher in the background (or check progress periodically if that tool is unavailable); never wait until the user asks to discover a stall.
- Assign each task to one person only. If the direction is wrong or the issue is already resolved, the upper tier proactively halts it.
- Watch the lower tier via actual side effects (new commits, modified files, progress files, delivery files) rather than trusting status labels like "working"; if nothing changed, stay quiet and do not interrupt.
- Record a reasonable time limit for the work type when dispatching; swap assignees only when the lower tier errors out, cannot start a turn, or produces no output past the time limit (without waiting for the old one to finish); otherwise follow "do not swap mid-task"; if the lower tier is waiting for a reply, answer directly to unblock it.
- Never disable any check or change it to "pass even on failure"; relaxing thresholds requires verification evidence and upper-tier review.

## Verify Premises on Consecutive Failures

- When fixes in the same direction fail the same gate twice in a row, stop first, write down the single shared premise assumed by those attempts, and find an observation where "if the premise is wrong, the result will differ" to test it (look at where failures are distributed, not just failure magnitude).
- Do not start a third edit before the premise is written down and verified.
- When the upper tier receives a "still red on the second attempt" report, require the premise and evidence first rather than directly approving the next edit.

## Interruption and Handoff

### Pausing

When a pause is needed (closing the laptop lid, logging out, stepping away, or halted by the upper tier):

- Finish the current small step or roll it back; do not open new tasks and halt lower tiers.
- Do not perform irreversible actions (no push, no release).
- Keep half-edited files recoverable: on an isolated branch you may make a `wip:` commit; in a shared workspace, clearly record which files are half-edited in the progress file.
- Write handoff notes in the progress file: goal, how far it got, what has been verified, current state, exact first action next, key files, and known pitfalls.
- State in the report: "This is a pause, not completion." If explicit instructions to continue are received, do not pause.

### Resuming

When returning from an interruption or starting a new conversation after handoff:

- Read the progress file and handoff file first, treat the predecessor's records as facts, and do not re-investigate from scratch or re-run already completed reproductions or acceptance tests; only compare "done vs. remaining" to find the pickup point.
- However, items the predecessor claimed "passed / fixed" must be re-verified on the actual artifact against the original goal before counting (self-reporting is not evidence).
