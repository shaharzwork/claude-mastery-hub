# Lessons Learned

## 1. Purpose

A running log of lessons learned from building software with AI. Each entry captures a real situation, what it revealed, and what changes as a result. This file grows over time — one lesson at a time. The goal is not documentation for its own sake, but a record that actively changes how the next project gets built.

---

## 2. Lesson Template

```
### [SHORT TITLE]
**Date:** YYYY-MM-DD
**Category:** [Architecture | Development | Testing | Claude Code | Session Management | Project Management | AI Tools]

**Context:**
What situation produced this lesson. 1–2 sentences.

**Lesson:**
What you learned. 1–2 sentences. Be specific — generic lessons are useless.

**Do differently:**
One actionable sentence. What changes next time.
```

---

## 3. Categories

| Category | What belongs here |
|---|---|
| **Architecture** | Structural decisions, design patterns, trade-off calls |
| **Development** | Implementation habits, coding patterns, scope discipline |
| **Testing** | Test coverage gaps, regression surprises, verification failures |
| **Claude Code** | Claude behavior, prompt patterns, context management |
| **Session Management** | Session sprawl, naming, context degradation, handoffs |
| **Project Management** | Planning, scoping, estimation, prioritization |
| **AI Tools** | Tool selection, tool limitations, workflow integration |

---

## 4. How to Record a Lesson

**When to record:**
- Immediately after something goes wrong and you understand why
- After a session that was unexpectedly smooth — good patterns deserve recording too
- At the end of a project phase, before starting the next one

**Where it goes:**
Add new lessons at the bottom of this file, under the relevant category heading in Section 5. Most recent entries last — the file reads as a timeline.

**Rules:**
- One lesson per entry. If a situation produced multiple lessons, write multiple entries.
- Be specific. "Claude needs clear context" is not a lesson. "Pasting CLAUDE.md at the start of every session eliminated 80% of incorrect assumptions" is.
- Write it within 24 hours of the situation. Lessons written later are reconstructions, not observations.

---

## 5. Example Lesson

### Refactoring during a feature session doubled the review time
**Date:** 2025-01-15
**Category:** Development

**Context:**
Midway through implementing a login feature, Claude noticed an inconsistency in the error handling pattern and refactored it inline. The feature worked, but the diff contained both the feature and the refactor.

**Lesson:**
A mixed diff means mixed responsibility. When the feature broke in testing, it was impossible to tell whether the bug came from the feature logic or the refactoring. Review took twice as long as it should have.

**Do differently:**
When Claude flags an improvement outside the current scope, paste it into a "Refactor later" note and explicitly tell Claude to continue without acting on it.

---

<!-- Add new lessons below, grouped by category -->
