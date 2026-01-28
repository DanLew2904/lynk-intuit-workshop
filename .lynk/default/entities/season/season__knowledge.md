---
type: knowledge
domain: "default"
entity: "season"
---

## Entity Summary
The `season` entity represents comprehensive NBA season-level information, including championship results, MVP awards, and Finals series details. Each record represents one complete NBA season with its championship outcome, containing data about the Finals matchup, series results, and major award winners. This is a dimension table with one row per season, providing season-level context and championship history for analysis.

**Aliases:** `seasons`, `nba_seasons`, `season_results`, `championship_seasons`, `finals_results`, `season_summary`

## Important Notes

### Data Granularity & Structure
- **One Row Per Season:** Each record represents one complete NBA season (single key: `season_year`)
- **Season Identifier Format:** `season_year` uses a numeric year format (e.g., 2023 for the 2023-24 season)
- **Championship Focus:** Each record contains the Finals outcome and championship results for that season
- **Award Winners:** Includes both regular season MVP and Finals MVP for each season
- **MVP players:** Unless specified otherwise, use `finals_mvp` when asked about MVP player.

### When To Use This Entity
- **Championship History Analysis:** Researching which teams won championships and when
- **Finals Series Analysis:** Analyzing competitiveness and outcomes of NBA Finals matchups
- **Dynasty Research:** Identifying teams that won multiple consecutive championships
- **MVP Tracking:** Analyzing MVP award winners over time
- **Season Context:** Providing season-level context for other analyses (joining to games, players, teams)
- **Competitive Balance Studies:** Analyzing how championship competitiveness has changed over time

### When NOT To Use This Entity
- **Regular Season Performance:** Use team or game entities for regular season statistics
- **Individual Game Results:** Use `game` or `team_game` entities for specific game outcomes
- **Player Season Statistics:** Use `player_team` or player performance entities for season-level player stats
- **Team Season Records:** Use team performance entities for win-loss records and season statistics

### Data Characteristics
- **Season Format:** `season_year` is numeric (2023), `season_display` formula provides human-readable format ("2023-24")
- **Finals Results:** `champ_wins` and `def_wins` show number of games won in the Finals series
- **Series Length:** NBA Finals is best-of-7, so series length ranges from 4 (sweep) to 7 games
- **Team Names:** `champion_team` and `defeated_team` contain full team names as strings
- **Win Percentages:** `champion_win_percentage` is stored as 0-100 format (e.g., 66.67 for 4-2 series)

### Important Distinctions

#### Season Year Convention
- **Season Year:** Represents the starting year of the season (2023 means 2023-24 season)
- **Display Format:** Use `season_display` formula for "2023-24" format instead of just "2023"

#### Championship vs Regular Season
- **Championship Data Only:** This entity contains Finals/playoff championship results, not regular season records
- **Regular Season MVP:** `regular_season_mvp` is based on regular season performance, not playoffs
- **Finals MVP:** `finals_mvp` is based on Finals series performance specifically

#### Series Competitiveness Indicators
- **Sweep:** `is_sweep` = TRUE when `def_wins` = 0 (4-0 series)
- **Seven-Game Series:** `is_seven_game_series` = TRUE when series went the full distance (4-3)
- **Close Series:** `is_close_series` = TRUE when margin is 2 games or fewer (4-2, 4-3 series)

#### Win Counts
- **Champion Wins:** `champ_wins` is always 4 (needed to win championship in best-of-7)
- **Defeated Wins:** `def_wins` ranges from 0-3 (how many games loser won before elimination)

### Common Pitfalls to Avoid

1. **DON'T use this entity for regular season analysis** - This contains championship/Finals data only, not regular season performance
2. **DON'T calculate total games as champ_wins + def_wins without verification** - Use `series_length` formula which handles this correctly
3. **DON'T use percentages as decimals** - `champion_win_percentage` is stored as 0-100, not 0-1 format