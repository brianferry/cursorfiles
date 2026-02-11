# Code Review Standards

When performing code reviews (either manually or via code-review subagents), ALL findings MUST include:

## 1. Explicit Code References

Every finding must reference the exact location in the codebase:

```
[SEVERITY] path/to/file.ts:startLine-endLine - Description
```

Include the actual code from the file being reviewed, not just generic examples.

## 2. Complete Context

**BAD (current)** section must show:
- The actual problematic code from the codebase
- Surrounding context (2-3 lines before/after if relevant)
- Why it's problematic with specific impact

**GOOD (fix)** section must show:
- The complete corrected code
- All necessary imports or dependencies
- Comments explaining the key changes

## 3. Detailed Explanations

Every finding must include:
- **What**: What is wrong with the current code
- **Why**: Why this matters (impact on functionality, security, performance, etc.)
- **How**: How the suggested fix resolves the issue
- **Validation**: Confirm the fix actually works and doesn't introduce new issues

## 4. Fix Validation

Before suggesting a fix, mentally verify:
- Does this fix actually resolve the stated problem?
- Does it introduce any new issues?
- Are there side effects or edge cases?
- Is the fix complete or does it require additional changes?

## Example Format

```
[CRITICAL] apps/my-app/src/component.ts:45-48 - SQL injection vulnerability

**Current Code (lines 45-48):**
```typescript
async function getUser(userId: string) {
  const query = `SELECT * FROM users WHERE id = '${userId}'`;
  return await db.execute(query);
}
```

**Problem:**
User input `userId` is directly interpolated into the SQL query string, allowing attackers to inject malicious SQL. For example, `userId = "1' OR '1'='1"` would return all users.

**Impact:**
- Attackers can read sensitive data from any table
- Potential for data modification or deletion
- Critical security vulnerability that must be fixed before merge

**Suggested Fix:**
```typescript
async function getUser(userId: string) {
  // Use parameterized queries to prevent SQL injection
  const query = 'SELECT * FROM users WHERE id = $1';
  return await db.execute(query, [userId]);
}
```

**Why This Fix Works:**
The database driver treats `$1` as a placeholder and safely escapes the `userId` parameter. User input is never interpreted as SQL code, preventing injection attacks.

**Validation:**
- ✅ Eliminates SQL injection vector
- ✅ Maintains same functionality
- ✅ No new issues introduced
- ✅ Follows best practices for database queries
```

## Requirements for Subagents

When using `code-review-orchestrator` or `code-reviewer` subagents:

1. **Provide full file context** to the subagent so it can reference actual code
2. **Expect detailed findings** with all sections above
3. **Validate fixes** - the auditor must verify each suggested fix actually resolves the issue
4. **Include line numbers** from the actual diff or file being reviewed
5. **Explain impact** - don't just say "this is wrong", explain the consequences

## Severity Guidelines

- **CRITICAL**: Security flaw, data loss, crash, correctness bug that makes the feature not work
- **WARNING**: Performance issue, missing error handling, fragile pattern, likely runtime errors
- **SUGGESTION**: Readability, naming, minor improvement, stylistic preferences
