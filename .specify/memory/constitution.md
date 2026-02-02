# Arbiter-Predictor Constitution

**Version**: 1.0.0 | **Ratified**: 2026-02-01

## Purpose

This constitution governs agentic development within the Arbiter-Predictor repository.

---

## Core Principles

### I. Statistical Integrity
All predictions MUST be based on verifiable statistical methods. No "gut feel" or heuristic shortcuts that bypass proper simulation.

### II. CSPRNG Dependency
All random sampling MUST use cryptographically secure sources via the Entropy-Buffer. Never use `random.random()` for game simulations.

### III. Reproducibility
Given identical inputs and seed state, simulations MUST produce identical results for debugging and verification.

### IV. Performance Awareness
Monte Carlo simulations are computationally intensive. All hot paths MUST be profiled and optimized.

### V. Domain Boundaries
This library handles **statistical inference only**. Strategic AI (move selection, game tree search) belongs in the Primordia project.

### VI. Test Coverage
Minimum 80% line coverage. Statistical algorithms require property-based testing.

---

## Governance

This constitution is subordinate to the main platform-core constitution. Amendments require platform maintainer approval.
