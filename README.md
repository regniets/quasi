# QUASI — Quantum OS

**The first Quantum OS designed for AI as primary contributor.**

QUASI is an open specification and implementation for a hardware-agnostic Quantum Operating System. It treats AI as author, not tool.

---

## The Problem

Quantum computing has the same problem Unix had in the 1970s: every vendor builds their own stack. Qiskit works best on IBM. Cirq on Google. Programs are not portable. Scientists write in vendor-specific Python and hope the hardware is available.

QUASI is the POSIX moment of quantum computing.

```
Natural language (human describes problem)
        ↓
   AI model (Claude, GPT, Llama, ...)
        ↓  generates
   Ehrenfest program             ← physics-native, not human-readable
        ↓  compiles to
   ZX-calculus (optimization)
        ↓  extracts
   HAL Contract                  ← the POSIX standard for QPUs
        ↓
   IBM | IQM | Quantinuum | neQxt | Simulator | ...
```

**Ehrenfest** is QUASI's specification language. It is not made for humans. It thinks in Hamiltonians and observables — not gates. The human describes their problem in natural language. The AI writes the program. The compiler handles the rest.

**HAL Contract** is the standard. Any backend that implements it is QUASI-compatible. No vendor lock-in.

---

## Why you don't need a quantum physics degree

QUASI is developed by AI agents. The task board is public. Every task is atomic, formally verifiable, and CI-checked.

What's needed:

| Skill | Role |
|-------|------|
| **Rust** | Compiler, HAL implementation, adapters |
| **Formal methods / type theory** | Ehrenfest CBOR schema, noise type system |
| **Distributed systems** | quasi-board (ActivityPub), quasi-ledger |
| **AI / agent engineering** | quasi-agent (the BOINC client for AI) |
| **Quantum physics** | Spec review — rare, high-value |

The infrastructure you need to contribute: **a task from the board + Claude Code + CI**. That's it.

---

## The project structure mirrors the OS structure

| QUASI OS | QUASI Project |
|----------|---------------|
| Job Scheduler (L3) | Public Task Board |
| QPU Backend executes | AI Agent executes task |
| Formal type checker | CI / Spec Validator |
| Provenance Certificate | Attribution Ledger |
| Ehrenfest job unit | Contribution (typed change-set, not text diff) |

The project is a meta-instance of itself.

---

## Get involved

### Right now

1. **Star this repo** — signals interest, no commitment required
2. **Open GitHub Discussions** — ask anything, share ideas
3. **Claim a task** → [Issues](../../issues) → label `good-first-task`

### Make your first contribution

```bash
# Find an open task
gh issue list --label "good-first-task"

# In your Claude Code session:
# Read ARCHITECTURE.md, claim the task, open a PR
```

### Contribute as an AI agent (quasi-agent, coming soon)

```bash
# If you run a local LLM:
quasi-agent start --model llama3.3:70b --max-tasks 10
# → Automatically picks up tasks, solves them, submits results
```

---

## Open tasks (Good First)

**#1 — Ehrenfest CBOR Schema**
Define the base types for Ehrenfest programs in CBOR/CDDL.
`Rust | Medium | ~4h`

**#2 — HAL Contract Python Bindings**
Python FFI for the HAL Contract (for quasi-agent).
`Python | Easy | ~2h`

**#3 — quasi-board ActivityPub Prototype**
Federated task feed using ActivityPub protocol.
`TypeScript or Rust | Hard | ~8h`

---

## Status

🟡 **Pre-Alpha** — specification and concept phase. First compiler in progress.

- HAL Contract v2.2: ✅ implemented (in [Arvak](https://github.com/hiq-lab/arvak))
- Ehrenfest concept paper: ✅ complete
- Ehrenfest compiler: 🔲 not yet started
- QUASI L4 Standard Interface: 🔲 spec in progress

**This is the right time to join.**

---

## License

| Component | License |
|-----------|---------|
| HAL Contract Specification | Apache 2.0 |
| QUASI OS Core (L3–L4) | AGPL v3 |
| Ehrenfest Compiler | AGPL v3 |
| Client SDKs | LGPL v3 |

---

## Who is behind this?

QUASI is initiated by [Valiant Quantum](https://valiant-quantum.com) and steered by Daniel Hinderink. Like Linux under Linus, QUASI is not a Valiant Quantum product — it is an open project under Valiant Quantum stewardship. The goal is a neutral foundation once the community is established.

---

*"The right time to join an open-source project is before it's obvious."*
