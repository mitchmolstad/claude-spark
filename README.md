# claude-spark

A project template for Claude Code that gives your AI collaborator persistent memory across sessions.

Run `/spark` once. Get a full documentation scaffold. Every session after that, Claude knows exactly where you left off.

## What It Does

Claude Code has no memory between sessions. This template solves that with structured documentation that acts as external memory, plus skills that automate the read/write cycle.

**`/spark`** interviews you about your project and generates:

| File                   | Purpose                                    |
| ---------------------- | ------------------------------------------ |
| `SPEC.md`              | Requirements, success criteria, scope      |
| `CLAUDE.md`            | Project constitution and interaction rules |
| `STATUS.md`            | Current phase and next priorities          |
| `TECHNICAL-CORE.md`    | Schema, endpoints, recent decisions        |
| `docs/ARCHITECTURE.md` | System design and guardrails               |
| `BACKLOG.md`           | Prioritized roadmap                        |
| `PROJECT-LOG.md`       | Session history                            |

## The Session Lifecycle

```
/start → WORK → /deploy-and-verify → /done → /clear
  │        │           │                │        │
  ▼        ▼           ▼                ▼        ▼
Hydrate  Execute    Validate        Persist   Release
Context  Tasks      Changes         State     Memory
```

1. **`/start`** — Reads your docs, confirms schema/endpoints are in context, identifies the next task
2. **Work** — Build features, fix bugs, stay on the critical path
3. **`/deploy-and-verify`** — Git sync, build, deploy, verify in production
4. **`/done`** — Updates all documentation with what happened this session
5. **`/clear`** — Resets the context window (state is safely persisted in markdown)

## Getting Started

1. **Clone this template:**

   ```bash
   gh repo create my-project --template your-username/claude-spark --clone
   cd my-project
   ```

2. **Run the spark interview:**

   ```
   /spark
   ```

3. **Start your first session:**
   ```
   /start
   ```

## Skills Reference

### Core Skills (included)

| Skill                            | Purpose                                       |
| -------------------------------- | --------------------------------------------- |
| `/spark`                         | One-time project interview and doc generation |
| `/start`                         | Begin session, hydrate context                |
| `/done`                          | End session, persist state to docs            |
| `/strategy`                      | Strategic advisor conversation (no code)      |
| `/deploy-and-verify`             | Git sync, build, deploy, verify               |
| `systematic-debugging`           | Root-cause-first debugging process            |
| `test-driven-development`        | Red-green-refactor TDD workflow               |
| `verification-before-completion` | Evidence before claims                        |
| `dispatching-parallel-agents`    | Parallel agent task delegation                |
| `frontend-design`                | Distinctive, production-grade UI design       |

### Optional Skills

Copy from `optional-skills/` into `.claude/skills/` if relevant:

| Skill                  | Use If                                              |
| ---------------------- | --------------------------------------------------- |
| `design-audit`         | Your project has a UI (UX heuristic evaluation)     |
| `react-best-practices` | You're using React/Next.js (Vercel perf guidelines) |

See [`optional-skills/README.md`](optional-skills/README.md) for installation.

### Agents

| Agent               | Purpose                                      |
| ------------------- | -------------------------------------------- |
| `doc-updater`       | Detects drift between code and documentation |
| `security-reviewer` | Pre-deploy vulnerability scan                |

## How It Works

The workflow is built on one insight: **AI collaboration scales through externalized structure, not internal memory.**

By encoding project knowledge in version-controlled markdown:

- Knowledge survives across sessions
- Knowledge survives model upgrades
- Knowledge can be reviewed and corrected by humans
- Knowledge accumulates rather than resets

See [`docs/WORKFLOW-ARCHITECTURE.md`](docs/WORKFLOW-ARCHITECTURE.md) for the full philosophy and architecture.

## Origin

Extracted from the [Scraps](https://scraps.kitchen) project, where this workflow powered 112 features shipped across 4 weeks of occasional solo development with Claude Code.

## Acknowledgments

The workflow patterns in this template were shaped by ideas and practices from across the Claude Code community:

- **[Superpowers](https://github.com/obra/superpowers)** by Jesse Vincent — Core engineering skills (systematic-debugging, TDD, verification-before-completion, dispatching-parallel-agents, frontend-design)
- **[@seconds_0](https://x.com/seconds_0)** — The spark interview concept was inspired by their project onboarding prompt
- **[Boris Cherny](https://github.com/anthropics/claude-code-best-practices)** / Anthropic — Claude Code best practices and CLAUDE.md conventions
- **[Tommy Geoco](https://github.com/tommygeoco/ui-audit)** — Design audit skill (included in optional-skills, MIT licensed)
- **[Vercel Engineering](https://github.com/vercel)** — React best practices skill (included in optional-skills, MIT licensed)
- The broader Claude Code community on X/Twitter and GitHub, whose shared experiments with skills, agents, and session workflows informed many of the patterns here

## License

MIT
