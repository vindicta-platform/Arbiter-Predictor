# Feature Proposal: Monte Carlo Combat Simulator

**Proposal ID**: FEAT-017  
**Author**: Unified Product Architect (Autonomous)  
**Created**: 2026-02-01  
**Status**: Draft  
**Priority**: High  

---

## Part A: Software Design Document (SDD)

### 1. Executive Summary

Implement a Monte Carlo combat simulator that runs thousands of combat iterations to provide probability distributions for combat outcomes, helping players make optimal tactical decisions.

### 2. System Architecture

#### 2.1 Current State
- Basic dice rolling
- No combat simulation
- Single outcome calculation
- No statistical analysis

#### 2.2 Proposed Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                 Monte Carlo Combat Engine                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                   Combat Setup                          │    │
│  │   Attacker profiles | Defender profiles | Rules         │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                  │
│                              ▼                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Simulation Engine                          │    │
│  │   - 10,000+ iterations per query                        │    │
│  │   - Parallel execution                                  │    │
│  │   - Statistical aggregation                             │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                  │
│                              ▼                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Results Distribution                       │    │
│  │   Mean | Median | Std Dev | Percentiles | Histogram    │    │
│  └─────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
```

#### 2.3 File Changes

```
Arbiter-Predictor/
├── src/
│   └── arbiter/
│       ├── monte_carlo/
│       │   ├── __init__.py      [NEW]
│       │   ├── simulator.py     [NEW] Core simulation engine
│       │   ├── profiles.py      [NEW] Unit profile definitions
│       │   └── rules.py         [NEW] Combat rule implementations
│       ├── stats/
│       │   ├── __init__.py      [NEW]
│       │   ├── distribution.py  [NEW] Statistical analysis
│       │   └── visualization.py [NEW] Chart generation
│       └── engine.py            [MODIFY] Integration
├── tests/
│   └── test_monte_carlo.py      [NEW]
└── docs/
    └── monte-carlo.md           [NEW]
```

### 3. Combat Profile

```python
@dataclass
class AttackProfile:
    name: str
    attacks: int
    skill: int           # WS or BS
    strength: int
    ap: int
    damage: str          # "D6", "2", "D3+1"
    abilities: list[str] # ["lethal_hits", "devastating_wounds"]

@dataclass  
class DefenseProfile:
    name: str
    toughness: int
    save: int
    invuln: Optional[int]
    wounds: int
    fnp: Optional[int]
    abilities: list[str] # ["stealth", "-1_to_wound"]
```

### 4. Simulation Output

```python
@dataclass
class SimulationResult:
    iterations: int
    damage_stats: DistributionStats
    kills_stats: DistributionStats
    histogram: dict[int, int]
    percentiles: dict[int, float]  # 10th, 25th, 50th, 75th, 90th
    
@dataclass
class DistributionStats:
    mean: float
    median: float
    std_dev: float
    min: int
    max: int
```

### 5. Parallel Execution

```python
async def simulate(
    attacker: AttackProfile,
    defender: DefenseProfile,
    iterations: int = 10000
) -> SimulationResult:
    """Run Monte Carlo simulation with parallel workers."""
    
    with ProcessPoolExecutor(max_workers=cpu_count()) as pool:
        batches = split_iterations(iterations, cpu_count())
        results = await asyncio.gather(*[
            pool.submit(run_batch, attacker, defender, batch)
            for batch in batches
        ])
    
    return aggregate_results(results)
```

---

## Part B: Behavior Driven Development (BDD)

### User Stories

#### US-001: Combat Prediction
**As a** competitive player  
**I want to** simulate combat outcomes  
**So that** I can choose optimal shooting/charge targets

#### US-002: List Optimization
**As a** list builder  
**I want to** compare weapon profiles  
**So that** I can choose the best loadout

#### US-003: Risk Assessment
**As a** tactical player  
**I want to** see probability ranges  
**So that** I understand best/worst case scenarios

### Acceptance Criteria

```gherkin
Feature: Monte Carlo Combat Simulator

  Scenario: Simulate shooting attack
    Given an attacker profile with 20 bolter shots
    And a defender profile of 10 Ork Boyz
    When I run the simulation with 10000 iterations
    Then I should receive a damage distribution
    And expected kills with confidence intervals
    And a histogram of outcomes

  Scenario: Compare weapon profiles
    Given two weapon profiles (meltagun vs lascannon)
    And the same target profile
    When I run comparative simulation
    Then I should see which weapon performs better
    And the margin of difference with confidence level

  Scenario: Account for special rules
    Given an attacker with "Lethal Hits"
    When running the simulation
    Then critical hits should be auto-wounds
    And the result should reflect increased damage
```

---

## Implementation Estimate

| Phase | Effort | Dependencies |
|-------|--------|--------------|
| Core Simulator | 8 hours | Entropy-Buffer |
| Combat Rules | 6 hours | Game rules |
| Statistical Analysis | 4 hours | NumPy |
| Parallel Execution | 4 hours | None |
| Visualization | 4 hours | Matplotlib |
| Testing | 4 hours | None |
| **Total** | **30 hours** | |
