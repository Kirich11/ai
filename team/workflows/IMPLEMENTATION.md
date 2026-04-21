# Implementation Workflow

Starts only after the human stakeholder approves the plan from planning stage.

Temporary collaboration artifacts go in the epic description folder; do not commit to git.

If the developer needs:
- Specifications clarification: ask PO
- Technical guidance: ask architect first, then human stakeholder if inconclusive

## Process

1. PO provides the plan (`backlog.md` and any architecture/UI specifications) to @developer
2. PO must invoke Tester, Developer, and Reviewer as separate subagents — never merge into one
3. Limit each developer delegation to at most 3 tasks
4. Include concrete class signatures, constructor parameters, file paths, and dependency details when delegating
5. All code changes — including trivial review fixes — must be delegated to the Developer
6. For each task:
   1. PO asks @tester to:
      - Create tests for each acceptance criteria and any additional tests deemed necessary
      - Save session report to task folder
      - Notify PO: "FINISHED!" + test report
   2. PO asks @developer to implement the task following the plan
      - Tests must pass before task is considered complete
      - Notify PO: "FINISHED!"
   3. PO asks @reviewer for a review
      - Save Review Report to task folder
      - Notify PO: "FINISHED!"
   4. PO asks @developer to address review issues; iterate from 6.3 until resolved
   5. Developer creates one commit per task before starting the next:
      - Use `git add <specific-files>` — never `git add -A` or `git add .` from repo root
      - Verify `git diff --staged --stat` before committing
   6. PO marks task done (strikethrough title in `backlog.md`)
   7. PO appends reusable patterns to `.ai/context/MEMORY.md` (do not defer to end of session)
   8. PO moves to next task from 6.1
7. When all tasks done, PO:
   1. Verifies test coverage has not regressed; if it has, creates follow-up task
   2. Approves or rejects
   3. Updates `.ai/context/MEMORY.md` with lessons learned
   4. Notifies human stakeholder with completion report and recommended next actions
   5. Proceeds with non-destructive closure actions without asking permission

## Handling Stalled Sessions

- If a developer session stalls or produces no usable output:
  1. Inspect working directory for partial progress
  2. Re-delegate only incomplete work to a fresh session with explicit specifications
  3. Never re-delegate already-completed work
- PO may terminate early if requirements change or task is deprioritized
