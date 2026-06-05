# Testing

A guided workflow for verifying a feature or bug fix from handoff to approval decision.

---

## Before You Start

You need a testing handoff from either `prompts/development.md` or `prompts/bug-fixing.md` before opening Claude.

If no handoff exists, go back and produce one first. Testing without a handoff means guessing what was built — results will be incomplete and unreliable.

---

## Step 1 — Review the Handoff

**Goal:**
Fully understand what was built or fixed before generating a single test case. Misreading the handoff produces tests that miss the point.

**What to do in Claude Code:**
Open a new session. Name it: `[project-name] — Testing — [feature or fix name]`
Paste CLAUDE.md and the testing handoff at the top.

**Exact prompt to paste:**
```
Here is the project context:

[paste CLAUDE.md]

Here is the testing handoff:

[paste handoff]

Before we create test cases, confirm your understanding:
- What was built or fixed
- What the expected behavior is
- Which files were changed
- What was flagged as uncertain or risky during implementation

Flag anything in the handoff that is ambiguous or missing before we continue.
```

**What answer to expect:**
A clear summary of what was built, the expected behavior, and any gaps or ambiguities in the handoff. If Claude flags a gap, resolve it before generating test cases — do not test against an unclear specification.

**What to do next:**
Resolve all flagged ambiguities. Then go to Step 2.

---

## Step 2 — Generate Test Cases

**Goal:**
Produce a complete list of test cases covering all expected behavior before running anything.

**What to do in Claude Code:**
Continue the testing session.

**Exact prompt to paste:**
```
Generate a full set of test cases for this change.

Cover three categories:

1. Happy path — expected inputs producing expected outputs
2. Edge cases — unusual but valid inputs or states
3. Failure cases — invalid inputs, missing data, API errors, timeouts

For each test case:
- ID (T01, T02, ...)
- Category
- Input or condition
- Expected output or behavior
- How to verify it

Do not run anything yet. I will review the list first.
```

**What answer to expect:**
A numbered test case list with all three categories represented. Review it before running anything — add cases that Claude cannot know about from context alone (e.g. known edge cases in your system, previous bug patterns).

**What to do next:**
Review and supplement the list. Then go to Step 3.

---

## Step 3 — Identify Edge Cases

**Goal:**
Go deeper than the obvious inputs. Surface hidden assumptions in the implementation that the standard test cases would not catch.

**What to do in Claude Code:**
Continue the testing session.

**Exact prompt to paste:**
```
Look at this implementation and think adversarially.

[paste the relevant changed code or describe the feature in detail]

What inputs, states, or sequences of events would expose a hidden assumption or cause unexpected behavior?

Focus specifically on things the happy path and standard edge cases would not catch:
- Boundary values
- Concurrent or sequential operations
- Missing or null data in unexpected places
- Behavior that depends on external state or timing

Add any new cases to the test list with ID and category.
```

**What answer to expect:**
A short list of additional adversarial test cases that go beyond the obvious. If Claude produces nothing new, ask it to focus specifically on boundary conditions and state-dependent behavior.

**What to do next:**
Add the new cases to the master test list. Then go to Step 4.

---

## Step 4 — Assess Regression Risks

**Goal:**
Identify what existing behavior could have been broken by this change, beyond what the feature tests cover.

**What to do in Claude Code:**
Continue the testing session.

**Exact prompt to paste:**
```
These files were changed in this implementation:

[list changed files from the handoff]

For each file:
- What existing behavior depends on the changed code?
- What other parts of the system call or import this file?
- What could break that the current test cases do not cover?

Add regression test cases to the list where needed. If you believe there are no regression risks, explain why — do not just state it.
```

**What answer to expect:**
A specific list of regression risks with supporting reasoning, plus additional test cases for anything not already covered. "No regression risks" is rarely correct — push back if Claude states it without explanation.

**What to do next:**
Add regression cases to the master test list. Then go to Step 5.

---

## Step 5 — Run the Tests

**Goal:**
Execute every test case, document each result, and surface failures before reaching the approval decision.

**What to do in Claude Code:**
Run the tests yourself — Claude cannot execute your code. Use this prompt to prepare and document results.

**Exact prompt to paste:**
```
Here is the complete test list:

[paste full test case list]

I am going to run these tests now. Produce a results table I can fill in:

| ID | Description | Expected | Actual | Status |
|----|-------------|----------|--------|--------|

Leave the Actual and Status columns blank. I will fill them in as I run each case.
```

**What answer to expect:**
A clean results table with all test case IDs and descriptions pre-filled. Run every test case and record the actual result and status (Pass / Fail / Blocked) for each one.

**What to do next:**
If all cases pass: go to Step 6.
If any case fails: note the failing IDs and continue to Step 6 anyway — complete the full run before making a decision.

---

## Step 6 — Complete the Validation Checklist

**Goal:**
Verify correctness, conventions, and quality beyond functional behavior.

**What to do in Claude Code:**
Continue the testing session. Paste the completed results table.

**Exact prompt to paste:**
```
Testing is complete. Here are the results:

[paste completed results table]

Now run through the validation checklist. Give a clear Yes / No for each item:

1. Does the implementation match the behavioral contract in the architecture note?
2. Does it follow the conventions in CLAUDE.md?
3. Does it handle errors and bad input correctly?
4. Are there any hardcoded values that should not be hardcoded?
5. Are there any obvious performance concerns?
6. Are there any security gaps — unvalidated input, missing auth, sensitive data exposed?
7. Is the code readable without the author needing to explain it?

For every No: describe specifically what is wrong and how serious it is.
```

**What answer to expect:**
A Yes/No answer per item with specific descriptions for any No. Vague answers like "could be improved" are not acceptable — ask Claude to be specific about what is wrong and what the impact is.

**What to do next:**
Tally the results: failed test cases + checklist Nos. Then go to Step 7.

---

## Step 7 — Approve or Return

**Goal:**
Make an explicit decision: the change is approved, or it returns to development with a clear list of issues.

**What to do in Claude Code:**
Continue the testing session.

**Exact prompt to paste:**
```
Based on the test results and validation checklist, produce a final testing report.

Include:
- Overall decision: Approved or Returned
- Summary of what was tested (2–3 sentences)
- Passed cases: count
- Failed cases: list with ID and description
- Checklist failures: list with description
- If Returned: a prioritized list of issues to fix, each with enough detail to act on

Keep it concise. This report goes to the next session.
```

**What answer to expect:**
A structured report with a clear Approved or Returned decision and specific, actionable findings.

---

**If Approved:**
The feature or fix is done. Record any relevant lessons in `lessons/index.md`. Move to the next feature starting at `prompts/architecture.md`.

**If Returned:**
Copy the issue list from the report. Open the appropriate workflow:
- Code issues → `prompts/bug-fixing.md`
- Structural issues → `prompts/refactoring.md`
- Scope or behavioral issues → `prompts/architecture.md`

Attach the testing report to the next session so the issues are not reconstructed from scratch.
