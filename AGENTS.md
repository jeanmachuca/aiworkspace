# AI Workspace

This is a **workspace scaffold** for creating independent projects. Root is **not** a code project — it's workspace-level tooling config.

## Structure

- Each subfolder = standalone git repo, own language/framework, own CI, own `AGENTS.md`
- `.gitignore` uses `/*/` to ignore all subdirectories; only `/.github/` and `/.opencode/` are tracked at root
- Root config (`opencode.json`, `.pre-commit-config.yaml`, `.github/`) is **scaffolding** — do not apply to subfolder repos
- `.opencode/instructions/git-workflow.md` has `alwaysApply: true` — loaded every session; governs git operations across all subfolder repos

## What's ready at root

| Thing | Detail |
|-------|--------|
| `opencode.json` | Local inference at `http://api:8000/v1` + cloud model aliases |
| `.pre-commit-config.yaml` | Generic hooks only (trailing-whitespace, EOF fixer, check-yaml, check-added-large-files) |
| `dev-requirements.txt` | Only `pre-commit` |
| CI workflows (`.github/workflows/`) | All **placeholder/TODO** — every project needs its own CI |

## Container is ephemeral

`gh` auth, `~/.profile` (GH_TOKEN, GIT_SSH_COMMAND) are lost on rebuild. Bootstrap:

```bash
./setup.sh
```

See `.opencode/instructions/git-workflow.md` for SSH/GH_TOKEN details (always applied).

## GitHub workflows

- Pushing `feature/*`, `fix/*`, or `bugfix/*` auto-opens a PR to `development` (`open-pr-to-development.yml`)
- PRs to `main` must come from `development` (`main-pr-source.yml`)
- CI workflows are placeholders — replace with per-project lint/test commands

## Creating a new project

```bash
mkdir <project>
cd <project> && git init
# Add AGENTS.md, CI, tooling config, dependencies
# Create on GitHub, git remote add origin <url>
# Push per git-workflow.md (topic branch → PR)
```

Subfolder projects define their own commands and pre-commit hooks. Root commands are only `pre-commit install` and `./setup.sh`.
