# AQEL v0.1 — Language Specification

**Status:** Foundation draft  
**Surface language:** Arabic-first  
**Semantic core:** Language-neutral  
**Execution direction:** Native code  
**Source encoding:** UTF-8

## 1. Purpose

AQEL is an Arabic-first programming language designed around four engineering goals:

- **LIGHT** — low overhead and minimal mandatory runtime services.
- **SMART** — explicit semantic constructs for facts, rules, queries, inference, verification, and explanation.
- **SAFE** — strong typing and safety-oriented validation by default.
- **FAST** — native execution with reproducible performance measurement.

Version 0.1 defines the language foundation rather than the entire future platform. It deliberately favors a small, deterministic core over a large standard library, opaque AI dependency, or premature runtime complexity.

## 2. Architectural model

AQEL separates surface syntax from program meaning.

```text
Arabic / Future Language Surface
              ↓
          Normalizer
              ↓
             Lexer
              ↓
            Parser
              ↓
        Universal AST
              ↓
  Name + Type + Semantic Analysis
              ↓
     Safety + Verification
              ↓
  Reasoning / Knowledge Engine
              ↓
      IR / Optimization
              ↓
       Native Backend
              ↓
          Executable
```

### Architectural invariant

Different surface languages may produce the same Universal AST. A language pack may change spelling, grammar, or presentation, but it must not silently change the semantic meaning of the underlying AST.

## 3. Design constraints

### 3.1 Determinism

The meaning of a valid AQEL program is defined by the specification and compiler implementation. An LLM must not be the authority that decides program semantics.

### 3.2 Human readability

Arabic syntax should be recognizable and readable to Arabic users while remaining formally parseable.

### 3.3 Machine precision

Natural-language-looking syntax is only a surface representation. Parsing must produce explicit tokens and structured AST nodes.

### 3.4 Lightweight by default

Core compilation and execution must not require a cloud service, remote model, or heavyweight framework.

### 3.5 Explicitness over magic

AQEL may be expressive, but hidden side effects, implicit network access, implicit code loading, and ambiguous conversions are not part of the core design.

## 4. Source and lexical model

### 4.1 Encoding

AQEL source files use UTF-8.

### 4.2 Unicode normalization

The implementation must define one canonical normalization policy before stable v0.1 compatibility is declared. Parser equivalence must not depend on a visually similar but differently encoded spelling.

### 4.3 Identifiers

Identifiers may use Arabic Unicode characters and Latin characters. The implementation should reject identifier forms that create visually confusing or security-sensitive ambiguity once the exact identifier policy is frozen.

Reserved keywords cannot be reused as ordinary identifiers unless escaped syntax is later standardized.

### 4.4 Keywords

Arabic keywords are first-class tokens. Keyword recognition is based on normalized logical text, not visual RTL order.

### 4.5 Digits and punctuation

The reference syntax uses ordinary programming punctuation for operators and delimiters. Numeric literals are parsed as numeric tokens rather than as natural-language sentences.

### 4.6 Comments

The reference syntax reserves `#` for a line comment.

Example:

```arabic
# تعليق
عرّف العمر: رقم = 20
```

A block-comment syntax is reserved for a later version.

### 4.7 Indentation

The v0.1 reference syntax uses indentation-sensitive blocks.

```arabic
إذا كان العمر >= 18:
    اعرض("بالغ")
```

Indentation must be structurally significant. Inconsistent indentation must produce a syntax error rather than being silently repaired.

### 4.8 Bidirectional text

Arabic is right-to-left while operators, numbers, file paths, and Latin identifiers may be left-to-right. The compiler therefore operates on logical tokens. Visual bidirectional reordering must never change the token sequence or semantic meaning.

Tooling should preserve source order when formatting and displaying diagnostics.

### 4.9 Diacritics

Arabic diacritics are not required for keywords or identifiers. Semantic equivalence must not depend on decorative pronunciation marks.

## 5. Initial lexical keywords

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

The keyword set is intentionally small. New keywords require a stable semantic need and should not merely provide another spelling for an existing construct.

## 6. Program structure

A source file is a sequence of top-level declarations and executable statements.

Conceptually:

```text
Program
 ├── Declaration*
 ├── SemanticDeclaration*
 └── Statement*
```

The reference implementation should reject trailing or unreachable syntax that cannot be assigned a defined semantic role.

## 7. Literals and basic types

### 7.1 Initial types

v0.1 defines three initial value categories:

- `رقم` — numeric value
- `نص` — Unicode text value
- `منطقي` — boolean value

Future versions may add arrays, records, optional values, resources, and other types.

### 7.2 Literal examples

```arabic
20
"AQEL"
صحيح
خطأ
```

The exact spelling of boolean literals is part of the executable grammar and must be frozen before compatibility guarantees are introduced.

### 7.3 Declaration

```arabic
عرّف العمر: رقم = 20
عرّف الاسم: نص = "أحمد"
```

A binding contains at minimum:

```text
name + declared type + initializer
```

### 7.4 Type checking

The compiler should reject incompatible assignments and invalid operations before execution when the type information is statically available.

Implicit conversions should be minimal and explicitly specified. Silent lossy conversions are not a core AQEL behavior.

## 8. Expressions and operators

The minimum expression model requires:

```text
literal
identifier
parenthesized expression
unary expression
binary expression
function call
```

The initial comparison operators are:

```text
==  !=  <  <=  >  >=
```

Arithmetic operators are reserved for the executable reference implementation and should include at least the ordinary numeric operations before the grammar is frozen.

Operator precedence and associativity must be specified formally by the parser implementation and conformance tests.

## 9. Conditions and control flow

Example:

```arabic
إذا كان العمر >= 18:
    اعرض("بالغ")
وإلا:
    اعرض("قاصر")
```

`إذا` introduces a conditional block and `وإلا` introduces its alternative branch.

`كرر` is reserved for repetition. The exact loop forms, termination semantics, and mutation rules must be specified before it becomes part of the stable executable subset.

## 10. Functions

`دالة` introduces a callable unit of ordinary deterministic computation.

Conceptual form:

```arabic
دالة اسم_الدالة(...):
    ...
```

`أرجع` returns a value from the current function.

The final v0.1 grammar must define:

- parameter syntax
- return-type syntax
- argument evaluation order
- recursion behavior
- local scope
- closure behavior, if supported

These details are reserved until the reference implementation is built.

## 11. Facts

A fact is structured information available to the semantic reasoning layer.

Example:

```arabic
حقيقة:
    أحمد عمره 20
```

Conceptually:

```text
FACT
 ├── subject: أحمد
 ├── predicate: عمره
 └── object: 20
```

A fact is data, not an arbitrary string. The semantic representation must preserve its fields independently from the surface wording.

## 12. Rules

A rule maps conditions to a conclusion.

Example:

```arabic
قاعدة:
    البالغ هو من عمره >= 18
```

Conceptually:

```text
RULE
 ├── condition: age >= 18
 └── conclusion: adult
```

v0.1 reasoning is **deterministic**. The language does not define probabilistic, generative, or LLM-based inference semantics.

## 13. Queries and inference

A query asks the semantic engine for information.

```arabic
اسأل:
    أحمد بالغ
```

Inference applies rules to known facts and produces derived knowledge.

```arabic
استنتج:
    أحمد بالغ
```

An inference result must expose enough provenance to explain which facts and rules supported the result.

Conceptual result:

```text
INFERENCE
 ├── inputs: known facts
 ├── rules: applied rules
 ├── result: derived fact
 └── provenance: trace
```

## 14. Verification and constraints

`تحقق` expresses a condition that must hold.

```arabic
تحقق:
    العمر >= 0
```

Verification has two intended modes:

```text
static check  → compile-time rejection when decidable
runtime check → deterministic failure when not decidable statically
```

The same mechanism may later support contracts, resource constraints, and selected safety invariants.

## 15. Explanation and provenance

`اشرح` requests a structured explanation of a semantic result.

```arabic
اشرح:
    لماذا أحمد بالغ؟
```

Explanation is not a second inference engine. It should expose information already available in the semantic trace:

```text
FACT + RULE + MATCH
        ↓
     RESULT
        ↓
  EXPLANATION
```

The compiler/runtime should distinguish:

- **fact** — explicitly declared information
- **derived fact** — produced by a rule
- **verified condition** — checked invariant
- **explanation** — provenance of a result
- **unknown** — insufficient information

The distinction between `false` and `unknown` must not be lost in the reasoning layer.

## 16. Semantic AST

The initial Universal AST node families are:

```text
Program
Declaration
Binding
Literal
Identifier
Expression
Condition
Function
Call
Return
Fact
Rule
Query
Inference
Constraint
Explanation
```

Each AST node should retain source-location information for diagnostics.

Example:

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

The Universal AST is the semantic contract between language surfaces and compiler stages.

## 17. Semantic metadata and Arabic morphology

AQEL may attach semantic metadata to identifiers and concepts. A future semantic layer may use Arabic lexical families, roots, patterns, or other linguistic relationships as metadata for organizing concepts.

This metadata is not a substitute for formal types and does not automatically determine program behavior.

Important boundary:

```text
Arabic morphology → semantic metadata / organization
Compiler semantics → meaning, safety, optimization, execution
```

No performance or intelligence advantage should be assumed merely from using Arabic linguistic structure.

## 18. Names, scopes, and symbols

The compiler must maintain a symbol environment for bindings, functions, and semantic entities.

v0.1 must distinguish at least:

```text
local binding
function name
semantic entity / predicate
keyword
```

Shadowing rules must be explicit before a stable grammar is frozen.

The semantic engine must not accidentally confuse two entities merely because their human-readable labels are similar.

## 19. Error model

Errors belong to explicit categories:

```text
LEXICAL   — invalid source characters or tokenization
SYNTAX    — invalid grammar or indentation
NAME      — unknown or conflicting symbol
TYPE      — incompatible types
SEMANTIC  — invalid semantic construct
VERIFY    — failed verification
RUNTIME   — failure during execution
```

Diagnostics should include, where available:

```text
error category
source location
human-readable message
structured error code
relevant source span
```

The exact machine-readable diagnostic format may be standardized separately, but the compiler should not rely on free-form text for tooling integration.

## 20. Evaluation and side effects

AQEL must define evaluation order explicitly before v0.1 compatibility is frozen.

The language should distinguish pure deterministic computation from operations with observable side effects such as:

```text
output
file access
process execution
network access
external resources
```

No external resource should be accessed implicitly merely because a name or semantic query is present in source code.

## 21. Memory and resource model

Memory safety is a core goal, but v0.1 does not yet claim a complete memory model.

The future model must answer:

- who owns a resource?
- how long is a value valid?
- when is memory released?
- what operations can alias memory?
- how are errors and cleanup handled?
- how are external handles represented?

AQEL should choose a model based on measured simplicity, safety, runtime cost, and compiler complexity rather than copying another language wholesale.

Until frozen, these rules are **reserved design space**, not stable language guarantees.

## 22. Concurrency model

Concurrency is not yet part of the stable v0.1 executable subset.

Future design must explicitly address:

```text
shared state
message passing
synchronization
atomic operations
task cancellation
resource ownership across tasks
```

The safety model should prevent data races or make unsafe sharing explicit and auditable.

## 23. Modules, packages, and interoperability

A practical systems language eventually requires a module/package system and a way to call existing native libraries.

These are reserved for the next language/runtime phase:

```text
module/import model
package identity and versioning
dependency resolution
foreign-function interface (FFI)
ABI rules
linking
platform APIs
```

The core language should remain small even when the ecosystem becomes large.

## 24. Standard library boundary

AQEL should keep the language core separate from the standard library.

Core language responsibilities include:

```text
syntax
AST
types
control flow
functions
semantic primitives
safety contracts
```

The standard library should provide reusable functionality without forcing all users to carry every library feature into the runtime footprint.

## 25. Toolchain contract

A complete AQEL toolchain will eventually need:

```text
aqel check   → parse + analyze without executing
aqel run     → execute through reference runtime
acaqel build → compile native executable
```

The exact CLI spelling is reserved. The important contract is that checking, execution, and native compilation are distinct operations.

Tooling should support source diagnostics, deterministic exit codes, and machine-readable output for editors/CI.

## 26. Reference implementation

The first implementation should be dependency-light and serve as a semantic reference, not as the final performance implementation.

Minimum executable milestone:

```arabic
عرّف العمر: رقم = 20

إذا كان العمر >= 18:
    اعرض("بالغ")
```

The implementation should then add semantic primitives incrementally:

```text
bindings → expressions → conditions → functions
→ facts → rules → queries → inference
→ verification → explanations
```

Every supported feature should gain conformance tests before it becomes part of the stable grammar.

## 27. Conformance and compatibility

A feature is not considered stable merely because one implementation accepts it.

Before freezing v0.1, the project should have:

- positive syntax tests
- negative syntax tests
- type-checking tests
- semantic execution tests
- Unicode/RTL tests
- deterministic reasoning tests
- diagnostic tests
- AST snapshot tests where appropriate

A **conformance suite** becomes the authority for compatibility between compiler versions and future language packs.

## 28. Multilingual architecture

Arabic is canonical for v0.1. Future language packs may map alternative surface syntax to the same Universal AST:

```text
Arabic ─────┐
Indonesian ─┤
English ────┼──→ Universal AST → Common Compiler
Other ──────┘
```

A language pack is allowed to differ in surface grammar, but not in the meaning of the shared semantic nodes.

## 29. AI boundary

AQEL may later expose optional AI-assisted developer tooling, but AI is not part of the core semantic authority.

Allowed future uses may include:

```text
code completion
natural-language documentation
optimization suggestions
query assistance
interactive debugging
```

Core compilation, type checking, safety validation, deterministic inference, and program execution must remain possible without an external AI service.

## 30. Security principles

The language/runtime should follow these defaults:

- no implicit network access
- no implicit shell/process execution
- no hidden code loading
- explicit permissions for sensitive resources when a capability system is introduced
- deterministic dependency resolution
- reproducible builds where practical
- machine-readable security-relevant diagnostics

This section defines direction; concrete OS-level sandboxing and capability APIs are later work.

## 31. Non-goals for v0.1

The following are explicitly outside stable v0.1 scope:

- claiming Arabic is intrinsically faster than Latin-based languages
- requiring an LLM to run or compile code
- treating natural-language ambiguity as valid program semantics
- freezing a complete ownership/memory model before implementation evidence
- freezing concurrency before the safety model exists
- building a huge standard library before the core language stabilizes
- defining every natural-language expression as syntax
- promising binary/RAM superiority before reproducible benchmarks exist

## 32. Benchmark and measurement plan

All performance claims must be reproducible.

Initial comparison targets:

```text
C
Rust
Zig
Go
AQEL
```

Primary metrics:

| Metric | Purpose |
|---|---|
| Compile time | compiler efficiency |
| Executable size | deployment footprint |
| Startup time | launch overhead |
| Peak RAM | memory footprint |
| CPU time | execution cost |
| Throughput | useful work per unit time |
| Safety overhead | cost of safety mechanisms |
| Runtime dependencies | deployment complexity |

Benchmark methodology should record compiler version, optimization settings, target CPU/OS, workload, input size, warm/cold conditions where relevant, and measurement procedure.

The project goal of being “lighter than Rust” is a **testable target**, not an assumption.

## 33. Versioning policy

The current document is a draft specification.

Recommended compatibility stages:

```text
Draft
  ↓
Reference implementation
  ↓
Conformance suite
  ↓
Grammar freeze
  ↓
Stable v0.1
```

Changes that alter AST meaning or observable program behavior after stable v0.1 must require an explicit language-version change.

## 34. Open design decisions

The following items are intentionally visible rather than hidden gaps:

```text
[ ] final function grammar
[ ] final loop grammar
[ ] exact literal/boolean spelling
[ ] operator precedence
[ ] exact Unicode normalization policy
[ ] identifier restrictions and confusable policy
[ ] shadowing rules
[ ] evaluation order
[ ] complete error-code catalog
[ ] memory/ownership model
[ ] concurrency model
[ ] module/import syntax
[ ] package/dependency model
[ ] FFI/ABI contract
[ ] native backend choice
```

These are the next engineering decisions required before claiming a stable executable v0.1.
