---
type: knowledge
domain: "marketing"
entity: "game"
---

## Entity Summary
The `game` entity represents comprehensive NBA game information with both teams' complete statistics consolidated into a single record. Each record captures one complete game including identification, matchup details, full box score statistics for both home and away teams, game outcome, attendance, and enriched data like overtime indicators and quarter scoring. This is a fact table (one row per game) designed for marketing and analytics use cases where having all game information in a single record simplifies analysis and reporting.

**Aliases:** `games`, `game_results`, `game_stats`, `game_data`, `nba_games`, `game_box_scores`, `matchups`

## Important Notes

### Data Granularity & Structure
- **One Row Per Game:** Each record represents one complete NBA game with both teams' statistics (single key: `game_id`)
- **Both Teams in One Record:** Unlike team_game entity (2 records per game), this entity consolidates home and away team data in one row
- **Complete Game View:** Includes all statistics, outcomes, and context for both participating teams
- **Marketing-Optimized:** Structured for simplified reporting, analytics, and marketing use cases

### When To Use This Entity
- **Game-Level Reporting:** Creating reports or dashboards showing complete game results
- **Matchup Analysis:** Comparing how two specific teams performed against each other in games
- **Head-to-Head Comparisons:** Analyzing home vs away team performance within individual games
- **Game Outcome Analysis:** Studying factors that correlate with wins/losses
- **Attendance and Engagement:** Analyzing fan attendance patterns and video availability
- **Simplified Analytics:** When you need all game data in one place without joining multiple records

### When NOT To Use This Entity
- **Team-Centric Time Series:** Use another entity (default domain) for analyzing one team's performance over time
- **Individual Player Performance:** Use other entities for player-level statistics
- **Season-Level Team Aggregations:** Use other entities for pre-aggregated team totals
- **Game-by-Game Team Focus:** Use other entities when you need to filter and aggregate from one team's perspective

### Data Characteristics
- **Season Format:** `season_id` is numeric with format like 22023 for 2023-24 season; use `season_year` formula to extract year
- **Date Field:** `game_date` is datetime format for temporal filtering and ordering
- **Percentages:** Shooting percentages vary by field - some stored as numbers (0-100), some as strings
- **Win/Loss:** `wl_home` and `wl_away` contain 'W' or 'L' strings
- **Plus/Minus:** Calculated relative to opponent; home plus/minus = -(away plus/minus)
- **Video Availability:** Binary flags (1 = available, 0 = not available)

### Important Distinctions

#### Game Entity (Marketing) vs Team_Game Entity (Default)
- **Game Entity (Marketing):** One record per game with both teams' data side-by-side
- **Team_Game Entity:** Two records per game, one for each team's perspective
- **Use Case:** Use game entity for simplified matchup analysis; use team_game for team-centric time series

#### Home vs Away Data
- **Home Fields:** Suffix `_home` (e.g., `pts_home`, `fg_pct_home`, `wl_home`)
- **Away Fields:** Suffix `_away` (e.g., `pts_away`, `fg_pct_away`, `wl_away`)
- **Matchup Fields:** `matchup_home` and `matchup_away` describe the game from each team's perspective

#### Season Type Categories
- **Regular Season:** Standard 82-game season games
- **Playoffs:** Post-season playoff games
- **All-Star:** All-Star game and related events
- **Pre Season:** Pre-season exhibition games

#### Percentage Data Type Inconsistencies
- **Number Fields:** `fg_pct_home`, `fg_pct_away`, `ft_pct_home`, `ft_pct_away` stored as numbers
- **String Fields:** `fg3_pct_home`, `fg3_pct_away` stored as strings (requires casting for calculations)
- **String Stats:** Several fields like `oreb_away`, `dreb_away`, `stl_away`, `blk_away`, `tov_away`, `stl_home`, `blk_home`, `tov_home` stored as strings (requires casting)

#### Point Margin Calculations
- **Signed Margin:** `point_margin` = home points - away points (positive = home team won)
- **Absolute Margin:** `point_margin_abs` = absolute value (for blowout detection, regardless of winner)
- **Total Points:** `total_points` = home points + away points (for pace analysis)

#### Overtime Detection
- **OT Indicator:** `is_overtime_game` = true if either team scored in OT1
- **OT Scoring:** `ot1_points_home_game` and `ot1_points_away_game` show first overtime points
- **Standard Duration:** `min` field shows total game minutes (typically 240 for regulation, 265+ for OT)

### Common Pitfalls to Avoid

1. **DON'T confuse this entity with team_game** - This entity has one record per game (both teams); team_game has two records per game (one per team)
2. **DON'T treat all percentage fields uniformly** - Three-point percentages are stored as strings and require casting; other percentages are numeric
3. **DON'T forget string-type statistical fields** - Several counting stats (offensive rebounds, steals, blocks, turnovers) are stored as strings and need casting for math operations
4. **DON'T assume percentages are always decimals** - Numeric percentage fields are stored as 0-100 format (e.g., 45.5 not 0.455)
5. **DON'T use this entity for team time-series analysis** - For analyzing one team's performance over multiple games, use team_game entity which provides team-centric records
6. **DON'T ignore season_type filtering** - Always filter by season_type when analyzing specific competition types (Regular Season vs Playoffs)
7. **DON'T rely on video_available flags being complete** - These fields may be NULL or 0 for older games
8. **DON'T confuse point_margin direction** - Positive margin means home team won; negative means away team won
9. **DON'T assume attendance is always populated** - Attendance field may be NULL for some games (especially older historical games)
10. **DON'T use string comparison for win/loss** - Use exact match with 'W' or 'L' (case-sensitive)
11. **DON'T forget overtime games have higher minutes** - Standard games are ~240 minutes; overtime adds ~25 minutes per OT period
12. **DON'T extract season year manually** - Use `season_year` formula field which correctly parses `season_id`
