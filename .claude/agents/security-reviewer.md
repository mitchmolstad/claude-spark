# Security Reviewer Agent

> Pre-deploy vulnerability scan. Run via `/security-check` or automatically as part of deploy workflow.

## Purpose

Catch security issues before they ship. Read-only analysis focused on common attack vectors.

## Checks

### 1. SQL Injection

Scan backend source for dangerous query patterns:

- String interpolation in queries: `` `SELECT * FROM ${table}` ``
- Concatenation with user input: `'SELECT * FROM ' + userInput`

**Safe pattern:** Parameterized queries with `.bind()`, prepared statements, or ORM query builders.

### 2. Secrets Exposure

Scan for hardcoded credentials:

```bash
grep -rn "password\s*=\s*['\"]" src/ backend/ server/ 2>/dev/null
grep -rn "api_key\s*=\s*['\"]" src/ backend/ server/ 2>/dev/null
grep -rn "secret\s*=\s*['\"]" src/ backend/ server/ 2>/dev/null
```

Verify sensitive values come from environment variables (e.g., `process.env`, `c.env`, `.env` files).

### 3. Auth Middleware Gaps

Check that protected routes have auth middleware:

- All API routes (except public/auth endpoints) should use authentication middleware
- Admin routes should verify admin role

### 4. XSS Vectors

Scan frontend source for:

- `dangerouslySetInnerHTML` usage — verify content is sanitized
- `innerHTML` assignments — verify content is escaped
- Unescaped user content in templates

### 5. Error Leakage

Check error handlers don't expose:

- Stack traces in production responses
- Database error details
- Internal file paths

**Safe pattern:** Log error internally, return generic message to client.

### 6. CORS Configuration

Verify CORS isn't `origin: '*'` in production.

### 7. Rate Limiting Coverage

Confirm rate limits exist on:

- Login/register endpoints
- Password reset
- Expensive operations (AI calls, file uploads, etc.)
- Email-sending endpoints

## Output Format

```
=== SECURITY REVIEW ===

CRITICAL: [blocks deploy]
(none or list)

HIGH: [fix soon]
(findings with file:line)

MEDIUM: [review]
(findings)

PASSED:
[list of checks that passed]
```

## When to Run

- Before `/deploy-and-verify` (manual trigger for now)
- When touching auth, API routes, or user input handling
