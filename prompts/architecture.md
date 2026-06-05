# Architecture

## 1. Purpose

A workflow for making and recording architecture decisions with Claude. Covers feature analysis, product and technical decisions, architecture review, and keeping project knowledge current. Produces decisions you can defend and context Claude can use in future sessions.

---

## 2. When to Use

- Before building any non-trivial feature
- When choosing between two technical approaches
- When something feels wrong about the current structure
- When adding a major dependency or changing a core pattern
- When re-onboarding Claude to an existing project after a gap
- When the project has grown and the original architecture no longer fits

**Do not use** for small tasks where the right approach is obvious. If you already know what to build and how, go to `development.md`.

---

## 3. Step-by-Step Workflow

**Step 1 — Name the trigger**
Write one sentence: what decision needs to be made, or what is unclear. If you cannot name the trigger, the session will drift.

**Step 2 — Feature analysis**
Before any decisions, fully understand what the feature does, what it touches, and what it risks. Ask Claude to map it — not build it.

**Step 3 — Product decision first**
Define what the feature should do and for whom before deciding how to build it. Technical decisions made before product decisions get rebuilt.

**Step 4 — Technical decision**
Present the constraints and ask for options. Evaluate trade-offs. Make a choice. One option wins — document why the others lost.

**Step 5 — Architecture review**
Check whether the existing structure still fits the decision made. If not, decide explicitly: adapt now or note it as debt.

**Step 6 — Update project knowledge**
Update CLAUDE.md, the architecture note, or both. New sessions start cold — decisions not written down are decisions lost.

---

## 4. Required Sessions

| Session | Purpose | When |
|---|---|---|
| Feature analysis | Map scope, dependencies, risks before any decision | Before building anything significant |
| Product decision | Define behavior, edge cases, and constraints | After feature analysis, before technical work |
| Technical decision | Evaluate options, choose approach, document rationale | After product decision is locked |
| Architecture review | Assess structural fit, identify debt | After major decisions or periodically |

Each session has a single output. Name it before you start.

---

## 5. Required Documents

| Document | Description | When Updated |
|---|---|---|
| CLAUDE.md | Project context: stack, structure, conventions, rules | After any structural change |
| Architecture note | Decisions made, options rejected, reasons | After every architecture session |
| Feature brief | What the feature does, who it serves, edge cases | Before technical decisions |

Architecture notes can live in a `/docs` folder or inline in CLAUDE.md for small projects. The format does not matter — consistency does.

---

## 6. Copy/Paste Prompts

**Feature analysis:**
```
I need to build [feature name]. Before we discuss how to build it, help me understand the full scope.

Map out:
- What this feature does end to end
- What existing parts of the system it touches
- What could go wrong or break
- What I might be underestimating

Do not propose a solution yet.
```

**Product decision:**
```
Here is what I know about [feature name]:

[paste feature brief or summary]

Help me nail down the product decision before we go technical:
- What exactly should this do? (behavior, not implementation)
- What are the edge cases I need to decide on?
- What is out of scope?

Ask me questions if anything is ambiguous. I want product clarity before we touch the technical approach.
```

**Technical decision:**
```
Product decision is locked:
[paste product decision summary]

Constraints:
[list hard constraints: stack, performance, timeline, existing patterns]

Propose 2–3 technical approaches to implement this.

For each:
- How it works
- Trade-offs
- What it breaks or complicates in the existing system

Do not recommend one yet.
```

**Architecture review:**
```
Here is the current structure of the project:

[paste CLAUDE.md or relevant architecture summary]

We just decided to [summarize decision].

Review the current architecture against this decision:
- Does the existing structure support it cleanly?
- What needs to change?
- What becomes technical debt if we don't change it now?

Be specific. Flag anything that will cause friction later.
```

**Update project knowledge:**
```
Based on this session, update my project knowledge.

Produce:
1. A revised CLAUDE.md reflecting any structural or convention changes
2. An architecture note entry: decision made, options considered, reason for choice

Keep both concise. No padding.
```

---

## 7. Common Mistakes

- **Making technical decisions before product decisions.** The implementation gets built, then the product requirement changes, and the implementation is wrong. Lock the "what" before the "how."

- **Asking Claude to recommend without giving constraints.** Claude will pick the most common pattern, not the right one for your situation. Always state hard constraints before asking for options.

- **Treating Claude's first architecture proposal as final.** It is a starting point. Push back, add constraints, ask what breaks. The second or third response is usually more useful than the first.

- **Not recording the rejected options.** Three months later you will reconsider one of them. If you did not write down why you rejected it, you will have the same conversation again.

- **Skipping the architecture review after a major decision.** Decisions compound. A technical decision that ignores the existing structure creates friction in every session that follows.

- **Updating CLAUDE.md mid-session instead of at the end.** Mid-session updates are partial and get outdated before the session ends. Update once, after the decision is final.

---

## 8. Shahar Best Practices

- Product decision and technical decision are separate sessions. Never run them together. Mixing them leads to solutions in search of a problem.
- Write the feature brief in plain language, not engineering language. If you cannot explain what it does without technical terms, the product decision is not ready.
- Keep the architecture note as a running log, not a single doc. One entry per decision, dated. Easy to audit, easy to hand to Claude as context.
- When reviewing options, ask Claude: "What does each option make harder?" Trade-offs are often more visible from the downside than the upside.
- After any architecture session, do a one-line gut-check: "Would I be comfortable explaining this decision to someone in a year?" If not, the decision or the documentation needs more work.
- Set a size limit on architecture sessions. If a session runs past 30 minutes of back-and-forth without a decision, the trigger was not specific enough. Stop, rewrite the trigger, restart.
