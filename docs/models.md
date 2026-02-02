# Models

## Elo Rating

Standard Elo with K-factor adjustment.

```python
from arbiter import EloSystem

elo = EloSystem(k=32)
new_rating = elo.update(winner=1500, loser=1450)
```

## Faction Matchup

Historic win rate matrix.

```python
from arbiter import MatchupMatrix

matrix = MatchupMatrix()
win_rate = matrix.get("Space Marines", "Tyranids")
```
