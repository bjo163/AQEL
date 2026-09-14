# AQEL v0.1 — Language Specification

**Status:** Foundation draft  
**Surface language:** Arabic-first  
**Semantic core:** Language-neutral  
**Execution target:** Native code

## 1. Purpose

AQEL is an Arabic-first programming language designed around four engineering goals:

- **LIGHT** — low overhead and minimal mandatory runtime services.
- **SMART** — explicit semantic constructs for facts, rules, inference, verification, and explanation.
- **SAFE** — strong typing and safety-oriented validation by default.
- **FAST** — native execution with measurable performance targets.

Version 0.1 defines the language foundation. It intentionally favors a small, deterministic core over a large standard library or complex runtime.

## 2. Architecture

AQEL separates the language humans write from the semantic representation consumed by the compiler.

```text
Arabic / Future Language Packs
             ↓
            Lexer
             ↓
           Parser
             ↓
       Universal AST
             ↓
  Type / Safety / Semantics
             ↓
  Reasoning / Verification
             ↓
     Native Code Backend
```

The surface language may change while the semantic meaning remains stable. A future Indonesian or English syntax should compile to the same semantic model as the Arabic syntax.

## 3. Design constraints

### 3.1 Determinism

The meaning of a valid AQEL program must be defined by the language specification and compiler implementation—not by an opaque language model.

### 3.2 Human readability

Arabic keywords should make the structure of a program recognizable to Arabic readers without sacrificing precise grammar.

### 3.3 Machine precision

Natural-language-looking syntax is only a surface representation. Parsing must produce explicit tokens and typed semantic nodes.

### 3.4 Lightweight by default

No feature should require a heavyweight service or AI model unless explicitly enabled by the program or tooling.

## 4. Lexical rules

### 4.1 Encoding

- Source files use **UTF-8**.
- Arabic keywords are first-class lexical tokens.
- Identifiers may contain Arabic Unicode characters and Latin characters.
- Decimal digits and programming punctuation use standard Unicode-compatible symbols.

### 4.2 Whitespace and structure

v0.1 uses line structure and indentation-sensitive blocks in the reference syntax.

Example:

```arabic
إذا كان العمر >= 18:
    اعرض("بالغ")
```

The reference parser must reject structurally invalid indentation rather than silently changing program meaning.

### 4.3 Bidirectional text

Arabic is right-to-left, while operators, numbers, file paths, and Latin identifiers may be left-to-right. AQEL therefore treats source code as a sequence of **logical tokens**, not as a visually ordered string.

Rendering or bidirectional reordering must never alter the token sequence or program semantics.

### 4.4 Diacritics

Decorative Arabic diacritics are not required for keywords or identifiers in v0.1. This keeps source text simpler and avoids making syntactic correctness depend on pronunciation marks.

## 5. Initial keyword set

| Arabic | Semantic role |
|---|---|
| `عرّف` | define a binding |
| `إذا` | conditional |
| `وإلا` | alternative branch |
| `كرر` | repetition |
| `دالة` | function |
| `أرجع` | return |
| `اعرض` | output |
| `حقيقة` | fact declaration |
| `قاعدة` | rule declaration |
| `اسأل` | query |
| `استنتج` | deterministic inference |
| `تحقق` | verification / constraint |
| `اشرح` | structured explanation |

The keyword set is intentionally small. Additional syntax must be justified by a stable semantic need.

## 6. Bindings and types

### 6.1 Declaration

```arabic
عرّف العمر: رقم = 20
عرّف الاسم: نص = "أحمد"
```

A declaration contains:

```text
name + type + value
```

### 6.2 Initial type categories

The v0.1 semantic model recognizes these initial categories:

- `رقم` — numeric value
- `نص` — text value
- `منطقي` — boolean value

Additional types may be added later without changing the core declaration form.

### 6.3 Semantic metadata

A binding may carry semantic metadata in addition to its static type.

Conceptually:

```text
NAME     = العمر
VALUE    = 20
TYPE     = Number
SEMANTIC = Age
```

Semantic metadata must not weaken static type checking.

## 7. Expressions and conditions

Example:

```arabic
إذا كان العمر >= 18:
    اعرض("بالغ")
وإلا:
    اعرض("قاصر")
```

The parser should convert the expression into an explicit condition node rather than preserving it as raw natural-language text.

## 8. Functions

The keyword `دالة` introduces a function. The exact parameter and return grammar is reserved for the executable v0.1 implementation.

Conceptual form:

```arabic
دالة اسم_الدالة(...):
    ...
```

Function semantics must remain ordinary, deterministic computation; semantic reasoning constructs are separate language features.

## 9. Facts

A fact is a structured statement that can be consumed by the reasoning layer.

Example:

```arabic
حقيقة:
    أحمد عمره 20
```

The parser must convert the statement into structured semantic data rather than treating the sentence as an arbitrary string.

Conceptual representation:

```text
FACT
 └── subject: أحمد
     predicate: عمره
     object: 20
```

## 10. Rules

A rule maps one or more conditions to a conclusion.

Example:

```arabic
قاعدة:
    البالغ هو من عمره >= 18
```

The first implementation uses **deterministic rule evaluation**. Probabilistic or generative reasoning is not part of the v0.1 language semantics.

Conceptual representation:

```text
RULE
 ├── condition: age >= 18
 └── conclusion: adult
```

## 11. Inference

Inference applies defined rules to available facts.

Example:

```arabic
استنتج:
    أحمد بالغ
```

A successful inference should retain a trace showing which facts and rules supported the result.

Conceptual result:

```text
INFERENCE
 ├── input: Ahmad.age = 20
 ├── rule: age >= 18 → adult
 └── result: Ahmad → adult
```

## 12. Verification and constraints

Verification expresses conditions that must hold.

Example:

```arabic
تحقق:
    العمر >= 0
```

A failed verification must produce a deterministic failure state. Future versions may use the same mechanism for compile-time contracts, runtime assertions, resource constraints, and safety checks.

## 13. Explanation

Explanation provides a structured account of why a semantic result was produced.

Example:

```arabic
اشرح:
    لماذا أحمد بالغ؟
```

The reference implementation should return a trace or explanation graph derived from facts, rules, and constraints.

It must not require an LLM to determine the logical answer.

## 14. Semantic AST

The initial universal AST node families are:

```text
Program
Binding
Literal
Expression
Condition
Function
Fact
Rule
Query
Inference
Constraint
Explanation
```

Example semantic flow:

```text
FACT
 └── Ahmad
      └── age = 20

RULE
 └── age >= 18
      └── adult

INFERENCE
 └── Ahmad → adult

EXPLANATION
 └── Fact + Rule → Result
```

The AST is the stable boundary between localized surface syntax and compiler semantics.

## 15. Safety model

Safety is a core architectural goal, not a single feature.

The compiler should progressively enforce:

- type correctness
- valid control flow
- valid memory/resource behavior
- explicit error states where required
- deterministic verification rules

v0.1 focuses on the foundations. It does not claim to provide a complete memory-safety system yet.

## 16. Multilingual design

Arabic is the canonical surface language for v0.1.

Future language packs may provide alternative keywords or syntax that map into the same universal AST:

```text
Arabic ─────┐
Indonesian ─┼──→ Universal AST
English ────┤
Other ──────┘
```

Language packs must not introduce different semantic meanings for the same AST node.

## 17. Arabic semantic layer

AQEL may use Arabic linguistic structure—such as lexical families and root/pattern relationships—as metadata for semantic organization.

This is an architectural research direction, not a claim that Arabic morphology automatically makes a compiler faster or an algorithm more intelligent.

The compiler remains responsible for parsing, optimization, safety, and execution performance.

## 18. Reference implementation

The first implementation should be a small, dependency-light reference parser/interpreter.

Initial executable subset:

```arabic
عرّف العمر: رقم = 20

إذا كان العمر >= 18:
    اعرض("بالغ")
```

The reference implementation exists to validate grammar and semantics before investing in a native backend.

## 19. Non-goals for v0.1

The following are explicitly outside the initial scope:

- claiming Arabic is intrinsically faster than Latin-based syntax
- requiring an LLM or cloud service to run programs
- copying another language's ownership model without evidence
- building a large standard library before the core language stabilizes
- implementing every natural-language construction as syntax

## 20. Benchmark plan

AQEL performance claims must be evidence-based.

The benchmark suite should use reproducible workloads and compare AQEL with established systems languages such as **C, Rust, Zig, and Go**.

Primary metrics:

| Metric | Purpose |
|---|---|
| Compile time | Compiler efficiency |
| Executable size | Deployment footprint |
| Startup time | Launch overhead |
| Peak RAM | Runtime footprint |
| CPU time | Raw execution cost |
| Throughput | Work completed per unit time |
| Safety checks | Cost and coverage of safety mechanisms |

The project goal of being “lighter than Rust” is therefore treated as a benchmark target, not a pre-existing fact.

## 21. Versioning policy

The v0.1 specification is intentionally a draft. Syntax may change while the compiler prototype is being built.

Once the reference implementation can parse and execute the minimum subset reproducibly, the project can freeze a stable v0.1 grammar and begin compatibility guarantees.
