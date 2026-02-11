---
name: code-review
description: Review code for security, performance, error handling, naming, testing, and API design with severity-based findings (CRITICAL/WARNING/SUGGESTION). Use when reviewing pull requests, diffs, or code changes. Pair with the software-principles skill for foundational correctness (KISS, DRY, YAGNI, SOLID).
---

# Code Review

For foundational correctness checks (KISS, DRY, YAGNI, SOLID, SoC, Composition, Demeter, Fail Fast, POLA, Boy Scout), apply the software-principles skill.

This skill covers the review process and categories beyond those principles.

---

## IMPORTANT: Review Standards

**Before you begin, read the detailed review standards:**

All code reviews MUST follow the comprehensive standards defined in `STANDARDS.md` (located in this same directory).

Key requirements:
- Include explicit code references with actual file paths and line numbers
- Show the actual problematic code from the codebase, not generic examples
- Provide complete context and detailed explanations (What, Why, How, Validation)
- Validate that suggested fixes actually resolve the issue without introducing new problems
- Format findings with Current Code, Problem, Impact, Suggested Fix, Why This Fix Works, and Validation sections

Read `STANDARDS.md` now for the complete format and example.

---

## Output Format

Every finding must include severity, location, and a fix:

```
[CRITICAL] src/api/users.ts:42 - SQL injection via string interpolation

  // BAD (current)
  db.query(`SELECT * FROM users WHERE id = '${id}'`);

  // GOOD (fix)
  db.query("SELECT * FROM users WHERE id = $1", [id]);
```

| Level | Meaning | Action |
|-------|---------|--------|
| CRITICAL | Security flaw, data loss, crash, correctness bug | Must fix before merge |
| WARNING | Performance issue, missing error handling, fragile pattern | Should fix before merge |
| SUGGESTION | Readability, naming, minor improvement | Optional, do not block merge |

---

## Review Matrix

| Category | CRITICAL | WARNING | SUGGESTION |
|----------|----------|---------|------------|
| Security | Injection, exposed secrets, auth bypass | Missing input validation, broad permissions | Stricter CSP headers |
| Performance | Unbounded query in request path | N+1 queries, allocation in hot loop | Caching opportunity |
| Error Handling | Empty catch on critical path | Generic catch-all, null error signals | More specific error types |
| Naming | Actively misleading names | Unclear abbreviations, `any` types | Slightly better phrasing |
| Testing | No tests for critical change | Untested business logic path | Brittle assertion style |
| API Design | Breaking public contract | Leaking internal types | Missing deprecation notice |

---

## Before You Review

Read the PR description and linked issue first. Understand *what* changed and *why* before commenting on *how*.

DO NOT start reviewing code without understanding the intent of the change.

---

## 1. Security

DO NOT interpolate user input into queries, commands, HTML, or URLs.
DO NOT commit secrets, tokens, API keys, or credentials.
DO NOT disable auth, CORS, or CSP to work around a bug.

```typescript
// BAD: SQL injection via interpolation
db.query(`SELECT * FROM users WHERE name = '${name}'`);

// GOOD: parameterized query
db.query("SELECT * FROM users WHERE name = $1", [name]);
```

```typescript
// BAD: secret in source control
const API_KEY = "sk_live_9a8b7c6d";

// GOOD: from environment, fail fast if missing
const API_KEY = process.env.API_KEY;
if (!API_KEY) throw new Error("API_KEY environment variable not configured");
```

---

## 2. Performance

DO NOT execute queries inside loops -- batch them.
DO NOT allow unbounded queries without pagination or limits.
DO NOT allocate objects in tight loops when pre-allocation works.

```typescript
// BAD: N+1 query
const users: User[] = [];
for (const id of ids) {
  users.push(await db.findUser(id));
}

// GOOD: single batch query
const users = await db.findUsersByIds(ids);
```

---

## 3. Error Handling

DO NOT use empty catch blocks.
DO NOT return null/undefined as an error signal -- throw or use a Result type.
DO NOT catch generic Error without re-throwing or handling specifically.
DO NOT log errors without context (include IDs, operation name, relevant input).

```typescript
// BAD: swallowed error hides root cause
try { await processPayment(order); } catch (e) {}

// GOOD: contextual re-throw preserves chain
try {
  await processPayment(order);
} catch (error) {
  throw new PaymentError(`Payment failed for order ${order.id}`, { cause: error });
}
```

```typescript
// BAD: null hides the failure mode
function findUser(id: string): User | null {
  try { return db.find(id); }
  catch { return null; }  // caller can't tell "not found" from "DB down"
}

// GOOD: distinct error for each failure mode
function findUser(id: string): User {
  const user = db.find(id);
  if (!user) throw new NotFoundError(`User ${id} not found`);
  return user;
}
```

---

## 4. Naming & Readability

DO NOT use `any` when a concrete type exists.
DO NOT use single-letter variables outside trivial lambdas or loop indices.
DO NOT use magic numbers -- extract named constants.
DO NOT use names that mislead (e.g., `isReady` returning a string).

```typescript
// BAD: opaque names, untyped, magic number
function proc(d: any[], n: number) {
  return d.filter(x => x.score > 3).slice(0, n);
}

// GOOD: intent-revealing names, typed, named constant
const MIN_SCORE = 3;
function getTopScoringItems(items: Item[], count: number): Item[] {
  return items.filter(item => item.score > MIN_SCORE).slice(0, count);
}
```

---

## 5. Testing

DO NOT approve business logic changes without test coverage for new paths.
DO NOT assert on private or internal state -- test observable behavior.
DO NOT write tests that break when internals are safely refactored.

```typescript
// BAD: coupled to internal cache implementation
expect(service.internalMap.size).toBe(2);

// GOOD: tests the public contract
expect(await service.listItems()).toHaveLength(2);
```

```typescript
// BAD: brittle full-object snapshot
expect(result).toMatchSnapshot();

// GOOD: assert specific expected properties
expect(result).toMatchObject({ status: "completed", total: 42 });
```

---

## 6. API & Interface Design

DO NOT remove or rename public fields without a deprecation path.
DO NOT expose internal implementation types through public interfaces.
DO NOT introduce breaking changes without a version bump.

```typescript
// BAD: removed field breaks consumers
interface OrderResponse { id: string; total: number; }

// GOOD: deprecate, add replacement, remove in next major
interface OrderResponse {
  id: string;
  total: number;
  /** @deprecated Use totalAmount instead */
  totalAmount: number;
}
```

---

## Reviewer Constraints

DO NOT nitpick formatting or style that a linter/formatter handles automatically.
DO NOT rewrite the author's working approach in your preferred style.
DO NOT flag issues in code outside the scope of the change.
DO NOT leave vague comments ("this is wrong") -- always explain *why* and suggest a concrete *fix*.
DO NOT block a PR for SUGGESTION-level findings.
