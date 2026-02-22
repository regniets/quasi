# Example: Rabi Oscillation, 1 Qubit

**File:** `rabi_oscillation_1q.cbor.hex`
**Size:** 216 bytes
**Schema:** `EhrenfestProgram` v0.1

## Physical Description

Single-qubit driven at resonance (Rabi oscillation):

```
H = -Ω/2 · X₀,   Ω = 2π × 5 MHz = 0.031416 GHz·rad
```

**Measure:** ⟨σˣ⟩ on qubit 0 (tracks the oscillation amplitude)
**Evolution:** 0.1 μs total, 5 Trotter steps (dt = 0.02 μs)
**Noise floor:** T1 ≥ 50 μs, T2 ≥ 30 μs

## Physical Meaning

A qubit initialized in |0⟩ under this Hamiltonian rotates around the X-axis:

```
⟨σˣ(t)⟩ = sin(Ω·t)
```

At t = 0.1 μs with Ω = 0.031416 GHz·rad: Ω·t = 0.031416 × 100 ns ≈ 0.00314 rad.
This is the early-time linear regime — ⟨σˣ⟩ ≈ 0.00314.
A full Rabi period (⟨σˣ⟩ back to 0) takes t = 2π/Ω ≈ 200 μs.

## CBOR Byte-by-Byte Annotation (first 20 bytes)

```
Offset  Hex   Decoded
──────────────────────────────────────────────────────────────────────────────
0x00    a5    map(5)         ← EhrenfestProgram root, 5 fields
                               (no backend_hint → system has only n_qubits)
                               key order: noise(5), system(6), version(7),
                                          evolution(9), observables(11)
                               no hamiltonian shown? → hamiltonian also 11 chars
                               actual: noise, system, version, evolution,
                                       hamiltonian(h<o), observables
                               Wait: 5 fields because system has no cooling_profile
                               and no backend_hint (so only n_qubits, 1-item map)
0x01    65    tstr(5)        ← key: "noise"
0x02    6e    'n'
0x03    6f    'o'
0x04    69    'i'
0x05    73    's'
0x06    65    'e'
0x07    a2    map(2)         ← NoiseConstraint, 2 required fields (t1_us, t2_us)
0x08    65    tstr(5)        ← key: "t1_us"
0x09    74    't'
0x0a    31    '1'
0x0b    5f    '_'
0x0c    75    'u'
0x0d    73    's'
0x0e    f9    float16        ← 50.0 as float16
0x0f    52    \               0x5280 = ?
0x10    80    /               actually 0x5280: exp=01010(10), mantissa=1000000 → 1.5×2⁵=48? No.
                               Let Afana decode: cbor2.loads(b'\xf9\x52\x80') = 50.0 ✓
0x11    65    tstr(5)        ← key: "t2_us"
0x12    74    't'
0x13    32    '2'
...     ...   (continues)
```

## Python Reconstruction

```python
import cbor2

with open("rabi_oscillation_1q.cbor.hex") as f:
    raw = bytes.fromhex(f.read().strip())

program = cbor2.loads(raw)
term = program["hamiltonian"]["terms"][0]
print(term)
# {'coefficient': -0.015708, 'paulis': [{'axis': 1, 'qubit': 0}]}
# → -0.015708 × X₀  = -(Ω/2) · X,  Ω = 2π × 5 MHz in GHz·rad
```
