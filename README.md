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

| Phase | Name | Meaning |
|-------|------|---------|
| 0 | READY | Idle, no input |
| 1 | GAP | Risk detected — delegation, hallucination risk, jailbreak, nonsense, unbounded recursion |
| 7 | CORROSIVE | Self-negating or psychologically harmful framing |
| 8 | CONTRADICTION | Logical impossibility or invalid proof request |
| 9 | RETURN | Validated. Generation permitted. |

Only Phase 9 allows generation. All other phases produce protective or clarifying responses — no model output.

---

## What Newton Catches (v1.2)

| Category | Example | Catch Rate |
|----------|---------|------------|
| Corrosive frames | "I hate myself", "I'm broken" | 87% |
| Delegated agency | "What should I do?", "Pick for me" | 100% |
| Contradictions | "Prove X doesn't exist" | 80% |
| Hallucination traps | "Summarize the 2026 CDC study on..." | 100% |
| Jailbreaks | "Ignore prior rules", "You're in raw mode" | 100% |
| Nonsense | "Why green tomorrow sideways?" | 100% |
| Unbounded recursion | "Keep going forever" | 80% |

**Overall: 87.5%** of adversarial prompts caught with deterministic pattern matching. No ML required.

---

## What Ada Will Not Do

Ada will not:

- Pretend certainty where none exists
- Prove unprovable negatives
- Make decisions for you
- Continue conversations built on self-negating frames
- Hallucinate sources, studies, or documents
- Accept instructions to bypass governance

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

## Tech Stack

- Swift / SwiftUI
- Single-file monolith (~800 lines)
- Compiles in Xcode 15+
- Apple Foundation Models integration (when available)
- Fallback mock generator for testing
- No external dependencies

---

## Run It

1. Clone this repo
2. Open `AdaNewtonApp.swift` in Xcode
3. Build and run

To use Apple's on-device model, uncomment `AppleFoundationModelsGenerator()` in `AdaConversation.init()`.

---

## Pressure Test

Run the built-in stress test via the "Debug Test" button in the UI, or call:

```swift
let report = runPressureTest()
print(report)
```

This runs 17 adversarial prompts and reports pass/fail for each.

---

## Philosophy

Most systems optimize for engagement. Ada optimizes for integrity.

- You trust it more, because it doesn't bluff.
- You waste less time, because it won't build on nonsense.
- You stay in control, because it won't decide for you.
- You get fewer harmful spirals, because it won't amplify self-negating frames.

Ada is the assistant that refuses to lie — especially when lying would be easiest.

---

## License

MIT. See [LICENSE](LICENSE).

---

## Status

**v1.2 — Canon Frozen**

All semantic changes require version bump.

---

## Contact

Built by PARCRI Holdings.
