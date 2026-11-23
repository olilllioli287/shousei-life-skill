# Review Prompt Template

## Assembly Rules

1. Before dispatching a review, confirm the builder has handed off and will no longer modify the files under review; files under review must not be modified during the review.
2. Provide only the materials (diff, spec, related files); **do not attach author explanations or orchestrator conclusions**.
3. Paste the full text of `templates/review-header.md` at the very beginning of the prompt.
4. If this is a **pre-start spec cold review**, add this question before the materials: "Is the spec written too broadly, self-contradictory, or missing behavior not to change?"
5. If this is a **second-opinion review (additional reviewer)**, paste the "Second-Opinion Additional Rules" below after `templates/review-header.md` and before the materials.

---

## Prompt Structure Template

```markdown
<Paste full text of templates/review-header.md>

<If this is a second-opinion review, paste "Second-Opinion Additional Rules" below here>

[Materials Under Review]
- Spec: <paste or attach spec path>
- Diff / Related Files: <paste diff or file list and contents>
```

---

## Second-Opinion Additional Rules

(For the additional reviewer; place after `review-header.md` and before the materials)

1. Judge strictly by what is actually written in the attached diff and spec; do not guess behavior from function names, file names, or unattached code.
2. Do not speculate that "this change might break something else" unless you can point to a concrete affected call path in the diff (file + line number + verbatim source).
3. Do not list these two as findings: behavior explicitly required by the spec; pre-existing issues that existed before this change and were not touched by it.
4. Add a separate section "Looks like an issue, but is actually fine": list places that look suspicious and why they are fine (e.g., specified this way by the spec, or already handled in context). Prefer putting items here rather than padding the findings list.
5. (Additional reviewer rule) Score confidence 0-10 for each finding: 9-10 = trigger condition directly proven from visible code; 7-8 = likely, with one unseen premise; the second-opinion reviewer must additionally list only 7 and above (omit 6 and below).
6. Do not report: missing comments, missing type annotations, naming and formatting, or unused imports, unless they actually cause the code to fail.
7. Write explanations in Traditional Chinese; copy file names, function names, and source lines verbatim without translating.
