<p align="center">
  <img src="https://ormus.solutions/mascot/pixellab_liquid_to_branch.gif" alt="Git Workflow Skills" width="128" style="image-rendering: pixelated;" />
</p>

<h1 align="center">Git Workflow Skills</h1>

<p align="center">
  <em>A Claude Code skill for a clean git workflow — branch, commit in logical units, write good messages, and open a reviewable PR.</em>
</p>

<p align="center">
  <a href="https://github.com/HermeticOrmus/git-workflow-skills/stargazers"><img src="https://img.shields.io/github/stars/HermeticOrmus/git-workflow-skills?style=flat-square&color=aa8142" alt="Stars" /></a>
  <a href="https://github.com/HermeticOrmus/git-workflow-skills/blob/main/LICENSE"><img src="https://img.shields.io/github/license/HermeticOrmus/git-workflow-skills?style=flat-square&color=aa8142" alt="License" /></a>
  <a href="https://github.com/HermeticOrmus/git-workflow-skills/commits"><img src="https://img.shields.io/github/last-commit/HermeticOrmus/git-workflow-skills?style=flat-square&color=aa8142" alt="Last Commit" /></a>
  <img src="https://img.shields.io/badge/Claude_Code-aa8142?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code" />
</p>

---

A single `CLAUDE.md` that walks any change through a git workflow built for review: start from fresh `main`, branch, commit in logical units, write conventional-commit messages, push, and open a pull request a reviewer can actually read.

## The problem

Sloppy git history is unreviewable and unrevertable.

- A reviewer cannot approve a 40-file commit that mixes a feature, a refactor, and a formatting pass. They either rubber-stamp it or block on it.
- A maintainer cannot bisect a branch where each commit is "wip" and "more changes."
- Nobody can cleanly revert a commit that bundled two unrelated ideas. Reverting the bug means reverting the feature too.
- A branch cut from stale `main` carries conflicts the author never wrote, and the PR diff lies about what actually changed.

The fix is not more tooling. It is a small set of habits that keep history legible: branch from fresh `main`, commit one idea at a time, name the commit by its type and intent, and keep the PR small enough to review.

## The workflow

1. **Start fresh**: pull `main`, confirm a clean tree, then branch. Never branch on top of stale `main` or unrelated changes.
2. **Branch with intent**: `feature/`, `fix/`, `chore/`, `docs/`, `refactor/` plus a short slug that states what the branch does.
3. **Commit in logical units**: one idea per commit. If the message needs "and," it is two commits. Use `git add -p` to split a dirty tree; read `git diff --staged` before every commit.
4. **Write conventional-commit messages**: `type(scope): description`, imperative mood, no trailing period. Add a body that explains why, not what.
5. **Push and open a reviewable PR**: one PR does one thing, stays small, and carries a description stating what changed, why, and how it was verified.
6. **Keep it reviewable as it evolves**: answer review feedback with new commits, and keep the branch current with `main` so the diff stays honest.

Full content: [`CLAUDE.md`](CLAUDE.md). Worked walkthroughs: [`EXAMPLES.md`](EXAMPLES.md).

## Install

### As a project CLAUDE.md

Drop [`CLAUDE.md`](CLAUDE.md) at the root of your repository. Claude Code picks it up automatically. Merge with existing project instructions if any.

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/HermeticOrmus/git-workflow-skills/main/CLAUDE.md
```

### As a Claude Code skill

The same content is packaged as a skill under [`skills/git-workflow/`](skills/git-workflow/) for `~/.claude/skills/`. Copy or symlink the folder into your skills directory; see the `SKILL.md` inside.

### As a `/git-workflow` slash command

Save [`CLAUDE.md`](CLAUDE.md) as `~/.claude/commands/git-workflow.md` to invoke the workflow on demand with `/git-workflow` in any Claude Code session.

```bash
curl -o ~/.claude/commands/git-workflow.md https://raw.githubusercontent.com/HermeticOrmus/git-workflow-skills/main/CLAUDE.md
```

### In Cursor

See [`CURSOR.md`](CURSOR.md) for the Cursor-rule equivalent at [`.cursor/rules/git-workflow.mdc`](.cursor/rules/git-workflow.mdc). The rule is committed with `alwaysApply: false`, so it loads when the work is git-related rather than on every turn.

### In other AI coding tools

If your tool reads a single instruction file at the project root, copy `CLAUDE.md` to whatever name your tool expects (`AGENTS.md`, `INSTRUCTIONS.md`, etc.).

## See also

- [`commit-standard-skills`](https://github.com/HermeticOrmus/commit-standard-skills): the commit-message convention as its own packaged skill. This repo references `type(scope): description`; that repo is the full convention.

## Contributing

PRs welcome, especially additional worked walkthroughs in [`EXAMPLES.md`](EXAMPLES.md), translations of the README, and adaptations of `CURSOR.md` for other AI coding tools (Windsurf, Cline, Aider, Continue, etc.).

## License

MIT. Use it, fork it, merge it into your own CLAUDE.md.
