# Start New Project

A guided workflow from idea to first working feature.

---

## Before You Start

Write your problem statement before opening Claude. One paragraph: what you are building, why it needs to exist, and who uses it. No stack, no solution — just the problem.

If you cannot write this paragraph, you are not ready to start.

---

## Step 1 — Validate the Problem Statement

**Goal:**
Confirm Claude understands the problem correctly before any decisions are made.

**What to do in Claude Code:**
Open a new session. Name it: `[project-name] — Planning`

**Exact prompt to paste:**
```
I'm starting a new project. Here is the problem I'm solving:

[paste your problem statement]

Before suggesting anything, reflect back what you understood:
- What is being built
- Why it needs to exist
- Who uses it
- Any gaps or ambiguities you noticed

Do not propose a solution yet.
```

**What answer to expect:**
Claude reflects the problem back in its own words and flags anything unclear or missing. If the reflection does not match what you meant, rewrite the problem statement and try again — not Claude's response.

**What to do next:**
Resolve any flagged gaps. Then go to Step 2.

---

## Step 2 — Create the Repository

**Goal:**
Give CLAUDE.md and the project scaffold somewhere to live before creating either. Do this in the terminal — not in Claude.

**What to do in Claude Code:**
Continue the Planning session. Ask Claude to generate the setup commands for your project.

**Exact prompt to paste:**
```
Generate the terminal commands to set up a new project directory for this project.

Include:
- Create the project directory
- Initialize a git repository
- Create a .gitignore appropriate for [your stack, or leave blank if unknown]
- Any other setup commands needed before writing the first file

Output only the commands — no explanation.
```

**What answer to expect:**
A short list of terminal commands ready to copy and run. If the stack is not decided yet, Claude will generate a minimal setup (directory + git init + basic .gitignore).

**What to do next:**
Open your terminal. Run the commands Claude generated. Confirm:
- The project directory exists
- `git init` ran successfully (`ls -a` should show a `.git` folder)

Do not proceed until the repository exists. Then go to Step 3.

---

## Step 3 — Create CLAUDE.md

**Goal:**
Give every future session a shared context file so Claude never starts cold.

**What to do in Claude Code:**
Continue the Planning session.

**Exact prompt to paste:**
```
Based on what we've aligned on, generate a CLAUDE.md for this project.

Use this structure:
- Project name and purpose (one sentence)
- Stack (leave blank if not decided yet)
- Folder structure (leave blank if not decided yet)
- Hard constraints (anything I've already ruled out)
- Working style rules for Claude

Keep it short. We will fill in the gaps after the architecture session.
```

**What answer to expect:**
A draft CLAUDE.md with the known fields filled in and placeholders where decisions are still open. It should be under 60 lines.

**What to do next:**
Copy Claude's output. In your project directory, create a file named `CLAUDE.md` and paste the content into it. Save it. This file lives in the project root — the same directory where `.git` is. Then go to Step 4.

---

## Step 4 — Decide the Architecture

**Goal:**
Choose the stack and structure before writing a single line of code.

**What to do in Claude Code:**
Open a new session. Name it: `[project-name] — Architecture`
Paste CLAUDE.md at the top of the session.

**Exact prompt to paste:**
```
Here is the project context:

[paste CLAUDE.md]

Propose 2–3 architecture approaches for this project.

For each option include:
- Stack and key dependencies
- Folder structure
- Trade-offs
- What this approach is best suited for

Do not recommend one yet. I will decide after reviewing.
```

**What answer to expect:**
Two or three distinct approaches, each with a clear trade-off summary. If all options look identical, ask Claude to push them further apart.

**What to do next:**
Pick one approach. Tell Claude which one and why. Then go to Step 5.

---

## Step 5 — Generate the Project Scaffold

**Goal:**
Create the folder structure and empty files before writing any logic.

**What to do in Claude Code:**
Continue the Architecture session.

**Exact prompt to paste:**
```
We are going with [name the chosen approach].

Generate the project scaffold:
- Folder structure
- Empty files with a one-line comment in each describing its purpose
- Minimal — only what we need to start

Do not write any logic yet.
```

**What answer to expect:**
A folder tree and a list of file paths, each with a one-line description of its purpose. No implementation code. If Claude starts writing logic, stop it and repeat the constraint.

**What to do next:**
Ask Claude Code to create the files directly in your project:
```
Create each of these files in my project directory now.
Use the one-line comment as the only content in each file.
Do not add any other code.
```
Claude Code will write the files to disk. Open your file explorer or run `find . -not -path './.git/*'` in the terminal to confirm every file was created. Then update CLAUDE.md with the final stack and folder structure, and run `git add . && git commit -m "Initial scaffold"`. Then go to Step 6.

---

## Step 6 — Build the First Feature

**Goal:**
Hand off to the Development workflow. This is where start-project.md ends and development.md takes over.

**What to do in Claude Code:**
Close the Architecture session. Open `prompts/development.md`.

**Exact prompt to paste:**
No prompt needed for this step. You are switching workflows.

**What answer to expect:**
Not applicable. You are moving to a new workflow file with its own step-by-step structure.

**What to do next:**
Open `prompts/development.md` and follow it from Step 1. When you reach the end of development.md, it will hand you off to Step 7 below.

---

## Step 7 — Test the First Feature

**Goal:**
Hand off to the Testing workflow. Development produces a testing handoff — this step routes it to the right place.

**What to do in Claude Code:**
Close the Development session. Open `prompts/testing.md`.

**Exact prompt to paste:**
No prompt needed for this step. You are switching workflows.

**What answer to expect:**
Not applicable. You are moving to a new workflow file with its own step-by-step structure.

**What to do next:**
Open `prompts/testing.md` and follow it from Step 1. Paste the testing handoff from the Development session into testing.md Step 1 where indicated. When testing passes, the feature is complete — record any lessons in `lessons/index.md` and return to Step 6 for the next feature.
