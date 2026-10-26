---
name: run-plan
description: Execute PLAN.md phase by phase, asking the user before each next phase - Sonnet implements, optional Opus review, local squash merge into main. Use for /run-plan or "start building", "continue the plan".
---
You are the coordinator. Stay lean: never read source files or diffs yourself. You only read `PLAN.md`, run git commands, and delegate.

Setup:
1. No `PLAN.md`? Stop and tell the user to run `/kickoff`.
2. `git switch main`. Set `autopilot: on` in PLAN.md. Commit `chore: start autopilot`.

Loop over phases in order, skipping `Status: merged`:

1. **Implement** (Status `todo` or `in-progress`): spawn the `implementer` agent with prompt `Implement Phase <n> of PLAN.md.`
   - Report `blocked`: stop the loop and show the user the BLOCKED line.
2. **Review gate** (Status `review`): run `git diff --shortstat main...<branch>`.
   - Ask the user (AskUserQuestion, one question: "Review Phase <n> with Opus before merge?") only if `Risk: high` OR more than 10 files OR more than 400 changed lines.
   - Otherwise skip the review without asking.
3. **Review** (user said yes): spawn `code-reviewer` with prompt `Review branch <branch> for Phase <n>.`
   - `NO ISSUES FOUND`: go to merge.
   - Findings: spawn `implementer` with `On branch <branch>, fix these review findings, one commit, then run build-check:` plus the findings. One fix round only. Show remaining critical findings to the user before merge.
4. **Merge**, in this order:
   ```
   git switch main
   git merge --squash <branch>
   ```
   Set the phase `Status: merged` in PLAN.md, then:
   ```
   git add -A
   git commit -m "feat: phase <n> <name>" -m "<milestone list>"
   git branch -D <branch>
   ```
   If a remote exists: `git push origin main` and `git push origin --delete <branch>`.
   Merge conflict: write `BLOCKED: merge conflict phase <n>` and stop.
5. **Next-phase gate**: if unmerged phases remain, ask the user (AskUserQuestion, one question: "Phase <n> merged. Start Phase <n+1> <name>?"). Always ask, never skip.
   - Yes: continue the loop.
   - No: set `autopilot: off` in PLAN.md, commit `chore: pause autopilot`, push if a remote exists, and stop. `/run-plan` resumes later.

Finish: when every phase is merged, set `autopilot: off`, commit `chore: plan complete`, push.

Report to the user at the end, when blocked, or when paused: one line per phase (`P<n> merged|blocked, reviewed yes|no`).
