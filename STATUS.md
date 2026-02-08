# STATUS.md - Erdős Problem #107

**Last updated:** 2026-02-08
**Current phase:** Write-up and bridge formalization

## Chat Links

- **ChatGPT:** https://chatgpt.com/c/69638df1-b020-832f-94f3-30cb53394ade

## Goal

Prove ES(6) = 17 via verified SAT: show no 17-point configuration in general position avoids a convex 6-gon.

## Current State

### Compute: Done

The signotope/CC encoding (GP3+CC+full-triangles) is UNSAT on the 17-point instance, with a verified LRAT proof.

**Artifacts (archived):**
- CNF: `artifacts/signotope-6-17/sig_6_17_gp3_cc_full.cnf`
- LRAT: `artifacts/signotope-6-17/sig_6_17_gp3_cc_full.lrat` (~952 MB)
- Checker output: VERIFIED UNSAT (cake_lpr)

**Lower bound:** 16-point counterexample found and verified (N=16 CAN avoid convex 6-gon).

### Lean SAT Spec: Soundness Complete

- Core definitions: GeneralPosition, ConvexPosition, ES(n)
- SAT spec with explicit literals (`Lit.pos`/`Lit.neg`)
- All clause-family soundness proved: swap, cycle, acyclic, GPRel, avoidClause
- `satSpecCNF_sound` proved (no `sorry` stubs in SATCNF.lean)
- DIMACS emitter (`EmitCNF.lean`) and `emit_cnf` executable working
- Build passes: `lake build Erdos107.SATCNF` (warnings only)

### Lean Bridge: Incomplete

`Bridge.lean` has stubs. Needs:
1. General position implies chirotope axioms
2. Convex hexagon implies forbidden 6-point pattern

### Write-Up: Not Started

Encoding correctness documentation (soundness argument for signotope+GP3+CC+full-triangles) needed for publishability.

## Progress

- [x] Lean 4 installed via elan
- [x] Fresh mathlib project created (`erdos107/`)
- [x] Core definitions formalized (GeneralPosition, ConvexPosition, ES(n))
- [x] CEGIS Python pipeline built (encode_om3.py, cegis_sat_loop.py, verify_no_alternating.py)
- [x] N=16 counterexample found and verified
- [x] Signotope encoding: N=17 UNSAT confirmed
- [x] LRAT proof generated and verified (cake_lpr)
- [x] Lean SAT spec with clause generators
- [x] Swap/cycle/acyclic soundness proved
- [x] GPRel soundness proved
- [x] avoidClause soundness proved
- [x] Full satSpecCNF soundness theorem proved
- [x] DIMACS emitter and check_unsat.sh script
- [ ] Bridge.lean: general position implies chirotope axioms
- [ ] Bridge.lean: convex hexagon implies forbidden pattern
- [ ] Package UNSAT result as replayable Lean theorem (CNF + certificate)
- [ ] Formalize 16-point witness in Lean (lower bound)
- [ ] Write-up: encoding correctness documentation

## Previous SAT/CEGIS Pipeline (superseded)

The iterative CEGIS approach reached ~10,000+ of 12,376 six-subsets for N=17 before being superseded by the signotope encoding, which solved the problem in a single SAT call.

State files retained: `erdos107/state_6_17_work.json` (32MB)

## Session Log

### 2026-01-11
- Project initialized
- Lean 4.26.0 installed, mathlib cache downloaded
- Core definitions in ErdosSzekeres.lean

### 2026-01-12
- Lazy SAT runs for N=16 (found counterexample) and N=17 (partial)
- CEGIS loop with witness batching
- Backtracking fix for empty candidate sets

### 2026-01-13
- CEGIS continued overnight (~10k/12k subsets)
- Lean pivot: added OrderType.Acyclic, SATSpec, CNF spec
- Proved swap/cycle/acyclic clause soundness
- Pivoted away from long CEGIS runs to focus on Lean proofs

### 2026-01-15
- Proved GPRel soundness, avoidClause soundness, satSpecCNF soundness (no sorry stubs)
- Added DIMACS emitter and check_unsat.sh

### 2026-01-20
- **BREAKTHROUGH:** Corrected CaDiCaL exit code interpretation (20 = UNSAT, not SAT)
- Signotope/CC encoding confirmed UNSAT for N=17

### 2026-01-21
- LRAT proof generated and verified (cake_lpr: VERIFIED UNSAT)
- OM3 proof run stopped
- Artifacts archived
