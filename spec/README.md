# Ehrenfest Specification — v0.1

The CBOR schema for QUASI's physics-native specification language.

**Schema file:** `ehrenfest-v0.1.cddl`
**Validation tool:** `cddl` (Ruby gem) — see [Validation](#validation) below.

---

## Type Reference

### `EhrenfestProgram`

The root type. Every valid Ehrenfest binary is an instance of this. Contains exactly six fields: `version`, `system`, `hamiltonian`, `evolution`, `observables`, `noise`. There is no optional top-level field — a physically underspecified program is rejected by Afana.

### `PhysicalContext`

Describes the quantum register: how many qubits, optionally which hardware and what cooling profile. The `backend_hint` is non-binding — Afana may select a different backend if it better satisfies the `NoiseConstraint`. The `cooling_profile` field is required only when the program demands sub-15 mK operating conditions (relevant for superconducting qubits in exotic regimes, or Alsvid hardware-attested computations).

### `Hamiltonian`

The energy operator of the quantum system, expressed as a weighted sum of Pauli string terms. The `constant_offset` is the scalar zero-point energy (common in molecular Hamiltonians after Jordan-Wigner transformation). Each `PauliTerm` carries a coefficient in GHz·rad and a list of `PauliOp`s that describe the tensor product: `{"axis": 3, "qubit": 0}` means σᶻ on qubit 0, identity on all others.

### `PauliAxis`

An enum: `I=0, X=1, Y=2, Z=3`. These are the four single-qubit Pauli operators. Every quantum observable can be decomposed into a sum of Pauli strings — this is the physical basis of the Ehrenfest representation.

### `Observable`

What the program measures, expressed in physics vocabulary. Five named variants: `SigmaZ` (⟨σᶻ⟩), `SigmaX` (⟨σˣ⟩), `Energy` (⟨H⟩), `Density` (reduced density matrix), `Fidelity` (overlap with target state). There is no "measure qubit in computational basis" observable — that is a HAL Contract concept, produced by Afana from the physics specification.

### `EvolutionTime`

How long the system evolves under the Hamiltonian and how to slice it. `total_us` and `steps` are the primary specification; `dt_us` is derived (`total_us / steps`) and required for fast deserialization. Afana uses `steps` to determine the Trotter product formula depth. More steps = more accurate but deeper circuit.

### `NoiseConstraint`

Hardware noise requirements enforced at **compile time** by Afana. If the target backend cannot satisfy `t1_us` and `t2_us`, the compilation fails with a type error — not a runtime error. This is Ehrenfest's noise-as-type-system: programs encode their hardware requirements the same way Rust encodes memory safety requirements. `gate_fidelity_min` and `readout_fidelity_min` are optional but strongly recommended for production programs.

---

## Why CBOR, not JSON?

Three reasons:

1. **Size.** A float64 in JSON is 22 bytes (`-1.0000000000000000`). In CBOR float16 it is 3 bytes. The transverse Ising example (361 bytes CBOR) would be ~1.4 KB as JSON.

2. **Binary field packing.** Pauli strings are naturally byte arrays — one byte per qubit, four possible values. JSON would encode them as arrays of integers or base64 strings, adding framing overhead. CBOR bstr is exact.

3. **Deterministic encoding for hashing.** Canonical CBOR (RFC 8949 §4.2) defines a single canonical byte sequence for any value, enabling `SHA256(canonical_cbor(program))` as a stable program identity. JSON has no canonical form (key ordering, whitespace, float precision all vary by encoder).

---

## Why not gates?

Ehrenfest programs express **physics**, not circuit implementation.

A Hamiltonian `H = -Z₀Z₁` means "these two spins are antiferromagnetically coupled." It does not mean CNOT. Whether Afana compiles this to two CNOT gates, one native CZ gate, or a pulse-level Mølmer-Sørensen interaction is Afana's decision, made based on the target hardware's native gate set and the noise constraints.

Expressing the problem as gates would:
- Tie the program to a specific gate set (IBM vs IQM vs Quantinuum differ significantly)
- Prevent ZX-calculus optimization (which operates on the physics representation)
- Make programs non-portable across hardware generations

Ehrenfest programs are portable. Gate sequences are not.

---

## Examples

| File | Description | Size |
|------|-------------|------|
| [`transverse_ising_2q.cbor.hex`](examples/transverse_ising_2q.cbor.hex) | 2-qubit transverse-field Ising model | 361 bytes |
| [`rabi_oscillation_1q.cbor.hex`](examples/rabi_oscillation_1q.cbor.hex) | 1-qubit Rabi oscillation | 216 bytes |
| [`heisenberg_4q.cbor.hex`](examples/heisenberg_4q.cbor.hex) | 4-qubit Heisenberg XXX chain | 683 bytes |

Annotation files (byte-by-byte breakdowns) are in the same directory with `.md` extension.

---

## Validation

### CDDL validation (canonical)

```bash
gem install cddl
# Convert hex to binary first:
python3 -c "import binascii; open('/tmp/ex.cbor','wb').write(binascii.unhexlify(open('spec/examples/transverse_ising_2q.cbor.hex').read().strip()))"
cddl spec/ehrenfest-v0.1.cddl validate /tmp/ex.cbor
```

### Python structural validation (CI)

```bash
python3 spec/tools/validate.py spec/examples/transverse_ising_2q.cbor.hex
python3 spec/tools/validate.py spec/examples/rabi_oscillation_1q.cbor.hex
python3 spec/tools/validate.py spec/examples/heisenberg_4q.cbor.hex
```

### Regenerate examples

```bash
python3 spec/tools/generate_examples.py
```

---

## References

- [RFC 8949](https://datatracker.ietf.org/doc/html/rfc8949) — CBOR
- [RFC 8610](https://datatracker.ietf.org/doc/html/rfc8610) — CDDL
- [Ehrenfest theorem](https://en.wikipedia.org/wiki/Ehrenfest_theorem) — why programs describe expectation values
- [ARCHITECTURE.md](../ARCHITECTURE.md) — Ehrenfest in the QUASI stack
- [HAL Contract](https://github.com/hiq-lab/arvak) — what Afana compiles Ehrenfest programs into
