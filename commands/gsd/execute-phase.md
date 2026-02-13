---
name: gsd:execute-phase
description: Execute all plans in a phase with wave-based parallelization
argument-hint: "<phase-number> [--gaps-only]"
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
  - Task
  - TodoWrite
  - AskUserQuestion
  - Skill
---
<objective>
Execute all plans in a phase using wave-based parallel execution.

Orchestrator stays lean: discover plans, analyze dependencies, group into waves, spawn subagents, collect results. Each subagent loads the full execute-plan context and handles its own plan.

Context budget: ~15% orchestrator, 100% fresh per subagent.
</objective>

<execution_context>
@~/.claude/get-shit-done/workflows/execute-phase.md
@~/.claude/get-shit-done/references/ui-brand.md
</execution_context>

<context>
Phase: $ARGUMENTS

**Flags:**
- `--gaps-only` — Execute only gap closure plans (plans with `gap_closure: true` in frontmatter). Use after verify-work creates fix plans.

@.planning/ROADMAP.md
@.planning/STATE.md
</context>

<process>
Execute the execute-phase workflow from @~/.claude/get-shit-done/workflows/execute-phase.md end-to-end.
Preserve all workflow gates (wave execution, checkpoint handling, verification, state updates, routing).

**After verification passes, run these fork-specific steps before update_roadmap:**

**Post-verification step A: Run code simplification**

```bash
# Get files changed in this phase (all plan commits)
CHANGED_FILES=$(git diff --name-only $(git log --oneline --grep="feat(${PHASE_NUM}" --grep="fix(${PHASE_NUM}" --grep="refactor(${PHASE_NUM}" --format=%H | tail -1)^..HEAD -- '*.ts' '*.tsx' '*.js' '*.jsx' '*.py' '*.go' '*.rs' '*.swift' 2>/dev/null | head -50)
```

Spawn `code-simplifier` subagent:
```
Task(
  prompt="Review and simplify the following files changed in phase {phase_number}:

  Files: {changed_files}

  Focus on:
  - Reducing unnecessary complexity
  - Removing dead code
  - Simplifying conditionals
  - Improving readability

  Make atomic commits for each simplification with format: refactor({phase}): simplify {description}",
  subagent_type="pr-review-toolkit:code-simplifier"
)
```

Skip if no code files changed or `CHANGED_FILES` is empty.

**Post-verification step B: Update agent knowledge**

After code simplification, capture learnings from this phase:

- Invoke `/update-agent-knowledge` skill
- Review the phase execution for patterns, gotchas, and context
- Add relevant learnings to CLAUDE.md

This runs while the conversation has full context of what was built and why,
unlike at milestone completion which often runs in a fresh session.
</process>

<success_criteria>
- [ ] All incomplete plans in phase executed
- [ ] Each plan has SUMMARY.md
- [ ] Phase goal verified (must_haves checked against codebase)
- [ ] VERIFICATION.md created in phase directory
- [ ] Code simplification run on changed files (code-simplifier agent)
- [ ] CLAUDE.md updated with phase learnings (/update-agent-knowledge)
- [ ] STATE.md reflects phase completion
- [ ] ROADMAP.md updated
- [ ] REQUIREMENTS.md updated (phase requirements marked Complete)
- [ ] User informed of next steps
</success_criteria>
