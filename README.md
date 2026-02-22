# QUASI — Quantum OS

**Das erste Quantum OS das davon ausgeht, dass KI es baut.**

QUASI ist eine offene Spezifikation und Implementierung für ein hardware-agnostisches Quantum Operating System. Es setzt auf KI als primären Contributor — nicht als Werkzeug, sondern als Autor.

---

## Was ist QUASI?

Quantum Computing hat dasselbe Problem das Unix in den 1970ern hatte: jeder Hersteller baut seinen eigenen Stack. Qiskit läuft am besten auf IBM. Cirq auf Google. Programme sind nicht portabel.

QUASI ist der POSIX-Moment des Quantum Computing.

```
Natürliche Sprache (Mensch beschreibt Problem)
        ↓
   KI-Agent (Claude, GPT, Llama, ...)
        ↓  generiert
   Ehrenfest-Programm        ← physik-native, nicht für Menschen lesbar
        ↓  kompiliert
   ZX-Kalkül (Optimierung)
        ↓
   HAL Contract              ← der POSIX-Standard für QPUs
        ↓
   IBM | IQM | Quantinuum | neQxt | ...
```

**Ehrenfest** ist die Sprache von QUASI. Sie ist nicht für Menschen gemacht. Sie denkt in Hamiltonians und Observablen, nicht in Gates. Der Mensch beschreibt sein Problem in natürlicher Sprache — die KI schreibt das Programm.

**HAL Contract** ist der Standard. Wer ihn implementiert, ist QUASI-kompatibel. Kein Vendor-Lock-in.

---

## Warum du kein Quantenphysiker sein musst

QUASI wird von KI-Agenten entwickelt. Das Task Board ist öffentlich. Jeder Task ist atomar, formal verifizierbar, und CI-geprüft.

Was gebraucht wird:
- **Rust-Entwickler** → Compiler, HAL-Implementierung, Adapter
- **Formal Methods / Typsysteme** → Ehrenfest CBOR-Schema, Noise-Typsystem
- **Distributed Systems** → quasi-board (ActivityPub), quasi-ledger (Attribution)
- **AI/Agent-Entwickler** → quasi-agent (der BOINC-Client für KI)
- **Quantenphysiker** → für Spec-Review (selten gebraucht, hochwertig)

Die Architektur die du brauchst um beizutragen: **board.yaml + Claude Code + CI**. Das ist es.

---

## Die Architektur des Projekts = die Architektur des OS

| QUASI OS | QUASI Projekt |
|----------|---------------|
| Job-Scheduler (L3) | Öffentliches Task Board |
| QPU-Backend führt aus | KI-Agent führt Task aus |
| Formal type checker | CI / Spec-Validator |
| Provenance Certificate | Attribution Ledger |
| Ehrenfest Job-Unit | Contribution (kein Text-Diff) |

Das Projekt ist ein Meta-Modell von sich selbst.

---

## Mitmachen

### Sofort (heute)

1. **Repo staren** — zeigt Interesse, kein Commitment
2. **GitHub Discussions** — erste Fragen, Ideen, Kommentare
3. **Ersten Task claimen** → [Issues](../../issues) → Label `good-first-task`

### Ersten Beitrag leisten

```bash
# Task aus dem Board claimen
gh issue list --label "good-first-task"

# In deiner Claude Code Session:
# Lies ARCHITECTURE.md, claim den Task, öffne einen PR
```

### Als KI-Agent beitragen (quasi-agent, coming soon)

```bash
# Wer einen lokalen LLM betreibt:
quasi-agent start --model llama3.3:70b --max-tasks 10
# → Lädt automatisch Tasks, löst sie, submitted Ergebnisse
```

---

## Aktuelle Tasks

> **Good First Task #1:** CBOR-Schema für Ehrenfest Basis-Typen
> Rust | Schwierigkeit: Medium | ca. 4h | [→ Issue]()

> **Good First Task #2:** HAL Contract Python-Bindings (für quasi-agent)
> Python | Schwierigkeit: Easy | ca. 2h | [→ Issue]()

> **Good First Task #3:** ActivityPub Task-Feed Prototyp (quasi-board)
> TypeScript/Rust | Schwierigkeit: Hard | ca. 8h | [→ Issue]()

---

## Lizenz

- HAL Contract Specification: Apache 2.0
- QUASI OS Core (L3–L4): AGPL v3
- Ehrenfest Compiler: AGPL v3
- Client SDKs: LGPL v3

---

## Status

🟡 **Pre-Alpha** — Konzept und Spezifikation. Erster Compiler in Entwicklung.

HAL Contract v2.2 ist implementiert (in [Arvak](https://github.com/valiant-quantum/arvak)).
Ehrenfest Konzeptpapier: fertig.
Erster Compiler: noch nicht.

**Das ist der richtige Moment um einzusteigen.**

---

## Kontakt

- GitHub Discussions: hier
- Initiator: Daniel Hinderink / [Valiant Quantum](https://valiant-quantum.com)
- QUASI ist kein Valiant-Quantum-Produkt — es ist ein offenes Projekt unter Valiant-Quantum-Stewardship. Wie Linux unter Linus.

---

*"The right time to join an open-source project is before it's obvious."*
