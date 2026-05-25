# CLAUDE.md

A git workflow for any change that will be reviewed: start from fresh `main`, branch, commit in logical units, write conventional-commit messages, push, and open a reviewable pull request. Merge with project-specific instructions as needed.

**Why this exists**: sloppy git history is unreviewable and unrevertable. A reviewer cannot approve a 40-file blob; a maintainer cannot bisect a tangled branch; nobody can cleanly revert a commit that mixed a feature with a refactor. The workflow below keeps history legible so review is fast and reverts are surgical.

**Tradeoff**: this biases toward small commits and small PRs over one big push. For a one-line typo fix, use judgment.

## 1. Start fresh

Branch from an up-to-date `main`. A branch cut from stale `main` carries conflicts you did not write.

```bash
git checkout main
git pull origin main
git status   # confirm a clean tree before branching
```

If `git status` shows uncommitted work you want to keep, stash it (`git stash`) or commit it on its own branch first. Do not branch on top of unrelated changes.

## 2. Branch with an intent-named branch

The branch name states the type and the intent. A reader should know what the branch does without reading the diff.

```bash
git checkout -b feature/order-export-csv
git checkout -b fix/session-refresh-null
git checkout -b chore/bump-eslint-9
```

Prefixes: `feature/` for new behavior, `fix/` for bug fixes, `chore/` for maintenance, `docs/` for documentation, `refactor/` for structure-only changes. Use a short hyphenated slug, not a ticket number alone.

## 3. Commit in logical units

A commit is one logical change. Not one file, not one session of work, one idea. If you cannot describe a commit without the word "and," it is two commits.

```bash
git add src/orders/export.ts src/orders/export.test.ts
git diff --staged          # read what you are about to commit
git commit -m "feat(orders): add CSV export for order history"
```

Stage selectively. `git add -p` lets you split a dirty working tree into separate staged hunks so unrelated changes land in separate commits. Read `git diff --staged` before every commit so you commit what you mean to.

Atomic commits are what make a branch revertable: a clean revert of one commit should undo exactly one idea, leaving everything else intact.

## 4. Write conventional-commit messages

The message format is `type(scope): description` in the subject line.

- `feat`: new behavior the user can observe
- `fix`: a bug fix
- `docs`: documentation only
- `refactor`: structure change with no behavior change
- `test`: adding or correcting tests
- `chore`: build, deps, tooling, config

Rules for the subject:

- Imperative mood: "add", not "added" or "adds".
- No trailing period, under ~72 characters.
- `scope` is the area touched (`orders`, `auth`, `ci`); omit it if the change is broad.

For anything non-trivial, add a body after a blank line explaining why, not what. The diff already shows what changed; the body records the reason a future reader cannot reconstruct.

```text
fix(auth): return null only when refresh token is absent

Session.refresh returned null for both an absent and an expired
refresh token, so callers could not distinguish "log in again"
from "retry". Expired now throws RefreshExpiredError.
```

## 5. Push and open a reviewable PR

Push the branch and open a pull request that a reviewer can actually review.

```bash
git push -u origin feature/order-export-csv
```

A reviewable PR:

- Does one thing. If the title needs "and," split the PR.
- Stays small. Hundreds of lines review well; thousands do not.
- Has a description that states what changed, why, and how it was verified.
- Lists the test that proves it works, or says why no test applies.

The PR description is for the reviewer, not the author. Write the context the reviewer needs to say yes, including anything surprising in the diff and anything intentionally left out of scope.

## 6. Keep PRs reviewable as they evolve

Review feedback is more commits, not rewritten history on a branch others are reading.

```bash
git add <files>
git commit -m "fix(orders): handle empty result set in CSV export"
git push
```

Keep the branch current with `main` as it ages. A long-lived branch drifts; rebase or merge `main` in so the PR diff stays honest about what it changes. Resolve conflicts on your branch, not in the reviewer's lap.

---

## Quick reference

```bash
# fresh start
git checkout main && git pull origin main && git status

# branch
git checkout -b feature/<slug>

# stage a logical unit, read it, commit it
git add -p
git diff --staged
git commit -m "feat(scope): imperative description"

# push and open PR
git push -u origin feature/<slug>
```

---

**Commit message convention**: the `type(scope): description` format above is Conventional Commits. For the full convention as its own packaged skill, see [`commit-standard-skills`](https://github.com/HermeticOrmus/commit-standard-skills).

**License**: MIT. Use it, fork it, merge it into your own.
