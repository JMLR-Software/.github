# .github/workflows/ — The reusable client-request workflow

## What This Workspace Is For

`client-request.yml` is called by a five-line stub in each client repo (`uses: JMLR-Software/.github/.github/workflows/client-request.yml@main`) when an issue is labelled `request`. It checks out the client repo and this repo, installs pnpm and Node, and runs `anthropics/claude-code-action@v1` with a fixed prompt.

## Layout

- `client-request.yml`: the only workflow. Gate: `github.event.label.name == 'request'`. Model: `--model claude-sonnet-5`. Tools: the `--allowedTools` list in `claude_args`. The Claude step carries `id: claude` so the step after it can upload `steps.claude.outputs.execution_file` as an artifact.

## Key Workflows

- **Change the prompt or tools:** edit `client-request.yml`, push to a branch, point the `<client-repo>` stub at `@<branch>` temporarily, file a test issue there, then restore `@main`.
- **Escalate the model:** change `--model claude-sonnet-5` to `claude-opus-5`; nothing else changes.
- **Debug a run:** `gh run list -R JMLR-Software/<client-repo> --workflow request.yml`, then `gh run view <id> --log`. The log prints only the totals — `total_cost_usd`, `num_turns`, `permission_denials_count`. For anything finer, download the run transcript: `gh run download <id> -R JMLR-Software/<client-repo>`. It names every denied tool call and the token breakdown behind the cost, which is what the allowlist and the client guides should be tuned against.
- **Read the cost of a run:** the `result` object at the end of the log. Measured on 2026-09-11 (`sam-ko-noodle`, two real requests): $0.55 over 35 turns and $0.42 over 32 turns. Prompt caching is on by default in the Action; those numbers are about a quarter of what the same turn counts would cost uncached, so cost tracks turn count, not repo size.

## Rules

- Keep `permissions` to `contents`, `pull-requests`, `issues`, and `id-token` write. `id-token` is what authenticates to Anthropic: the run's GitHub OIDC token is exchanged under federation rule `fdrl_014v5bEpUWcETCXBktvopiXd` (subject `repo:JMLR-Software/*`, service account `github-actions`). There is no API key and no secret; do not add one.
- Never widen `--allowedTools` to unrestricted `Bash`.
- `--allowedTools` is a scoped allowlist: `Read,Edit,Write,Glob,Grep`, specific `Bash(git <subcommand>:*)` entries (never a bare `Bash(git:*)`), `Bash(gh pr create:*)`, `Bash(gh issue comment:*)`, `Bash(pnpm build)`, `Bash(pnpm install:*)`, `Bash(convert:*)`, `Bash(identify:*)`, `Bash(ls:*)`, `Bash(mkdir:*)`, `Bash(cp:*)`, `Bash(mv:*)`, `Bash(file:*)`. `curl` and `python3` are deliberately absent — image resizing goes through ImageMagick (`convert`/`identify`), and nothing in the workflow needs to fetch arbitrary URLs or run arbitrary scripts.
- Widen the allowlist only from a transcript. A denied call costs a wasted turn, and turns are what a run costs, so the denials in a real run are worth reading — but each addition is a security decision, and `curl`, `python3`, and bare `git` stay out regardless of how often they are attempted.
- The run transcript artifact holds the owner's own words and the client's site content. It is only ever uploaded inside a private client repo, with a 14-day retention. Do not upload it in this public repo, and do not lengthen the retention.
