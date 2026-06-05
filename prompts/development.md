# Development

A guided workflow for implementing an approved feature from architecture plan to testing handoff.

---

## Before You Start

You need two things before opening Claude:
1. An approved architecture note saved in `project-notes/architecture-notes.md`
2. An up-to-date CLAUDE.md

If either is missing, go back to `prompts/architecture.md` first. Starting development without an approved plan is the most expensive mistake in this playbook.

---

## Step 1 — Open the Development Session

**Goal:**
Start the session with the right context so Claude understands the project, the decision, and the scope before touching any code.

**What to do in Claude Code:**
Open a new session. Name it: `[project-name] — Development — [feature name]`

**Exact prompt to paste:**
```
Here is the project context:

[paste CLAUDE.md]

Architecture decision for this feature:

[paste the relevant entry from project-notes/architecture-notes.md]

We are implementing this feature. Before we write any code:
- Confirm you understand the feature and its behavioral contract
- List the files we will need to create or modify
- Flag anything ambiguous before we start

Do not write code yet.
```

**What answer to expect:**
A file list, a confirmation of the feature scope and behavioral contract, and any clarifying questions. If Claude starts writing code immediately, stop it — confirmation always comes before implementation.

**What to do next:**
Answer any questions. Confirm the file list looks correct. Then go to Step 2.

---

## Step 2 — Inspect Existing Files

**Goal:**
Understand what already exists before changing anything. One incorrect assumption about existing code costs more time than reading every file.

**What to do in Claude Code:**
Continue the development session.

**Exact prompt to paste:**
```
Before we write anything, inspect the files we will be working with:

[list the files from Step 1]

For each file:
- Summarize what it currently does
- Note the patterns and conventions used
- Flag anything that could be affected by our changes

Do not suggest changes yet.
```

**What answer to expect:**
A per-file summary with patterns, conventions, and potential side effects flagged. If Claude flags an unexpected dependency or conflict, resolve it before writing any code. Do not work around it.

**What to do next:**
Review the summaries. Update the file list if the inspection revealed additional files that will be affected. Then go to Step 3.

---

## Step 3 — Write the Task Brief

**Goal:**
Define the exact scope of this session before a single line is written. The task brief is the boundary — anything outside it gets flagged, not built.

**What to do in Claude Code:**
Continue the development session.

**Exact prompt to paste:**
```
Based on the architecture note and the file inspection, write a task brief for this session.

Include:
- What needs to be built (one sentence)
- Files to create or modify
- Files explicitly out of scope
- Definition of done: what does working look like?

This brief is the constraint for everything we build in this session.
```

**What answer to expect:**
A short, specific task brief — no more than 10 lines. If it is longer, the scope is too large for one session. Split it now, not mid-implementation.

**What to do next:**
Confirm or adjust the brief. Once confirmed, treat it as locked. Then go to Step 4.

---

## Step 4 — Implement Step by Step

**Goal:**
Build the feature in small, verifiable steps. One logical change at a time — never stack unverified changes.

**What to do in Claude Code:**
Continue the development session. Repeat this prompt for each step.

**Exact prompt to paste:**
```
Implement only this step:

[describe one logical change — one function, one file, one integration point]

Constraints:
- Change only what is described above
- Do not refactor or improve anything outside this step
- Do not fix bugs you notice — flag them as notes
- Show me the change before we move on

I will verify this step before we continue.
```

**What answer to expect:**
The code change for that one step, clearly presented. Nothing more. If Claude changes more than described or refactors surrounding code, call it out and ask it to revert to scope.

**What to do next:**
Review the change. If it looks correct, confirm and move to the next step using the same prompt. Repeat until the task brief is complete. Then go to Step 5.

---

## Step 5 — Scope Check

**Goal:**
Verify the session stayed inside the task brief and nothing unexpected was changed.

**What to do in Claude Code:**
Continue the development session.

**Exact prompt to paste:**
```
Before we finish, run a scope check.

Task brief: [paste the brief from Step 3]

Review everything we changed in this session and confirm:
- Every change was inside the task brief
- Nothing was refactored or restructured outside scope
- No new dependencies were added without discussion
- The behavioral contract from the architecture note is satisfied

List anything that was flagged as a note but not acted on.
```

**What answer to expect:**
A clean confirmation that the implementation matches the task brief, plus a list of flagged notes for future sessions. If anything drifted out of scope, decide now: revert it and note it for later, or explicitly approve the change before continuing.

**What to do next:**
Resolve any out-of-scope items. Then go to Step 6.

---

## Step 6 — Review All Changes

**Goal:**
Read every changed file before closing the session. Claude's review is not a substitute — you are the final check.

**What to do in Claude Code:**
Continue the development session.

**Exact prompt to paste:**
```
Produce a final review of everything changed in this session.

For each file modified:
- What changed and why
- Any hardcoded values that should not be hardcoded
- Any error cases that are not handled
- Anything you are uncertain about

Be honest. Flag anything that needs a closer look.
```

**What answer to expect:**
A file-by-file review with specific flags. Pay attention to anything Claude marks as uncertain — those are the most likely sources of bugs in testing. Resolve blockers before closing the session.

**What to do next:**
Read the actual diff yourself, not just Claude's summary. Then go to Step 7.

---

## Step 7 — Write the Testing Handoff

**Goal:**
Produce a handoff document that makes the testing session fast and systematic.

**What to do in Claude Code:**
Continue the development session.

**Exact prompt to paste:**
```
The implementation is complete. Write a testing handoff.

Include:
- What was built (2–3 sentences)
- Files created or modified
- Happy path: the main flow to verify
- Edge cases to test
- Failure cases: invalid input, missing data, error conditions
- Regression risks: what existing behavior could have been affected
- Anything flagged as uncertain during implementation

Keep it concise. This goes directly into the test session.
```

**What answer to expect:**
A structured handoff document covering all the above points. If regression risks are listed as "none," push back — every change has a blast radius, however small.

**What to do next:**
Save the testing handoff. Commit all changes with a clear commit message. Open `prompts/testing.md` and paste this handoff into Step 1.
