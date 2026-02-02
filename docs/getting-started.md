# Getting Started

## Installation

```bash
uv pip install git+https://github.com/vindicta-platform/Arbiter-Predictor.git
```

## Usage

```python
from arbiter import Predictor

predictor = Predictor()
win_prob = predictor.predict(
    player_elo=1500,
    opponent_elo=1450,
    player_faction="Space Marines",
    opponent_faction="Tyranids"
)
```
