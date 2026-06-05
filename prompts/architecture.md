# Architecture

A guided workflow for analyzing and approving a new feature before development begins.

---

## Before You Start

Write a one-paragraph feature brief before opening Claude: what the feature does, who uses it, and what done looks like. No technical decisions yet — just the idea.

If you cannot describe the feature in one paragraph, it is not ready to analyze.

---

## Step 1 — Analyze the Feature

**Goal:**
Understand the full scope of the feature before making any decisions. Map what it touches, what it risks, and what might be underestimated.

**What to do in Claude Code:**
Open a new session. Name it: `[project-name] — Architecture — [feature name]`
Paste CLAUDE.md at the top of the session.

**Exact prompt to paste:**
```
Here is the project context:

[paste CLAUDE.md]

I want to add the following feature:

[paste your feature brief]

Before we discuss how to build it, map the full scope:
- What this feature does end to end
- Which existing parts of the system it touches
- What could break or regress
- What I might be underestimating

Do not propose a solution yet.
```

**What answer to expect:**
A structured map of the feature's scope, affected components, and risks. No technical proposal yet. If Claude jumps to a solution, redirect it — analysis before design.

**What to do next:**
Review the map. If the scope is larger or riskier than expected, revise the feature brief now. Then go to Step 2.

---

## Step 2 — Make the Product Decision

**Goal:**
Lock the behavior of the feature before choosing how to build it. Technical decisions made before product decisions get rebuilt.

**What to do in Claude Code:**
Continue the Architecture session from Step 1.

**Exact prompt to paste:**
```
Based on the scope analysis, help me nail down the product decision before we go technical.

Answer these:
- What exactly does this feature do? (behavior, not implementation)
- What are the edge cases I need to decide on?
- What is explicitly out of scope?

Ask me questions if anything is ambiguous. I want a locked product decision before we touch the technical approach.
```

**What answer to expect:**
A short list of behavioral definitions and edge cases with open questions for you to answer. Claude should ask — not assume — when something is unclear. Answer all questions before moving on.

**What to do next:**
Confirm the product decision in one or two sentences. Write it down — this becomes the behavioral contract for the feature. Then go to Step 3.

---

## Step 3 — Evaluate Technical Options

**Goal:**
Understand the trade-offs of different implementation approaches before committing to one.

**What to do in Claude Code:**
Continue the Architecture session.

**Exact prompt to paste:**
```
Product decision is locked:

[paste your product decision summary]

My constraints:
[list hard constraints: existing stack, performance needs, time, patterns to follow]

Propose 2–3 technical approaches to implement this feature.

For each approach:
- How it works
- Key trade-offs
- What it makes harder in the existing system
- What it breaks or complicates

Do not recommend one yet.
```

**What answer to expect:**
Two or three distinct approaches with honest trade-offs. Each should feel meaningfully different. If they look similar, ask Claude to push them further apart — closer to the extremes of the trade-off spectrum.

**What to do next:**
Review each option. Ask Claude: "What does each option make harder?" Then go to Step 4.

---

## Step 4 — Approve or Cancel

**Goal:**
Make an explicit decision: build the feature as proposed, build it differently, or cancel it.

**What to do in Claude Code:**
Continue the Architecture session.

**Exact prompt to paste:**
```
Based on the options, I want to:

[ ] Proceed with option [N] — [name it]
[ ] Proceed with a modified approach: [describe modification]
[ ] Cancel this feature for now — reason: [state reason]

If proceeding: confirm the chosen approach and summarize in 2–3 sentences what will be built and how.
If cancelling: summarize what was learned from the analysis that led to this decision.
```

**What answer to expect:**
If proceeding: a clear confirmation of the chosen approach with a brief rationale.
If cancelling: a short summary of the analysis findings. This is not a failure — a cancelled feature with documented reasoning is a good outcome.

**What to do next:**
If cancelled: record the decision in the architecture note and close the session. No further steps needed.
If approved: go to Step 5.

---

## Step 5 — Assess Architecture Impact

**Goal:**
Check whether the existing project structure supports the chosen approach, and identify what needs to change before development starts.

**What to do in Claude Code:**
Continue the Architecture session.

**Exact prompt to paste:**
```
The chosen approach is: [paste approved approach summary]

Review the current project structure against this decision:

[paste the relevant section of CLAUDE.md — folder structure, stack, conventions]

Tell me:
- Does the current structure support this cleanly?
- What needs to change before development starts?
- What becomes technical debt if we do not change it now?

Be specific. Flag anything that will create friction during development.
```

**What answer to expect:**
A short list of structural changes needed before development, and a separate list of optional improvements that can be deferred. If nothing needs to change, Claude should say so clearly.

**What to do next:**
Decide which changes to make now vs. defer. Then go to Step 6.

---

## Step 6 — Update Project Knowledge

**Goal:**
Record the decision and update CLAUDE.md so the next session starts with the correct context.

**What to do in Claude Code:**
Continue the Architecture session.

**Exact prompt to paste:**
```
The architecture decision is final. Produce two outputs:

1. An architecture note entry:
   - Feature name
   - Decision made
   - Options considered and why they were rejected
   - Behavioral contract (product decision summary)

2. An updated CLAUDE.md reflecting any structural or convention changes from this session.

Keep both concise. No padding.
```

**What answer to expect:**
A clean architecture note entry (5–10 lines) and a revised CLAUDE.md. If CLAUDE.md has not changed, Claude should confirm that explicitly — do not assume.

**What to do next:**
Save the architecture note entry to `project-notes/architecture-notes.md`. Update CLAUDE.md in the project. Commit both. Then go to Step 7.

---

## Step 7 — Hand Off to Development

**Goal:**
Confirm the architecture note is saved and route to the development workflow.

**What to do in Claude Code:**
No prompt needed for this step. Close the Architecture session.

**Exact prompt to paste:**
Not applicable. You are switching workflows.

**What answer to expect:**
Not applicable.

**What to do next:**
Confirm `project-notes/architecture-notes.md` has the entry from Step 6 and CLAUDE.md is up to date. Then open `prompts/development.md` and follow it from Step 1. The development workflow will open and prime the Development session.
