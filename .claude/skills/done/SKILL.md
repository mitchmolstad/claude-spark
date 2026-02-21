---
name: done
description: End the session by syncing all documentation with this session's progress
disable-model-invocation: true
---

End the session by syncing all documentation with this session's progress.

## Steps

### 1. Git Sync (Code First, Then Docs)

Before updating documentation, ensure code changes are committed and pushed:

1. Run `git status` to check for uncommitted changes
2. If there are code changes:
   - Stage all relevant files
   - Commit with a descriptive message:

     ```
     git commit -m "$(cat <<'EOF'
     [Brief description of what changed]

     Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
     EOF
     )"
     ```

   - Push to GitHub: `git push`

3. Confirm the sync status (commit hash or "no code changes")

**Note:** If a deploy was run earlier this session, code may already be pushed. Just verify with `git status`.

### 2. STATUS.md

- Set "Last Updated" to today's date with a brief summary
- Update "What's Built" if new features were shipped
- Update "Next Up" to reflect completed work and new priorities
- **Keep STATUS.md lean**: If completed items older than ~1 week exist, migrate them to PROJECT-LOG.md

### 3. TECHNICAL-CORE.md (if applicable)

- Add new database tables/columns to schema section
- Add new API endpoints
- Add entries to "Recent Decisions" (keep 10-15 most recent)
- **If >15 decisions**: Move older entries to TECHNICAL-REFERENCE.md

### 4. TECHNICAL-REFERENCE.md (if applicable)

- Receive archived decisions from TECHNICAL-CORE.md
- Update deployment or dev workflow notes if changed

### 5. ARCHITECTURE.md (rarely needed)

ARCHITECTURE.md is constitutional — it defines vision, patterns, and boundaries, not feature inventory.

**Only update if this session changed:**

- The Core Loop (new fundamental step in user journey)
- Architectural Patterns (new pattern all features should follow)
- Boundaries (where concerns live)
- Future Hooks (new monetization/scale consideration)

Most sessions won't touch this file. Feature additions go in STATUS.md and TECHNICAL-CORE.md.

### 6. BACKLOG.md (CRITICAL — cross-reference STATUS.md first)

**First**: Read BOTH STATUS.md AND BACKLOG.md to identify discrepancies:

- Check STATUS.md "Next Up" sections for bugs that may not exist in BACKLOG.md
- Any bug in STATUS.md MUST also be in BACKLOG.md

**Then**: Identify work completed:

- Which items from backlog were completed this session
- Which items in "Shipped" need to be added
- Any new bugs or tasks discovered that need to be logged

**Then**: Make the edits:

- Add any missing bugs from STATUS.md to BACKLOG.md
- Move completed items to "Shipped" section with `[x]` and completion date
- Update "Last Reviewed" date

**Verify**: Confirm aloud:

1. How many bugs were synced from STATUS.md to BACKLOG.md
2. What was marked shipped
3. What new bugs were added

### 7. CLAUDE.md Known Gotchas (if applicable)

If this session uncovered a significant "need to know" about working on the project (platform constraints, API quirks, patterns that don't work), add it to the Known Gotchas section of CLAUDE.md. Keep entries terse — one line per gotcha. Only add things that would save future sessions from repeating a mistake.

### 8. PROJECT-LOG.md

Add a new dated entry summarizing:

- What happened
- Technical decisions made
- Features built
- Files created/modified
- Bugs discovered (if any)
- Outcome

### 9. Final Confirmation

Confirm to the user with a summary:

```
Git: [commit hash pushed, or "already synced", or "no code changes"]
STATUS.md: [brief summary]
TECHNICAL-CORE.md: [updates or "no changes"]
BACKLOG.md: [X items shipped, Y bugs added]
PROJECT-LOG.md: [entry added]

Ready to /clear.
```

Note: ARCHITECTURE.md is constitutional and rarely updated. Only mention it if vision/patterns/boundaries changed.
