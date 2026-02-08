# TODO - Erdős Problem #107

## Completed

- [x] Lean 4 + mathlib setup (elan, lake, cache)
- [x] EmptyHexagonLean template cloned for reference
- [x] Core Lean definitions (GeneralPosition, ConvexPosition, ES(n))
- [x] CEGIS Python pipeline (encode_om3.py, cegis_sat_loop.py, verify_no_alternating.py)
- [x] N=16 counterexample found and verified
- [x] CaDiCaL + cake_lpr installed
- [x] Signotope encoding: N=17 UNSAT with verified LRAT proof
- [x] Lean SAT spec: all clause-family soundness proved (no sorry stubs)
- [x] DIMACS emitter (`emit_cnf`) and `check_unsat.sh`

## Next: Bridge Formalization

- [ ] Prove "general position implies chirotope axioms" in `Bridge.lean`
  - Define `orient(p_i, p_j, p_k)` via sign of 2x2 determinant
  - Show it's never 0 under "no three collinear"
  - Prove rank-3 chirotope constraints
- [ ] Prove "convex hexagon implies forbidden 6-point pattern" in `Bridge.lean`
  - Take six points in convex position, order around hull
  - Show induced chirotope matches one of the forbidden alternating patterns

## Next: Packaging

- [ ] Package UNSAT result as a Lean theorem replayable from CNF + LRAT certificate
- [ ] Formalize 16-point witness in Lean (lower bound half of ES(6)=17)

## Next: Write-Up

- [ ] Document encoding correctness (soundness argument for signotope+GP3+CC+full-triangles)
- [ ] This is the remaining piece for publishability

## Stretch

- [ ] Build CNF encoder for ES(7)=33 case
