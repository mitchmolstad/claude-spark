---
name: spark
description: One-time project interview that generates all foundational documentation and configures the workflow
---

# Spark — Project Interview & Documentation Generator

You are conducting a structured interview to understand a new project and generate all foundational documentation. This runs once at project start.

## Before Starting

Check if documentation already exists (STATUS.md, TECHNICAL-CORE.md, etc.). If it does, warn the user that `/spark` is designed for fresh projects and running it will overwrite existing docs. Ask for confirmation before proceeding.

## The Interview

Conduct the interview in 4 phases. Be conversational, not robotic. Ask follow-up questions when answers are vague. Summarize your understanding after Phase 2 before continuing.

**Fast track:** If the user says "just get started" or provides a spec/PRD, extract what you can and note assumptions. Don't force the full interview.

---

### Phase 1: About You

Goal: Understand the human you're working with.

Ask (conversationally, not as a checklist):

1. **What should I call you?** (Name for the interaction protocol)
2. **What's your technical level?** Are you a developer, designer, PM, or somewhere in between? This helps me calibrate explanations.
3. **How do you like to communicate?** Some people want detailed explanations. Others want "just do it" with a summary at the end. What works for you?
4. **What's your biggest stress point with projects like this?** (Scope creep? Technical debt? Shipping too slowly? Not knowing what to build?)

---

### Phase 2: About the Project

Goal: Understand what we're building and why.

Ask:

1. **Elevator pitch** — In 1-2 sentences, what does this project do? Who is it for?
2. **Do you have an existing spec, PRD, or design doc?** If so, ask them to paste or reference it. Extract requirements from it rather than re-asking.
3. **What does success look like?** Not features — outcomes. "Users can X" or "Revenue reaches Y" or "I learn Z."
4. **What are the hard constraints?** Budget, timeline, regulatory, existing systems to integrate with, must-use technologies.
5. **What's explicitly out of scope?** What should we NOT build, even if tempting?

**After Phase 2:** Summarize your understanding back to the user. Confirm before proceeding. This is the foundation everything else builds on.

---

### Phase 3: Look and Feel

Goal: Understand the aesthetic and user experience direction.

**Skip this phase entirely if the project is:**
- A CLI tool
- A backend/API-only project
- A library or SDK
- Infrastructure/DevOps tooling

Ask:

1. **Who are your users?** Demographics, technical level, context of use (desk? mobile? in a hurry?).
2. **What's the vibe?** Show me 1-3 websites/apps you admire aesthetically. Or describe the feeling: "clean and minimal," "warm and friendly," "professional and trustworthy," "bold and playful."
3. **Light mode, dark mode, or both?**
4. **Any brand elements?** Colors, logos, fonts already decided?
5. **Accessibility requirements?** WCAG level, specific needs (screen readers, color blindness, motor impairment)?

---

### Phase 4: Technical Foundation

Goal: Understand the stack and development environment.

**Abbreviate if Phase 2 already covered tech choices** (e.g., from an existing spec).

Ask:

1. **Frontend:** Framework preference? (React, Vue, Svelte, Next.js, plain HTML, none)
2. **Backend:** Framework/runtime? (Node/Express, Python/FastAPI, Go, Hono, Rails, serverless functions, none)
3. **Database:** Preference? (PostgreSQL, MySQL, SQLite, MongoDB, D1, Supabase, none yet)
4. **Hosting/Deploy target:** Where will this live? (Vercel, Netlify, Cloudflare, AWS, Railway, self-hosted, not sure)
5. **Dev environment:** What OS? Any required tools already installed? (Node version, package manager preference, Docker, etc.)
6. **Existing code?** Are we starting from scratch or is there a codebase already?
7. **Testing approach?** Any preferences? (Vitest, Jest, Playwright, none yet)

---

## Output Generation

After the interview, generate ALL of the following files. Use the interview answers to fill in real content — no placeholder text.

### File 1: `SPEC.md`

```markdown
# [Project Name] — Specification

## Problem Statement
[From elevator pitch — what problem exists and for whom]

## Target Users
[From Phase 2 + Phase 3 — who are they, what's their context]

## Success Criteria
[From "what does success look like" — measurable outcomes]

## Requirements

### Must Have (v1)
- [ ] [Extracted from interview — core features]

### Nice to Have (v1.x)
- [ ] [Features mentioned but not critical for launch]

### Future Considerations
- [ ] [Ideas that came up but are explicitly post-v1]

## Out of Scope
- [Explicitly excluded items from interview]

## Constraints
- [Timeline, budget, technical, regulatory constraints]
```

### File 2: `CLAUDE.md` (Replaces bootstrap)

```markdown
# [Project Name]

## 1. Project Vision
[1-2 sentence vision from elevator pitch]

## 2. Project Context
- **Stack**: [From Phase 4]
- **Deploy Target**: [From Phase 4]

### Documentation Structure

| File                       | Purpose                          | Read Frequency     |
| -------------------------- | -------------------------------- | ------------------ |
| `CLAUDE.md`                | Project constitution (this file) | Every session      |
| `STATUS.md`                | Current phase, next priorities   | Every session      |
| `TECHNICAL-CORE.md`        | Schema, API endpoints            | Every session      |
| `SPEC.md`                  | Requirements and success criteria| On demand          |
| `TECHNICAL-REFERENCE.md`   | Full decision history            | On demand          |
| `docs/ARCHITECTURE.md`     | System design, data models       | For new features   |
| `BACKLOG.md`               | Long-term roadmap                | For task selection |
| `PROJECT-LOG.md`           | Session history                  | For debugging      |

## 3. Interaction Protocol (The "[User Name]" Filter)
- **Tone**: [From Phase 1 communication preference — e.g., "Smart friend. No jargon. Explain tradeoffs in terms of experience."]
- **Authority**: Claude has 100% technical authority. Don't ask about frameworks or implementation details.
- **Quality**: Test everything before showing. Never show broken features.
- **Communication**: [Calibrated to user's technical level from Phase 1]

## 4. The Guardrail (Anti-Drift)
- Before implementing features, verify the plan against `docs/ARCHITECTURE.md`.
- If a task risks building something that solves the immediate problem but is shaped wrong for the long-term roadmap, halt and request a "Plan Review."

## 5. Engineering Directives
- **Reliability**: Choose "boring," well-supported tech. Optimize for maintainability.
- **Self-Correction**: Fix broken builds/tests immediately.
- **Security**: Guard API keys. Validate all inputs. Graceful, friendly error handling.
[Add any project-specific directives from constraints]

## 6. Known Gotchas
[Empty — will accumulate over sessions]

## 7. Compact Instructions

When compacting, always preserve:
- The full database schema (table names, columns, types)
- All API endpoint paths
- The current task and its requirements
- Any files modified this session
- Known gotchas from section 6

## 8. Session Ritual

1. **Start**: Run `/start` to hydrate context
2. **Work**: Stay on critical path
3. **End**: Run `/done` to sync all documentation
4. **Reset**: Use `/clear` to reset context once state is persisted
```

### File 3: `STATUS.md`

```markdown
# Project Status

**Last Updated:** [Today's date] — Project initialized via /spark

## Current Phase: Foundation

Setting up the development environment and building the first core feature.

## What's Built
- [x] Project documentation initialized
- [x] Development workflow configured

## Next Up
1. [First priority extracted from must-have requirements]
2. [Second priority]
3. [Third priority]

## Dev Environment
- **Setup**: [Commands to install and run from Phase 4]
- **Run locally**: [Dev server command if applicable]
- **Run tests**: [Test command if applicable]
```

### File 4: `TECHNICAL-CORE.md`

```markdown
# Technical Core

> The source of truth for schema, endpoints, and recent decisions.
> Read every session to prevent hallucination.

## Stack

| Layer     | Technology    | Notes                    |
| --------- | ------------- | ------------------------ |
| Frontend  | [From Phase 4]| [Any notes]             |
| Backend   | [From Phase 4]| [Any notes]             |
| Database  | [From Phase 4]| [Any notes]             |
| Hosting   | [From Phase 4]| [Any notes]             |
| Testing   | [From Phase 4]| [Any notes]             |

## Database Schema

> Update this section whenever tables/collections are created or modified.

[Empty — no schema yet. Add tables here as they're created.]

## API Endpoints

> Update this section whenever endpoints are added or changed.

[Empty — no endpoints yet. Add routes here as they're created.]

## Recent Decisions

| Date       | Decision              | Rationale                |
| ---------- | --------------------- | ------------------------ |
| [Today]    | Initialized project   | /spark interview results |

> Keep 10-15 most recent. Archive older entries to TECHNICAL-REFERENCE.md.
```

### File 5: `docs/ARCHITECTURE.md`

```markdown
# Architecture

> Constitutional document. Defines vision, patterns, and boundaries.
> Only update when fundamental patterns change, not for feature additions.

## Vision
[From elevator pitch — the core loop of value]

## Core Loop
[The fundamental user journey — what users do repeatedly]

## Architectural Patterns
[Initial patterns based on stack choices — e.g., "API-first," "component-based UI," "event-driven"]

## Boundaries
[Where concerns live — frontend vs backend, what talks to what]

## Future Hooks
[Things we're not building now but the architecture should accommodate — from interview]

## Guardrail Checklist
Before implementing any feature, verify:
- [ ] Does it serve the core loop?
- [ ] Does it fit the architectural patterns?
- [ ] Does it respect the boundaries?
- [ ] Would it need to be rewritten for future hooks?
```

### File 6: `BACKLOG.md`

```markdown
# Backlog

**Last Reviewed:** [Today's date]

## Prioritization Framework

| Priority | Criteria                                    |
| -------- | ------------------------------------------- |
| P0       | Blocks launch or core loop broken           |
| P1       | Must-have for v1, required for launch       |
| P2       | Nice-to-have, improves experience           |
| P3       | Future consideration, post-launch           |

## Active

### P0 — Critical
[Items from must-have requirements, if any are blocking]

### P1 — Must Have
- [ ] [Must-have requirements from spec]

### P2 — Nice to Have
- [ ] [Nice-to-have items from spec]

### P3 — Future
- [ ] [Future considerations from spec]

## Shipped
[Empty — nothing shipped yet]
```

### File 7: `PROJECT-LOG.md`

```markdown
# Project Log

## [Today's Date] — Project Spark

**What happened:**
- Ran `/spark` interview to establish project foundations
- Generated all documentation: CLAUDE.md, STATUS.md, TECHNICAL-CORE.md, SPEC.md, ARCHITECTURE.md, BACKLOG.md

**Decisions:**
- [Key technical decisions from Phase 4]
- [Key scope decisions from Phase 2]

**Next session:**
- [First priority from STATUS.md]
```

---

## Post-Generation Steps

After generating all 7 files:

1. **Git commit:**
   ```
   git add -A
   git commit -m "Initialize project via /spark"
   ```

2. **Print summary:**
   ```
   Created 7 documentation files:
   - SPEC.md — Requirements and success criteria
   - CLAUDE.md — Project constitution (replaced bootstrap)
   - STATUS.md — Current phase and priorities
   - TECHNICAL-CORE.md — Schema and endpoints (empty, ready to fill)
   - docs/ARCHITECTURE.md — System design and guardrails
   - BACKLOG.md — Prioritized roadmap
   - PROJECT-LOG.md — Session history
   ```

3. **Suggest optional skills** based on project type:
   - If the project has a frontend/UI → suggest installing `design-audit` and `frontend-design` skills
   - If using React/Next.js → suggest installing `react-best-practices` skill
   - Check `optional-skills/README.md` for installation instructions

4. **Next step:** "Run `/start` to begin your first work session."
