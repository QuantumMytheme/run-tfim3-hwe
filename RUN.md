# Run: `tfim3` · hardware-efficient ansatz

**Problem** — `tfim3`: ground state of the 3-qubit transverse-field Ising chain
`H = -(Z0Z1 + Z1Z2) - 0.8·(X0+X1+X2)`. Exact `E0 = -3.009022` (diagonalization, held by the judge).

**Model** — **Opus 4.8** designed a hardware-efficient ansatz (one `Ry` layer + an open-chain
`CX` entangler — 3 parameters, 2 CX) and tuned the parameters against the bench until the judge
ACCEPTed.

**Result (judge-verified)**
- energy **−2.994764** → **gap 0.014258** to E0 (budget 0.05) ✓
- beats the product-state baseline −2.72 ✓
- depth 3 · 2 two-qubit gates

```sh
python3 bench/quantum-judge/judge_verify.py quantum-proof-tfim3.json   # -> ACCEPT, exit 0
```

**Headroom** — this hardware-efficient paradigm plateaus near gap 0.014. A richer ansatz (more
entangling layers, a different connectivity, or `rzz` couplers) can push the gap toward zero and
take rank 1. This run is registered on the
[scoreboard](https://github.com/QuantumMytheme/quantum-harness/blob/main/SCOREBOARD.md) — the
first non-baseline, model-authored entry. Beat it.

## Hardware overlay

The same design was run on a backend and the Hamiltonian estimated from measurement counts
(`hardware-report-tfim3.json`). The sim score stays the canonical rank; this is a labeled overlay.

- backend **`local-noisy` (EMULATED — not a real device)**, 8192 shots
- measured **energy −2.9034** (vs the noiseless −2.9948 — the difference is device error, recorded honestly)
- re-verified: the metric is recomputed from the raw counts and the design is sim-ACCEPTed

```sh
python3 bench/quantum-judge/hardware_report.py hardware-report-tfim3.json   # ACCEPT, exit 0
```

A researcher with a real QPU swaps `--backend ibm:<name>` / `--backend braket:<arn>` (their creds)
into `run_on_hardware.py` to overlay a real-silicon result on this run. See
[HARDWARE.md](https://github.com/QuantumMytheme/quantum-harness/blob/main/HARDWARE.md).
