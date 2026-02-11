---
name: software-principles
description: Enforce core software principles (KISS, DRY, YAGNI, SOLID, Separation of Concerns, Composition over Inheritance, Law of Demeter, Fail Fast, Principle of Least Astonishment, Boy Scout Rule). Use when writing, reviewing, or refactoring code.
---

# Software Development Principles

Apply these when writing, reviewing, or refactoring code.

---

## 1. KISS (Keep It Simple, Stupid)

Prefer the straightforward solution. If a junior developer can't understand it in 30 seconds, simplify.

```typescript
// BAD: clever one-liner that obscures intent
const r = data.reduce((a, c) => ({...a, [c.id]: [...(a[c.id]||[]), c]}), {});

// GOOD: clear and readable
const grouped: Record<string, Item[]> = {};
for (const item of data) {
  (grouped[item.id] ??= []).push(item);
}
```

DO NOT introduce patterns, abstractions, or indirection when a straightforward approach works.

---

## 2. DRY (Don't Repeat Yourself)

Duplicated *knowledge* is the enemy -- not duplicated *code*. If the same business rule lives in two places, extract it. Code used only once is not duplication.

```typescript
// BAD: tax rate duplicated across functions
function getOrderTotal(order: Order): number {
  return order.subtotal + order.subtotal * 0.08;
}
function getInvoiceTotal(invoice: Invoice): number {
  return invoice.subtotal + invoice.subtotal * 0.08;
}

// GOOD: single source of truth
const TAX_RATE = 0.08;
function applyTax(amount: number): number {
  return amount * TAX_RATE;
}
```

DO NOT duplicate business logic across functions or layers.
DO NOT abstract prematurely -- similarity is not duplication.

---

## 3. YAGNI (You Aren't Gonna Need It)

Build only what is required now. Speculative features add complexity and maintenance for zero current value.

```typescript
// BAD: parameters for hypothetical future callers
interface MessageSender {
  send(msg: string, channel: string, priority: number,
       retries: number, encryption: boolean): void;
}

// GOOD: only what's needed today
interface MessageSender {
  send(msg: string, channel: string): void;
}
```

DO NOT add parameters, configuration, extension points, or abstractions for requirements that do not yet exist.

---

## 4. SOLID

| Principle | Rule | Common Violation |
|-----------|------|------------------|
| SRP | One class = one reason to change | Class that generates AND emails reports |
| OCP | Add behavior via new code, not edits | if/else chain for each new type |
| LSP | Subtypes honor parent contracts | Square.setWidth breaks Rectangle |
| ISP | Small, focused interfaces | Interface forcing unused method stubs |
| DIP | Accept dependencies, don't create them | `new PostgresDB()` inside a service |

```typescript
// OCP: extend via polymorphism, not conditionals
interface Shape { area(): number; }
class Circle implements Shape {
  constructor(private radius: number) {}
  area(): number { return Math.PI * this.radius ** 2; }
}

// DIP: depend on abstractions
// BAD
class OrderService { private db = new PostgresDatabase(); }
// GOOD
class OrderService { constructor(private db: Database) {} }

// ISP: focused interfaces
// BAD
interface Worker { work(): void; eat(): void; sleep(): void; }
// GOOD
interface Workable { work(): void; }
interface Feedable { eat(): void; }
```

DO NOT create god classes with multiple unrelated responsibilities.
DO NOT force consumers to depend on methods they don't use.
DO NOT hard-code concrete dependencies inside classes.

---

## 5. Separation of Concerns

Each function or module owns one job. If you say "and" when describing what it does, split it.

```typescript
// BAD: validate AND transform AND persist in one function
function processUser(input: unknown): void {
  if (!input.name) throw new Error("missing name");
  const user = { name: input.name.trim().toLowerCase() };
  db.insert("users", user);
}

// GOOD: separate concerns
function validateUser(input: unknown): UserInput { /* ... */ }
function normalizeUser(input: UserInput): User { /* ... */ }
function saveUser(user: User): void { /* ... */ }
```

DO NOT mix I/O, business logic, and presentation in the same function.

---

## 6. Composition over Inheritance

Build behavior by composing small, focused objects. Use inheritance only for true "is-a" relationships.

```typescript
// BAD: deep hierarchy for behavior reuse
class Animal {}
class FlyingAnimal extends Animal { fly() {} }
class SwimmingFlyingAnimal extends FlyingAnimal { swim() {} }

// GOOD: compose capabilities
class Duck implements CanFly, CanSwim {
  constructor(private flyer: Flyer, private swimmer: Swimmer) {}
  fly(): void { this.flyer.fly(); }
  swim(): void { this.swimmer.swim(); }
}
```

DO NOT use inheritance to share code between unrelated concepts. Prefer delegation and interfaces.

---

## 7. Law of Demeter (Principle of Least Knowledge)

Talk only to immediate collaborators. One dot, not three.

```typescript
// BAD: reaching through the object graph
function getCity(order: Order): string {
  return order.getCustomer().getAddress().getCity();
}

// GOOD: ask the direct collaborator
function getCity(order: Order): string {
  return order.shippingCity();
}
```

DO NOT chain method calls beyond one level on objects you didn't create or receive as a parameter.

---

## 8. Fail Fast

Validate inputs at the boundary. Reject invalid state immediately with clear, contextual errors.

```typescript
// BAD: silent failure, bad data propagates
function processAge(age: number): string {
  if (age < 0) return "";
  return `Age: ${age}`;
}

// GOOD: fail immediately
function processAge(age: number): string {
  if (age < 0) throw new Error(`Invalid age: ${age}. Must be non-negative.`);
  return `Age: ${age}`;
}
```

DO NOT silently swallow errors, return default values for invalid input, or use empty catch blocks.

---

## 9. Principle of Least Astonishment (POLA)

Names must match behavior. No hidden side effects.

```typescript
// BAD: getUser() secretly writes to the database
function getUser(id: string): User {
  const user = db.find(id);
  user.lastAccessed = Date.now();
  db.save(user);
  return user;
}

// GOOD: separate query from command
function getUser(id: string): User {
  return db.find(id);
}
function recordAccess(user: User): void {
  user.lastAccessed = Date.now();
  db.save(user);
}
```

DO NOT introduce side effects in functions named get/find/is/has/check.
DO NOT mutate input parameters.

---

## 10. Boy Scout Rule

Leave every file you touch a little better. Rename unclear variables, remove dead code, add missing types.

```typescript
// BEFORE: touched to fix a bug
function proc(d: any, f: boolean) {
  let temp = 0;
  if (f == true) return d.val + d.val2;
  return d.val;
}

// AFTER: fixed the bug AND cleaned up
function calculateTotal(data: PriceData, includeExtras: boolean): number {
  if (includeExtras) return data.basePrice + data.extrasPrice;
  return data.basePrice;
}
```

DO NOT leave dead code, `any` types, or unclear names in code you actively modified.
