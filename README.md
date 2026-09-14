# AQEL

**AQEL** is an Arabic-first, multilingual programming language built around four engineering goals:

> **LIGHT · SMART · SAFE · FAST**

AQEL explores a different programming model: humans write expressive, readable source code while the compiler translates that meaning into a small, predictable, high-performance execution model.

## Why AQEL?

Most programming languages are designed around machine-oriented syntax first and human language second. AQEL starts from the opposite direction:

```text
Human Language
      ↓
Semantic Parser
      ↓
Universal AST
      ↓
Safety + Reasoning
      ↓
Native Code
```

Arabic is the first-class surface language in v0.1. The underlying semantic core is language-neutral, so future language packs can support Indonesian, English, and other human languages without creating separate language semantics.

## The four pillars

| Pillar | Goal |
|---|---|
| **LIGHT** | Small runtime, low overhead, minimal dependencies, fast startup. |
| **SMART** | Native concepts for facts, rules, inference, verification, and explanation. |
| **SAFE** | Strong typing and safety-oriented semantics by default. |
| **FAST** | Native compilation with measurable performance targets. |

These are engineering goals. AQEL does not assume that any natural language is inherently faster or more intelligent than another.

## Arabic-first syntax

A basic AQEL program can read close to natural Arabic:

```arabic
عرّف العمر: رقم = 20

إذا كان العمر >= 18:
    اعرض("بالغ")
```

AQEL also experiments with semantic programming constructs:

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

The goal is not to hide computation behind vague natural-language interpretation. The goal is to give well-defined semantic constructs a human-readable surface syntax.

## Architecture

AQEL separates **surface language** from **semantic meaning**.

```text
Arabic / Future Language Packs
             ↓
        Lexer + Parser
             ↓
        Universal AST
             ↓
   Type / Safety / Semantics
             ↓
    Reasoning + Verification
             ↓
      Native Code Backend
```

This separation is important: language localization changes how programmers write programs, but it should not change what those programs mean.

## v0.1 status

AQEL is currently in the **foundation stage**. The specification defines the initial language direction, lexical policy, semantic concepts, and benchmark targets.

The immediate implementation goal is a small reference parser/interpreter that can validate the core syntax before a native backend is introduced.

## Roadmap

### Phase 1 — Foundation

- [x] Define project pillars
- [x] Define Arabic-first direction
- [x] Define UTF-8 and RTL lexical policy
- [x] Define initial semantic AST families
- [ ] Implement lexer
- [ ] Implement parser
- [ ] Add executable v0.1 examples

### Phase 2 — Semantic Core

- [ ] Static typing
- [ ] Safety checks
- [ ] Facts and rules
- [ ] Deterministic inference
- [ ] Verification / constraints
- [ ] Structured explanations

### Phase 3 — Native Execution

- [ ] Native code generation
- [ ] Minimal runtime
- [ ] Optimizer
- [ ] Cross-platform build support

### Phase 4 — Measurement

- [ ] Reproducible benchmark suite
- [ ] Binary-size comparison
- [ ] Peak-RAM comparison
- [ ] Startup-time comparison
- [ ] Compile-time comparison
- [ ] Runtime-performance comparison

Initial comparison targets: **C, Rust, Zig, and Go**.

> “Lighter than Rust” is a target to test with benchmarks, not a claim made in advance.

## Documentation

- [`docs/SPEC_V0.1.md`](docs/SPEC_V0.1.md) — language specification draft
- [`examples/hello.aqel`](examples/hello.aqel) — first example program

## Design principles

**Human-readable, machine-precise.** Natural-language syntax must still map to deterministic semantics.

**Semantic core first.** Surface syntax may evolve, but the universal semantic representation should remain stable.

**Safety by construction.** Invalid programs should be rejected as early as practical.

**Intelligence without mandatory AI.** Facts, rules, constraints, inference, and explanations should be deterministic and lightweight. Optional AI-assisted tooling can be added later without making it a runtime requirement.

**Measure, do not assume.** Claims about speed, memory, binary size, or developer productivity must be supported by reproducible experiments.

## License

The project license has not yet been selected.
