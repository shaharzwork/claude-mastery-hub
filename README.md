# AI Software Engineering Playbook

A personal playbook for building software projects with AI.

---

## Recommended Workflow

```
New Project
↓
Architecture
↓
Development
↓
Testing
```

**Common routes:**

| Situation | Go to |
|---|---|
| Development discovers a bug | Bug Fixing → Testing |
| Bug Fixing reveals a design problem | Architecture |
| Testing finds a defect | Bug Fixing |
| Refactoring changes architecture | Architecture → Development → Testing |

---

## What do you want to do?

### 🚀 Start a New Project
Define the problem, create CLAUDE.md, set up architecture, and ship a working scaffold before writing any logic.
→ [prompts/start-project.md](prompts/start-project.md)

---

### 🏗️ Architecture
Analyze a feature, make product and technical decisions, review structure, and keep project knowledge current.
→ [prompts/architecture.md](prompts/architecture.md)

---

### 💻 Development
Implement approved work in small safe steps. Inspect files before coding. Stay in scope. End with a testing handoff.
→ [prompts/development.md](prompts/development.md)

---

### 🧪 Testing
Review a handoff, generate test cases, identify edge cases and regression risks, and produce a clear pass/fail result.
→ [prompts/testing.md](prompts/testing.md)

---

### 🐞 Fix a Bug
Diagnose root cause before writing any fix. Assess risk, approve approach, implement narrow change, verify with tests.
→ [prompts/bug-fixing.md](prompts/bug-fixing.md)

---

### ♻️ Refactoring
Improve structure without changing behavior. Small incremental steps, regression tests after each, with rollback checkpoints.
→ [prompts/refactoring.md](prompts/refactoring.md)

---

### 📂 Session Management
Decide when to start a new session or continue an existing one. Name sessions, track states, avoid sprawl.
→ [prompts/session-management.md](prompts/session-management.md)

---

### 📝 CLAUDE.md Template
Generic project context template. Copy into any new project and fill in the blanks before the first session.
→ [templates/CLAUDE.md](templates/CLAUDE.md)

---

### 💡 Lessons Learned
A running log of what worked, what didn't, and what changes next time.
→ [lessons/index.md](lessons/index.md)
