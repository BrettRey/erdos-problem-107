# Verified UNSAT for N=17, no convex 6‑gon (signotope + GP3 + CC)

## Statement proved by the certificate
This certificate establishes the following **upper bound**:

> Every set of 17 points in general position in the plane contains a convex 6‑gon.

Equivalently, there is **no** 17‑point general‑position configuration that avoids a convex 6‑gon.

This is an **upper bound** result (g(6) ≤ 17). To conclude g(6)=17, one also needs a
16‑point configuration with no convex 6‑gon (not included here).

## Encoding summary
We encode the combinatorial structure of planar point sets via a sign‑assignment to
oriented triples, plus standard axioms used in the literature. The encoding uses:

1) **Orientation variables**
- Boolean variable o(i,j,k) for each ordered triple with i<j<k.
- Boolean value represents orientation (CCW vs CW); zero is not allowed, matching
  general position (no three collinear).

2) **Signotope axioms (4‑tuple alternation forbidden)**
For every a<b<c<d, forbid the alternating patterns on the 4‑tuple:
- (+ − + −) and (− + − +) over the sequence
  (a,b,c), (a,b,d), (a,c,d), (b,c,d).

3) **3‑term Grassmann–Plücker (GP3)**
For each a and b<c<d<e, add the 3‑term GP relation on the oriented triples
(a,b,c), (a,b,d), (a,b,e), (a,c,d), (a,c,e), (a,d,e).

4) **CC‑system axioms**
- Interiority: if tqr and ptr and pqt, then pqr.
- Transitivity: if tsp and tsq and tsr and tpq and tqr, then tpr.

5) **No‑6‑gon constraint via “inside a triangle”**
For every 6‑set S and each point p in S, we introduce an inside‑triangle predicate
for every triangle (a,b,c) ⊆ S\{p}:

p is inside triangle (a,b,c) iff the three edge orientations with p match the
triangle orientation. Then we require:

- For each 6‑set S: **at least one** p in S lies inside **some** triangle from S\{p}.

This is equivalent to “S is not in convex position.”

6) **Symmetry breaking (canonical position)**
- Fix o(0,i,j)=True for all 1 ≤ i < j ≤ N−1.
- Fix o(1,2,3)=True to eliminate reflection.

## Instance details
- CNF header: `p cnf 131580 1402417`
- Files:
  - `sig_6_17_gp3_cc_full.cnf` (CNF)
  - `sig_6_17_gp3_cc_full.lrat` (LRAT proof)
  - `sig_6_17_gp3_cc_full_lrat_check.log` (checker output)

## Commands used
Proof generation:
```
cadical --congruence=0 --report --lrat=1 \
  /tmp/sig_6_17_gp3_cc_full.cnf /tmp/sig_6_17_gp3_cc_full.lrat
```

Proof verification:
```
cake_lpr /tmp/sig_6_17_gp3_cc_full.cnf /tmp/sig_6_17_gp3_cc_full.lrat
```

Checker output (see log):
- `s VERIFIED UNSAT`

## Scope and interpretation
The encoding is a standard combinatorial model for realizable planar point sets
in general position. UNSAT therefore certifies that **no** 17‑point general‑position
configuration avoids a convex 6‑gon, yielding the upper bound g(6) ≤ 17.

