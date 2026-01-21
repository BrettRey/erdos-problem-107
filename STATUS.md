# STATUS.md - Erdős Problem #107

**Last updated:** 2026-01-21
**Current phase:** Verified signotope LRAT proof (UNSAT)

## Chat Links

- **ChatGPT:** https://chatgpt.com/c/69638df1-b020-832f-94f3-30cb53394ade

## Goal

Prove ES(6) = 17 via verified SAT: show no 16-point configuration in general position avoids a convex 6-gon.

## Current State

### BREAKTHROUGH: Signotope Approach Verified (2026-01-21)

**Key correction:** CaDiCaL exit code 20 = UNSAT (not SAT as previously misread).

**Result:** The signotope/CC encoding (GP3+CC+full-triangles) is UNSAT on the 17-point instance, with a verified LRAT proof.

**Artifacts:**
- CNF: `/tmp/sig_6_17_gp3_cc_full.cnf`
- LRAT: `/tmp/sig_6_17_gp3_cc_full.lrat` (~952 MB)
- Checker log: `erdos107/logs/sig_6_17_gp3_cc_full_lrat_check.log` (VERIFIED UNSAT)

**OM3 backup:** Stopped (no longer needed).

### Previous SAT/CEGIS Pipeline (superseded)
- **N=16 case:** Found valid counterexample (verified) - 16 points CAN avoid convex 6-gon
- **N=17 case:** CEGIS loop iterated through ~10,000+ of 12,376 six-subsets
- **Status:** Superseded by signotope approach
- **Latest state:** `erdos107/state_6_17_work.json` (32MB)

### Lean Formalization
- Core definitions complete (GeneralPosition, ConvexPosition, ES(n))
- SAT spec with explicit literals (`Lit.pos`/`Lit.neg`)
- **Proved:** swap/cycle/acyclic clause soundness
- **Stubbed:** GPRel soundness, avoidClause soundness, full satSpecCNF soundness
- Build passes: `lake build Erdos107.SATCNF` (warnings only)

## Progress

- [x] Lean 4 installed via elan
- [x] Fresh mathlib project created (`erdos107/`)
- [x] Core definitions formalized
- [x] CEGIS Python pipeline built (encode_om3.py, cegis_sat_loop.py, verify_no_alternating.py)
- [x] N=16 counterexample found and verified
- [x] N=17 CEGIS iterations (partial saturation)
- [x] Lean SAT spec with clause generators
- [x] Swap/cycle/acyclic soundness proved
- [ ] GPRel/avoidance soundness
- [ ] Full satSpec soundness theorem
- [ ] N=17 UNSAT (or full saturation)

## Open Questions

- Resume CEGIS with better heuristics, or wait for Lean proofs to mature?
- Is the "tail" fundamentally hard, or is there a smarter subset ordering?
- Could parallel SAT solving help?

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
- Decision: keep state files, resume CEGIS later if needed

### 2026-01-14
- STATUS.md updated to reflect actual progress

### 2026-01-20
- **BREAKTHROUGH:** Corrected CaDiCaL exit code interpretation (20 = UNSAT, not SAT)
- All signotope scouts (GP3-only, GP3+CC, GP3+CC+full-triangles) confirmed UNSAT
- Signotope/CC encoding sufficient to refute 17-point "no convex 6-gon" instance

### 2026-01-21
- LRAT proof generated and verified (cake_lpr: VERIFIED UNSAT)
- OM3 proof run stopped
