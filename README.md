# Erdős Problem #107 (Erdős–Szekeres): ES(6) = 17 via a certified SAT pipeline

This repository is an experiment in end-to-end checkability: take a combinatorial-geometry statement, reduce it to a finite combinatorial object, encode that object as SAT, and have the critical computational step accompanied by a machine-checkable certificate.

The motivating question was pragmatic rather than heroic. Given the recent surge of LLM-assisted mathematics, could someone with minimal prior background use an LLM as a tool to assemble a proof pipeline whose correctness does not depend on trusting the LLM? That person is me. I understand basically nothing about discrete geometry in general or this problem in particular. The work was all done with ChatGPT 5.2 pro and codex.

---

## What Problem #107 is

Problem #107 in the Erdős Problems Database is the Erdős–Szekeres “Happy Ending” problem.

Let `f(n)` be the least number such that any set of `f(n)` points in the plane in general position (no three collinear) contains `n` points that are the vertices of a convex `n`-gon. The conjecture is:

`f(n) = 2^(n−2) + 1`.

The full conjecture is open for `n ≥ 7`. This repo targets the first genuinely nontrivial case `n = 6`, i.e. **ES(6) = f(6) = 17**.

---

## What ES(6) = 17 says

ES(6) = 17 has two halves:

1. **Upper bound (≤ 17):** every set of 17 planar points in general position contains 6 points in convex position (a convex hexagon).
2. **Lower bound (≥ 17):** there exists a configuration of 16 planar points in general position with no convex hexagon.

The second half is a witness construction. The first half is the hard classification step; historically it’s where computer assistance becomes useful.

---

## Strategy: geometry → order types → SAT

The core move is to replace coordinates with combinatorial data.

### 1) Order types / chirotopes
For a point set in general position, every ordered triple `(i, j, k)` has a well-defined orientation (clockwise vs counterclockwise). Collecting these orientations gives an **order type** (often formalised as a rank-3 **chirotope**), which satisfies a small family of axioms (alternation plus a Grassmann–Plücker-style constraint).

### 2) “Contains a convex hexagon” is detectable from the order type
Whether a set contains 6 points in convex position depends only on the order type. In particular, if six points lie on a convex hexagon, then (after ordering them cyclically around the hull) their induced chirotope agrees with the “alternating” 6-point pattern. So “no convex hexagon” can be expressed as a finite list of forbidden 6-point patterns.

### 3) Encode as SAT
Introduce Boolean variables for the orientation of each triple. Add CNF clauses enforcing the chirotope axioms, and add clauses forbidding each 6-point alternating pattern. Intended meaning:

- a satisfying assignment  ⇔  an abstract order type on `N` points with **no** convex 6-gon.

For `N = 17`, the claim `ES(6) ≤ 17` becomes:

- there is **no** such order type  ⇔  the corresponding CNF is **UNSAT**.

### 4) Certificate-checked UNSAT
A modern SAT solver (CaDiCaL) can emit an LRAT proof of unsatisfiability. That proof can be verified by a small independent checker (here, `cake_lpr`). This reduces the trust burden: we don’t merely “trust the solver output”; we check a proof object.

### 5) Bridging back to geometry (Lean)
The remaining mathematical work is to connect a real planar point configuration to the abstract chirotope layer:

- every point set in general position induces a chirotope satisfying the axioms; and
- a convex hexagon in the point set implies the presence of one of the forbidden 6-point patterns.

With those bridge lemmas, UNSAT implies the geometric statement “every 17-point set contains a convex hexagon”.

---

## Repository layout

Top level:

- `erdos107/` — main Lean 4 project, SAT encoding tooling, and scripts.
- `EmptyHexagonLean/` — upstream template/reference project (a larger verified SAT-based geometry result).
- `artifacts/` — generated CNFs / proofs / maps (reproducible outputs).

Inside `erdos107/` (high-level map):

- `Erdos107/ErdosSzekeres.lean` — geometry-facing definitions and (eventually) witnesses (e.g. a 16-point construction).
- `Erdos107/OrderType.lean` — abstract order types / chirotopes.
- `Erdos107/Bridge.lean` — geometry ↔ order-type bridge (currently incomplete).
- `Erdos107/SATCNF.lean` — Lean definition of the CNF specification.
- `Erdos107/EmitCNF.lean` — DIMACS emitter.
- `scripts/encode_om3.py` — small standalone encoder (useful for sanity checks).
- `scripts/check_unsat.sh` — helper for running CaDiCaL and verifying LRAT with `cake_lpr`.

---

## Reproducing the SAT side

From inside `erdos107/`:

### Build Lean
```bash
lake build
````

### Small sanity-check encodings

```bash
python3 scripts/encode_om3.py --n 4 --N 4 --out om3_4_4.cnf
cadical om3_4_4.cnf
```

### Emit DIMACS from Lean

```bash
lake exe emit_cnf 6 17 out.cnf out.map.txt
```

### Check UNSAT with a certificate

(Requires `cadical` and `cake_lpr`.)

```bash
scripts/check_unsat.sh 6 17 out.cnf
```

---

## What is “proved” here, and what remains

This repo aims to minimise the trust gap:

* SAT results are intended to be accompanied by LRAT certificates checked by an independent verifier.
* The reduction is made explicit in Lean, with a dedicated bridge layer.

What remains for a fully internalised Lean proof of **ES(6) = 17** is to replace the `Bridge.lean` stubs with the geometric → chirotope correctness lemmas, and to package the UNSAT result as a Lean theorem replayable from the generated CNF + certificate.

---

## Roadmap: completing the bridge (geometry → SAT → geometry)

1. **State the bridge goal as a single Lean theorem.**
   Input: `P : Fin 17 → ℝ²` + “general position”.
   Output: if `P` has no convex hexagon, then its induced chirotope yields a satisfying assignment for the CNF.

2. **Choose a Lean-friendly definition of “convex hexagon”.**
   A tractable route is: “there exists a cyclic ordering of six points whose orientation pattern matches the alternating 6-point chirotope”.

3. **Prove “general position ⇒ chirotope axioms”.**
   Define `orient(p_i, p_j, p_k)` via the sign of a 2×2 determinant. Show it’s never 0 under “no three collinear”, and prove the rank-3 chirotope constraints.

4. **Prove “convex hexagon ⇒ forbidden 6-point pattern”.**
   Take six points in convex position, order them around the hull, and show their induced chirotope matches one of the forbidden alternating patterns (up to reversal/global sign convention).

5. **Regression-test the CNF specification.**
   Cross-check the Python encoder and the Lean emitter on small instances (SAT/UNSAT answers, clause counts, variable mapping), to avoid proving theorems about the wrong CNF.

6. **Pin artefacts (CNF + LRAT + hashes).**
   Store the instance CNF, variable map, and LRAT proof (or LFS pointers), and record SHA256s so future reruns are meaningfully comparable.

7. **Do the lower bound (16-point witness) in the same trust-minimised style.**
   Either:

   * give explicit coordinates and prove “no 6-subset is convex”; or
   * give an abstract chirotope witness for `N = 16` avoiding the forbidden patterns (optionally realise geometrically later).

---

## References

* Erdős Problems Database, Problem #107:
  [https://www.erdosproblems.com/107](https://www.erdosproblems.com/107)

* Background on the n=6 case (computer-assisted proof history) and later asymptotic work:
  [https://arxiv.org/abs/1608.01336](https://arxiv.org/abs/1608.01336)

```

::contentReference[oaicite:0]{index=0}
```
