# Signotope 6-gon avoidance (N=17) — Verified UNSAT

This folder archives the CNF instance, LRAT proof, and checker output for the
signotope + GP3 + CC + full-triangles encoding at N=17, n=6.

## Files
- `sig_6_17_gp3_cc_full.cnf` — CNF instance
- `sig_6_17_gp3_cc_full.lrat` — LRAT proof from CaDiCaL
- `sig_6_17_gp3_cc_full_lrat_check.log` — cake_lpr verification log
- `SHA256SUMS` — hashes for the above

## CNF header
- `p cnf 131580 1402417`

## Commands used
Solve + proof:
```
cadical --congruence=0 --report --lrat=1 \
  /tmp/sig_6_17_gp3_cc_full.cnf /tmp/sig_6_17_gp3_cc_full.lrat
```

Verify:
```
cake_lpr /tmp/sig_6_17_gp3_cc_full.cnf /tmp/sig_6_17_gp3_cc_full.lrat
```

## Verification result
From `sig_6_17_gp3_cc_full_lrat_check.log`:
- `s VERIFIED UNSAT`
