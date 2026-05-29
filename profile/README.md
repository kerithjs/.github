<div align="center">

<img src="logo.svg" alt="Kerith" width="250" height="250" />

# KerithJS

**The architectural standard for Node.js and TypeScript.**

_What Spring Boot did for Java — built natively for Node._

[![npm](https://img.shields.io/npm/v/@kerith/core?color=e4f222&label=%40kerith%2Fcore&style=flat-square)](https://www.npmjs.com/package/@kerith/core)
[![npm](https://img.shields.io/npm/v/@kerith/identifiers?color=e4f222&label=%40kerith%2Fidentifiers&style=flat-square)](https://www.npmjs.com/package/@kerith/identifiers)
[![npm](https://img.shields.io/npm/v/@kerith/eslint-plugin?color=e4f222&label=%40kerith%2Feslint&style=flat-square)](https://www.npmjs.com/package/@kerith/eslint-plugin)
[![License: MIT](https://img.shields.io/badge/license-MIT-e4f222?style=flat-square)](./LICENSE)
[![kerith.dev](https://img.shields.io/badge/docs-kerith.dev-e4f222?style=flat-square)](https://docs.kerith.dev)

</div>

---

Every serious Node.js project eventually builds the same things: a consistent module structure, a dependency graph someone has to maintain manually, guards that are really just middleware with a different name, configuration scattered across environment files. The architecture that makes it all hold together is invented from scratch, every time, by whoever was on the team when the project started.

Kerith is what happens when you stop inventing that and start building on a standard.

---

## What Kerith is

Kerith is an opinionated backend framework for Node.js and TypeScript. It gives every project a predictable, auditable architecture — inferred from the filesystem, enforced by tooling, and legible to both developers and AI agents without reading every file.

A project built with Kerith is recognizable. A developer who knows Kerith can enter any Kerith project and be productive. The architecture is not documented somewhere — it is the code.

That is what Spring Boot did for Java enterprise. Kerith does the same for Node.js and TypeScript, built from first principles for how Node.js actually works — not borrowed from Java and ported over.

---

## The core principles

**The code is yours.** Kerith can be removed from a project and what remains is Express and TypeScript. Handlers are functions. Services are classes. The server is Express. What Kerith adds is structure and memory — not a cage. Adopt it without surrendering ownership.

**Structure must be visible.** The dependency graph is not a diagram in a wiki. It is the real state of the system right now, generated from the code that already exists. `kerith check` tells you what modules touch the database, what routes have no guards, what modules have high coupling — in seconds, without reading a single file.

**Magic has a cost.** Every time a framework does something you cannot see, you take on debt. Kerith is explicit. The bootstrap is deterministic. The pipeline is declarative. When something fails at 3am, the stack trace points to your code — not to framework internals.

**Runtime zero.** Kerith pays its cost at bootstrap, then disappears. No DI container resolving dependencies on every request. No reflection. No overhead. What runs is your code on Express. Cold starts are 80–120ms. That is not a benchmark — it is a consequence of the design.

**Observability is not optional.** A system that cannot explain itself cannot be improved. The module graph, NITS identity tracking, and `kerith check` are part of the core, not add-ons. The system is designed to be auditable from day one.

**The filesystem is the source of truth.** Hierarchy is inferred, not declared. A file at `src/billing/payments/index.ts` belongs to the `payments` module of the `billing` domain. The code does not repeat that information. The filesystem and the code cannot contradict each other.

---

## What makes Kerith different

### NITS — Node Identity Tracking System

Every module has a persistent ID stored in a Shadow File. That ID survives renames, moves, and domain changes. When `src/modules/payments` becomes `src/billing/payments`, Kerith knows it is the same module — because the Shadow File traveled with it.

The project has architectural memory. Kerith can answer questions no other Node.js tool can answer:

- When was this module created?
- How many times has it moved?
- Which domain did it belong to before?
- Which modules have had the most structural churn in the last six months?

That memory is the foundation of long-term architectural observability.

### Identifiers — not decorators

Kerith classifies every entity in the system through identifiers. They are not decorators. They do not use reflection. They are valid TypeScript functions that name what something is and, when behavior is activated, generate exactly that behavior.

```ts
// Classifies. Generates alias @client/db. No magic.
export const db = Client('db', drizzle(connection));

// Classifies. Produces a real Express middleware.
export const authGuard = Guard('auth', (req, res, next) => { ... });

// Classifies. Registers a cron schedule at bootstrap.
export const cleanup = Cron('0 2 * * *', async () => { ... });
```

Identifiers that only classify have zero runtime cost. Identifiers with activated behavior generate exactly that behavior — nothing more. The distinction is explicit and documented.

### The architectural contract

When a behavior is activated, the system enforces the contract that comes with it. A `Client('db')` generates the alias `@client/db`. Anything that was importing the file directly is now violating the contract. `kerith check` tells you exactly which files, which imports, and what they need to use instead.

The framework does not guess. It does not warn loosely. It tells you precisely what is inconsistent and what correct looks like.

### What Kerith guarantees — and what it does not

Kerith guarantees that the architecture is connected correctly. Module boundaries are respected. Aliases point where they should. Declared dependencies are the real dependencies. The system is structurally sound.

Kerith does not guarantee that the logic inside a `Service` is correct. That a `Repository` query returns what you expect. That a `Guard` rejects exactly the cases it should. That is the developer's responsibility, enforced by tests. Always was.

This boundary is not a limitation — it is honesty. You know exactly what to trust Kerith for and what remains yours.

---

## The ecosystem

| Package                                                                        | Description                                                              |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| [`@kerith/core`](https://www.npmjs.com/package/@kerith/core)                   | The engine. Deterministic 17-step bootstrap, NITS, dependency graph, CLI |
| [`@kerith/identifiers`](https://www.npmjs.com/package/@kerith/identifiers)     | 80+ structural and logical identifiers for every backend entity type     |
| [`@kerith/eslint-plugin`](https://www.npmjs.com/package/@kerith/eslint-plugin) | Architectural rules enforced at edit time — before the server runs       |
| [`create-kerith`](https://www.npmjs.com/package/create-kerith)                 | `npm create kerith@latest` — correct structure from the first commit     |

---

## Kerith and AI agents

A Kerith project is designed to be read — not just by developers, but by AI agents.

The identifier system gives every entity a semantic name. The filesystem hierarchy makes the domain structure unambiguous. The dependency graph is queryable. The ESLint plugin rules are executable constraints, not style suggestions.

When an agent generates code for a Kerith project, it operates against an explicit architectural contract. It knows where things go. It knows what boundaries cannot be crossed. It knows what a violation looks like — because `kerith check` will tell it.

This is the difference between an agent that approximates your architecture and one that respects it by construction. Kerith is the map. The agent is the builder. The map does not negotiate.

---

## Getting started

```bash
npm create kerith@latest
```

→ [docs.kerith.dev](https://docs.kerith.dev) for the full documentation.

---

## Built by Vlynk Studios

KerithJS is built and maintained by a single developer from Porto Alegre, Brazil, under [Vlynk Studios](https://github.com/vlynk-studios).

One person. Clear principles. No investors. No roadmap driven by someone else's priorities.

The V2.0.0 release will ship when the foundation is solid enough for others to build on — not before, not after. When it does, the contribution guidelines, style standards, and design principles will be open.

Until then: the code is being built the way Kerith says code should be built. Deliberately, with memory, and without shortcuts.

---

<div align="center">

**MIT License — the code is yours.**

[npm](https://www.npmjs.com/org/kerith) · [github.com/KerithJS](https://github.com/KerithJS)

_Built by Vlynk Studios — Porto Alegre, Brasil._

</div># .github
