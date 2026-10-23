# Project

<!-- One paragraph: what this project is. Filled by /kickoff. -->

## Commands

<!-- Filled by planner. Build-check runs scripts/check.sh if present, else auto-detects the stack. -->
- dev:
- test:
- check: `.claude/skills/build-check/check.sh`

## Workflow

- `PLAN.md` is the source of truth: phases, milestones, status. Keep it current.
- Planning: Opus (`planner` agent). Coding: Sonnet (`implementer` agent). Review: Opus (`code-reviewer` agent), only when the user agrees.
- One branch per phase: `feat/p<N>-<slug>`. One commit per milestone. Push after each commit if a remote exists.
- Merge into `main` only with a local squash merge, after the optional review.
- Use `/kickoff` for a new project and `/run-plan` to execute PLAN.md.
- Need the user? Write `BLOCKED: <reason>` in PLAN.md and stop.

## Token rules

- Run the `repo-man` skill before exploring unfamiliar code. Open only the files the map points to.
- Read file ranges, not whole large files. Do not re-read files you just edited.
- Build/test output: errors only.
- Subagents return the short report format their definition gives. No prose recaps.

## Conventions

- Conventional Commits (`feat:`, `fix:`, `chore:`, `refactor:`, `test:`, `docs:`).
- Match existing code style. No new dependencies without a reason in the commit body.
- Never commit secrets or `.env` files.
