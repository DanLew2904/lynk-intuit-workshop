---
type: task-instructions
domain: "default"
entity: "team"
tasks: "text-to-sql"
---

# Team Entity Rules

## Always Do
- Display teams using `full_name` or combine `city` and `nickname` for team identification

## Query Best Practices

1. **Geographic Filtering:** Use `state` or `city` for location-based queries
2. **Arena Information:** Include `arena` and `arena_capacity` when querying venue details from the team_details source

## Query Examples

**Example 1:**
user: List all NBA teams with their basic information
agent:
```sql
SELECT id, full_name, city, nickname, abbreviation, state, year_founded
FROM entity('team')
ORDER BY full_name;
```

**Example 2:**
user: Teams with their arena information
agent:
```sql
SELECT full_name, city, arena, arena_capacity, arena_size_category
FROM entity('team')
WHERE arena IS NOT NULL
ORDER BY arena_capacity DESC;
```

**Example 3:**
user: Teams with best overall win percentage
agent:
```sql
SELECT full_name,
       city,
       total_wins,
       total_losses,
       win_percentage,
       total_games
FROM entity('team')
WHERE total_games > 0
ORDER BY win_percentage DESC
LIMIT 10;
```

**Example 4:**
user: Home court advantage analysis by team
agent:
```sql
SELECT full_name,
       total_home_wins,
       total_away_wins,
       team_home_win_percentage,
       team_away_win_percentage,
       team_home_court_advantage
FROM entity('team')
WHERE total_games > 0
ORDER BY team_home_court_advantage DESC;
```

**Example 5:**
user: Teams by state with their draft history
agent:
```sql
SELECT state,
       METRIC(count_teams) as team_count,
       METRIC(avg_dunks_per_team) as avg_team_dunks,
       METRIC(high_dunk_team_percentage) as high_dunk_pct
FROM entity('team')
GROUP BY state
ORDER BY team_count DESC;
```
