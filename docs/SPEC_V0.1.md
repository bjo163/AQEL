# AQEL v0.1 — Language Specification Draft

## 1. Purpose

AQEL is an Arabic-first programming language with a language-neutral semantic core. v0.1 prioritizes a small grammar, predictable semantics, and a path toward native compilation.

## 2. Core principles

### LIGHT
The language should avoid mandatory heavyweight runtime services. Features should be modular and included only when required.

### SMART
The language can represent facts, rules, inference, verification, and explanations as explicit semantic constructs.

### SAFE
The compiler should reject invalid types and unsafe operations before execution whenever practical.

### FAST
The implementation should target native code and measurable low-overhead execution.

## 3. Lexical policy

- Source files are UTF-8.
- Arabic keywords are first-class.
- Identifiers may support Arabic Unicode characters and Latin characters.
- Digits and programming punctuation use ordinary Unicode-compatible symbols.
- The parser must operate on logical tokens rather than visual RTL ordering.
- Bidirectional rendering must never change program semantics.

## 4. Initial keywords

| Arabic | Semantic role |
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
| `تحقق` | verification/constraint |
| `اشرح` | explanation |

## 5. Variables

```arabic
عرّف العمر: رقم = 20
عرّف الاسم: نص = "أحمد"
```

The semantic representation should retain both static type and optional semantic metadata.

## 6. Conditions

```arabic
إذا كان العمر >= 18:
    اعرض("بالغ")
وإلا:
    اعرض("قاصر")
```

## 7. Facts

A fact declares information that can be used by the semantic/logic engine.

```arabic
حقيقة:
    أحمد عمره 20
```

Facts are represented independently from surface-language wording in the semantic AST.

## 8. Rules

A rule maps conditions to a conclusion.

```arabic
قاعدة:
    البالغ هو من عمره >= 18
```

The first implementation should use deterministic rules rather than probabilistic AI.

## 9. Inference

```arabic
استنتج:
    أحمد بالغ
```

Inference should produce a traceable result containing the facts and rules used.

## 10. Verification

```arabic
تحقق:
    العمر >= 0
```

Verification is intended to become a compiler/runtime safety primitive. v0.1 should keep the semantics deterministic and explainable.

## 11. Explanation

```arabic
اشرح:
    لماذا أحمد بالغ؟
```

The reference implementation should return an explanation graph or structured trace rather than relying on an opaque language model.

## 12. Semantic AST

The initial semantic node families are:

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

## 13. Multilingual architecture

Arabic is the canonical v0.1 surface language. Future language packs may map Indonesian, English, and other natural-language keywords into the same semantic AST.

The semantic core must not contain language-specific assumptions.

## 14. Non-goals for v0.1

- No claim that Arabic is intrinsically faster than Latin.
- No mandatory LLM dependency.
- No complex ownership system copied from another language without measurement.
- No large standard library before the core language is stable.

## 15. Benchmark goals

AQEL will eventually be measured against C, Rust, Zig, and Go using reproducible workloads:

- compiler time
- executable size
- startup time
- peak RAM
- CPU time
- throughput
- safety checks

The phrase “lighter than Rust” is a project target, not an assumption.
