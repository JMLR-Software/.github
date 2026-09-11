# .github/workflows/ — The reusable client-request workflow

## What This Workspace Is For

`client-request.yml` is called by a five-line stub in each client repo (`uses: JMLR-Software/.github/.github/workflows/client-request.yml@main`) when an issue is labelled `request`. It checks out the client repo and this repo, installs pnpm and Node, and runs `anthropics/claude-code-action@v1` with a fixed prompt.

## Layout

- `client-request.yml`: the only workflow. Gate: `github.event.label.name == 'request'`. Model: `--model claude-sonnet-5`. Tools: the `--allowedTools` list in `claude_args`.

## Key Workflows

- **Change the prompt or tools:** edit `client-request.yml`, push to a branch, point the Simple Cuts stub at `@<branch>` temporarily, file a test issue there, then restore `@main`.
- **Escalate the model:** change `--model claude-sonnet-5` to `claude-opus-5`; nothing else changes.
- **Debug a run:** `gh run list -R JMLR-Software/simple-cuts --workflow request.yml`, then `gh run view <id> --log`.

## Rules

- Keep `permissions` to `contents`, `pull-requests`, `issues`, and `id-token` write. `id-token` is what authenticates to Anthropic: the run's GitHub OIDC token is exchanged under federation rule `fdrl_014v5bEpUWcETCXBktvopiXd` (subject `repo:JMLR-Software/*`, service account `github-actions`). There is no API key and no secret; do not add one.
- Never widen `--allowedTools` to unrestricted `Bash`.
