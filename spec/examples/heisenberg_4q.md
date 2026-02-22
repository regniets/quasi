# Example: Heisenberg XXX Model, 4 Qubits

**File:** `heisenberg_4q.cbor.hex`
**Size:** 683 bytes
**Schema:** `EhrenfestProgram` v0.1

## Physical Description

4-qubit Heisenberg XXX spin chain (nearest-neighbor, open boundary):

```
H = J · Σ⟨i,j⟩ (X_i X_j + Y_i Y_j + Z_i Z_j)
```

with J = 1.0 GHz·rad and pairs (0,1), (1,2), (2,3). 9 Pauli terms total.

**Measure:** Energy ⟨ψ|H|ψ⟩ in GHz·rad
**Evolution:** 2.0 μs total, 20 Trotter steps (dt = 0.1 μs)
**Noise floor:** T1 ≥ 150 μs, T2 ≥ 100 μs, gate ≥ 0.9995, readout ≥ 0.995

## Physical Meaning

The SU(2)-symmetric Heisenberg model is exactly solvable via Bethe ansatz.
The ground state energy per site for open chain: E₀/J ≈ -1.286 × 3 = -3.857 GHz·rad for 4 sites.
This makes it an ideal benchmark: there is an exact answer to compare against.

Uses cases in QUASI:
- **Ground-state energy estimation** benchmark (compare against classical exact diagonalization)
- **Quantum advantage demonstration** at >20 qubits where classical simulation becomes expensive
- **Noise sensitivity study**: the energy observable is particularly sensitive to T2 errors

## CBOR Byte-by-Byte Annotation (first 20 bytes)

```
Offset  Hex   Decoded
──────────────────────────────────────────────────────────────────────────────
0x00    a5    map(5)         ← EhrenfestProgram root (no backend_hint in system)
0x01    65    tstr(5)        ← key: "noise" (length 5, sorted first)
0x02    6e    'n'
0x03    6f    'o'
0x04    69    'i'
0x05    73    's'
0x06    65    'e'
0x07    a4    map(4)         ← NoiseConstraint, all 4 fields present:
                               t1_us, t2_us, gate_fidelity_min, readout_fidelity_min
0x08    65    tstr(5)        ← key: "t1_us"
0x09    74    't'
0x0a    31    '1'
0x0b    5f    '_'
0x0c    75    'u'
0x0d    73    's'
0x0e    f9    float16        ← 150.0 as float16
0x0f    57    \               0x5780 = 150.0 ✓
0x10    80    /
0x11    65    tstr(5)        ← key: "t2_us"
0x12    74    't'
0x13    32    '2'
...     ...   (continues)
```

### Note: 9-term Hamiltonian encoding

The Hamiltonian encodes 9 PauliTerms as an array. Each PauliTerm is a 2-item map:
`{"coefficient": 1.0, "paulis": [{"axis": N, "qubit": M}, {"axis": N, "qubit": M}]}`.

Canonical CBOR uses float16 for `1.0` (2 bytes) and uint for qubit indices and axis values (1 byte each).
Total Hamiltonian encoding: ~330 of the 683 bytes.

## Python Reconstruction

```python
import cbor2

with open("heisenberg_4q.cbor.hex") as f:
    raw = bytes.fromhex(f.read().strip())

program = cbor2.loads(raw)
print(f"Qubits: {program['system']['n_qubits']}")
print(f"Hamiltonian terms: {len(program['hamiltonian']['terms'])}")
print(f"Observable: {program['observables'][0]['type']}")
print(f"T1/T2: {program['noise']['t1_us']}/{program['noise']['t2_us']} μs")
# Qubits: 4
# Hamiltonian terms: 9
# Observable: E
# T1/T2: 150.0/100.0 μs
```
