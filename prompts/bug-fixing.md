# Bug Fixing

## 1. Purpose

A workflow for diagnosing and fixing bugs with Claude. The core discipline: understand before fixing. Claude will produce a plausible fix immediately if asked — that fix may be wrong, incomplete, or create new bugs. This workflow prevents that by separating diagnosis, risk assessment, and approval from implementation. Output is a narrow fix with a clear rationale and a handoff for testing.

---

## 2. When to Use

- A bug has been reported or discovered
- A test case fails unexpectedly
- Behavior does not match the product decision or specification
- A previous fix did not fully resolve the issue

**Do not use** for feature changes or improvements that surface during bug investigation. If the investigation reveals a design problem rather than a code problem, stop and route to `architecture.md`. Fixing a design flaw as if it were a bug produces fragile patches.

---

## 3. Step-by-Step Workflow

**Step 1 — Reproduce the bug before doing anything else**
Confirm the bug is real and repeatable. Document the exact steps, inputs, and observed vs. expected behavior. A bug you cannot reproduce cannot be reliably fixed.

**Step 2 — Root cause analysis**
Ask Claude to analyze the bug — not fix it. The goal is to identify the exact line, function, or logic where the failure originates. Do not accept "the bug is in X" — ask why X is wrong.

**Step 3 — Confirm the root cause**
Do not move forward until you agree with the diagnosis. If something feels off, ask Claude to challenge its own analysis. A fix built on a wrong root cause creates a new bug.

**Step 4 — Risk assessment**
Before proposing a fix, assess the blast radius. What does changing the root cause affect? What other behavior depends on the broken code? What could regress?

**Step 5 — Propose the fix approach**
Ask Claude to describe how to fix it — not write the code. One or two sentences: what changes, where, and why that is the right change. Review this before implementation.

**Step 6 — Approve the approach**
Explicitly confirm the approach before any code is written. This is the gate. If the approach is wrong, it is cheap to change a sentence. It is not cheap to revert an implementation.

**Step 7 — Implement the fix**
Narrow change only. One root cause, one fix. If the implementation starts touching more than the agreed scope, stop and reassess.

**Step 8 — Write the testing handoff**
Document what was fixed, what caused it, what to verify, and what regression risks exist. Hand off to `testing.md`.

---

## 4. Required Sessions

| Session | Purpose | When |
|---|---|---|
| Diagnosis | Reproduce bug, identify root cause, confirm it | Before any fix work |
| Risk & approach | Assess blast radius, propose fix approach, get approval | After root cause is confirmed |
| Implementation | Write the narrow, approved fix | After approach is approved |
| Verification | Confirm fix works, check regressions | After implementation, via `testing.md` |

Never combine diagnosis and implementation in one session. The pressure to fix immediately is how wrong fixes get shipped.

---

## 5. Required Documents

| Document | Description | When Created |
|---|---|---|
| Bug report | Steps to reproduce, observed vs. expected behavior | Before diagnosis session |
| Root cause note | Exact location and reason for the failure | After diagnosis session |
| Risk assessment | What the fix could affect, regression candidates | After root cause is confirmed |
| Fix brief | Approved approach: what changes, where, why | Before implementation |
| Testing handoff | What was fixed, how to verify, regression risks | After implementation |

---

## 6. Copy/Paste Prompts

**Reproduce and document the bug:**
```
I have a bug to investigate. Here is what I know:

Steps to reproduce:
[list steps]

Observed behavior:
[what actually happens]

Expected behavior:
[what should happen]

Relevant files or code:
[paste or list]

Do not suggest a fix yet. First, confirm you can trace the failure from these steps.
```

**Root cause analysis:**
```
Based on the bug report, analyze the root cause.

I want to know:
- The exact location where the failure originates (file, function, line if possible)
- Why that code produces the observed behavior
- Whether this is a logic error, a data problem, a missing check, or something else

Do not propose a fix yet. Explain what is wrong and why.
```

**Challenge the diagnosis:**
```
You've identified [root cause summary] as the cause.

Challenge your own analysis:
- What evidence supports this diagnosis?
- What evidence could contradict it?
- Is there another location where this failure could originate?
- Could this be a symptom of a deeper problem rather than the root cause itself?
```

**Risk assessment:**
```
The root cause is: [root cause summary]

The fix will require changing: [file/function/logic]

Assess the risk:
- What other code depends on this file or function?
- What existing behavior could change as a side effect?
- What regression risks should I test for?
- Is the blast radius narrow or broad?
```

**Propose fix approach:**
```
Root cause confirmed: [root cause summary]
Risk assessment complete: [brief summary of risks]

Now propose the fix approach — not the code, the approach.

Describe in 2–3 sentences:
- What needs to change
- Where the change goes
- Why this is the correct fix and not a workaround

I will approve the approach before any code is written.
```

**Implement the approved fix:**
```
Fix approach approved:
[paste approved approach]

Implement only this fix.

Constraints:
- Change only what is described in the approved approach
- Do not refactor or improve surrounding code
- Flag anything unexpected you encounter while implementing

Show me the change before finalizing.
```

**Testing handoff:**
```
The fix has been implemented.

Write a testing handoff with:
- What was broken and what caused it (root cause summary)
- What was changed to fix it
- How to verify the fix: exact steps, inputs, expected outcome
- Regression risks to check based on the risk assessment
- Anything still uncertain or worth monitoring

Keep it concise. This goes directly into the test session.
```

---

## 7. Common Mistakes

- **Asking Claude to fix before diagnosing.** Claude will produce a confident, plausible fix. It may be wrong. A wrong fix masks the real bug and adds new ones. Diagnosis is not optional.

- **Accepting the first root cause without challenging it.** Claude identifies the most obvious cause. The actual root cause is sometimes one level deeper. Always ask Claude to challenge its own diagnosis.

- **Broad fixes.** A fix that touches more than the identified root cause is not a fix — it is a guess applied to multiple places. Narrow fixes are safer, easier to test, and easier to revert.

- **Skipping risk assessment.** Every bug fix has a blast radius. Skipping this step means finding out about it in production.

- **Implementing without explicit approval.** The transition from approach to implementation should be a conscious decision, not a continuation of the same thought. Make it a separate step.

- **Not writing the testing handoff.** Verifying a fix without documentation leads to incomplete testing. The handoff is what makes testing systematic rather than intuitive.

- **Conflating bugs with design flaws.** If the root cause is a bad design decision, patching it in place creates a fragile workaround. Route design problems to `architecture.md`.

---

## 8. Shahar Best Practices

- Write the bug report before opening Claude. The act of writing it often reveals the cause. If you find the bug while writing, you still need the report — it becomes the testing handoff input.
- Ask Claude to identify the root cause, then ask a follow-up: "Is this the root cause or a symptom?" This single question catches shallow diagnoses more reliably than any other prompt.
- Keep the fix branch isolated. One bug, one branch, one PR. Mixing a bug fix with a feature or refactor makes the fix untraceable.
- If the risk assessment shows a broad blast radius, consider whether a narrow workaround is safer than the correct fix right now. Sometimes the right fix is a future architecture change, not today's patch.
- After a fix is verified, spend two minutes asking: why did this bug exist? Was it a missing test, an unclear spec, a gap in CLAUDE.md conventions? If so, fix the gap — not just the symptom.
- Bugs that reappear are always a documentation problem. Either the root cause was not fully understood, or the fix was not recorded clearly enough to prevent recurrence.
