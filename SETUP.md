# Setup Instructions

## Step 0: Install Lean 4 via elan

```bash
# Install elan (Lean version manager)
curl https://raw.githubusercontent.com/leanprover/elan/master/elan-init.sh -sSf | sh

# Restart terminal or source profile
source ~/.zshrc  # or ~/.bash_profile

# Verify
elan --version
lean --version
```

## Step 1: Install VS Code Extension

1. Open VS Code
2. Extensions (Cmd+Shift+X)
3. Search "lean4"
4. Install "lean4" by leanprover

## Step 2a: Clone EmptyHexagonLean (Option A - recommended first)

```bash
cd /Users/brettreynolds/Documents/LLM-CLI-projects/papers/Erdos_Problem_107

git clone https://github.com/bsubercaseaux/EmptyHexagonLean.git

cd EmptyHexagonLean/Lean

# Download mathlib cache (avoids hours of compilation)
lake exe cache get

# Build
lake build
```

If successful, you can run the convex 6-gon encoder to see the full pipeline.

## Step 2b: Create Fresh Project (Option B)

```bash
cd /Users/brettreynolds/Documents/LLM-CLI-projects/papers/Erdos_Problem_107

# Create new mathlib-backed project
lake +leanprover-community/mathlib4:v4.x.x new erdos107 math

cd erdos107

# Download mathlib cache
lake exe cache get

# Test import
echo 'import Mathlib' > Test.lean
lake build Test.lean
```

## Step 3: SAT Infrastructure (optional, later)

### CaDiCaL (SAT solver)

```bash
brew install cadical
# or build from source: https://github.com/arminbiere/cadical
```

### cake_lpr (verified LRAT checker)

```bash
# CakeML-based verified checker
# See: https://github.com/tanyongkiam/cake_lpr
```

## Troubleshooting

**"lake: command not found"**
→ Restart terminal after elan install, or run `source ~/.zshrc`

**Mathlib cache download slow/fails**
→ Try `lake exe cache get!` (force re-download)

**VS Code not recognizing Lean files**
→ Open the project folder (not individual files) in VS Code
