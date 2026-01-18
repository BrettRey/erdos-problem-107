# ChatGPT 5.2 Pro Plan (2026-01-11)

Original instructions for the formalization approach.

---

## Context

For "1) D" (formalisation), you're in a much better place than you think, because there is already a Lean-based template for exactly the kind of Erdős–Szekeres-style, "geometry → SAT → verified UNSAT" workflow that people think could plausibly scale to the 33-point case eventually.

The key point is: you don't need to be a mathematician to contribute meaningfully on this track. The valuable work is (i) getting the formal statement and the reduction right, (ii) turning informal geometry into crisp, reusable definitions/lemmas, and (iii) making the computation step modular and checkable.

## Step 0: Get Lean working in VS Code

The current official recommendation is to install Lean through VS Code + the Lean 4 extension, which walks you through setup.

If you prefer command-line control later, the Lean community also documents macOS installation via elan (Lean's version manager).

## Step 1: Create (or clone) a mathlib-backed Lean project

You will want a proper Lean "project" (Lake package), not a single loose file. The Lean community page gives both the VS Code UI method (via the ∀ menu) and command-line instructions, including the `lake … new … math` template that pulls in mathlib.

## Step 2: Clone the best existing template: EmptyHexagonLean

This repo is directly in the neighbourhood of Erdős Problem #107 because it formalises a discrete-geometry-to-SAT reduction in Lean, with a structure meant to generalise.

- **Repo:** [EmptyHexagonLean](https://github.com/bsubercaseaux/EmptyHexagonLean) (public)
- **Paper-level description:** two-step structure (reduction + solving), and the motivation for verifying the reduction in Lean

Crucially, the repo already includes:
- a combinatorial "orientation of triples" geometry layer
- symmetry breaking / canonicalisation machinery
- and CNF generators

And it has an "easy win" that's basically #107's solved n=6 milestone:
- It can generate the CNF for "no convex 6‑gon in n points," and the README says the n=17 instance can be solved in under a minute on most machines.

So your first milestone can be purely operational:
1. clone the repo
2. build the Lean formalisation (`lake exe cache get`, `lake build`)
3. run the encoder for the convex 6‑gon case (so you see the whole pipeline end-to-end)

You can do all of that on a MacBook Pro without touching the 17,000 CPU-hour end of the world.

## Step 3 (optional, later): install a free solver + proof checker

EmptyHexagonLean's README explicitly documents a reproducible route using:
- **CaDiCaL** (SAT solver), and
- **cake_lpr** (a verified LRAT proof checker)

plus a couple of small Python dependencies.

You do not need this on day 1 if your immediate goal is "formalisation," but it becomes useful once you want to validate SAT claims independently.

## What to do next for Erdős Problem #107

### (1) Don't start from scratch. Start by understanding and reusing the abstractions that already worked.

The empty-hexagon verification explicitly frames its framework as reusable for "other SAT-based results in geometry" and as the kind of engineering needed to verify SAT-based combinatorial geometry proofs.

That is very close to the plausible attack surface for #107: not "write a human proof of f(7)=33", but "make the reduction airtight so that the only remaining step is an UNSAT certificate."

### (2) Use existing mathlib definitions whenever possible

Mathlib already has:
- convex independence primitives (a natural formal proxy for "convex position")
- a developed convex-hull library
- a standard collinearity notion in affine spaces (useful for "general position = no three collinear")

A very realistic early contribution is to build the "glue layer" that makes the #107 statement sit naturally on top of existing mathlib geometry (instead of bespoke combinatorial axioms).

### (3) Aim for a Lean "blueprint" rather than a proof

In Lean terms, success early on looks like:
- a file that states the #107 conjecture cleanly
- definitions for "general position," "convex position," and "Erdős–Szekeres number ES(n)"
- and a proved chain of implications of the form:

```
geometric counterexample
→ orientation assignment satisfying axioms
→ satisfying assignment of a CNF formula
```

so that UNSAT of the CNF kills the counterexample.

## How to use GPT-5.2 Pro effectively for Lean

When you prompt for Lean, the productive pattern is "small lemma + tight context + compile loop":
- Give it your current imports and the exact goal state (copy from Lean Infoview)
- Demand a proof that compiles with only those imports
- If it proposes a lemma name, make it also tell you where it comes from (file/module), or else to propose a Loogle query instead
- Iterate by pasting the actual error messages back

This is boring, but it's what turns GPT output into checked artefacts.

## Immediate Options

### Option A: "Get the template building"

Clone EmptyHexagonLean and run the build steps from its README (`lake exe cache get`, `lake build` inside the Lean/ folder).

If anything fails, paste the terminal output (just the last ~40 lines is usually enough).

### Option B: "Start a fresh #107 project skeleton"

Create a new mathlib project (via the ∀ menu or via the `lake … new … math` workflow) and confirm you can import Mathlib in a test file.

Then tell me the project name you chose, and I'll give you a first "ErdosSzekeres107.lean" file whose only job is to introduce the core definitions in a mathlib-native way (no SAT yet).

---

*If you do A first, you'll get a working mental model of what a successful "geometry + SAT + verification" formalisation looks like in practice, which makes everything about #107 much less abstract.*
