# AGENTS.md — Working Instructions for this Repository

This file documents how work should be done in this repository. It is meant for
any contributor (human or AI-assisted) and any future session that picks up
where a prior one left off.

## Process Rules

### 1. Reproducibility before fixing
- Before changing code, capture a **reproducible** failure: the exact command
  run, the working directory, and the output that shows the problem.
- Save the command + output as evidence the issue is real.
- A fix without a prior reproduction is a guess.

### 2. Before/After evidence
- Every change ships with:
  - **Before:** the failing state, with the command and output that demonstrated
    the problem.
  - **After:** the same command (or an equivalent) re-run, showing the new
    correct output.
- This is the only reliable way to prove a fix worked.

### 3. Test logs and reports
- Attach the full output of any test, check, or linter that exercises the
  changed code, not just a one-line summary.
- If a test was added in the same change, show that it fails before the fix
  and passes after.
- If a CI check (e.g. `bash scripts/check-divisions.sh`) is part of the change,
  include its output verbatim.

### 4. Scope discipline
- One change = one commit = one concern.
- Do not mix a docs typo fix with a code refactor.
- A PR that touches files outside its stated scope should be split or
  re-scoped before review.

### 5. Identity and attribution
- Commits and PRs are attributed to the **real human contributor** who set
  the local `git config user.name` and `user.email`.
- AI assistance must be disclosed in the PR body if the contribution guidance
  requires it, but commit authorship stays with the human.
- Never attribute work to a person who did not author it.

### 6. Project rules take priority
- `CONTRIBUTING.md` is the source of truth for contribution rules.
- `SECURITY.md` governs secrets, credentials, and executable code in agent
  files.
- `.gitattributes` (LF) and `.gitignore` (no build output) are enforced by
  CI; do not bypass them.

### 7. CI gates before pushing
Run these before every push, every time:
```bash
bash scripts/check-divisions.sh
bash scripts/check-tools.sh
bash scripts/check-runbooks.sh
bash scripts/lint-agents.sh 2>&1 | tail -3
```
A push that breaks CI is much harder to recover from than one that was never
pushed.

## Local Workflow

```
edit files -> reproduce failure -> write fix -> run check scripts ->
re-run reproduction -> commit -> push -> open PR with evidence
```

## When picking up a stale branch
- Re-run all four CI checks; CI may have moved on since the branch was cut.
- Re-read the issue or PR conversation; maintainer feedback may have arrived
  while the branch sat idle.
- Rebase on `main` (fork) or `git pull --rebase` (direct clone) before adding
  new commits.

## Honesty about what was tested
- Mark a checkbox only if the check genuinely passed.
- "Tested in real scenarios" in a PR template means you actually ran the
  change in the field — not just that you ran `bash scripts/lint-agents.sh`.
- If you only ran scripts locally, say so.
