# Planning Workflow

Goal: Produce a plan composed of:
- Tasks backlog in `backlog.md` (clear, small, prioritized tasks with acceptance criteria)
- Architecture details specification (if necessary)
- UI details specification (if necessary)

All planning artifacts go in the epic description folder; do not commit to git.

## Process

1. PO creates `<task-folder>/backlog.md`
2. PO asks @architect to review backlog and add architecture guidelines
   - Architect must produce a concrete directory tree mapping `ARCHITECTURE.md` structure to the project
   - Report saved to `<task-folder>/ARCH-YYYY-MM-DD-NNN.md`
3. If UI work needed, PO asks @ui-designer and/or @ux-designer for UI guidelines
   - Reports saved to `<task-folder>/UI-YYYY-MM-DD-NNN.md` and `<task-folder>/UX-YYYY-MM-DD-NNN.md`
4. PO asks @critic for feedback, saved to `<task-folder>/CRITIC-YYYY-MM-DD-NNN.md`
   - Address necessary issues, justify unaddressed ones
   - Terminate loop if progress stalls; ask human stakeholder for direction
   - Iterate from step 3 until no issues remain
5. PO asks human stakeholder for final approval
6. On approval, proceed to implementation: `.ai/team/workflows/IMPLEMENTATION.md`

## Backlog Rules

- Known bugs discovered during planning must have cost/benefit evaluation before marking out of scope
- Sorting tasks must specify sort key and invariant justification
- Use cases returning data to presentation must specify an application-layer DTO with named class and fields
- Refactoring tasks must include acceptance criterion: "Refactored code produces byte-identical output for same input"
- URL-building tasks must include acceptance criterion requiring URL encoding on interpolated segments
- If `composer.json` exists, include Task 0 acceptance criterion: package name is `vendor/package-name` (lowercase, hyphens)

## Post-Planning

Document lessons learned in `.ai/context/MEMORY.md`:
- Reusable Q&A from Architect, Critic, and human stakeholder interactions
