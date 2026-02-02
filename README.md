# Arbiter-Predictor

A statistical library for win probability calculations from WARScribe inputs, designed to power tactical decision-making in competitive Warhammer 40k.

## Overview

Arbiter-Predictor calculates win probabilities by running Monte Carlo simulations against game state inputs. It serves as the analytical engine behind the Vindicta Platform's prediction systems.

## Features

- **Monte Carlo Simulation**: Statistical win probability from thousands of simulated outcomes
- **WARScribe Integration**: Accepts standardized notation inputs
- **Probability Distributions**: Full outcome distributions, not just point estimates
- **Performance Optimized**: Vectorized calculations for batch processing

## Installation

Install from source using uv:

```bash
uv pip install git+https://github.com/vindicta-platform/Arbiter-Predictor.git
```

Or clone and install locally:

```bash
git clone https://github.com/vindicta-platform/Arbiter-Predictor.git
cd Arbiter-Predictor
uv pip install -e .
```

## Quick Start

```python
from arbiter_predictor import WinPredictor

predictor = WinPredictor()
result = predictor.analyze(
    army_a="WARScribe notation...",
    army_b="WARScribe notation...",
    simulations=10000
)

print(f"Army A Win Probability: {result.win_prob_a:.2%}")
print(f"Army B Win Probability: {result.win_prob_b:.2%}")
```

## Architecture

```
┌──────────────────┐     ┌─────────────────┐     ┌────────────────┐
│ WARScribe Input  │────▶│ Arbiter-        │────▶│ Win            │
│ (Game State)     │     │ Predictor       │     │ Probabilities  │
└──────────────────┘     └─────────────────┘     └────────────────┘
                                │
                                ▼
                         ┌─────────────────┐
                         │ Primordia AI    │
                         │ (Future)        │
                         └─────────────────┘
```

## Roadmap Note

The domain boundaries between this library and the future "Primordia AI" project are evolving. Arbiter-Predictor focuses on **statistical inference**, while Primordia will handle **strategic AI** (similar to Stockfish for chess).

## Related Repositories

| Repository | Relationship |
|------------|-------------|
| [platform-core](https://github.com/vindicta-platform/platform-core) | Parent platform |
| [WARScribe-Parser](https://github.com/vindicta-platform/WARScribe-Parser) | Input data source |
| [Entropy-Buffer](https://github.com/vindicta-platform/Entropy-Buffer) | CSPRNG for simulations |

## License

MIT License - See [LICENSE](./LICENSE) for details.
