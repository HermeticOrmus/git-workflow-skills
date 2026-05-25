# Using this repo with Cursor

This project includes a **Cursor project rule** so the git workflow guidelines are available when you work here.

## In this repository

1. Open the folder in Cursor.
2. The rule [`.cursor/rules/git-workflow.mdc`](.cursor/rules/git-workflow.mdc) is committed with `alwaysApply: false`, so it loads situationally when the work is git-related rather than on every turn.
3. In Cursor, confirm under **Settings → Rules**, where `git-workflow` should appear.

## Use the same guidelines in another project

**Cursor (recommended)**: Copy `.cursor/rules/git-workflow.mdc` into that project's `.cursor/rules/` directory (create the folders if needed). Merge with existing rules as you like.

**Other AI coding tools**: If a stack only supports a root instruction file, copy [`CLAUDE.md`](CLAUDE.md) into that project instead (or merge its contents into your existing instructions). Most modern AI coding tools (Claude Code, Continue, Cline, Windsurf, Aider) read a root-level instruction file.

## Optional: personal Agent Skills

If you want the same content as a reusable skill under `~/.cursor/skills`, use [`skills/git-workflow/SKILL.md`](skills/git-workflow/SKILL.md). Copy or symlink it into your personal skills directory.
