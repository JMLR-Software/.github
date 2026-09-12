# JMLR-Software/.github

Shared pieces for every JMLR client site repo:

- `.github/workflows/client-request.yml`: reusable workflow. A client repo's stub calls it when an issue is labelled `request`; it runs Claude Code, which opens a pull request with the change and a drafted client reply.
- `client-site-rules.md`: the rules Claude reads at the start of every run.
- `new-repo-checklist.md`: what every new repo in the org does first. Running `/scaffold-workspaces` (a `CLAUDE.md` router and a `CONTEXT.md` per workspace) is required before the first feature commit.

**This repo is public and must never contain client data, photos, phone numbers, or secrets.** Client repos are private; each one carries only a stub workflow, an issue form, and its own `CLAUDE.md`.

Design: `local-sites/docs/superpowers/specs/2026-09-10-client-request-workflow-design.md`.
