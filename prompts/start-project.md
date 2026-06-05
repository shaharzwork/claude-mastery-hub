# Start New Project

## 1. Purpose

A repeatable workflow for starting a new software project with Claude as a development partner. Covers everything from problem definition to writing the first line of code — in the right order, with the right context primed.

---

## 2. When to Use

- Starting a brand new project from scratch
- Taking over an existing codebase for the first time
- Spinning up a new service or module within a larger system
- Restarting a stalled project with a clean approach

**Do not use** for small tasks or single-file scripts. Jump straight to `development.md` instead.

---

## 3. Step-by-Step Workflow

**Step 1 — Define the problem before opening Claude**
Write one paragraph describing what you are building and why. No code, no stack decisions yet. If you cannot write this paragraph, you are not ready to start.

**Step 2 — Create CLAUDE.md**
Copy from `templates/CLAUDE.md`. Fill in project name, purpose, stack (if known), and any hard constraints. This is the first file in the repo.

**Step 3 — Open a planning session**
Share the problem statement. Ask Claude to reflect it back and identify gaps before proposing anything. Fix misunderstandings here — not in code.

**Step 4 — Decide on architecture**
Ask Claude for 2–3 architecture options with trade-offs. Pick one. Document the decision and the reason in a single paragraph. See `prompts/architecture.md`.

**Step 5 — Generate project scaffold**
Ask Claude to create the folder structure and empty files. Review before accepting. Do not let Claude write logic yet.

**Step 6 — Write CLAUDE.md final version**
Update with the decided stack, folder structure, conventions, and any rules Claude must follow throughout the project.

**Step 7 — Open the first development session**
Start fresh. Attach CLAUDE.md. Begin with the first real task.

---

## 4. Required Sessions

| Session | Purpose | When |
|---|---|---|
| Planning | Define problem, identify gaps, align on scope | Before any code |
| Architecture | Explore options, decide stack and structure | After planning |
| Scaffold | Generate folder structure and empty files | After architecture |
| Development | First feature or task | After scaffold |

Keep sessions focused. One session per concern.

---

## 5. Required Documents

| Document | Description | When Created |
|---|---|---|
| Problem statement | 1–3 paragraphs. What, why, who. No solution yet. | Before planning session |
| CLAUDE.md | Project context file for Claude Code | Before scaffold session |
| Architecture note | 1 paragraph. Decision + reason. | After architecture session |

---

## 6. Copy/Paste Prompts

**Prime the planning session:**
```
I'm starting a new project. Here's the problem I'm solving:

[paste problem statement]

Before proposing anything, reflect back what you understood. Identify any gaps or ambiguities in what I've described.
```

**Request architecture options:**
```
Based on what we've aligned on, propose 2–3 architecture approaches for this project.

For each option include:
- Core structure
- Stack recommendation
- Key trade-offs
- What this approach is best suited for

Do not recommend one yet. I'll decide after reviewing.
```

**Generate project scaffold:**
```
We're going with [chosen architecture].

Create the folder structure and empty files for this project.
- No logic yet, only structure
- Include a one-line comment in each file describing its purpose
- Keep it minimal — only what we need to start
```

**Finalize CLAUDE.md:**
```
Based on everything we've decided in this session, update my CLAUDE.md with:
- Final stack and key dependencies
- Folder structure overview
- Naming conventions
- Any rules you should follow throughout this project

Use the existing CLAUDE.md as the base.
```

---

## 7. Common Mistakes

- **Starting to code before the problem is clear.** Symptoms: Claude asks clarifying questions mid-task, scope keeps shifting, first draft gets thrown away.

- **Skipping CLAUDE.md.** Every new session starts cold. Without CLAUDE.md, you re-explain context every time.

- **Letting Claude decide the stack unprompted.** Claude will pick something reasonable but not necessarily what fits your constraints. Always state hard constraints upfront.

- **Giant first session.** Trying to plan, architect, scaffold, and build in one conversation leads to drift. One session per concern.

- **Accepting the scaffold without reviewing it.** Claude will add files that seem logical but don't match your intent. Review the structure before moving on.

- **Not documenting the architecture decision.** Two weeks later you won't remember why you made the choice. One paragraph is enough.

---

## 8. Shahar Best Practices

- Write the problem statement in a notes app first, without Claude open. Forces clarity before asking anything.
- Keep CLAUDE.md under 100 lines. If it grows beyond that, it's carrying too much — split into separate docs.
- Treat the planning session as a pressure test: if Claude misunderstands the problem statement, rewrite the statement, not Claude's response.
- Decide the stack before the architecture session, or explicitly ask Claude to factor in a constraint. Open-ended stack decisions waste session time.
- Name sessions in Claude as: `[project-name] — Planning`, `[project-name] — Scaffold`, etc. Makes it easy to return to the right context.
- After the scaffold is accepted, commit it immediately before writing any logic. Clean baseline to diff against.
