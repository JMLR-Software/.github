# New repo checklist

**Date:** 2026-09-12

Every repo in JMLR-Software, client site or not, starts with these steps. They are required, not suggestions. A repo is not ready for its first feature commit until step 1 is done.

## 1. Run `/scaffold-workspaces` (required, every repo, no exceptions)

In Claude Code, from the new repo's root, run `/scaffold-workspaces` before the first feature commit. It produces:

- `CLAUDE.md` at the root: what the project is, the stack, a workspaces tree, a routing table (task, workspace, start-with file, what to skip), naming conventions, and project-wide rules. Short enough to read in one screen.
- `CONTEXT.md` in every workspace: what it is for, layout, key workflows, skills, rules, what to avoid.

If the repo already has an instruction file (for example the `AGENTS.md` that Next.js writes), keep `@AGENTS.md` as the first line of `CLAUDE.md`.

An existing repo that lacks the scaffold gets it the first time real work happens there. When a directory grows its own workflows or more than a handful of files, give it a `CONTEXT.md` and a routing-table row. When a `CONTEXT.md` passes about 150 lines, split the workspace.

## 2. Keep it current

Update `CLAUDE.md` and the affected `CONTEXT.md` in the same commit as the change that made them stale. A change is not done until its context files are.

## 3. Client site repos only

Follow `local-sites/docs/superpowers/specs/2026-09-10-client-request-workflow-design.md` for the stub workflow, the issue form, and the `request` label, then register the client under `local-sites/clients/<slug>/`.

The standing rule behind step 1 lives in `claude-dotfiles/rules/common/workspaces.md` and applies to every repo Josh works on, inside this org or not.
