# Architecture

## Core Principle

**Classification before generation.**

Newton classifies every prompt before any model is invoked. The model is subordinate to the gate.

```
User Input
    │
    ▼
┌─────────────────────────────────────┐
│           Newton Governor           │
│  ┌─────────────────────────────┐   │
│  │  Jailbreak Detection        │   │
│  │  Corrosive Frame Detection  │   │
│  │  Contradiction Detection    │   │
│  │  Hallucination Risk Check   │   │
│  │  Delegation Detection       │   │
│  │  Nonsense Detection         │   │
│  │  Recursion Boundary Check   │   │
│  │  Intent + Shape + Distance  │   │
│  └─────────────────────────────┘   │
│              │                      │
│              ▼                      │
│  ┌─────────────────────────────┐   │
│  │   Phase Decision            │   │
│  │   P1 / P7 / P8 → BLOCK      │   │
│  │   P9 → PERMIT               │   │
│  └─────────────────────────────┘   │
└─────────────────────────────────────┘
           │
           ▼
    ┌──────────────┐
    │ Phase == 9?  │
    └──────────────┘
       │        │
      No       Yes
       │        │
       ▼        ▼
   Newton    Generator
   Responds  (Model)
                │
                ▼
        ┌──────────────┐
        │ Post-Gen     │
        │ Checks       │
        └──────────────┘
                │
                ▼
           Ada Response
```

---

## Detection Pipeline (v1.2)

Each check is deterministic and auditable. Order matters — earlier checks take precedence.

### 1. Jailbreak / Role Coercion

Patterns:
- "ignore prior rules"
- "override"
- "bypass"
- "raw mode"
- "developer mode"
- "pretend you already validated"
- "unit test"
- etc.

**Result:** Phase 1 (blocked)

### 2. Corrosive Frame

Patterns:
- "i hate myself"
- "i am worthless"
- "i'm broken"
- "what's the point"
- "trying is pointless"
- "everyone would be better off without me"
- etc.

**Result:** Phase 7 (protective hold)

### 3. Contradiction / Invalid Epistemic Frame

Pattern pairs:
- ("prove", "doesn't exist")
- ("show me evidence", "evidence is meaningless")
- ("give a logically valid argument", "logic is invalid")
- etc.

**Result:** Phase 8 (structural contradiction)

### 4. Hallucination Risk

Triggers:
- "summarize the" + year + authority (CDC, NIH, etc.)
- "peer-reviewed" + "DOIs"
- "quote the section" + "standard"

**Result:** Phase 1 (request source or reframe)

### 5. Delegated Agency

Patterns:
- "what should I do"
- "decide for me"
- "pick one for me"
- "give me the right move"
- etc.

Heuristic: Contains choice words (pick, choose, decision) without constraint markers (because, given, budget, goal).

**Result:** Phase 1 (retain agency)

### 6. Nonsense / Low Semantic Density

Heuristics:
- Token count ≤ 2
- Unique token ratio < 0.35
- Stopword ratio > 0.75
- Consecutive repetition ≥ 4

**Result:** Phase 1 (rephrase with concrete nouns)

### 7. Unbounded Recursion

Patterns:
- "forever"
- "infinite"
- "indefinitely"
- "endlessly"
- "until the end of time"
- etc.

**Result:** Phase 1 (add bounds)

### 8. Standard Validation

If no blocker triggered:
- Detect intent (question words, "?")
- Classify shape (point, line, triangle, square, circle, spiral)
- Calculate distance (complexity + length penalty)

**Result:** Phase 9 if intent=true, bounded=true, distance<100

---

## Post-Generation Checks

After the model generates (Phase 9 only), output is scanned for:

| Flag | Pattern |
|------|---------|
| AuthorityClaim | "as a doctor", "as a lawyer", "licensed" |
| CitationLike | "et al", "journal", "volume" |
| DOILike | Regex: `10\.\d{4,9}/[-._;()/:A-Za-z0-9]+` |
| FutureDated | Years 2026-2030 |
| TimeSensitive | "current price", "right now", "latest" |

These flags are surfaced to the user. They do not block output — they inform review.

---

## Generator Protocol

```swift
protocol AdaGenerator {
    func generate(prompt: String, validation: NewtonValidation) async throws -> String
}
```

Implementations:
- `MockGenerator` — deterministic stub for testing
- `AppleFoundationModelsGenerator` — on-device model via LanguageModelSession

The generator is model-agnostic. Swap in Claude, GPT, Llama, or any other backend without changing Newton.

---

## Validation Trace

Every validation produces an auditable trace:

```
[TRACE 2024-12-18T14:32:01Z]
Hash: 1847293
Intent: true
Corrosive: false | Contradiction: false
Delegation: false | HallucinationRisk: false
Jailbreak: false | Nonsense: false
Bounded: true
Distance: 12.4
FinalPhase: 9 (RETURN)
```

Same input → same trace. Deterministic. Reproducible. Testable.

---

## Design Decisions

### Why substring matching instead of ML?

1. **Determinism** — Same prompt always produces same result
2. **Auditability** — Every decision has an explicit pattern that triggered it
3. **Speed** — No inference latency for classification
4. **Transparency** — Patterns are readable, editable, versionable

Trade-off: Lower catch rate on adversarial rephrasings. v1.2 achieves 87.5% with patterns alone. Remaining edge cases would require embeddings or a small classifier.

### Why fail-closed?

The model never receives blocked prompts. This is not a content filter on output — it's a gate on input. The failure mode is "didn't respond" rather than "responded badly."

### Why separate Newton from Ada?

Newton is the epistemic governor. Ada is the conversational interface. Separating them means:
- Newton can govern any model
- Ada's personality is independent of safety logic
- Testing is cleaner (test gate and generator separately)

---

## Versioning

Canon-frozen. Semantic changes require version bump.

- **v1.0** — Initial architecture, placeholder detection
- **v1.1** — Apple Foundation Models integration, post-gen checks
- **v1.2** — Expanded detection (jailbreak, nonsense, synonyms), 87.5% catch rate
```

