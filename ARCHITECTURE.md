# QUASI Architecture

## The Stack

```
Natural Language (human describes problem)
        ↓
   AI Model (Claude, GPT, Llama, ...)
        ↓  generates
   Ehrenfest program (.ef)       ← physics-native, not human-readable
        ↓  compiles to
   ZX-calculus (optimization)
        ↓  extracts gate sequences
   HAL Contract (L0)             ← the POSIX of QPUs
        ↓
   IBM | IQM | Quantinuum | neQxt | Simulator | ...
```

## Layer Model

| Layer | Name | Status | License |
|-------|------|--------|---------|
| L0 | HAL Contract Specification | ✅ v2.2 (in Arvak) | Apache 2.0 |
| L1 | Hardware Adapters | ✅ IBM, IQM, Scaleway, AQT | Apache 2.0 |
| L2 | Compiler / IR | ✅ arvak-compile, arvak-ir | Apache 2.0 |
| L3 | QUASI Runtime Services | 🔲 Specified, not built | AGPL v3 |
| L4 | QUASI Standard Interface | 🔲 Concept (see below) | Apache 2.0 |
| L5 | Application Libraries | 🔲 Community grows this | Various |
| — | ZX-calculus | ✅ PyZX (MIT) | MIT |
| — | Ehrenfest Language | 🔲 Concept paper done | AGPL v3 |

## Ehrenfest

Ehrenfest is QUASI's specification language. Named after Paul Ehrenfest (1880–1933), whose theorem bridges quantum expectation values to classical equations of motion — exactly what the language does.

**Key properties:**
- Not human-readable by design. Format: CBOR binary.
- Physics-native: Hamiltonians, observables, evolution times — not gates
- Noise-as-type-system: exceeding T2 is a compile-time type error
- AI-primary: optimized for LLM generation, not human writing

The human never sees an Ehrenfest program. The loop is:
`human intent → AI → Ehrenfest → compiler → QPU → result → AI → human`

## QUASI Standard Interface (L4) — Sketch

```rust
// Observable-oriented, not bitstring-oriented
trait QuantumContext {
    fn submit(&self, program: &EhrenfestProgram, shots: u32) -> JobHandle;
    fn await_result<O: Observable>(&self, job: JobHandle) -> O::Output;
    fn capabilities(&self) -> PhysicalContext;
}

trait NoiseContext {
    fn current_t1(&self) -> Duration;
    fn current_t2(&self) -> Duration;
    fn cooling_profile(&self) -> Option<CoolingProfile>;
}

trait ProvenanceContext {
    fn attest(&self) -> ProvenanceCertificate;  // Alsvid integration
}
```

## The Coherence Principle

The project structure mirrors the OS structure mirrors the language structure:

| QUASI OS | QUASI Project |
|----------|---------------|
| L3 Job Scheduler | Public Task Board |
| QPU Backend executes | AI Agent executes task |
| Formal type checker | CI / Spec Validator |
| Provenance Certificate | Attribution Ledger |
| Ehrenfest job unit | Contribution (typed change-set) |

## Further Reading

- [HAL Contract v2.2](https://github.com/valiant-quantum/arvak) — the foundation
- Ehrenfest Concept Paper — in progress
- QUASI Governance — in progress
