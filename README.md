# AQEL

> **Arabic-first programming. Universal semantics. Deterministic execution.**

**AQEL** is a programming language project built around four engineering goals:

**LIGHT · SMART · SAFE · FAST**

AQEL starts with an Arabic-first language surface and a language-neutral semantic core. The aim is simple: make source code readable to humans, precise for machines, and practical to run.

> **Current status:** pre-release development toward **AQEL v0.1**.

---

## Why AQEL?

Most programming languages force the programmer to think in the language's syntax first. AQEL explores another direction: an Arabic-native surface that maps into a precise internal representation without depending on an AI system to decide what the code means.

AQEL is **not** “Python translated into Arabic”. The project is designed around a semantic pipeline:

```text
Arabic / Future Language Packs
              ↓
          Normalizer
              ↓
             Lexer
              ↓
            Parser
              ↓
        Universal AST
              ↓
   Name + Type + Safety Analysis
              ↓
    Semantic / SMART Engine
              ↓
        IR / Optimization
              ↓
        Native Execution
```

The architecture keeps the **language surface** separate from the **meaning executed by the compiler**. That gives AQEL a path to Indonesian, English, and other language surfaces later without duplicating the language's semantic core.

---

## LIGHT · SMART · SAFE · FAST

| Pillar | AQEL engineering target |
|---|---|
| **LIGHT** | Small runtime, low overhead, minimal mandatory dependencies, fast startup. |
| **SMART** | First-class semantic operations for facts, rules, queries, inference, verification, and explanations. |
| **SAFE** | Strong typing, deterministic behavior, and early rejection of invalid programs. |
| **FAST** | Native-oriented execution with reproducible measurements instead of performance assumptions. |

These are **engineering goals to measure**, not marketing claims. In particular, Arabic itself is not assumed to make programs faster or more intelligent.

---

## Arabic-first syntax

AQEL treats Arabic as a first-class programming surface.

### Basic program

```arabic
عرّف الاسم: نص = "AQEL"
اعرض("مرحبا من " + الاسم)
```

### Conditional

```arabic
عرّف العمر: رقم = 20

إذا كان العمر >= 18:
    اعرض("بالغ")
وإلا:
    اعرض("قاصر")
```

### Semantic / SMART direction

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

The last example describes the semantic direction of AQEL. The first release will only claim constructs that are actually implemented and covered by conformance tests.

### Core keywords

| Arabic | Meaning |
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

Diacritics are not intended to be mandatory. Unicode normalization, identifiers, RTL/Bidi behavior, indentation, literals, and precedence are specified explicitly so that readable Arabic source still has machine-precise meaning.

---

## Universal semantic core

AQEL keeps language-specific syntax at the edge:

```text
Arabic ─────┐
Indonesian ─┤
English ────┼──→ Universal AST → Semantic Core → Execution
Other ──────┘
```

This is the foundation for multilingual programming without creating multiple incompatible implementations of the same language semantics.

---

## v0.1: the first usable release

The first release is intentionally small.

A **usable AQEL v0.1** means a new user can:

1. obtain/build the AQEL CLI with minimal setup;
2. create a `.aqel` file;
3. run it deterministically;
4. use the documented Arabic-first core syntax;
5. receive stable diagnostics for invalid programs;
6. run the examples and conformance suite;
7. reproduce the documented release build.

The release is **not** complete merely because the specification exists.

### Planned v0.1 CLI

```text
aqel run <file.aqel>
aqel check <file.aqel>
aqel version
aqel help
```

These commands become the public interface only after their implementation and integration tests are complete.

---

## Quick Start

### From source

During the current pre-release phase, the exact build/install commands are being finalized in the release-engineering issues.

The intended first-run experience is:

```text
git clone https://github.com/bjo163/AQEL.git
cd AQEL
<build-command>
aqel run examples/hello.aqel
```

Expected example source:

```arabic
# AQEL v0.1 example

عرّف الاسم: نص = "AQEL"
اعرض("مرحبا من " + الاسم)
```

The final README will replace `<build-command>` with the verified release command before the v0.1 tag is published.

---

## Project structure

```text
AQEL/
├── docs/
│   └── SPEC_V0.1.md
├── examples/
│   └── hello.aqel
├── README.md
└── ... implementation / tests / tooling ...
```

The repository is being built in small, auditable steps rather than introducing a large runtime before the language contracts are stable.

---

## Development roadmap

The master execution plan is **Issue #40 — AQEL v0.1 Release Tracker**.

### 1. Language contract

- [x] Arabic-first direction
- [x] Universal semantic-core direction
- [x] UTF-8 / RTL policy direction
- [ ] normalization contract
- [ ] identifier rules
- [ ] literals and strings
- [ ] expressions and precedence
- [ ] scopes and name resolution
- [ ] v0.1 type rules
- [ ] indentation and blocks

### 2. Compiler front end

- [ ] source loader
- [ ] lexer
- [ ] parser
- [ ] Universal AST
- [ ] resolver
- [ ] type checker
- [ ] diagnostic engine

### 3. Execution + SMART semantics

- [ ] reference interpreter
- [ ] functions
- [ ] deterministic evaluation order
- [ ] fact store
- [ ] rule evaluator
- [ ] inference engine
- [ ] verification / constraints
- [ ] provenance and explanations

### 4. Usable CLI + tests

- [ ] reference CLI
- [ ] parser/lexer conformance tests
- [ ] semantic/type conformance tests
- [ ] end-to-end examples
- [ ] negative/error tests
- [ ] deterministic conformance runner

### 5. Release engineering

- [ ] build system
- [ ] install/distribution path
- [ ] CI
- [ ] reproducible release artifacts
- [ ] minimum standard library
- [ ] README / Quick Start verification
- [ ] v0.1 compatibility gate
- [ ] release-candidate validation

---

## What AQEL deliberately does **not** promise yet

AQEL v0.1 should not freeze or claim more than the implementation can prove.

Deferred or evolving areas include:

- final long-term memory/ownership model;
- production concurrency model;
- stable cross-version native ABI;
- package registry and ecosystem tooling;
- fully optimized native backend;
- remote AI/LLM dependency for core language meaning.

The principle is:

> **Ship a small language that works completely before growing a large language that works partially.**

---

## Documentation

- [`docs/SPEC_V0.1.md`](docs/SPEC_V0.1.md) — v0.1 language and semantic specification
- [`examples/hello.aqel`](examples/hello.aqel) — first source example
- [Issue #40](https://github.com/bjo163/AQEL/issues/40) — release tracker

---

## Engineering principles

**Human-readable, machine-precise.** Readability never replaces formal semantics.

**Semantic core first.** The Universal AST is the boundary between language surfaces and compiler meaning.

**Safety by construction.** Prefer early, deterministic rejection over ambiguous behavior.

**Intelligence without mandatory AI.** Semantic reasoning should be inspectable and deterministic.

**Minimal by default.** Every core runtime feature has a cost and must justify its place.

**Measure, do not assume.** RAM, binary size, startup time, compile time, and runtime performance require reproducible evidence.

---

## Contributing

AQEL is being developed incrementally. The best contribution is a small, testable change that matches the current specification and release tracker.

Before introducing new syntax or runtime behavior, check the relevant issue and the v0.1 specification first.

---

## License

A project license has not yet been selected.

---

## Project status

**Target:** first usable AQEL release — `v0.1.0`

**Current phase:** implementation foundation

**Primary tracker:** [AQEL v0.1 Release Tracker](https://github.com/bjo163/AQEL/issues/40)
