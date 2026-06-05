# Development

## 1. Purpose

A workflow for implementing approved work with Claude. Architecture is decided, product decision is locked — this is execution. The discipline here is staying in the lane: build what was agreed, in small safe steps, without redesigning along the way. Output is working code and a clean handoff for testing.

---

## 2. When to Use

- Implementing a feature after architecture and product decisions are finalized
- Building out a task from a defined spec or ticket
- Continuing work from a previous session with a clear next step

**Do not use** if the architecture is still open or the product decision is unclear. Go back to `architecture.md` first. Starting development on an unresolved design is the most expensive mistake in this playbook.

---

## 3. Step-by-Step Workflow

**Step 1 — Confirm the work is approved**
Before opening Claude, verify: architecture note exists, product decision is written, CLAUDE.md is current. If any of these are missing, stop.

**Step 2 — Inspect relevant files before writing any code**
Read every file that will be touched. Ask Claude to summarize what each one does and how they connect. Do not write a single line until this is done.

**Step 3 — Write a task brief**
One short paragraph: what needs to change, in which files, and what done looks like. This is your scope boundary for the session.

**Step 4 — Implement in small steps**
One logical change at a time. After each step, verify the code compiles or runs before moving on. Never stack multiple unverified changes.

**Step 5 — Stay in scope**
If Claude suggests an improvement, refactor, or structural change outside the task brief — note it, do not act on it. Use `refactoring.md` later.

**Step 6 — Review before finishing**
Read every changed file before closing the session. Check for unintended side effects, hardcoded values, or logic that doesn't match the product decision.

**Step 7 — Create testing handoff**
Write a short note describing what was built, what to test, and any known edge cases. This goes to `testing.md` or directly into the test session.

---

## 4. Required Sessions

| Session | Purpose | When |
|---|---|---|
| File inspection | Understand what exists before changing it | Start of every development session |
| Implementation | Build the approved change in small steps | After inspection |
| Review | Read all changed files, catch drift | Before closing the session |

Keep each development session focused on one task. Multiple tasks in one session produce tangled diffs and hard-to-trace bugs.

---

## 5. Required Documents

| Document | Description | When Used |
|---|---|---|
| Task brief | What to build, which files, what done looks like | Written before first keystroke |
| CLAUDE.md | Project context — must be current before starting | Attached at session start |
| Architecture note | The approved design this work implements | Referenced during implementation |
| Testing handoff | What was built, what to test, edge cases | Written at session end |

---

## 6. Copy/Paste Prompts

**File inspection:**
```
Before we write any code, I need to understand what already exists.

Here are the files we'll be working with:
[list file paths]

For each file:
- Summarize what it does
- Note any patterns, conventions, or constraints I should follow
- Flag anything that could be affected by the change we're about to make

Do not suggest changes yet.
```

**Open a development session:**
```
Here is the context for this session:

[paste CLAUDE.md or relevant section]

Architecture decision:
[paste architecture note]

Task brief:
[paste task brief]

We are implementing exactly this. If you see something that should be improved but is outside this scope, flag it as a note — do not act on it.

Start by confirming you understand the task and the boundary.
```

**Small step implementation:**
```
Implement only this step:
[describe one logical change]

Files to change:
[list specific files]

Do not change anything outside these files. Do not refactor while implementing. Show me the change and explain it before moving on.
```

**Scope check:**
```
Before we continue — are we still inside the task brief?

Task brief: [paste brief]

Review what has been changed so far and confirm:
- Everything changed was in scope
- Nothing was refactored or restructured unexpectedly
- We are on track to finish the task as defined
```

**Testing handoff:**
```
We've finished implementing [feature/task name].

Write a testing handoff with:
- Summary of what was built (2–3 sentences)
- Files changed and what changed in each
- What to test: happy path, edge cases, failure cases
- Anything I'm unsure about or that needs closer review

Keep it short. This goes directly into the test session.
```

---

## 7. Common Mistakes

- **Skipping file inspection.** Writing code without reading what exists leads to duplicated logic, broken conventions, and bugs that take longer to find than the feature took to build.

- **Letting Claude refactor while implementing.** Claude will improve things it notices along the way. Each unsolicited change is an unreviewed change. Scope creep in code is invisible until it breaks something.

- **Multiple tasks in one session.** The session loses focus, the diff becomes large, and tracing a bug means untangling two features at once.

- **Not writing the task brief before starting.** Without a written scope boundary, "done" is undefined and the session drifts until you run out of context or patience.

- **Stacking unverified changes.** Implementing step 2 before verifying step 1 means when something breaks, you don't know which step caused it.

- **Skipping the review step.** Claude occasionally introduces hardcoded values, changes a function signature silently, or adds an import that doesn't belong. A five-minute read catches all of this.

- **Not writing the testing handoff.** Opening a test session cold without a handoff means reconstructing what was built from the code. Slow and error-prone.

---

## 8. Shahar Best Practices

- Write the task brief before opening Claude, not inside the session. Thinking and implementing are separate modes — mixing them degrades both.
- One session per task. If a task feels too big for one session, split the task, not the session discipline.
- When Claude flags something out of scope, paste it into a running notes doc titled "Refactor later." Keeps the session clean without losing the observation.
- Commit after each completed step, not after the full task. Small commits mean small rollbacks. `git commit` is a save point, not a ceremony.
- If you hit something unexpected mid-implementation — a file that doesn't match the architecture, a pattern that contradicts CLAUDE.md — stop. Do not work around it. Fix the context first.
- Read the diff before every commit. Not Claude's summary of the diff — the actual diff. You are the last line of review.
