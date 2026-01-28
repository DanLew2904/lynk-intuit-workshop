---
type: task-instructions
domain: "default"
entity: "player_game"
tasks: "text-to-sql"
---

# Player Game Entity Rules

## Always Do
- Display players using `first_name` and `last_name` (or concatenate for full name)

## Query Best Practices
- Filter `is_inactive = FALSE` to exclude inactive players from performance analysis, UNLESS explicitly asked about inactive players
- `home = TRUE` means the player's team was home; `win = TRUE` means the player's team won

## Query Examples

**Example 1:** 
user: Player game statistics for a specific game (excluding inactive)
agent:
```sql
SELECT first_name, last_name, points, assists, rebounds_total, num_minutes
FROM entity('player_game')
WHERE game_id = '12345' AND is_inactive = FALSE
ORDER BY points DESC;
```

**Example 2:** 
user: Player's season averages (Regular Season only)
agent:
```sql
SELECT first_name, last_name,
       AVG(points) AS avg_points,
       AVG(assists) AS avg_assists,
       AVG(rebounds_total) AS avg_rebounds
FROM entity('player_game')
WHERE person_id = '12345' 
  AND game_type = 'Regular Season' 
  AND is_inactive = FALSE
GROUP BY person_id, first_name, last_name;
```

**Example 3:** 
user: Top scoring games (all-time)
agent: 
```sql
SELECT first_name, last_name, game_date, points, player_team_name, opponent_team_name
FROM entity('player_game')
WHERE is_inactive = FALSE
ORDER BY points DESC
LIMIT 10;
```

**Example 4:** Home vs away performance comparison
```sql
SELECT home,
       AVG(points) AS avg_points,
       AVG(field_goals_pct) AS avg_fg_pct,
       COUNT(*) AS games
FROM entity('player_game')
WHERE person_id = '12345' 
  AND is_inactive = FALSE
  AND game_type = 'Regular Season'
GROUP BY home;
```