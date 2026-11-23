# Spec: <One-Line Title>

Tier: Medium | Large    Task ID: <ID>    Date: <YYYY-MM-DD>

## 1. What Problem This Solves and What Happens If Not Done
(2-3 lines; include the original requirement description)

## 2. Premises (For Large Tier, User Must Approve or Reject Each Item)
- We assume: ...
- We assume: ...

## 3. Options (Required for Large Tier; Do Not Write Sections Below Until User Chooses)
| Option | Change Scope (Files / Lines) | Risk | Notes |
| --- | --- | --- | --- |
| A: Minimal Change | | | |
| B: Ideal Version | | | |
User choice: <A / B, with decision record>

## 4. Behavior to Change (Write as "Input / Scenario -> Outcome", No Vague Wording)
- `<input or action>` -> <observable outcome, exit code, saved file contents>
- ...

## 5. Behavior Not to Change (NOT in Scope)
- These valid inputs must continue to pass as before: ...
- Things intentionally left untouched and why: ...

## 6. Fact Verification (Include `file:line` for Each Item; Write "Unverified" If Not Checked)
- Whether this has already been implemented or fixed: ...
- Related call sites: ...
- Existing utilities or functions that can be reused: ...

## 7. Failure Paths
- Normal: ...  Null/empty value: ...  Empty collection: ...  Errors (unreadable file, timeout, permissions): ...
- Whether it can fail silently (invisible to user, not logged): ...
- When touching delete / archive / replace / restore: list a "Type x Action -> Data to Retain" matrix
| Type \ Action | | |
| --- | --- | --- |

## 8. Completion Criteria (For the Test Author to Write Acceptance Tests)
- ...
Items requiring manual verification by the user:
- ...

## 9. Where to Fix
Prefer a single-point fix in a shared function; list of files to modify (builder may only touch these): ...

## Review Log
- Round 1 (cold reviewer, date): conclusion / open items
- User decision: <decision details>
