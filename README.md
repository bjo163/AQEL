# AQEL

**AQEL** is an Arabic-first, multilingual programming language designed around four core goals:

> **LIGHT · SMART · SAFE · FAST**

AQEL is intended to make programs easier to express at the human-language level while keeping the execution model small, native, and performance-oriented.

## Vision

AQEL separates the language humans write from the semantic representation used by the compiler:

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

Arabic is the first-class surface syntax. The architecture is language-neutral so Indonesian, English, and other languages can map to the same semantic core later.

## Design pillars

- **LIGHT** — small runtime, low overhead, minimal dependencies.
- **SMART** — facts, rules, inference, verification, and explanations are native concepts.
- **SAFE** — strong typing and memory-safety-oriented design by default.
- **FAST** — native compilation, fast startup, and measurable performance targets.

## Arabic-first syntax

Example:

```arabic
عرّف العمر: رقم = 20

إذا كان العمر >= 18:
    اعرض("بالغ")
```

Semantic example:

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

## Status

AQEL is in the **v0.1 foundation stage**. The current goal is to define a minimal grammar and semantic core before implementing a native compiler.

## Roadmap

1. Define the lexical rules and Unicode/RTL policy.
2. Define the v0.1 grammar.
3. Build a reference parser/interpreter.
4. Build the semantic AST.
5. Add static typing and safety checks.
6. Add fact/rule/inference primitives.
7. Add a native backend.
8. Benchmark binary size, RAM, startup time, compile time, and execution speed against C, Rust, Zig, and Go.

## Principles

AQEL does **not** assume that Arabic itself makes software faster or more intelligent. Arabic is the human-facing language layer; intelligence and performance come from the compiler, semantic model, optimizer, and runtime design.

## License

License to be selected with the project maintainers.
