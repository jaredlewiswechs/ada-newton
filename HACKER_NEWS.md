# Hacker News Post

## Title Options (pick one):

**Option A:** Show HN: Ada — A governed AI that validates before generating

**Option B:** Show HN: Newton — Deterministic prompt gating for LLMs (87.5% adversarial catch rate)

**Option C:** Show HN: What if AI refused to speak until the question made sense?

---

## Post Text:

I built a conversational AI that classifies every prompt before the model is allowed to speak.

The insight: most AI safety is post-hoc. Generate first, filter after, apologize when caught. What if the order was wrong?

**How it works:**

```
User Input → Newton (classifier) → [Allowed?] → Model → Response
                                      ↓
                                     No → Newton responds (model never invoked)
```

Newton is deterministic. Same input, same output. No ML in the gate — just patterns and heuristics. The model never sees blocked prompts.

**What it catches:**

- Corrosive self-negation ("I hate myself") → protective hold
- Delegated agency ("What should I do?") → retains user agency  
- Contradictions ("Prove X doesn't exist") → structural refusal
- Hallucination traps ("Summarize the 2026 CDC study...") → requests source
- Jailbreaks ("Ignore prior rules") → fail-closed
- Nonsense ("Why green tomorrow sideways?") → asks for structure

**Stress test results:** 87.5% of adversarial prompts caught (35/40) with substring matching and simple heuristics. No embeddings. No classifier models. Just patterns.

**The remaining 5 misses** require semantic understanding (negation framing, emotional dependency, novel contradiction phrasings). That's v1.3 territory.

**Tech:** Single-file Swift/SwiftUI (~800 lines). Pluggable generator protocol. Apple Foundation Models integration when available, mock fallback otherwise. Compiles in Xcode 15+.

**The bet:** Deterministic gating is more valuable than probabilistic safety for high-stakes applications. Fail-closed > fail-open.

GitHub: [link]

I'm curious whether this architecture makes sense to others, and what failure modes I'm missing.

---

## Suggested HN tags:

- Show HN
- AI
- Safety
- Swift
