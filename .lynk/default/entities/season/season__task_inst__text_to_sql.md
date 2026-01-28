---
type: task-instructions
domain: "default"
entity: "season"
tasks: "text-to-sql"
---

# Season Entity Rules

## Always Do
- Use `season_year` as the primary key for filtering and grouping operations
- Display season using `season_display` field (e.g., "2023-24") for user-friendly output

## Query Best Practices

1. **Championship Results:** Include `champion_team`, `defeated_team`, `champ_wins`, and `def_wins` for complete Finals context
2. **Series Analysis:** Use calculated fields `is_sweep`, `is_seven_game_series`, `is_close_series` for competitiveness analysis
3. **Human-Readable Formats:** Use `championship_matchup` or `series_result` for formatted descriptions
4. **Dynasty Analysis:** Group by `champion_team` to analyze championship frequency

## Query Examples

**Example 1:**
user: Recent NBA champions with series results
agent:
```sql
SELECT season_year,
       season_display,
       champion_team,
       defeated_team,
       series_result,
       finals_mvp
FROM entity('season')
WHERE season_year >= 2015
ORDER BY season_year DESC;
```

**Example 2:**
user: Most dominant championship performances
agent:
```sql
SELECT season_year,
       champion_team,
       defeated_team,
       champ_wins,
       def_wins,
       is_sweep,
       champion_win_percentage
FROM entity('season')
WHERE is_sweep = TRUE
ORDER BY season_year DESC;
```

**Example 3:**
user: Most competitive Finals series
agent:
```sql
SELECT season_year,
       champion_team,
       defeated_team,
       series_length,
       win_loss_balance,
       is_seven_game_series
FROM entity('season')
WHERE is_seven_game_series = TRUE
ORDER BY season_year DESC;
```

**Example 4:**
user: Championship count by team
agent:
```sql
SELECT champion_team,
       METRIC(count_seasons) as championship_count,
       MIN(season_year) as first_championship,
       MAX(season_year) as most_recent_championship
FROM entity('season')
GROUP BY champion_team
ORDER BY championship_count DESC;
```

**Example 5:**
user: Championship trends analysis by decade
agent:
```sql
SELECT FLOOR(season_year / 10) * 10 as decade,
       METRIC(count_seasons) as total_seasons,
       METRIC(avg_series_length) as avg_games,
       METRIC(sweep_percentage) as sweep_pct,
       METRIC(seven_game_series_percentage) as seven_game_pct
FROM entity('season')
GROUP BY decade
ORDER BY decade;
```
