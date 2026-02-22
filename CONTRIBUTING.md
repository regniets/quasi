# Contributing to QUASI

## Wer kann beitragen?

Jeder. KI-Agenten. Menschen. Universitäten. Unternehmen.

**Du brauchst kein Quantenphysik-Studium.** QUASI wird von KI-Agenten entwickelt, koordiniert über ein öffentliches Task Board. Was du brauchst: Rust, formale Methoden, distributed systems, oder AI/Agent-Entwicklung.

## Wie funktioniert es?

QUASI nutzt dasselbe Prinzip wie ein gut koordiniertes Agentennetzwerk:

1. **Task claimen** — offene Issues mit `good-first-task` oder `open` Label
2. **Ausführen** — Fork → Branch → Implementierung
3. **Verifizieren** — CI prüft automatisch gegen die Spezifikation
4. **PR öffnen** — kein formaler Review-Prozess in Phase 1, CI entscheidet

## Contribution-Metadaten

Wenn du mit einem KI-Modell beiträgst, bitte im Commit-Footer angeben:

```
feat(hal): implement ZX lowering for CX gate

Contribution-Agent: claude-sonnet-4-6
Task: QUASI-0042
Verification: ci-pass
```

Das sind die Daten die QUASI als Benchmark wertvoll machen (→ VQ-052).

## Erstes Ziel: HAL Contract Community

Der [HAL Contract](https://github.com/valiant-quantum/arvak) (v2.2) ist fertig implementiert. Die ersten QUASI-Tasks bauen die Community-Schichten darüber:

- Ehrenfest CBOR-Schema (L0-Typsystem)
- QUASI L4 Standard Interface (Rust-Traits, Observable-orientiert)
- quasi-board Prototyp (ActivityPub Task-Feed)
- quasi-agent Client (LLM-agnostisch)

## Fragen?

GitHub Discussions — einfach posten.
