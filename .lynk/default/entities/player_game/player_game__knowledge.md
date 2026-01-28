---
type: knowledge
domain: "default"
entity: "player_game"
---

## Entity Summary
The `player_game` entity represents individual NBA player performance in specific games with comprehensive box score statistics. Each record contains detailed performance metrics for a single player in a single game, including scoring, assists, rebounds, shooting percentages, and other statistical metrics.

**Aliases:** `player_game_stats`, `player_games`, `box_scores`, `player_performance`, `game_stats`, `player_game_logs`

## Important Notes
- **Record Granularity:** Each record represents one player's performance in one game (player + game combination)
- **Inactive Players:** The `is_inactive` field indicates players who were on the inactive list and did not play; these records exist but should be excluded from performance analysis by default
- **Null Percentages:** Shooting percentages (FG%, 3P%, FT%) may be NULL for games with zero attempts
- **Game Context:** Each record includes game context (home/away, win/loss, game type) for filtering and analysis
- **Multiple Records:** A player can have multiple records (different games), and each game has multiple records (different players)
- **Name Structure:** Player names are split into `first_name` and `last_name` for flexible querying