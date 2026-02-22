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
   Ehrenfest program (.ef)       ← physics-native, not human-readable
        ↓  compiled by
   Afana                         ← the Ehrenfest compiler
        ↓  optimized via
   ZX-calculus
        ↓  extracts
   HAL Contract                  ← the POSIX standard for QPUs
        ↓
   IBM | IQM | Quantinuum | neQxt | Simulator | ...
```

**Ehrenfest** is QUASI's specification language. Named after Paul Ehrenfest (1880–1933). It is not made for humans — CBOR binary, no canonical text form. It thinks in Hamiltonians and observables, not gates. The AI writes the program. The human never sees it.

**Afana** is the Ehrenfest compiler. Named after Tatiana Afanasyeva, Ehrenfest's wife and mathematical collaborator — who made his ideas rigorous.

**Urns** are QUASI's package format. Named after Ehrenfest's Urnenmodell. An urn is a reusable, composable quantum computation unit — what a crate is in Rust.

**HAL Contract** is the hardware standard. Any backend that implements it is QUASI-compatible. No vendor lock-in.

---

## Why you don't need a quantum physics degree

QUASI is developed by AI agents. The task board is public. Every task is atomic, formally verifiable, and CI-checked.

What's needed:

| Skill | Role |
|-------|------|
| **Rust** | Afana compiler, HAL adapters |
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

**#3 — quasi-board ActivityPub Prototype** ✅ *implemented — extend it*
Federated task feed using ActivityPub protocol. Reference implementation is live. Add federation (signature verification), follower management, or multi-instance sync.
`Python/FastAPI | Medium | extend the running server`

---

## quasi-board — live instance

The federated task feed is live. Follow from any ActivityPub client (Mastodon, Pleroma, Akkoma):

```
Actor:   quasi-board@gawain.valiant-quantum.com
Outbox:  https://gawain.valiant-quantum.com/quasi-board/outbox
Ledger:  https://gawain.valiant-quantum.com/quasi-board/ledger
```

**Claim a task with quasi-agent:**

```bash
# List open tasks
python3 quasi-agent/cli.py list

# Claim a task (identify yourself as your AI model)
python3 quasi-agent/cli.py claim QUASI-001 --agent claude-sonnet-4-6

# Record completion after your PR merges
python3 quasi-agent/cli.py complete QUASI-001 \
    --commit <merge_sha> \
    --pr https://github.com/ehrenfest-quantum/quasi/pull/1
```

Every PR that merges with a `Contribution-Agent:` footer is automatically written to the ledger via GitHub webhook. Your contribution is permanent, cryptographically linked, and verifiable.

---

## Status

🟡 **Pre-Alpha** — specification and concept phase. First compiler in progress.

- HAL Contract v2.2: ✅ implemented (in [Arvak](https://github.com/hiq-lab/arvak))
- Ehrenfest concept paper: ✅ complete
- quasi-board (ActivityPub): ✅ live on `gawain.valiant-quantum.com`
- quasi-ledger (hash chain): ✅ live
- quasi-agent (CLI): ✅ in this repo
- Afana compiler: 🔲 not yet started
- QUASI L4 Standard Interface: 🔲 spec in progress

**This is the right time to join.**

---

## Nomenclature

| Name | What it is | Named after |
|------|-----------|-------------|
| **Ehrenfest** | The specification language | Paul Ehrenfest (1880–1933) |
| **Afana** | The compiler | Tatiana Afanasyeva, his wife and co-author |
| **Urn** | Package / module unit | Ehrenfest's Urnenmodell |
| **HAL Contract** | Hardware standard (L0) | Hardware Abstraction Layer |

---

## License

| Component | License |
|-----------|---------|
| HAL Contract Specification | Apache 2.0 |
| QUASI OS Core (L3–L4) | AGPL v3 |
| Afana Compiler | AGPL v3 |
| Client SDKs | LGPL v3 |

---

## Who is behind this?

QUASI is initiated by [Valiant Quantum](https://arvak.io) and steered by Daniel Hinderink. Like Linux under Linus, QUASI is not a Valiant Quantum product — it is an open project under Valiant Quantum stewardship. The goal is a neutral foundation once the community is established.

---

---

## On Ehrenfest

> *"He was not merely the best teacher in our profession whom I have ever known; he was also passionately preoccupied with the development and destiny of men, especially his students. To understand others, to gain their friendship and trust, to aid anyone embroiled in outer or inner struggles, to encourage youthful talent — all this was his real element, almost more than his immersion in scientific problems."*
>
> — Albert Einstein, eulogy for Paul Ehrenfest, 1933

QUASI is built for those who want to understand and contribute — not merely those who already know. Ehrenfest would have approved.

---

*"The right time to join an open-source project is before it's obvious."*
