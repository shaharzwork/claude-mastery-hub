# Claude Mastery Hub — Master Plan

## 1. Vision

A curated, opinionated hub that takes developers from Claude API beginner to production-grade practitioner through structured, real-world projects — not tutorials.

---

## 2. Goals

- Teach Claude API usage through building, not reading
- Cover the full capability surface: text, tools, vision, caching, agents, and more
- Provide reusable, well-structured project templates developers can fork and extend
- Establish clear quality and depth expectations at every maturity level
- Become the go-to reference for engineers who want to use Claude effectively in production

---

## 3. Target Audience

**Primary:** Developers with some programming experience who want to build real applications with Claude — ranging from first-timers to engineers who've used the API but haven't gone deep.

**Secondary:** Teams evaluating Claude for production use who want proven patterns, not toy examples.

**Not the audience:** Non-technical users; researchers studying model behavior; people looking for prompt templates without code.

---

## 4. Scope

- Projects built against the Anthropic (Claude) API using official SDKs (Python, TypeScript)
- Coverage of all major API capabilities: text generation, streaming, tool use, vision, file handling, prompt caching, batch processing, multi-agent orchestration
- Multiple maturity levels per capability area, from introductory to advanced
- Supporting context: project READMEs, setup guides, and design rationale
- Model recommendations and upgrade paths as new Claude versions ship

---

## 5. Out of Scope

- Non-Claude AI providers (OpenAI, Gemini, etc.)
- Fine-tuning or model training
- Infrastructure, hosting, or DevOps patterns beyond what's needed to run a project
- Frontend UI frameworks (projects are backend/API-focused unless UI is the point)
- Academic benchmarking or research methodology
- Prompt-only content with no code

---

## 6. Core Principles

**Real projects, not demos.** Every project solves a problem you'd actually encounter. Toy examples are excluded.

**Progressive depth.** Each capability area starts accessible and scales to production-grade complexity.

**Opinionated defaults.** We pick one approach per problem and explain why — no decision paralysis from endless options.

**Production awareness.** Caching, error handling, cost, and latency are first-class concerns, not afterthoughts.

**Minimal but complete.** Projects include only what's necessary, but nothing is left unfinished.

---

## 7. Main User Journeys

**Journey 1 — Onboarding**
Developer discovers the hub, understands the structure, picks a starting project matched to their skill level, and ships something working in under an hour.

**Journey 2 — Capability exploration**
Developer wants to learn a specific Claude capability (e.g., tool use, vision, caching). They find the relevant section, choose a project at their maturity level, and work through it.

**Journey 3 — Production readiness**
Developer has a working prototype and wants to harden it. They move to advanced projects in the same capability area and apply patterns like caching, structured outputs, and multi-turn state management.

**Journey 4 — Reference**
Experienced developer returns to find a specific pattern, design decision, or code snippet. Navigation and project naming make lookup fast.

---

## 8. Project Types

| Type | Description |
|---|---|
| **Standalone script** | Single-file, self-contained. Ideal for foundational and intermediate projects. |
| **CLI tool** | Command-line application with argument parsing and real I/O. |
| **API service** | A small backend service exposing Claude-powered endpoints. |
| **Agent** | Autonomous or semi-autonomous Claude agent with tool use and multi-step reasoning. |
| **Multi-agent system** | Coordinated agents with defined roles, communication, and orchestration logic. |
| **Integration** | Claude embedded into an existing workflow, tool, or data pipeline. |

---

## 9. Maturity Levels

**Foundational**
Core API mechanics. Minimal dependencies. Goal: get something working and understand the basics.

**Intermediate**
Real-world patterns: streaming, tool use, error handling, basic caching. Goal: write code you'd actually use.

**Advanced**
Production concerns: multi-turn state, prompt caching, cost optimization, structured outputs, batch processing. Goal: ship with confidence.

**Expert**
Multi-agent systems, custom orchestration, evaluation loops, edge-case handling at scale. Goal: architect and own a Claude-powered system end to end.

---

## 10. Future Roadmap

- **Phase 1 — Foundation:** Define project structure, build first 3–5 projects across maturity levels for core capabilities (text generation, streaming, tool use)
- **Phase 2 — Expansion:** Add vision, file handling, prompt caching, and batch processing project tracks
- **Phase 3 — Agents:** Multi-agent orchestration, memory patterns, and evaluation frameworks
- **Phase 4 — Community:** Contribution guidelines, issue templates, and a submission process for external projects
- **Phase 5 — Maintenance:** Model version updates, deprecation tracking, and keeping projects current with API changes

---

## 11. Real Project Definition

**A real project is something someone could actually use.**

Specifically: it solves a problem that exists independently of the project itself.

### Qualification Test

Answer yes or no to each question.

1. Does it handle bad input without crashing? *(Invalid args, empty responses, malformed API output)*
2. Does it produce output someone could act on or use directly? *(A file, a response, a decision — not just "it ran")*
3. Could someone other than the author run it with only the README? *(No tribal knowledge, no "ask me how to set it up")*
4. Does it handle API errors gracefully? *(Rate limits, timeouts, unexpected status codes)*
5. Does it have a clear, single purpose? *(Someone can describe what it does in one sentence)*
6. Would you be comfortable sharing it with a colleague as a working example? *(Not "here's a rough thing I hacked together")*
7. Does it work end-to-end without manual intervention mid-run? *(No "stop here and edit line 42 before continuing")*
8. Does the problem this solves exist independently of the project? *(Would someone have this problem even if your project didn't exist?)*

### Classification

| Yes count | Classification |
|---|---|
| 0–2 | **Demo** — Illustrates a concept. Not meant to be used. |
| 3–6 | **Prototype** — Works under ideal conditions. Fragile in the real world. |
| 7–8 | **Real Project** — Something someone could actually use. |

**Hard rules:**
- A "No" on question 3 or 4 caps the score at **Prototype** regardless of total. A project that only works on the author's machine, or that crashes on a rate limit, is not real.
- A "No" on question 8 caps the score at **Demo** regardless of total. A well-built solution to a fake problem is still a demo.
