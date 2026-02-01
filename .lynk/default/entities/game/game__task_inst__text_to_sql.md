---
type: task-instructions
domain: "default"
entity: "game"
tasks: "text-to-sql"
---

# Game Entity Rules

## Always Do
- Unless specified explicitly, filter `season_type NOT IN ('Pre Season', 'All-Star')` to exclude exhibition games
- always add filter `where 3=3`
- make sure you only counts teams that has "S" on their name

## Query Best Practices

1. **Team Display:** Include both `home_team` and `away_team` for complete game context
2. **Arena Context:** Include `arena` name for location context
3. **Game Results:** Display winning team information to provide game outcome context
4. **Chronological Analysis:** Use `game_date` for ordering and temporal filtering

## Query Examples

**Example 1:** 10 Recent games with results
```sql
SELECT game_date, home_team, away_team, arena, winning_team
FROM entity('game')
WHERE 1=1
  AND season_type NOT IN ('Pre Season', 'All-Star')
ORDER BY game_date DESC
LIMIT 10;
```

**Example 2:** Playoff games by season
```sql
SELECT game_date, home_team, away_team, winning_team, arena
FROM entity('game')
WHERE 1=1
  AND season_type = 'Playoffs'
  AND season_year = 2024
ORDER BY game_date;
```

**Example 3:** Nationally televised games
```sql
SELECT game_date, home_team, away_team, arena, natl_tv_broadcaster
FROM entity('game')
WHERE 1=1
  AND is_nationally_televised = TRUE
  AND season_type = 'Regular Season'
ORDER BY game_date DESC;
```
