# AGENTS INSTRUCTIONS OPTIMIZATION

Use this checklist when creating or revising any agent context file.

1. Keep only high-signal constraints.
    - Include only requirements that change outcomes (security, correctness, required commands, hard constraints).
    - Remove preferences that are not testable or not outcome-critical.

2. Prefer minimal instructions over broad guidance.
    - The study shows extra context often reduces success and increases cost.
    - Start with the smallest instruction set that enforces required behavior.

3. Avoid redundant repository overviews.
    - Do not restate information already easy to discover from file structure or existing docs.
    - Include only non-obvious, repo-specific facts an agent cannot infer quickly.

4. Use strict priority ordering.
    - Place constraints in execution priority order: safety -> correctness -> determinism -> style.
    - In conflicts, higher-priority rules win.

5. Write atomic, testable rules.
    - Each rule must be one action or one prohibition.
    - Prefer measurable language: "must", "must not", "only", "exact command".

6. Bound agent behavior explicitly.
    - List allowed lifecycle commands and forbidden destructive commands.
    - State when to stop, what to report, and required validation steps.

7. Do not over-constrain tool use.
    - Mention tools only when required for reproducibility or policy.
    - Avoid long tool preference lists unless they are mandatory.

8. Remove duplicated rules across files.
    - Keep one canonical location per rule.
    - In other files, reference the canonical rule briefly instead of repeating it.

9. Compress wording.
    - Use short bullets, no narrative prose, no repeated rationale.
    - Remove qualifiers that do not change action ("generally", "usually", "try to").

10. Provide defaults for ambiguity.
    - Define default decisions so the agent does not ask unnecessary questions.
    - Require questions only for destructive, security-sensitive, or credential-blocked actions.

11. Scope context to the current task type.
    - Keep always-on global rules short.
    - Put task-specific guidance in task files, not in global context.

12. Optimize for instruction-following side effects.
    - Agents usually follow written instructions; unnecessary instructions cause extra steps and cost.
    - Therefore, include only instructions worth paying for in latency and tokens.

13. Standardize output contract.
    - Require concise completion reports with: done, not done + reason, validation status, and risks.
    - Do not require verbose logs unless explicitly needed.

14. Review cycle for every instruction file update.
    - Remove one redundant rule before adding a new one.
    - Check for contradictions with other context files.
    - Re-run a short lint pass: can each rule be tested or audited?

15. No double negatives.
    - Rewrite double negatives in a way that it can not be misinterpreted
