# CLAUDE.md - Erdős Problem #107

## Project Goal

Formalize Erdős Problem #107 (Erdős–Szekeres conjecture) in Lean 4, aiming for a verified reduction from geometry to SAT.

**The conjecture:** ES(n) = 2^(n-2) + 1, where ES(n) is the minimum number of points in general position guaranteeing a convex n-gon.

**Known results:**
- ES(4) = 5 (classical)
- ES(5) = 9 (classical)
- ES(6) = 17 (Szekeres & Peters 2006, SAT-based)
- ES(7) = 33 (conjectured, unproven)

## Approach

Following the EmptyHexagonLean template:
1. Formalize core definitions (general position, convex position, ES(n))
2. Build reduction: geometric counterexample → orientation assignment → CNF formula
3. UNSAT of CNF kills counterexample

## Role: Technical Collaborator

Claude can:
- Help with Lean syntax and mathlib navigation
- Debug compilation errors
- Suggest proof strategies
- Write glue code between definitions

Claude should:
- Demand exact error messages before proposing fixes
- Verify lemma names exist in mathlib before using them
- Use Loogle queries when uncertain about mathlib API
- Keep proofs compilable, not "pretty nonsense"

## Key Resources

- **Template repo:** [EmptyHexagonLean](https://github.com/bsubercaseaux/EmptyHexagonLean)
- **Mathlib docs:** [mathlib4 docs](https://leanprover-community.github.io/mathlib4_docs/)
- **Loogle (lemma search):** [loogle.lean-lang.org](https://loogle.lean-lang.org/)

## Mathlib primitives to use

- Convex independence: `Mathlib.Analysis.Convex.Independent`
- Convex hull: `Mathlib.Analysis.Convex.Hull`
- Collinearity: `Mathlib.LinearAlgebra.AffineSpace.Independent`

## Build Commands

```bash
# After Lean setup
lake exe cache get   # Download mathlib cache
lake build           # Build project
```

## SAT Infrastructure (install later)

- **CaDiCaL:** SAT solver
- **cake_lpr:** Verified LRAT proof checker

Not needed for initial formalization work.
