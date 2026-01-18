# TODO - Erdős Problem #107

## Immediate (Today)

- [ ] Install elan (Lean version manager)
- [ ] Install VS Code Lean 4 extension
- [ ] Verify `lean --version` works

## Option A: Clone Template First (Recommended)

- [ ] Clone EmptyHexagonLean
- [ ] Run `lake exe cache get` in Lean/ folder
- [ ] Run `lake build`
- [ ] Run convex 6-gon encoder (see pipeline end-to-end)

## Option B: Fresh Project

- [ ] Create mathlib project via `lake new erdos107 math`
- [ ] Download mathlib cache
- [ ] Create ErdosSzekeres107.lean with core definitions

## Later

- [ ] Install CaDiCaL (SAT solver)
- [ ] Install cake_lpr (verified LRAT checker)
- [ ] Formalize reduction chain
- [ ] Build CNF encoder for ES(7)=33 case

## Notes

GPT-5.2 Pro recommends Option A first to get a mental model of the full pipeline before writing custom code.
