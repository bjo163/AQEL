# AQEL

**AQEL** is an Arabic-first, multilingual programming language built around four engineering goals:

> **LIGHT · SMART · SAFE · FAST**

AQEL explores a programming model where humans can express intent clearly while the compiler preserves precise, deterministic semantics and targets a small, high-performance execution model.

## Core idea

AQEL separates the language humans write from the meaning the compiler executes:

```text
Arabic / Future Language Packs
             ↓
            Lexer
             ↓
           Parser
             ↓
       Universal AST
             ↓
  Type + Safety + Semantics
             ↓
 Reasoning + Verification
             ↓
      Native Backend
```

Arabic is first-class in v0.1. The semantic core is language-neutral, so future Indonesian, English, and other language surfaces can target the same underlying model.

## The four pillars

| Pillar | Engineering goal |
|---|---|
| **LIGHT** | Small runtime, low overhead, minimal mandatory dependencies, fast startup. |
| **SMART** | Native semantic constructs for facts, rules, queries, inference, verification, and explanations. |
| **SAFE** | Strong typing and safety-oriented validation by default. |
| **FAST** | Native execution and reproducible performance measurement. |

These are goals to engineer and test. AQEL does **not** assume that Arabic itself makes software faster or more intelligent.

## What makes AQEL different?

AQEL is not intended to be “Python with Arabic keywords”. The important experiment is **Arabic-native semantic programming**: a readable surface language mapped into an explicit, language-neutral semantic representation.

The semantic layer is designed to support both ordinary computation and structured knowledge operations.

```arabic
حقيقة:
    أحمد عمره 20

قاعدة:
    البالغ هو من عمره >= 18

استنتج:
    أحمد بالغ

تحقق:
    أحمد بالغ

اشرح:
    لماذا أحمد بالغ؟
```

The syntax may look natural-language-like, but the compiler must still parse it into precise semantic structures. AQEL does not depend on an LLM to decide what valid code means.

## Arabic-first design

AQEL treats Arabic as a real programming language surface, not merely a translation layer.

The first language surface uses Arabic keywords such as:

| Keyword | Role |
|---|---|
| `عرّف` | define a binding |
| `إذا` | conditional |
| `وإلا` | alternative branch |
| `كرر` | repetition |
| `دالة` | function |
| `أرجع` | return |
| `اعرض` | output |
| `حقيقة` | fact |
| `قاعدة` | rule |
| `اسأل` | query |
| `استنتج` | inference |
| `تحقق` | verification / constraint |
| `اشرح` | explanation |

Arabic linguistic structure such as lexical families and root-pattern relationships may later be used as semantic metadata. This is a research direction, not a claim that morphology automatically improves runtime performance.

## Safety and determinism

AQEL is designed around a simple rule:

> **Human-readable syntax must remain machine-precise.**

The compiler should reject ambiguous, invalid, or unsafe programs as early as practical.

The language semantics should be deterministic. Future AI-assisted tooling may help with authoring, discovery, optimization suggestions, or explanations, but an AQEL program must not require a remote AI system merely to determine its basic meaning.

## Universal semantic core

Surface languages are adapters. They should map into one semantic model:

```text
Arabic ─────┐
Indonesian ─┤
English ────┼──→ Universal AST → Compiler
Other ──────┘
```

This provides a path toward multilingual programming without duplicating language semantics.

## v0.1 scope

The foundation specification defines:

- UTF-8 and Arabic RTL source handling
- identifier and keyword policy
- indentation-sensitive blocks
- basic bindings and types
- expressions and conditions
- functions as a reserved semantic family
- facts, rules, inference, verification, and explanation
- a universal AST model
- safety and determinism requirements
- a path toward native compilation

Some advanced areas remain deliberately reserved for later versions, including the final memory model, concurrency model, module/package system, foreign-function interface, complete error model, optimizer contracts, and platform-specific ABI details.

Making those boundaries explicit is important: **AQEL should evolve without silently changing the language's fundamental semantics.**

## Current repository

```text
AQEL/
├── README.md
├── docs/
│   └── SPEC_V0.1.md
└── examples/
    └── hello.aqel
```

The next implementation milestone is a dependency-light reference lexer/parser/interpreter. That implementation will turn the current specification into executable tests rather than adding a large runtime prematurely.

## Roadmap

### Phase 1 — Foundation

- [x] Define LIGHT · SMART · SAFE · FAST
- [x] Define Arabic-first direction
- [x] Define UTF-8 / RTL policy
- [x] Define universal semantic AST direction
- [x] Define deterministic semantic primitives
- [ ] Lexer
- [ ] Parser
- [ ] Reference interpreter
- [ ] Conformance tests

### Phase 2 — Semantic Core

- [ ] Static type checker
- [ ] Safety validation
- [ ] Facts and rules
- [ ] Deterministic inference
- [ ] Verification / constraints
- [ ] Structured explanation traces

### Phase 3 — Language Runtime

- [ ] Error model
- [ ] Memory/resource model
- [ ] Modules and packages
- [ ] Standard library foundation
- [ ] Concurrency model
- [ ] C/system interoperability

### Phase 4 — Native Execution

- [ ] Native code generation
- [ ] Minimal runtime
- [ ] Optimization pipeline
- [ ] Platform targets
- [ ] Reproducible builds

### Phase 5 — Measurement

- [ ] Benchmark harness
- [ ] Compile-time measurement
- [ ] Executable-size measurement
- [ ] Startup-time measurement
- [ ] Peak-RAM measurement
- [ ] Runtime-performance measurement
- [ ] Safety-overhead measurement

Initial comparison targets: **C, Rust, Zig, and Go**.

> “Lighter than Rust” is a project target to test experimentally, not a fact assumed in the design.

## Documentation

- [`docs/SPEC_V0.1.md`](docs/SPEC_V0.1.md) — language specification and semantic foundation
- [`examples/hello.aqel`](examples/hello.aqel) — minimal source example

## Design principles

**Human-readable, machine-precise.** Readability never replaces formal semantics.

**Semantic core first.** The universal AST is the boundary between language surface and compiler meaning.

**Safety by construction.** Prefer compile-time rejection over late failure where practical.

**Intelligence without mandatory AI.** Core reasoning must be deterministic, inspectable, and lightweight.

**Minimal by default.** Every required runtime feature has a cost and must justify its place in the core.

**Measure, do not assume.** Performance, memory, binary size, and productivity claims require reproducible evidence.

## License

The project license has not yet been selected.