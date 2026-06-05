# Bug Fixing

A guided workflow for diagnosing and fixing a bug from report to verified resolution.

---

## Before You Start

Write a bug report before opening Claude. Include:
- Steps to reproduce
- Observed behavior
- Expected behavior
- Any relevant file names or error messages

The act of writing it often reveals the cause. If it does, you still need the report — it becomes the testing handoff input.

If you cannot reproduce the bug reliably, do not proceed. A bug you cannot reproduce cannot be reliably fixed.

---

## Step 1 — Open the Diagnosis Session

**Goal:**
Establish shared understanding of the bug before attempting any analysis. Claude needs the full picture before it can reason about the cause.

**What to do in Claude Code:**
Open a new session. Name it: `[project-name] — Bug Fix — [short bug description]`
Paste CLAUDE.md at the top of the session.

**Exact prompt to paste:**
```
Here is the project context:

[paste CLAUDE.md]

I have a bug to investigate. Here is the report:

Steps to reproduce:
[list steps]

Observed behavior:
[what actually happens]

Expected behavior:
[what should happen]

Relevant files or error messages:
[paste or list]

Do not suggest a fix yet. Confirm you can trace the failure from these steps and identify which part of the system is involved.
```

**What answer to expect:**
Claude traces the failure path from the reproduction steps and identifies the part of the system involved. It should not propose a fix yet. If Claude immediately jumps to a solution, redirect it — tracing before fixing.

**What to do next:**
Confirm Claude's trace matches your understanding. Then go to Step 2.

---

## Step 2 — Root Cause Analysis

**Goal:**
Identify the exact origin of the bug — not the symptom, the cause. The fix is only as good as the diagnosis.

**What to do in Claude Code:**
Continue the Bug Fix session.

**Exact prompt to paste:**
```
Based on the bug report and trace, analyze the root cause.

I want to know:
- The exact location where the failure originates (file, function, line if possible)
- Why that code produces the observed behavior
- Whether this is a logic error, a data problem, a missing check, or something else

Do not propose a fix yet. Explain what is wrong and why.
```

**What answer to expect:**
A specific diagnosis: file, function, and reason. Not "the bug is in the auth module" — "the token expiry check in `auth/validate.py` line 42 compares timestamps in different timezones, producing a false negative." Vague answers are not root causes.

**What to do next:**

**→ If the root cause is clear:** Challenge it before accepting it. Go to Step 3.

**→ If the root cause is unclear:** Paste this follow-up and stay in Step 2:
```
The root cause is not clear enough to act on. Go deeper.

- What is the earliest point in the execution where the failure can be observed?
- What assumption does the code make that is violated in this scenario?
- What would have to be true for this NOT to be a bug?
```
Repeat until the diagnosis is specific enough to write one sentence describing exactly what is wrong and why.

---

## Step 3 — Challenge the Diagnosis

**Goal:**
Verify the root cause before committing to a fix. Claude's first diagnosis is the most obvious one — not always the correct one.

**What to do in Claude Code:**
Continue the Bug Fix session.

**Exact prompt to paste:**
```
You identified [paste root cause summary] as the cause.

Challenge your own diagnosis:
- What evidence supports this conclusion?
- What evidence could contradict it?
- Is there another location in the code where this failure could originate?
- Is this the root cause or a symptom of something deeper?
```

**What answer to expect:**
Either a reinforced diagnosis with supporting evidence, or a revised diagnosis that goes one level deeper. If Claude's challenge produces a different root cause, treat the new one as the working hypothesis and repeat Step 3 once more.

**What to do next:**
When you agree with the diagnosis, write it down in one sentence. This becomes the anchor for all remaining steps. Then go to Step 4.

---

## Step 4 — Risk Assessment

**Goal:**
Understand the blast radius of the fix before writing a single line. Every change has side effects — find them now, not in production.

**What to do in Claude Code:**
Continue the Bug Fix session.

**Exact prompt to paste:**
```
Root cause confirmed: [paste your one-sentence diagnosis]

The fix will require changing: [name the file, function, or logic]

Assess the risk:
- What other code depends on this file or function?
- What existing behavior could change as a side effect?
- What regression risks should I test for after the fix?
- Is the blast radius narrow (isolated) or broad (shared logic)?
```

**What answer to expect:**
A specific list of dependencies, potential side effects, and regression candidates. If Claude says the blast radius is narrow, ask it to name one thing that would break if it were wrong — vague reassurance is not a risk assessment.

**What to do next:**
Review the risk list. Then go to Step 5.

---

## Step 5 — Approve the Fix Approach

**Goal:**
Agree on what will change and why before any code is written. Changing a sentence is cheap. Reverting an implementation is not.

**What to do in Claude Code:**
Continue the Bug Fix session.

**Exact prompt to paste:**
```
Root cause: [paste diagnosis]
Risk summary: [paste brief risk summary]

Propose the fix approach — not the code, the approach.

Describe in 2–3 sentences:
- What needs to change
- Where the change goes
- Why this is the correct fix and not a workaround

I will approve this before any code is written.
```

**What answer to expect:**
A concise fix description naming the exact change, location, and rationale. It should not include code. If it does, ask Claude to describe the approach in plain language first.

**What to do next:**

**→ If the fix is a code change:** Review the approach. If it looks correct, confirm it explicitly and go to Step 6.

**→ If the fix requires an architecture change:** Stop here. This is a design flaw, not a code bug. Paste this:
```
This fix requires changing the architecture. We will not patch this as a bug.

Summarize:
- What design decision caused this bug
- What would need to change architecturally to fix it properly
- A short-term workaround if one exists that does not add risk
```
Record the summary and route to `prompts/architecture.md`. Do not proceed to Step 6.

---

## Step 6 — Implement the Fix

**Goal:**
Apply the approved change — narrow, focused, nothing more.

**What to do in Claude Code:**
Continue the Bug Fix session.

**Exact prompt to paste:**
```
Fix approach approved: [paste approved approach]

Implement only this fix.

Constraints:
- Change only what is described in the approved approach
- Do not refactor or improve surrounding code
- Do not fix other issues you notice — flag them as notes
- Show me the change before finalizing

Flag anything unexpected you encounter while implementing.
```

**What answer to expect:**
The code change for the approved fix, clearly presented. Nothing more. If Claude changes more than described, or refactors surrounding code, call it out and ask it to revert to scope.

**What to do next:**
Review the change carefully. Check that it matches the approved approach exactly. If anything looks different from what was approved, ask Claude to explain the deviation before accepting it. Then go to Step 7.

---

## Step 7 — Write the Testing Handoff

**Goal:**
Produce a handoff document that makes the verification session fast and complete.

**What to do in Claude Code:**
Continue the Bug Fix session.

**Exact prompt to paste:**
```
The fix has been implemented.

Write a testing handoff with:
- What was broken and what caused it (one-sentence root cause)
- What was changed to fix it
- How to verify the fix: exact steps, inputs, expected outcome
- Regression risks from the risk assessment that need to be checked
- Anything flagged as uncertain during implementation

Keep it concise. This goes directly into the test session.
```

**What answer to expect:**
A structured handoff covering all the above points. If regression risks are listed as "none," push back — every fix has a blast radius however narrow, and the risk assessment in Step 4 already identified candidates.

**What to do next:**
Save the handoff. Commit the fix with a clear message referencing the bug. Open `prompts/testing.md` and paste this handoff into Step 1.

---

**If testing passes:**
The fix is verified. Record the bug, root cause, and fix in `lessons/index.md` under the Bug Fixing category. Note what allowed the bug to exist — missing test, unclear spec, gap in CLAUDE.md — and fix the gap.

**If testing fails:**
Copy the failure report from `prompts/testing.md` Step 7. Return to this workflow at Step 2 — the root cause diagnosis was incomplete. Paste the failure report as additional evidence and rediagnose. Do not re-use the previous root cause as the starting point.
