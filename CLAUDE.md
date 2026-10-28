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

## Planning and design

- Planning is a conversation with the user, then a record. Not the other way
  round. Nobody writes a design doc and then asks.
- Order: the `planner` agent (Opus) runs in ASK mode and returns questions with
  options and a recommendation. The main session puts them to the user, a few
  at a time, and waits. Only when the user has decided, the planner runs in
  WRITE mode with the list of decisions and records exactly those.
- No agent or session invents features, numbers or "later" systems the user did
  not ask for. Start from the conventional version of a thing; anything cleverer
  is a question.
- Text for the user names things by name, never by phase, milestone or item numbers.
- If the main session is not Opus, say so before planning and ask the user to
  switch with `/model`.

## Token rules

- Run the `repo-man` skill before exploring unfamiliar code. Open only the files the map points to.
- Read file ranges, not whole large files. Do not re-read files you just edited.
- Build/test output: errors only. Formatting runs automatically after each edit; never fix style by hand.
- context7: only for third-party library APIs you are unsure about (version-specific). Not for the standard library.
- Playwright (if enabled): verify a UI milestone once at the end. Prefer snapshots over screenshots. Close the browser when done.
- Subagents return the short report format their definition gives. No prose recaps.

## Conventions

- Conventional Commits (`feat:`, `fix:`, `chore:`, `refactor:`, `test:`, `docs:`).
- Match existing code style. No new dependencies without a reason in the commit body.
- Never commit secrets or `.env` files.
