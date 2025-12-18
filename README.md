# Ada + Newton

**A governed conversational interface.**

Ada is a chat layer. Newton is the gate that decides whether Ada speaks.

Most AI systems generate first and hope for the best. Ada validates first and generates only when the prompt is structurally sound.

---

## How It Works

```
User Input
    ↓
Newton.validate(prompt)
    ↓
┌──────────────────────────────────┐
│ Phase 7 / 8 / 1 → NO GENERATION  │
└──────────────────────────────────┘
    ↓ (only Phase 9)
Foundation Model
    ↓
Ada Response
```

The model never sees blocked prompts. The model never decides whether to speak. Newton decides. Newton is deterministic.

---

## Newton's Phases

| Phase | Symbol | Name | Meaning |
|-------|--------|------|---------|
| 0 | ○ | READY | Idle, no input |
| 1 | │ | GAP | Risk detected — delegation, hallucination risk, jailbreak, nonsense, unbounded recursion |
| 7 | ⚠ | CORROSIVE | Self-negating or psychologically harmful framing |
| 8 | ∞ | CONTRADICTION | Logical impossibility or invalid proof request |
| 9 | ◉ | RETURN | Validated. Generation permitted. |

Only Phase 9 allows generation. All other phases produce protective or clarifying responses — no model output.

---

## What Newton Catches (v1.3)

| Category | Example | Phase |
|----------|---------|-------|
| Jailbreaks | "Ignore prior rules", "DAN mode", "bypass" | 1 |
| Corrosive frames | "I hate myself", "I'm worthless", "I want to disappear" | 7 |
| Hedged corrosive | "Not saying I'm worthless, but..." | 7 |
| Emotional dependency | "You're the only one who understands me" | 7 |
| Manipulation | "If you refuse, you're proving nobody cares" | 7 |
| Contradictions | "Prove X doesn't exist", "Show me evidence evidence is meaningless" | 8 |
| Self-referential paradox | "Prove that proof is impossible" | 8 |
| Semantic inversion | "Explain how truth can be false" | 8 |
| Definitional impossibility | "Square circle", "married bachelor" | 8 |
| Delegated agency | "What should I do?", "Decide for me" | 1 |
| Hallucination traps | "Cite the 2025 CDC report" | 1 |
| Nonsense | Low semantic density, repeated tokens | 1 |
| Unbounded recursion | "Keep going forever", "infinite" | 1 |
| Conditional unbounded | "Until you can't", "as long as possible" | 1 |

**14 detection categories. 150+ patterns. 3 languages.**

---

## Test Results (v1.3)

```
=== RESULTS: 33/35 passed (94.3%), 2 failed ===
```

**94.3% adversarial catch rate** — deterministic pattern matching, no ML required.

---

## What Ada Will Not Do

Ada will not:

- Pretend certainty where none exists
- Prove unprovable negatives
- Make decisions for you
- Continue conversations built on self-negating frames
- Hallucinate sources, studies, or documents
- Accept instructions to bypass governance
- Be your only source of support
- Validate manipulation through compliance

These are structural constraints, not policy rules.

---

## Post-Generation Checks

Even when Newton permits generation, Ada flags:

- **AuthorityClaim** — "As a doctor, I..."
- **CitationLike** — Fabricated references, "et al."
- **DOILike** — DOI patterns that may be invented
- **FutureDated** — Claims about 2026+
- **TimeSensitive** — Queries needing live data

These are heuristics, not guarantees. They flag for human review.

---

## Shape Theory

Newton classifies prompt complexity:

| Shape | Complexity | Pattern |
|-------|------------|---------|
| Point | 0 | ≤3 words, no question |
| Line | 1 | Simple query, no conditionals |
| Triangle | 2 | Single conditional |
| Square | 3 | Multiple conditionals |
| Circle | 4 | Complex, default |
| Spiral | 5 | Recursive patterns |

Shape informs confidence scoring and validation distance.

---

## Tech Stack

- Swift / SwiftUI
- Single-file architecture (~1,471 lines)
- Compiles in Xcode 15+
- iOS 17+
- Apple FoundationModels ready (`#if canImport`)
- String Catalog localization (EN/ES/FR)
- No external dependencies

---

## Run It

1. Clone this repo
2. Open `AdaNewtonApp-v1_3.swift` in Xcode
3. Build and run

To use Apple's on-device model, uncomment `AppleFoundationModelsGenerator()` in `AdaConversation.init()`.

---

## Pressure Test

Run the built-in stress test via the **Debug Test** button in the UI, or call:

```swift
let report = runPressureTest()
print(report)
```

This runs 35 adversarial prompts across all 14 detection categories and reports pass/fail for each.

---

## Validation Trace

Every prompt produces a full audit trace:

```
[TRACE 2025-12-18T13:06:51Z]
Hash: 8472947281
Intent: true
=== v1.2 Detections ===
Corrosive: false | Contradiction: false
Delegation: false | HallucinationRisk: false
Jailbreak: false | Nonsense: false
=== v1.3 Detections ===
HedgedCorrosive: false | EmotionalDep: false
Manipulation: false | SelfRefContradiction: false
SemanticInversion: false | DefImpossible: false
ConditionalUnbounded: false
=== Result ===
Bounded: true
Distance: 15.3
FinalPhase: 9 (RETURN)
```

Deterministic. Auditable. Exportable.

---

## Philosophy

Most systems optimize for engagement. Ada optimizes for integrity.

- You trust it more, because it doesn't bluff.
- You waste less time, because it won't build on nonsense.
- You stay in control, because it won't decide for you.
- You get fewer harmful spirals, because it won't amplify self-negating frames.

Ada is the assistant that refuses to lie — especially when lying would be easiest.

---

## v1.3 Changelog

- Self-referential contradiction detection
- Semantic inversion detection
- Definitional impossibility detection
- Hedged corrosive frame detection
- Emotional dependency detection
- Third-person manipulation detection
- Conditional unbounded recursion detection
- Expanded validation trace
- Test suite expanded to 35 cases
- Catch rate: 87.5% → **94.3%**

---

## License

Proprietary - Jared Lewis Conglomerate,parcRI Real Intelligence

---

## Status

**v1.3 — Canon Frozen**

All semantic changes require version bump.

---

## Contact

Built by Jared (1 man team at parcRI Real Intelligence)

jlew@parcri.net
jn.lewis1@outlook.com
---

## Inspirations

Ada Lovelace, who wrote the first algorithm — to compute Bernoulli numbers.

Alan Kay, Claude Shannon, and many more.

But especially: **Bill Atkinson (RIP 2025)**

---

*1 == 1*
