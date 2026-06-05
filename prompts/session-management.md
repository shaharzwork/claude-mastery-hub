# Session Management

## 1. Purpose

A system for managing Claude sessions across a project's lifetime. Covers when to start a new session, when to continue an existing one, how to name sessions, how to track session states, and how to avoid the sprawl that makes context unmanageable. A well-managed session history is the difference between a project where Claude has useful context and one where every session starts from scratch.

---

## 2. When to Use

- At the start of any new piece of work, before opening Claude
- When a session feels long and unfocused
- When you're about to start a different type of work (e.g., switching from architecture to development)
- When returning to a project after a gap
- When a session has been completed and needs to be filed

---

## 3. Step-by-Step Workflow

**Step 1 — Before opening Claude, check existing sessions**
Is there an ACTIVE session for this type of work? If yes, continue it. If no, create a new one. Do not open a new session by default — check first.

**Step 2 — Determine whether to continue or create**
Continue the same session if: the concern is the same, the context is still valid, and the session is not FULL.
Create a new session if: the concern has changed, the session is FULL, significant time has passed, or you are switching workflow type (architecture → development → testing).

**Step 3 — Name the session before starting**
Use the naming convention before writing the first message. Renaming a session after the fact is better than not naming it, but naming it first sets the right frame.

**Step 4 — Prime the session**
Start every session by attaching or pasting CLAUDE.md. Add any relevant prior decisions (architecture notes, task briefs) that Claude needs to operate correctly. Do not assume Claude remembers anything from a previous session.

**Step 5 — Work within the session's scope**
One session, one concern. If the work drifts to a different workflow type, stop — open the correct session type and continue there.

**Step 6 — Update session state when done**
At the end of a session, mark its state. Is it ACTIVE (ongoing), FULL (needs continuation), LEGACY (task complete, for reference), or ARCHIVE (closed, no longer relevant)?

**Step 7 — Review session list periodically**
Once a week or at the start of a new project phase, review all sessions. Promote, archive, or close as needed. Session sprawl accumulates silently.

---

## 4. Session States

| State | Meaning | Action |
|---|---|---|
| **ACTIVE** | Current session, work in progress | Continue here for this type of work |
| **FULL** | Context window long, quality degrading | Start a new session, prime with summary of this one |
| **LEGACY** | Task complete, kept for reference | Do not continue; read only |
| **ARCHIVE** | Closed, no longer needed | Can be deleted or stored |

**How to tell a session is FULL:** Responses become less precise, Claude references earlier decisions incorrectly, or the session has more than 30–40 exchanges. When in doubt, summarize and start fresh.

---

## 5. Session Naming Convention

```
[project-name] — [workflow-type] — [topic]
```

**Workflow types:** Planning, Architecture, Development, Testing, Bug Fix, Refactor

**Examples:**
```
my-app — Architecture — Auth System
my-app — Development — User Login
my-app — Testing — Login Feature
my-app — Bug Fix — Token Expiry
my-app — Refactor — Auth Module
```

**Rules:**
- Always include the project name — sessions from different projects look identical without it
- Use the workflow type from this playbook — it signals which prompt file governs the session
- Topic should be specific enough to distinguish from other sessions of the same type
- Do not use dates in session names — they become meaningless and create false urgency

---

## 6. Required Sessions

Sessions are not a required artifact here — session management *governs* the sessions created by every other workflow. The rule is:

| Workflow | Session type to open |
|---|---|
| Start New Project | Planning → Architecture → Development |
| Architecture decision | Architecture |
| Feature implementation | Development |
| Testing a feature or fix | Testing |
| Bug investigation and fix | Bug Fix |
| Code cleanup | Refactor |

One session per workflow type per concern. Do not run Architecture and Development in the same session.

---

## 7. Required Documents

| Document | Description | When Used |
|---|---|---|
| CLAUDE.md | Project context — prime every session with this | Start of every session |
| Session index | List of current sessions, states, and purposes | Maintained and reviewed weekly |

**Session index format (simple):**
```
my-app — Architecture — Auth System         LEGACY
my-app — Development — User Login           ACTIVE
my-app — Testing — Login Feature            ACTIVE
my-app — Bug Fix — Token Expiry             ARCHIVE
```

The session index can live in CLAUDE.md, a notes app, or anywhere you will actually look at it. Format does not matter. Discipline does.

---

## 8. Copy/Paste Prompts

**Prime a new session:**
```
Starting a new [workflow type] session for [project name].

Here is the project context:
[paste CLAUDE.md]

Relevant prior decisions for this session:
[paste architecture note / task brief / bug report — whatever applies]

This session covers: [one sentence description of scope]

Confirm you have the context you need before we start.
```

**Summarize a FULL session for handoff:**
```
This session has become long. Before we continue in a new session, summarize everything I need to carry forward:

- Decisions made
- Current state of the work
- Open questions or unresolved issues
- What the next session should start with

Be concise. This summary will be pasted into the new session as its starting context.
```

**Open a continuation session:**
```
Continuing from a previous [workflow type] session.

Summary from last session:
[paste summary]

Current project context:
[paste CLAUDE.md]

We are continuing with: [one sentence on where we left off]
```

**Session state review:**
```
Help me review my current sessions for [project name].

Here is the session list:
[paste session index]

For each session, based on what I've told you about the project state:
- Is it still relevant?
- Should it be ACTIVE, LEGACY, or ARCHIVE?
- Is anything missing — a session type I should have open but don't?
```

---

## 9. Common Mistakes

- **Opening a new session by default.** The default action should be to check existing sessions first. New sessions start cold — every new session is a context cost.

- **No session name.** Unnamed sessions become indistinguishable within days. Naming takes five seconds and saves significant time when returning to a project.

- **Running architecture and development in the same session.** These are different modes of thinking and different prompt workflows. Mixing them produces sessions that are unfocused and hard to continue.

- **Continuing a FULL session.** As context grows, Claude's precision degrades. Responses reference earlier decisions incorrectly. The cost of continuing a FULL session is compounding inaccuracy.

- **Never archiving old sessions.** Sessions accumulate. After a month, a list of 20 unnamed sessions is a liability, not an asset. Dead sessions create noise when searching for context.

- **Priming with too much context.** Pasting the entire project history into every session is not priming — it is noise. Prime with CLAUDE.md and only the decisions directly relevant to this session's scope.

- **Treating sessions as disposable.** A LEGACY session contains the reasoning behind decisions. Before archiving, confirm the decisions are recorded in CLAUDE.md or an architecture note.

---

## 10. Shahar Best Practices

- Maintain the session index in CLAUDE.md under a `## Sessions` section. It travels with the project and is always one paste away from being useful.
- Every Monday morning, review the session index. Anything that has not been touched in two weeks is either LEGACY or ARCHIVE. Prune aggressively.
- When a session goes FULL mid-task, do not power through. Stop, run the summary prompt, open a new session, and continue. Five minutes of overhead prevents an hour of degraded responses.
- Treat the session name as a commitment. If the work has drifted so far from the session name that the name no longer applies, that is a signal the session has lost focus — not a signal to rename it.
- Never share context verbally inside a session ("as we discussed earlier"). If the context matters, paste it explicitly. Verbal references degrade as session length grows.
- A session with no clear next action at the end is not done — it is abandoned. Before closing any session, write one sentence: what happens next, and in which session.
