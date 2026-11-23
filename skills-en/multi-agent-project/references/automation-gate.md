# Automation Entry Gate, Rule Writing, and General Principles

## Automation Entry Gate

- Before letting AI start work or release on its own (auto-release, auto-resume after interruption, scheduled dispatch, auto-merge), ask two questions first:
  1. If it goes wrong, can it be rolled back?
  2. Can correctness be verified programmatically?
  If either answer is "no", do not make it fully automatic; stop and wait for human confirmation instead.
- Every automation or long-running dispatch must have a budget cap (time or quota, written in the dispatch brief or schedule); when reached, stop and report rather than extending on your own.
- Do it by hand first or watch it run once before turning it into a reusable tool; write the escape hatch (how to stop, how to recover) beforehand.

## How to Write Rules

- Keep only sentences that change decisions in rules; delete anything you are unsure about keeping; put incident narratives (who said what on which day, what got stuck) into memory, the task board, or progress files, not into rule files. Judgment-based rules may include one short example of "what a mistake looks like".
- Whenever the primary model changes, re-audit the rule files; after changing an important rule, pick one or two real tasks and run them before and after the revision to see whether the model reads it and changes behavior.

## General Quota and Safety Red Lines

- **Do not swap assignees/models mid-task**: Do not swap assignees mid-task, and the orchestrator does not swap models mid-task.
- **Use soon-to-reset quotas**: Before a time-windowed quota resets, spend it on reviews or batches—otherwise it goes to waste.
- **High-cost ideation models**: Use only with user consent and time-boxed clearance for ideation and alternative-angle review; do not use for batches or reading code.
- **Private data**: Private code and logs must not enter public free pools that retain prompts or train on them; route minor batch edits on private code to cheap models or local models inside a private workspace.
- **Test isolation**: Always run tests using temporary directories and test configs without touching production data, keychains, or the user's personal settings.
- **Irreversible confirmation**: Irreversible operations such as deleting, overwriting, or changing settings require confirmation from the user in person.
