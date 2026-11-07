# Working on FreshOps

## Daily session

Spend about 90 minutes: review the next task, complete one useful deliverable, verify it, then record the result and next step. Day numbers are work sessions, not a streak requirement.

Read [the problem statement](docs/problem-statement.md) before expanding scope. The [roadmap](docs/roadmap.md) is a planning estimate; change it when evidence warrants a different approach.

## Meaningful commits

A commit should capture one coherent, reviewable change. A completed endpoint, a fixed inventory bug, an evaluation report, or a useful design decision can each be a commit. Save related changes together. Do not create empty commits, backdate work, or split individual keystrokes to inflate activity.

Suggested prefixes:

| Prefix | Use |
|---|---|
| `feat:` | A usable new behavior |
| `fix:` | A corrected behavior |
| `docs:` | A substantive explanation, plan, or report |
| `test:` | Meaningful verification of a risk or requirement |
| `chore:` | Repository or tooling maintenance |
| `refactor:` | Internal structure changes preserving behavior |

Prefer `fix: reject duplicate sales rows during import` over `update files`.

Before committing, inspect exactly what you will publish:

```powershell
git status --short
git diff
git add -- path/to/changed-file
git diff --cached
git diff --cached --check
git commit -m "docs: explain the decision and its scope"
```

Run checks appropriate to the actual change. Documentation changes need reviewed facts and working relative links; application logic needs the relevant behavior checks. Add setup and test commands when those tools exist.

## Branches and pull requests

The initial documentation can be committed directly to `main`. For a feature spanning several sessions:

```powershell
git switch -c feat/csv-import
# Implement, inspect, verify, and commit the related work.
git push -u origin feat/csv-import
```

Open a pull request describing the user-visible change, why it matters, how it was verified, and any remaining limitations. Keep scope small enough to review. There is no required number of commits per day.

## Data and secrets

- Keep `.env` files, credentials, raw datasets, and trained artifacts out of Git.
- Commit sanitized configuration examples and small synthetic fixtures only when useful.
- Attribute public datasets and reused code. Their licenses are separate from any future project license.
- Label benchmark, synthetic, and pilot evidence separately.
- Do not publish customer records without explicit authorization.

## Progress records

Write a short record when a session produces a decision or result worth preserving: completed work, evidence, unresolved questions, and next action. A progress note should contain useful information, not exist solely to generate a commit.
