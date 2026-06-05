# Refactoring

## 1. Purpose

A workflow for improving code structure without changing behavior. The core discipline: behavior is frozen during a refactor. No new features, no bug fixes, no design changes — only structural improvement. Claude will drift toward improvement if left unconstrained; this workflow holds the line. Output is cleaner code that does exactly what it did before, verified by regression testing.

---

## 2. When to Use

- After a feature is working and tested and the code is hard to read or maintain
- When patterns across the codebase have become inconsistent
- When preparing for a larger feature that the current structure would make difficult
- When a development or bug-fix session flagged structural issues but stayed in scope

**Do not use** when behavior also needs to change. If the refactor requires changing what the code does — even slightly — split it: refactor first to a clean structure, then build the change on top. Mixing both in one pass makes regression testing unreliable and rollbacks painful.

---

## 3. Step-by-Step Workflow

**Step 1 — Establish a regression baseline**
Confirm that existing behavior is tested and passing before touching a single line. If tests do not exist, write them first. A refactor without a baseline is indistinguishable from a bug.

**Step 2 — Classify the refactor**
Small/local: contained to one file or function, low blast radius, can proceed directly.
Large/structural: spans multiple files, changes interfaces or patterns, requires architecture review first.

**Step 3 — Architecture review for large refactors**
Before implementing a large refactor, review the current structure and confirm the target structure is an improvement — not a lateral move. Use `architecture.md`. Do not skip this for large changes.

**Step 4 — Write the refactor brief**
Define exactly what improves and what must not change. Name the files in scope. State the behavioral contract: inputs, outputs, and side effects stay identical.

**Step 5 — Implement in small incremental steps**
One logical improvement at a time. Rename a function. Extract a class. Flatten a conditional. After each step: run regression tests, confirm behavior is intact, commit.

**Step 6 — Run regression tests after every step**
Not at the end — after each step. This is the mechanism that makes small steps safe. If something breaks, you know exactly which step caused it.

**Step 7 — Assess rollback at each commit**
Each commit should leave the codebase in a working state. If a step cannot be committed in isolation without breaking something, the step is too large — split it further.

---

## 4. Required Sessions

| Session | Purpose | When |
|---|---|---|
| Classification | Determine scope, classify as small or large | Before any work |
| Architecture review | Validate target structure for large refactors | After classification, large refactors only |
| Implementation | Incremental structural improvements | After brief is written and approved |
| Regression check | Verify behavior is intact across all changes | After each step and at completion |

For small refactors, classification and implementation can share a session. Large refactors always require a separate architecture review session.

---

## 5. Required Documents

| Document | Description | When Created |
|---|---|---|
| Refactor brief | What improves, what files are in scope, behavioral contract | Before implementation |
| Regression baseline | Passing tests confirming current behavior | Before first change |
| Architecture note | Target structure decision and rationale | Large refactors only, before implementation |
| Rollback log | List of commits, each leaving the codebase in a working state | Maintained throughout |

---

## 6. Copy/Paste Prompts

**Classify the refactor:**
```
I want to refactor [describe area of code].

Help me classify this before we start:
- What is the scope? (files, functions, interfaces affected)
- Is this local (contained to one area) or structural (spans multiple files or changes patterns)?
- What is the blast radius if something goes wrong?
- Are there existing tests that cover this code?

Do not suggest improvements yet. I want to understand the scope first.
```

**Architecture review for large refactors:**
```
I'm planning a structural refactor.

Current structure:
[describe or paste current structure]

Proposed target structure:
[describe what you want it to look like]

Review this before I start:
- Is the target structure a clear improvement or a lateral move?
- What does it make easier?
- What does it make harder or break?
- Are there dependencies or callers I haven't accounted for?
```

**Write the refactor brief:**
```
Based on our review, write a refactor brief for this work.

Include:
- What structural improvement is being made
- Files in scope
- Files explicitly out of scope
- Behavioral contract: what inputs, outputs, and side effects must remain identical
- Definition of done

This brief is the constraint for the entire refactor. Nothing outside it gets changed.
```

**Incremental implementation:**
```
Implement only this step of the refactor:
[describe one logical structural change]

Constraints:
- Change only what is described
- Do not fix bugs you notice
- Do not add features
- Do not improve anything outside the stated step
- Behavior must be identical before and after

Show me the change. I will run regression tests before we continue.
```

**Regression check:**
```
I've completed [describe step].

Review what was changed and confirm:
- Is the behavioral contract still intact?
- Are there any inputs or code paths that now behave differently?
- Is there anything that callers of the changed code would notice?

I am about to run regression tests. Flag anything you're uncertain about before I do.
```

**Rollback assessment:**
```
We are at this point in the refactor:
[describe completed steps and current state]

Assess rollback posture:
- Is the codebase in a working state right now?
- If we stopped here, would anything be broken or half-done?
- What is the safest rollback point if the next step goes wrong?
```

---

## 7. Common Mistakes

- **Mixing refactoring with feature work.** The moment behavior changes, you are no longer refactoring. Regression tests become invalid and rollback becomes complicated. Keep the two strictly separate.

- **No regression baseline before starting.** Without passing tests before the first change, you cannot tell whether a failure came from the refactor or was already there.

- **Large sweeping changes instead of small steps.** Claude can rewrite an entire module in one response. The result may be structurally better and behaviorally different in subtle ways. Small steps are the only reliable way to catch this.

- **Skipping architecture review for large refactors.** A large refactor without a validated target structure risks trading one set of problems for another. The review is what confirms the destination is worth going to.

- **No rollback plan.** A refactor that cannot be undone is a risk that should have been a smaller step. Every commit should be a safe stopping point.

- **Fixing bugs during a refactor.** If you find a bug, note it and continue. Fix it in a separate session via `bug-fixing.md`. Fixing bugs during a refactor makes it impossible to attribute regressions correctly.

- **"I'll clean it up as I go."** Undefined scope is not a refactor brief. Without a written constraint, every improvement looks justified and the session never ends.

---

## 8. Shahar Best Practices

- The behavioral contract is the most important line in the refactor brief. Write it before anything else: "Given the same inputs, the code produces the same outputs and the same side effects." If this cannot be written, the refactor is not ready to start.
- Commit after every step, even if the step feels trivial. The commit log is your rollback map. A day of small commits is infinitely safer than one large commit at the end.
- When Claude suggests an improvement outside the current step, paste it into the "Refactor later" notes doc from `development.md`. Do not act on it. The list grows — that is fine. Scope creep is not.
- Run regression tests yourself, not via Claude. Claude cannot run your test suite. Do not ask it to confirm behavior — confirm it with actual test output.
- Before starting any large refactor, set a time budget. If the refactor takes longer than planned, it is either larger than classified or the steps are too big. Both are signals to stop and reassess.
- After a large refactor is complete, update CLAUDE.md with the new structural conventions. The next session should start with the new reality, not the old one.
