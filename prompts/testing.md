# Testing

## 1. Purpose

A workflow for testing implemented work with Claude. Takes a handoff from development or bug-fixing and produces verified, working code with documented test cases, edge case coverage, regression risk assessment, and a clear pass/fail result. Testing is not optional — it is the gate between implementation and done.

---

## 2. When to Use

- After receiving a testing handoff from a development session
- After receiving a testing handoff from a bug-fix session
- Before merging or deploying any non-trivial change
- When verifying that a fix did not introduce new problems

**Do not use** as a substitute for a missing handoff. If no handoff exists, go back to `development.md` or `bug-fixing.md` and produce one first. Testing without a handoff means guessing what was built.

---

## 3. Step-by-Step Workflow

**Step 1 — Review the handoff**
Read the full testing handoff before doing anything else. Confirm you understand: what was built or fixed, which files changed, and what the expected behavior is.

**Step 2 — Generate test cases**
Ask Claude to produce test cases from the handoff. Cover happy path, edge cases, and failure cases. Review the list before running anything — add or remove cases based on your knowledge of the system.

**Step 3 — Identify edge cases**
Go deeper than the obvious inputs. Ask Claude to think adversarially: what inputs or states would expose a hidden assumption in the implementation?

**Step 4 — Assess regression risks**
Identify what existing behavior could have been broken by the change. Check the files that were modified and their dependencies.

**Step 5 — Execute tests**
Run through the test cases. Document each result: pass, fail, or unexpected behavior. Do not skip a failing case — every failure is information.

**Step 6 — Complete the validation checklist**
Confirm the implementation matches the product decision, follows project conventions, handles errors correctly, and has no obvious performance or security issues.

**Step 7 — Record the result**
Pass or fail — write it down with evidence. If fail, summarize what broke and hand off to `bug-fixing.md`. If pass, mark the task complete and update CLAUDE.md if behavior changed.

---

## 4. Required Sessions

| Session | Purpose | When |
|---|---|---|
| Test planning | Review handoff, generate test cases, identify edge cases and regression risks | Before running any tests |
| Test execution | Run through test cases, document results | After test planning |
| Regression review | Verify existing behavior is intact | After test execution, before marking done |

Do not combine test planning and execution in one session. Planning requires stepping back; execution requires focus on results.

---

## 5. Required Documents

| Document | Description | Source |
|---|---|---|
| Testing handoff | What was built/fixed, files changed, known edge cases | `development.md` or `bug-fixing.md` |
| Test cases | Full list: happy path, edge cases, failure cases | Generated in test planning session |
| Validation checklist | Per-task checklist confirming correctness, conventions, error handling | Completed during test execution |
| Test results | Pass/fail per case, summary of outcome | Written during test execution |

---

## 6. Copy/Paste Prompts

**Review the handoff:**
```
Here is the testing handoff for this task:

[paste handoff]

Before we generate test cases, confirm you understand:
- What was built or fixed
- What the expected behavior is
- Which files were changed

Flag anything in the handoff that is ambiguous or missing.
```

**Generate test cases:**
```
Based on this handoff, generate a full set of test cases.

Include:
- Happy path: expected inputs producing expected outputs
- Edge cases: unusual but valid inputs
- Failure cases: invalid inputs, missing data, API errors

For each test case:
- Input or condition
- Expected outcome
- How to verify it

Do not run anything yet. I will review the list first.
```

**Edge case analysis:**
```
Look at this implementation and think adversarially.

[paste relevant code or describe the feature]

What inputs, states, or sequences of events would expose a hidden assumption or cause unexpected behavior?

Focus on things the happy path test cases would not catch.
```

**Regression risk assessment:**
```
These files were changed in this implementation:

[list changed files]

Review each file and identify:
- What existing behavior depends on the changed code
- What other parts of the system call or import these files
- What could break that the test cases do not currently cover

Be specific. I want to know what to check, not just that regression is possible.
```

**Validation checklist:**
```
We have completed testing for [feature/fix name].

Run through this validation checklist and give me a clear yes/no for each item:

1. Does the implementation match the product decision?
2. Does it follow the conventions in CLAUDE.md?
3. Does it handle errors and bad input correctly?
4. Are there any hardcoded values that should not be hardcoded?
5. Are there any obvious performance concerns?
6. Are there any security considerations that were not addressed?
7. Is the code readable without requiring the author to explain it?

For any "no" — describe specifically what is wrong.
```

---

## 7. Common Mistakes

- **Testing only the happy path.** The happy path is the least likely failure mode in production. Edge cases and failure cases are where bugs live.

- **Skipping the handoff review.** Starting test case generation without fully understanding what was built leads to test cases that miss the point.

- **Treating Claude-generated test cases as complete.** Claude generates what it can infer from the handoff. You know the system. Add cases Claude cannot know about.

- **Not checking regression risks.** Every change has a blast radius. Skipping regression review means discovering the blast radius in production.

- **Merging on "it seems to work."** Testing without documented results is not testing — it is optimism. Every session should end with a written pass/fail.

- **Ignoring failing test cases.** A failing case that gets noted and skipped is a known bug being shipped. Treat every failure as a blocker until explicitly decided otherwise.

- **Not updating CLAUDE.md when behavior changes.** If the test confirms new behavior that differs from what CLAUDE.md describes, the doc is now wrong. Fix it before the next session.

---

## 8. Shahar Best Practices

- Read the handoff twice before generating test cases. The second read always surfaces something the first one missed.
- Write the test cases as a numbered list before running any of them. Reviewing the list as a whole reveals gaps that are invisible case by case.
- Ask Claude for edge cases separately from the main test case generation. When edge cases are mixed in, they get less attention and less depth.
- Treat the regression risk session as non-negotiable for any change that touches shared utilities, data models, or API interfaces.
- A failing test is not a problem — it is the test working. The problem is shipping without running the test.
- Keep test results as a short written record, even if informal: date, task, cases run, result. Builds a paper trail for future debugging and for training your own instincts.
- If the validation checklist turns up more than one "no," do not patch them in the test session. Hand off to `bug-fixing.md` or `refactoring.md` cleanly.
