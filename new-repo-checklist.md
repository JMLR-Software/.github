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

## 3. Protection is enforced at the org level, not by this checklist (nothing to do)

A new repo is already covered the moment it is created. Two **organisation** rulesets target `~ALL` repositories on `~DEFAULT_BRANCH`:

| Ruleset | Enforces |
|---|---|
| `default-branch-basics` | no branch deletion, no force-push |
| `main-via-pr` | changes to the default branch go through a pull request (`required_approving_review_count: 0`, so a solo operator merges his own) |

Secret scanning and push protection are on for every existing repo, and the org defaults `secret_scanning_enabled_for_new_repositories` and `secret_scanning_push_protection_enabled_for_new_repositories` are both true, so a new repo inherits them. Push protection blocks a commit containing a recognised credential at push time — this is what makes "no secrets in any repo" a rule rather than a wish.

All of this was set on 2026-09-13. Before that, the PR rule existed only on `jmlr-dev` and only as text in each repo's `CLAUDE.md`, so the three client storefronts — where a bad push reaches a real business — were the unprotected ones.

### Which repos the pull-request rule applies to

Josh's line, 2026-09-13: **enforce it wherever a bad push reaches someone other than Josh.** Anything public or client-facing is in; a personal working repo is out.

| Repo | In or out | Why |
|---|---|---|
| `simple-cuts`, `sam-ko-noodle`, `deborah-burke-henderson` | in | live client storefronts |
| `jmlr-dev` | in | public, and it is the business's own site |
| `.github` | in | public, and its workflow runs on every client repo |
| `local-sites` | **out** | Josh's planning and docs repo: no deploy, no public surface, and a PR per doc edit is friction with nothing on the other side of it |

`local-sites` is in the org ruleset's `repository_name.exclude`. It keeps `default-branch-basics`, so the branch still cannot be deleted or force-pushed — the exemption is from the review step, not from the safety rails.

A new repo defaults to **in**, because `~ALL` is the include list. Exempt one only by adding it to `exclude`, never by weakening or deleting the rule.

**The one thing still per-repo:** a **required status check** is deliberately not defaulted. Add one only once the repo's deploy check reports under a stable name, and **name the check the repo actually produces.** A required check that never reports makes every PR permanently unmergeable — `jmlr-dev` requires `Cloudflare Pages` and will lock itself out the moment it moves to Workers.

## 4. Hosting is Workers, never Pages (required, every site repo)

Decided 2026-09-11, restated 2026-09-13: every site is a Cloudflare **Worker with static assets**. Do not create a Pages project for a new site, and do not copy a Pages setup from an older repo.

A new site repo ships `.node-version` (22; Workers Builds reads the file, where Pages read a `NODE_VERSION` env var) and a `wrangler.jsonc` holding the whole deploy configuration — assets directory, `not_found_handling`, `workers_dev`, `preview_urls`, and the custom domain as a `routes` entry. Copy `sam-ko-noodle`'s, which is the reference. Connect the repo to **Workers Builds** in the dashboard (`pnpm build`, then `npx wrangler deploy`, production branch `main`) so deploys do not depend on anyone's laptop.

The two sites still on Pages are migrating; the recipe, order and risks are `local-sites/docs/superpowers/plans/2026-09-11-pages-to-workers-migration.md`. Nothing new should join them.

## 5. Client site repos only

Follow `local-sites/docs/superpowers/specs/2026-09-10-client-request-workflow-design.md` for the stub workflow, the issue form, and the `request` label, then register the client under `local-sites/clients/<slug>/`.

The standing rule behind step 1 lives in `claude-dotfiles/rules/common/workspaces.md` and applies to every repo Josh works on, inside this org or not.
