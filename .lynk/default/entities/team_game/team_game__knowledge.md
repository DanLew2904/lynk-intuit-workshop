---
type: knowledge
domain: "default"
entity: "team_game"
---

## Entity Summary
The `team_game` entity is a fact table containing team-level performance statistics for each game in NBA history. Each record represents one team's performance in one specific game, including comprehensive box score statistics (points, assists, rebounds, shooting percentages), game context (home/away, win/loss, season type), and opponent information. This is a game-level fact table where each game has exactly two records - one for each team that played.

**Aliases:** `team_game_stats`, `team_games`, `team_performance`, `team_box_scores`, `game_team_stats`, `team_game_logs`

## Important Notes

### Data Granularity & Structure
- **One Row Per Team Per Game:** Each record represents one team's performance in one specific game (composite key: `team_full_name`, `game_id`)
- **Two Records Per Game:** Every game has exactly two records - one for each team (home team and away team)
- **Game-Level Granularity:** This is NOT aggregated data; each row contains statistics from a single game
- **Aggregated Player Data:** Team statistics are the sum of all individual player performances in that game (aggregated from player_game)
- **Time Period:** Game-by-game data spanning NBA seasons

### When To Use This Entity
- **Game-by-Game Team Performance:** Analyzing team performance in specific games or sequences of games
- **Win/Loss Analysis:** Tracking team wins, losses, and win percentages over time
- **Home vs Away Performance:** Comparing team performance at home versus on the road
- **Head-to-Head Matchups:** Analyzing team performance against specific opponents
- **Season Performance Trends:** Tracking team statistics over the course of a season
- **Shooting Efficiency Analysis:** Analyzing field goal, three-point, and free throw shooting performance of a team
- **Team Statistics Comparison:** Comparing teams on various statistical categories (points, assists, rebounds, etc.)
- **Recent Form Analysis:** Examining team performance over their last N games
- **Playoff vs Regular Season:** Comparing team performance across different season types

### When NOT To Use This Entity
- **Individual Player Performance:** Use `player_game` entity for player-specific game statistics
- **Injury or Roster Information:** Use `player_team` or other entities for roster composition
- **Coaching Information:** While you can join to `coaching_staff`, use that entity directly for coaching-specific queries

### Data Characteristics
- **Date Format:** `game_date` is a datetime field that can be used for temporal filtering and ordering

### Important Distinctions

#### Shooting Percentages
- **Stored as Percentages:** Values are stored as 0-100 (e.g., 45.5 for 45.5%), NOT as decimals (0.455)
- **Clean Decimal Fields:** Use `field_goal_percentage_clean`, `three_point_percentage_clean`, `free_throw_percentage_clean` formula fields if you need 0-1 decimals
- **Null Percentages:** Percentages may be NULL if the team had zero attempts in that category

### Common Patterns
- **Recent Games:** Filter by `team_full_name` and order by `game_date DESC` with `LIMIT N`
- **Season Analysis:** Filter by `season_year` and optionally `season_type` (e.g., "Regular Season")
- **Home/Away Splits:** Group by `team_full_name` and aggregate separately for `home = TRUE` and `home = FALSE`
- **Head-to-Head:** Filter by `team_full_name` and `opponent_team_full_name` to analyze matchups
- **Win Streaks:** Order by `game_date` and use window functions to identify consecutive wins

### Common Pitfalls to Avoid

1. **DON'T forget each game has two records** - One for each team; filter by team or aggregate appropriately
2. **DON'T confuse team_game with player_game** - Use team_game for team stats, player_game for individual player stats
3. **DON'T assume percentages are decimals** - Shooting percentages are stored as 0-100, not 0-1 (use `_clean` fields for decimals)
7. **DON'T overlook opponent_team_full_name** - Use this for head-to-head analysis and matchup filtering
8. **DON'T calculate percentages from made/attempted** - Percentages are pre-calculated; use the existing percentage fields
