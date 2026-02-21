# Claude Code Workflow Architecture

> The cognitive scaffolding that enables stateless AI to maintain project continuity across sessions.

---

## The Core Problem

Claude Code (like all LLMs) has no persistent memory. Each session starts blank. Yet complex software projects require:

- Deep understanding of existing architecture
- Accumulated decisions and their rationale
- Protection against architectural drift
- Continuity of work across time

**This workflow solves that problem through structured documentation that serves as "external memory."**

---

## The Session Lifecycle

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           CLAUDE CODE SESSION LOOP                          │
└─────────────────────────────────────────────────────────────────────────────┘

  ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
  │  /start  │────▶│   WORK   │────▶│  DEPLOY  │────▶│  /done   │────▶│  /clear  │
  │          │     │          │     │ & VERIFY │     │          │     │          │
  └──────────┘     └──────────┘     └──────────┘     └──────────┘     └──────────┘
       │                │                │                │                │
       │                │                │                │                │
       ▼                ▼                ▼                ▼                ▼
  ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
  │ Hydrate  │     │ Execute  │     │ Validate │     │ Persist  │     │ Release  │
  │ Context  │     │ Tasks    │     │ Changes  │     │ State    │     │ Memory   │
  └──────────┘     └──────────┘     └──────────┘     └──────────┘     └──────────┘
```

---

## Phase 1: Context Hydration (`/start`)

**Purpose:** Transform a blank-slate AI into a project-aware collaborator.

### The "Project Map" (Always Read)

These files load every session to prevent hallucinating schemas or endpoints:

| File                | Content                                                |
| ------------------- | ------------------------------------------------------ |
| `CLAUDE.md`         | Project constitution, interaction protocol, guardrails |
| `STATUS.md`         | Current phase, production URLs, next priorities        |
| `TECHNICAL-CORE.md` | Database schema, API endpoints, recent decisions       |

### Conditional Loading

```
IF task is new feature:
    ALSO READ → docs/ARCHITECTURE.md (data models, system design)

IF debugging regression:
    ALSO READ → TECHNICAL-REFERENCE.md (decision history)
    ALSO READ → PROJECT-LOG.md (session history)
```

### Cognitive State After `/start`

```
┌─────────────────────────────────────────────────────────────────┐
│                    CLAUDE'S MENTAL MODEL                        │
├─────────────────────────────────────────────────────────────────┤
│  ✓ Database schema and relationships                            │
│  ✓ API endpoints and their behaviors                            │
│  ✓ Current production state                                     │
│  ✓ Recent decisions and their rationale                         │
│  ✓ Known gotchas (platform quirks, naming issues, caching)      │
│  ✓ Next priority from backlog                                   │
│  ✓ Interaction style (calibrated to user)                       │
│  ✓ Quality bar (test before showing, never show broken)         │
└─────────────────────────────────────────────────────────────────┘
```

---

## Phase 2: Work Execution

**Purpose:** Execute tasks while maintaining architectural alignment.

### Task Selection Logic

```
1. Check STATUS.md "Next Up" section
2. User confirms priority OR provides different task
3. If new feature → read ARCHITECTURE.md
4. If potential drift → request Plan Review
```

### The Guardrail (Anti-Drift)

Every feature implementation passes through an alignment check:

```
                    PROPOSED IMPLEMENTATION
                            │
                            ▼
               ┌────────────────────────┐
               │  Does this fit the     │
               │  long-term roadmap?    │
               │                        │
               │  (Scaling,             │
               │   monetization,        │
               │   future features)     │
               └────────────────────────┘
                     │           │
                    YES          NO
                     │           │
                     ▼           ▼
              ┌──────────┐  ┌──────────────────┐
              │ Proceed  │  │ HALT!            │
              │ with     │  │ Request          │
              │ impl     │  │ "Plan Review"    │
              └──────────┘  │                  │
                            │ This solves the  │
                            │ immediate task   │
                            │ but is shaped    │
                            │ wrong for the    │
                            │ future           │
                            └──────────────────┘
```

### Implementation Standards

```
┌─────────────────────────────────────────────────────────────────┐
│                     QUALITY CHECKS                              │
├─────────────────────────────────────────────────────────────────┤
│  • Test everything before showing user                          │
│  • Describe changes in experience terms, not code terms         │
│  • Fix broken builds/tests immediately (self-correction)        │
│  • Guard API keys and validate all inputs                       │
│  • Follow framework-specific best practices                     │
└─────────────────────────────────────────────────────────────────┘
```

---

## Phase 3: Deploy & Verify (`/deploy-and-verify`)

**Purpose:** Ship code to production and validate it works.

### Execution Order

```
┌──────────────────────────────────────────────────────────────────┐
│  1. GIT SYNC (Before Deploy)                                     │
│     • git status → stage → commit → push                         │
│     • Code saved to GitHub BEFORE deployment                     │
├──────────────────────────────────────────────────────────────────┤
│  2. BUILD & DEPLOY                                               │
│     • Run build command(s)                                       │
│     • Deploy to hosting platform                                 │
├──────────────────────────────────────────────────────────────────┤
│  3. VERIFY (Browser or API Testing)                              │
│     • Navigate to production URL                                 │
│     • Test core user flow end-to-end                             │
│     • Screenshot evidence                                        │
├──────────────────────────────────────────────────────────────────┤
│  4. REPORT                                                       │
│     • Git sync status (commit hash)                              │
│     • Deploy status (success/failure)                            │
│     • Verification status (what worked/failed)                   │
└──────────────────────────────────────────────────────────────────┘
```

---

## Phase 4: State Persistence (`/done`)

**Purpose:** Encode session learnings into documentation for future sessions.

### Documentation Update Flow

```
                          SESSION WORK COMPLETE
                                  │
          ┌───────────────────────┼───────────────────────┐
          ▼                       ▼                       ▼
    ┌──────────┐           ┌──────────┐           ┌──────────┐
    │ STATUS   │           │ TECHNICAL│           │ BACKLOG  │
    │   .md    │           │ -CORE.md │           │   .md    │
    └──────────┘           └──────────┘           └──────────┘
          │                       │                       │
    "Last Updated"          New schemas?           Mark shipped
    "What's Built"          New endpoints?         Add new bugs
    "Next Up"               New decisions?         Update status
          │                       │                       │
          └───────────────────────┼───────────────────────┘
                                  │
                                  ▼
                          ┌──────────────┐
                          │ PROJECT-LOG  │
                          │     .md      │
                          │              │
                          │ Dated entry: │
                          │ • What       │
                          │ • Decisions  │
                          │ • Files      │
                          │ • Outcome    │
                          └──────────────┘
```

### State Migration Rules

| From                        | To                     | Trigger                  |
| --------------------------- | ---------------------- | ------------------------ |
| STATUS.md completed items   | PROJECT-LOG.md         | Items older than ~1 week |
| TECHNICAL-CORE.md decisions | TECHNICAL-REFERENCE.md | More than 15 decisions   |

---

## Phase 5: Memory Release (`/clear`)

**Purpose:** Reset token usage while state is safely persisted.

```
                    BEFORE /clear
         ┌────────────────────────────────┐
         │  Context window: 85% full      │
         │  Token tax: High               │
         │  State: In conversation memory │
         └────────────────────────────────┘
                        │
                        ▼
                    AFTER /clear
         ┌────────────────────────────────┐
         │  Context window: Fresh         │
         │  Token tax: Minimal            │
         │  State: Persisted in markdown  │
         └────────────────────────────────┘
```

**Critical:** `/done` MUST run before `/clear`. Otherwise, session work is lost.

---

## The Documentation Hierarchy

```
                         ┌───────────────┐
                         │   CLAUDE.md   │
                         │ (Constitution)│
                         └───────┬───────┘
                                 │
         ┌───────────────────────┼───────────────────────┐
         │                       │                       │
         ▼                       ▼                       ▼
   ┌───────────┐          ┌───────────┐          ┌───────────┐
   │ STATUS.md │          │ TECHNICAL │          │ BACKLOG   │
   │  (State)  │          │ -CORE.md  │          │   .md     │
   │           │          │ (Schema)  │          │ (Roadmap) │
   │ • Phase   │          │ • Tables  │          │ • Shipped │
   │ • Builds  │          │ • APIs    │          │ • Pending │
   │ • Next Up │          │ • Recent  │          │ • Deferred│
   └───────────┘          └───────────┘          └───────────┘
         │                       │
         │              ┌────────┴────────┐
         │              │                 │
         ▼              ▼                 ▼
   ┌───────────┐  ┌───────────┐    ┌───────────┐
   │ PROJECT   │  │ TECHNICAL │    │ ARCHITEC- │
   │ -LOG.md   │  │-REFERENCE │    │ TURE.md   │
   │ (History) │  │ (Full Log)│    │ (Design)  │
   │           │  │           │    │           │
   │ Read for  │  │ Read for  │    │ Read for  │
   │ debugging │  │ decisions │    │ features  │
   └───────────┘  └───────────┘    └───────────┘
```

### Read Frequency by File

| File                   | Read Frequency      | Purpose                               |
| ---------------------- | ------------------- | ------------------------------------- |
| CLAUDE.md              | Every session       | Interaction rules, guardrails         |
| STATUS.md              | Every session       | Current state, priorities             |
| TECHNICAL-CORE.md      | Every session       | Schema, endpoints (never hallucinate) |
| ARCHITECTURE.md        | New features only   | Data models, system design            |
| TECHNICAL-REFERENCE.md | Debugging only      | Full decision history                 |
| PROJECT-LOG.md         | Debugging only      | Session-by-session history            |
| BACKLOG.md             | Task selection only | Roadmap, prioritization               |

---

## The Virtuous Cycle

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CROSS-SESSION LEARNING                              │
└─────────────────────────────────────────────────────────────────────────────┘

    SESSION N                    SESSION N+1                   SESSION N+2
        │                            │                             │
        ▼                            ▼                             ▼
   ┌─────────┐                  ┌─────────┐                   ┌─────────┐
   │  Work   │                  │  Work   │                   │  Work   │
   │  Done   │                  │  Done   │                   │  Done   │
   └────┬────┘                  └────┬────┘                   └────┬────┘
        │                            │                             │
        ▼                            ▼                             ▼
   ┌─────────┐                  ┌─────────┐                   ┌─────────┐
   │ /done   │                  │ /done   │                   │ /done   │
   │ writes  │                  │ writes  │                   │ writes  │
   └────┬────┘                  └────┬────┘                   └────┬────┘
        │                            │                             │
        └────────────────────────────┴─────────────────────────────┘
                                     │
                                     ▼
                         ┌───────────────────────┐
                         │    MARKDOWN FILES     │
                         │                       │
                         │  Accumulated:         │
                         │  • Schema evolution   │
                         │  • Decision history   │
                         │  • Known gotchas      │
                         │  • Shipped features   │
                         │  • Bug fixes          │
                         │  • Architectural      │
                         │    guardrails         │
                         └───────────────────────┘
                                     │
                    ┌────────────────┴────────────────┐
                    ▼                                 ▼
             Future sessions              Future projects
             inherit ALL                  can fork this
             accumulated                  scaffolding
             wisdom                       structure
```

---

## Key Insights

### 1. Documentation as Memory

Unlike human developers who carry context in their heads, Claude Code carries context in markdown files. The discipline of `/done` ensures nothing is lost.

### 2. Tiered Loading Reduces Token Tax

Only the essential files load every session. Deep history loads only when needed. This keeps per-session context lean and efficient.

### 3. Known Gotchas Prevent Repeat Mistakes

The "Known Gotchas" section in CLAUDE.md captures hard-won lessons — platform quirks, naming artifacts, caching issues, framework constraints. Each gotcha saves future sessions from repeating a mistake.

### 4. The Guardrail Prevents Drift

Short-term fixes that don't fit long-term architecture are flagged before implementation. Code that solves the immediate problem but is shaped wrong for the future gets a Plan Review.

### 5. Verification Closes the Loop

Browser testing or API verification ensures changes actually work in production — not just theoretically correct code.

---

## Workflow Commands Reference

| Command              | Phase | Purpose                                |
| -------------------- | ----- | -------------------------------------- |
| `/spark`             | Init  | One-time project interview + doc gen   |
| `/start`             | Begin | Hydrate context, identify next task    |
| `/deploy-and-verify` | Ship  | Git sync → build → deploy → verify     |
| `/done`              | End   | Persist state to all documentation     |
| `/clear`             | Reset | Release memory, ready for next session |

---

## Philosophical Foundation

This workflow embodies a key insight: **AI collaboration scales through externalized structure, not internal memory.**

By encoding project knowledge in version-controlled markdown:

- Knowledge survives across sessions
- Knowledge survives model upgrades
- Knowledge can be reviewed and corrected by humans
- Knowledge accumulates rather than resets

The result is a development partnership where the AI brings capabilities and the documentation brings continuity.

---

_Evolved from 112 features shipped with the Scraps project (scraps.kitchen). Open-sourced as part of claude-spark._
