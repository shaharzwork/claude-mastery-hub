# Refactoring

A guided workflow for improving code structure without changing behavior.

---

## Before You Start

Write one sentence describing the structural problem and one sentence describing the intended improvement. No behavior changes — only structural ones.

Examples of valid triggers:
- A function does three things and should do one
- Two modules share logic that belongs in a shared utility
- A pattern used in one place is inconsistent with the rest of the codebase

If improving the structure also requires changing what the code does, stop. That is a feature change. Go to `prompts/architecture.md` instead.

---

## Step 1 — Open the Refactoring Session

**Goal:**
Establish the structural problem and the intended improvement before analyzing anything. Claude needs a clear frame before it can assess impact.

**What to do in Claude Code:**
Open a new session. Name it: `[project-name] — Refactor — [short description]`
Paste CLAUDE.md at the top of the session.

**Exact prompt to paste:**
```
Here is the project context:

[paste CLAUDE.md]

I want to refactor the following:

Structural problem:
[one sentence describing what is wrong with the current structure]

Intended improvement:
[one sentence describing what the structure should look like after]

Before we do anything:
- Confirm you understand the structural change I'm describing
- Ask any clarifying questions before we assess scope

Do not suggest implementation yet.
```

**What answer to expect:**
Claude confirms the structural problem and the intended improvement in its own words, and asks any clarifying questions. If Claude's restatement does not match your intent, correct it before continuing — a misunderstood refactor produces the wrong structure.

**What to do next:**
Resolve any questions. Then go to Step 2.

---

## Step 2 — Verify No Behavior Change

**Goal:**
Lock the behavioral contract before touching any code. Behavior is frozen for the entire refactor. If anything needs to change functionally, it is not handled here.

**What to do in Claude Code:**
Continue the Refactor session.

**Exact prompt to paste:**
```
Before we proceed, confirm the behavioral contract.

Given the structural change I described, answer:
- What inputs, outputs, and side effects must remain identical after the refactor?
- Is there any scenario where this structural change would alter observable behavior?
- Is there anything I described that is actually a functional change rather than a structural one?
```

**What answer to expect:**
A clear behavioral contract — what stays identical — and an honest answer about whether any part of the described change is functional rather than structural. If Claude identifies a functional change mixed into the refactor, treat that as a decision point.

**What to do next:**

**→ If behavior is fully frozen:** Write the behavioral contract in one sentence. Keep it for Step 5. Go to Step 3.

**→ If a behavior change is mixed in:** The scope is not a pure refactor. Paste this:
```
Part of what I described changes behavior, not just structure.

Separate the two:
1. What can be refactored now without changing behavior
2. What requires a product or architecture decision first
```
Handle the structural part here. Route the functional part to `prompts/architecture.md` separately. Do not mix them.

---

## Step 3 — Establish the Regression Baseline

**Goal:**
Confirm existing behavior is tested and passing before any code is touched. A refactor without a baseline is indistinguishable from a bug.

**What to do in Claude Code:**
Continue the Refactor session.

**Exact prompt to paste:**
```
I need to establish a regression baseline before we start.

List the existing tests that cover the code we are about to refactor:
[paste relevant test file names or describe the test coverage]

For any logic that is not covered by tests:
- Identify it specifically
- Describe what behavior it produces that I need to verify manually

I will confirm tests are passing before we write a single line.
```

**What answer to expect:**
A list of tests that cover the affected code, and a specific list of any untested behavior that needs manual verification. If coverage is thin, Claude should say so explicitly — do not accept "tests should cover this."

**What to do next:**
Run the identified tests and confirm they pass. Note any gaps that require manual verification. Do not proceed until you have a confirmed passing baseline. Then go to Step 4.

---

## Step 4 — Analyze Impact

**Goal:**
Understand the blast radius of the structural change before approving scope. Some refactors look local but touch shared logic used in many places.

**What to do in Claude Code:**
Continue the Refactor session.

**Exact prompt to paste:**
```
Analyze the impact of this refactor.

Files and functions involved in the change:
[list them or paste relevant sections]

Tell me:
- What other files import or depend on the code being changed?
- What callers will be affected by a structural change here?
- Is the blast radius narrow (isolated to these files) or broad (shared utilities, public interfaces)?
- Is there anything about this refactor that would require coordinated changes across multiple areas?
```

**What answer to expect:**
A specific impact map: which files are affected, which callers need updating, and a clear narrow vs. broad blast radius assessment. If Claude assesses the blast radius as broad, treat that as a decision point.

**What to do next:**

**→ If the blast radius is narrow:** Go to Step 5.

**→ If the blast radius is broad:** This refactor carries architectural risk. Paste this:
```
The blast radius is broad. Before I approve this refactor, assess:
- Is this refactor worth the coordination cost right now?
- Would it be safer to isolate it behind an interface first?
- What is the minimum viable version of this refactor that carries lower risk?
```
If the risk cannot be reduced to a manageable scope, stop here and route to `prompts/architecture.md` to treat it as a structural design decision. Do not proceed with a high-risk refactor without an architecture review.

---

## Step 5 — Approve the Refactoring Scope

**Goal:**
Define exactly what will change and what will not, before implementation starts. This brief is the boundary for the entire refactor.

**What to do in Claude Code:**
Continue the Refactor session.

**Exact prompt to paste:**
```
Write a refactoring brief for this session.

Include:
- Structural improvement being made (one sentence)
- Files in scope
- Files explicitly out of scope
- Behavioral contract: [paste the one sentence from Step 2]
- Definition of done: what does the refactored code look like?

This brief is the constraint for everything we change. Nothing outside it gets touched.
```

**What answer to expect:**
A short, specific brief — under 15 lines. If it is longer, the scope is too large for one session. Split it now: identify the first discrete improvement, scope the brief to that, and treat the rest as a separate future refactor.

**What to do next:**
Confirm or adjust the brief. Once confirmed, treat it as locked. Then go to Step 6.

---

## Step 6 — Implement Step by Step

**Goal:**
Apply the refactor in small, individually verifiable steps. One logical improvement at a time — commit after each one.

**What to do in Claude Code:**
Continue the Refactor session. Repeat this prompt for each step.

**Exact prompt to paste:**
```
Implement only this step of the refactor:

[describe one logical structural change — rename a function, extract a class, flatten a conditional, move a utility]

Constraints:
- Change only what is described above
- Do not fix bugs you notice
- Do not improve anything outside this step
- Do not change behavior — inputs, outputs, and side effects stay identical
- Show me the change before we move on

I will run regression tests before we continue to the next step.
```

**What answer to expect:**
The code change for that single structural step, clearly presented. Nothing more. If Claude changes more than described, or fixes a bug it noticed, call it out — note the observation and continue in scope.

**What to do next:**
Review the change. Run regression tests. If tests pass, commit and move to the next step using the same prompt. If tests fail, stop — do not continue the refactor. Go to `prompts/bug-fixing.md` with the failing test as the bug report.

Repeat until the refactoring brief is complete. Then go to Step 7.

---

## Step 7 — Write the Testing Handoff

**Goal:**
Produce a handoff that makes the verification session complete and systematic.

**What to do in Claude Code:**
Continue the Refactor session.

**Exact prompt to paste:**
```
The refactor is complete. Write a testing handoff.

Include:
- What structural change was made (2–3 sentences)
- Files changed and what changed in each
- Behavioral contract confirmed: [paste from Step 2]
- Regression tests to run: what tests cover the changed code
- Manual verification needed: any behavior not covered by automated tests
- Anything flagged as a note during implementation that was not acted on

Keep it concise. This goes directly into the test session.
```

**What answer to expect:**
A structured handoff covering all the above. Pay particular attention to anything flagged as a note during implementation — these are candidates for future sessions, not loose ends left in the code.

**What to do next:**
Save the handoff. Commit all changes with a message that describes the structural improvement, not the implementation details. Open `prompts/testing.md` and paste this handoff into Step 1.

---

**If testing passes:**
The refactor is verified. Update CLAUDE.md if the refactor changed any structural conventions, naming patterns, or file organization. Record any useful observations in `lessons/index.md` under the Refactoring category.

**If testing fails:**
The refactor changed behavior — even if unintentionally. Do not attempt to fix it inside the refactor session. Copy the failing test details and go to `prompts/bug-fixing.md`. Treat the regression as a bug introduced by the refactor and diagnose it from scratch.
