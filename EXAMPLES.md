# Examples

Three worked walkthroughs of the workflow: a feature branch end to end, splitting a messy working tree into logical commits, and fixing a bad commit message.

---

## 1. A feature branch end to end

**Scenario**: add CSV export to the order history page. The change touches an export helper, the route that serves it, and a button in the UI.

### Start from fresh main

```bash
git checkout main
git pull origin main
git status   # nothing to commit, working tree clean
```

The tree is clean and `main` is current, so the branch will carry only your changes.

### Branch with intent

```bash
git checkout -b feature/order-export-csv
```

A reader sees `feature/order-export-csv` and knows the type (feature) and the intent (CSV export of orders) without opening the diff.

### Commit in logical units

The work has three distinct ideas, so it becomes three commits, each readable on its own.

```bash
# 1. the export helper plus its test
git add src/orders/export.ts src/orders/export.test.ts
git diff --staged
git commit -m "feat(orders): add CSV serializer for order history"

# 2. the route that exposes it
git add src/api/orders/export-route.ts
git diff --staged
git commit -m "feat(orders): serve order export at GET /api/orders/export"

# 3. the UI entry point
git add src/ui/OrderHistory.tsx
git diff --staged
git commit -m "feat(orders): add export button to order history page"
```

Each commit is one idea. If the bug-bash later finds the route returns the wrong content-type, that one commit reverts cleanly without touching the serializer or the button.

### Push and open the PR

```bash
git push -u origin feature/order-export-csv
```

PR description:

```text
Add CSV export to order history

What: new CSV serializer, a GET /api/orders/export route, and an
export button on the order history page.

Why: support requested a way for ops to pull order history into a
spreadsheet without DB access.

Verified: unit test on the serializer (export.test.ts), manual
download of a 2,000-row export confirmed UTF-8 and correct headers.

Out of scope: scheduled/emailed exports, tracked separately.
```

The reviewer reads three small commits and a description that states what, why, verification, and what was deliberately left out. That is a PR that gets approved on the first pass.

---

## 2. Splitting a messy working tree into logical commits

**Scenario**: you sat down to fix a null-handling bug, but along the way you also renamed a confusing variable and bumped a lint rule. Now `git status` shows three files changed for three unrelated reasons. Committing them together would produce a blob nobody can revert cleanly.

```bash
git status
# modified: src/auth/session.ts        (the bug fix AND the rename)
# modified: .eslintrc.json             (the lint bump)
# modified: src/auth/session.test.ts   (test for the bug fix)
```

### Stage the bug fix and its test first

`git add -p` walks the diff hunk by hunk so you can stage only the lines that belong to the bug fix, leaving the rename behind for now.

```bash
git add -p src/auth/session.ts
# stage the null-handling hunk: y
# skip the variable-rename hunk: n
git add src/auth/session.test.ts
git diff --staged          # confirm: only the fix and its test
git commit -m "fix(auth): return null only when refresh token is absent"
```

### Stage the rename as its own commit

```bash
git add src/auth/session.ts   # the remaining hunk, the rename
git diff --staged
git commit -m "refactor(auth): rename tok to refreshToken for clarity"
```

### Stage the tooling change last

```bash
git add .eslintrc.json
git commit -m "chore: enable no-floating-promises lint rule"
```

The single messy tree is now three commits with three different types. Each is revertable on its own, and the reviewer can approve the `fix` without getting tangled in the `chore`.

**The tell**: if a single commit message would need "and" (`fix null handling and rename the variable and bump lint`), it is more than one commit. `git add -p` is the tool that lets you honor that rule even after the changes are already in the working tree.

---

## 3. Fixing a bad commit message

**Scenario**: you just committed and the message is `fixed stuff`. It has not been pushed yet.

### Fix the most recent commit

`git commit --amend` rewrites the last commit's message in place.

```bash
git commit --amend -m "fix(orders): handle empty result set in CSV export"
```

Read the new message against the rules: type is `fix`, scope is `orders`, the description is imperative and specific, no trailing period. Good.

### Fix a message a few commits back

If the bad message is not the most recent commit and nothing is pushed yet, an interactive rebase lets you reword it.

```bash
git rebase -i HEAD~3
```

In the editor, change `pick` to `reword` (or `r`) on the offending commit, save, and git reopens an editor for just that message:

```text
# was: wip
feat(orders): add CSV serializer for order history
```

### A note on rewriting pushed history

Amend and reword rewrite history. That is safe on a local branch nobody else has pulled. Once a branch is pushed and someone may be building on it, rewriting it forces everyone else to recover from the change. The safe move on a shared branch is a new commit, not a rewrite:

```bash
git commit -m "docs: correct the misleading comment from the previous commit"
```

Fix messages before you push. After you push, prefer a new commit over rewriting what others can see.

---

## A note on cost

Every habit here costs a little time up front. Splitting a tree with `git add -p` is slower than `git add .`. Writing a real PR description is slower than leaving it blank. Naming a branch by intent is slower than `git checkout -b patch-2`.

The cost lands the other way when you skip it: the reviewer stalls on a tangled diff, the revert takes down a feature alongside the bug, and the bisect dead-ends on a string of `wip` commits. The discipline trades a small, predictable up-front cost for not paying a large, unpredictable one later.

## Further reading

- [`commit-standard-skills`](https://github.com/HermeticOrmus/commit-standard-skills): the full commit-message convention behind the `type(scope): description` format used throughout these examples.
