# Example: Transverse-Field Ising Model, 2 Qubits

**File:** `transverse_ising_2q.cbor.hex`
**Size:** 361 bytes
**Schema:** `EhrenfestProgram` v0.1

## Physical Description

2-qubit transverse-field Ising model near the quantum critical point (h/J = 0.5):

```
H = -J · Z₀Z₁  -  h · (X₀ + X₁)
```

with J = 1.0 GHz·rad, h = 0.5 GHz·rad.
The ZZ coupling creates spin-spin entanglement; the transverse field drives quantum fluctuations.
At h/J = 1.0 this model undergoes a quantum phase transition.

**Measure:** ⟨σᶻ⟩ on qubit 0 and qubit 1
**Evolution:** 1.0 μs total, 10 Trotter steps (dt = 0.1 μs)
**Noise floor:** T1 ≥ 100 μs, T2 ≥ 80 μs, gate fidelity ≥ 0.999
**Backend hint:** `ibm_torino`

## CBOR Byte-by-Byte Annotation (first 20 bytes)

CBOR canonical encoding (RFC 8949 §4.2) sorts map keys by length first, then lexicographically.
Top-level key order: `noise` (5), `system` (6), `version` (7), `evolution` (9), `hamiltonian` (11), `observables` (11→h<o).

```
Offset  Hex   Decoded
──────────────────────────────────────────────────────────────────────────────
0x00    a6    map(6)         ← EhrenfestProgram root, 6 fields
0x01    65    tstr(5)        ← key: "noise" (shortest key first)
0x02    6e    'n'
0x03    6f    'o'
0x04    69    'i'
0x05    73    's'
0x06    65    'e'
0x07    a3    map(3)         ← NoiseConstraint value, 3 fields
0x08    65    tstr(5)        ← key: "t1_us"
0x09    74    't'
0x0a    31    '1'
0x0b    5f    '_'
0x0c    75    'u'
0x0d    73    's'
0x0e    f9    float16        ← CBOR major 7, additional 25 = IEEE 754 float16
0x0f    56    \               float16: 0x5640 = 100.0 (T1 = 100 μs)
0x10    40    /
0x11    65    tstr(5)        ← key: "t2_us"
0x12    74    't'
0x13    32    '2'
...     ...   (continues)
```

### Note on float16

CBOR canonical encoding uses the smallest floating-point size that preserves the value exactly.
`100.0` and `80.0` both fit in IEEE 754 float16, so the encoder uses `f9` (2 bytes) instead of `f9`/`fb` (8 bytes).
`0.999` requires float64 (`fb`). This is standard canonical CBOR — Afana handles all three widths.

## Python Reconstruction

```python
import cbor2, binascii

with open("transverse_ising_2q.cbor.hex") as f:
    raw = bytes.fromhex(f.read().strip())

program = cbor2.loads(raw)
print(program["hamiltonian"]["terms"][0])
# {'coefficient': -1.0, 'paulis': [{'axis': 3, 'qubit': 0}, {'axis': 3, 'qubit': 1}]}
# → -1.0 × Z₀Z₁
```
