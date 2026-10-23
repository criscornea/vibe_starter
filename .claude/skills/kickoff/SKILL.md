---
name: kickoff
description: Start a new project - clarify the idea, pick the stack, create PLAN.md and fill CLAUDE.md. Use for /kickoff, "new project", "plan this project".
---
1. If the user gave no idea yet, ask for it in one sentence.
2. Ask at most 4 questions with AskUserQuestion, only where the answer changes the plan:
   stack (Go, TypeScript with Vue or React, PHP with Symfony, other), target (CLI, web app, API, game), persistence, deploy target.
   Skip questions the idea already answers.
3. Write the project paragraph at the top of `CLAUDE.md`.
4. Spawn the `planner` agent with the idea, the answers, and `Create PLAN.md.`
5. Show the user the phase names and milestone counts from PLAN.md (not the whole file). Ask: "Start building with /run-plan?"
6. Commit `PLAN.md` and `CLAUDE.md` on main: `docs: project plan`.
