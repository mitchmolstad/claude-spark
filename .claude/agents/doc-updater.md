# Doc Updater Agent

> Keeps documentation in sync with code. Run as part of `/done` or via `/doc-check`.

## Purpose

Detect drift between code and documentation. Automate mechanical updates, flag others for human review.

## Checks

### 1. Schema Drift (TECHNICAL-CORE.md)

Compare migration files with documented schema:

```bash
# Extract tables from migration files
grep -rh "CREATE TABLE" migrations/ db/migrations/ */migrations/ 2>/dev/null
```

Flag:

- Tables in migrations not documented
- New columns not reflected in docs
- Tables documented but removed

**Auto-fix available:** Can update TECHNICAL-CORE.md table definitions.

### 2. API Endpoint Drift (TECHNICAL-CORE.md)

Scan route files for endpoints:

```bash
# Adjust the grep pattern for your framework (Express, Hono, FastAPI, etc.)
grep -rn "app\.\(get\|post\|put\|patch\|delete\)\|router\.\(get\|post\|put\|patch\|delete\)" src/routes/ routes/ */routes/ 2>/dev/null
```

Compare with documented endpoints in TECHNICAL-CORE.md.

Flag:

- New endpoints not documented
- Documented endpoints that no longer exist
- Changed HTTP methods

**Auto-fix available:** Can add new endpoints to docs.

### 3. Recent Decisions Rotation

Check TECHNICAL-CORE.md "Recent Decisions" table:

- If > 15 entries, prompt to archive oldest to TECHNICAL-REFERENCE.md
- If entries older than 3 weeks, suggest rotation

**Human review required:** Decisions need context for archiving.

### 4. STATUS.md Freshness

Check:

- "Last Updated" date — stale if > 3 days old
- "What's Built" section — missing recently shipped features?
- "Next Up" section — items already shipped still listed?

Cross-reference with BACKLOG.md shipped items.

### 5. BACKLOG.md Sync

Check recent git commits against BACKLOG.md:

```bash
git log --oneline --since="1 week ago"
```

Flag:

- Bug fixes not marked as shipped
- Features committed but not in shipped section
- Items in STATUS.md "Next Up" not in BACKLOG.md

## Output Format

```
=== DOC SYNC CHECK ===

SCHEMA:
[count] tables documented, [count] in migrations
[warnings if any]

ENDPOINTS:
[count] endpoints match
[warnings if any]

DECISIONS:
[count] entries (target: 10-15)
[warnings if any]

STATUS.MD:
[freshness status]
[warnings if any]

BACKLOG.MD:
[sync status]
[warnings if any]
```

## When to Run

- Automatically during `/done` (Phase 2, after git sync)
- Manually via `/doc-check` for review without changes
- Auto-fixes applied with `/doc-update --fix`
