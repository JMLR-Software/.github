# New repo checklist

**Date:** 2026-09-12, revised 2026-09-13

Every repo in JMLR-Software, client site or not, starts with these steps. They are required, not suggestions. A repo is not ready for its first feature commit until step 1 is done.

## 1. Run `/scaffold-workspaces` (required, every repo, no exceptions)

In Claude Code, from the new repo's root, run `/scaffold-workspaces` before the first feature commit. It produces:

- `CLAUDE.md` at the root: what the project is, the stack, a workspaces tree, a routing table (task, workspace, start-with file, what to skip), naming conventions, and project-wide rules. Short enough to read in one screen.
- `CONTEXT.md` in every workspace: what it is for, layout, key workflows, skills, rules, what to avoid.

If the repo already has an instruction file (for example the `AGENTS.md` that Next.js writes), keep `@AGENTS.md` as the first line of `CLAUDE.md`.

An existing repo that lacks the scaffold gets it the first time real work happens there. When a directory grows its own workflows or more than a handful of files, give it a `CONTEXT.md` and a routing-table row. When a `CONTEXT.md` passes about 150 lines, split the workspace.

## 2. Keep it current

Update `CLAUDE.md` and the affected `CONTEXT.md` in the same commit as the change that made them stale. A change is not done until its context files are.

## 3. Protect `main` (required, every repo)

The default branch is changed by pull request only. Every repo's `CLAUDE.md` says so, but saying it is not enforcing it: on 2026-09-13 only `jmlr-dev` actually had the rule, and the three client sites — the ones where a bad push reaches a real business's storefront — did not.

Two rulesets, both on the default branch:

- **`default-branch-basics`** is an org-level ruleset and applies to new repos automatically. It blocks deletion and force-pushes. Nothing to do.
- **`main-via-pr`** is per-repo and must be created: a `pull_request` rule with `required_approving_review_count: 0`, so a solo operator can merge his own PR but nothing lands on `main` without one. Copy it from any existing repo:

  ```sh
  gh api repos/JMLR-Software/<existing>/rulesets --jq '.[]|select(.name=="main-via-pr")' > /tmp/rs.json
  gh api -X POST repos/JMLR-Software/<new>/rulesets --input /tmp/rs.json
  ```

Add a required status check only once the repo's deploy check reports under a stable name, and **name the check the repo actually produces.** A required check that never reports makes every PR permanently unmergeable — `jmlr-dev` required `Cloudflare Pages` and would have locked itself out the moment it moved to Workers.

## 4. Hosting is Workers, never Pages (required, every site repo)

Decided 2026-09-11, restated 2026-09-13: every site is a Cloudflare **Worker with static assets**. Do not create a Pages project for a new site, and do not copy a Pages setup from an older repo.

A new site repo ships `.node-version` (22; Workers Builds reads the file, where Pages read a `NODE_VERSION` env var) and a `wrangler.jsonc` holding the whole deploy configuration — assets directory, `not_found_handling`, `workers_dev`, `preview_urls`, and the custom domain as a `routes` entry. Copy `sam-ko-noodle`'s, which is the reference. Connect the repo to **Workers Builds** in the dashboard (`pnpm build`, then `npx wrangler deploy`, production branch `main`) so deploys do not depend on anyone's laptop.

The two sites still on Pages are migrating; the recipe, order and risks are `local-sites/docs/superpowers/plans/2026-09-11-pages-to-workers-migration.md`. Nothing new should join them.

## 5. Client site repos only

Follow `local-sites/docs/superpowers/specs/2026-09-10-client-request-workflow-design.md` for the stub workflow, the issue form, and the `request` label, then register the client under `local-sites/clients/<slug>/`.

The standing rule behind step 1 lives in `claude-dotfiles/rules/common/workspaces.md` and applies to every repo Josh works on, inside this org or not.
