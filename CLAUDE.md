# JMLR-Software/.github

Shared GitHub Actions workflow and site rules for every JMLR client site repo. Public. Nothing client-specific lives here.

## Workspaces

```
.github/
├── CLAUDE.md                        this file
├── README.md                        for humans: what this repo is
├── client-site-rules.md             the rules Claude reads at the start of every client run
├── new-repo-checklist.md            what every new repo in the org must do first (/scaffold-workspaces)
└── .github/workflows/
    ├── CONTEXT.md                   how to change and test the reusable workflow
    └── client-request.yml           the reusable workflow
```

## Routing table

| Task | Start with | Skip |
|---|---|---|
| Change what Claude may or may not do on a client site | `client-site-rules.md` | the workflow |
| Change how the run is triggered, its permissions, tools, or model | `.github/workflows/CONTEXT.md`, then `client-request.yml` | the rules file |
| Understand the whole design | `local-sites/docs/superpowers/specs/2026-09-10-client-request-workflow-design.md` | this repo |
| Start a new repo in the org | `new-repo-checklist.md` | everything else |

## Rules

1. Public repo: never a client name, phone number, photo, address, or secret. Test with placeholders.
2. Changes to the rules file or workflow affect every client repo on the next run; test on `<client-repo>` (`local-sites/docs/superpowers/plans/2026-09-10-request-execution-action.md`, Tasks 6 and 7) before merging.
3. The rules file is prose Claude reads at run time. Keep it under 120 lines; move background to the spec.
4. Every repo in the org runs `/scaffold-workspaces` before its first feature commit: a `CLAUDE.md` router plus a `CONTEXT.md` per workspace. Required, no exceptions; `new-repo-checklist.md` says what it produces and how to keep it current.
