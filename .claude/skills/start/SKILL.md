---
name: start
description: Begin a new session by hydrating project context and identifying the next task
disable-model-invocation: true
---

Begin a new session by hydrating project context and identifying the next task.

## Conditional Loading Protocol

### Always Read (The Project Map)

Read these files every session — we cannot risk hallucinating the schema or existing endpoints:

- `CLAUDE.md` (project constitution)
- `STATUS.md` (current state and next priorities)
- `TECHNICAL-CORE.md` (schema, API endpoints, recent decisions)

### Contextual Read

- If the task is a **new feature**: Also read `docs/ARCHITECTURE.md`
- If **debugging a regression**: Also read `TECHNICAL-REFERENCE.md` for full decision history

### Reference Only (Do Not Read Every Session)

- `BACKLOG.md` — Read only for task selection or prioritization
- `PROJECT-LOG.md` — Read only when historical context is missing
- `TECHNICAL-REFERENCE.md` — Full decision log, deployment details

## Steps

1. Read the Always Read files (CLAUDE.md, STATUS.md, TECHNICAL-CORE.md)

2. Confirm production state from STATUS.md — note the current deploy status, live URLs (if any), and stack.

3. Confirm that the database schema and API endpoints are in context from TECHNICAL-CORE.md.

4. Review the "Next Up" section in STATUS.md and identify the top priority task.

5. Briefly summarize:
   - What's currently built
   - What's next on the roadmap
   - Ask if the user wants to proceed with the top priority or has something else in mind

6. Confirm that you have the current schema and endpoints in context.

7. Add a reminder: "Consider whether this session needs Plan Mode for any non-trivial features."

Keep it concise.
