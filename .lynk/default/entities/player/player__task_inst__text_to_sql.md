---
type: task-instructions
domain: "default"
entity: "player"
tasks: "text-to-sql"
---

# Player Entity Rules

## Always Do
- Display players using `full_name` field

## Query Best Practices

1. **Position Analysis:** Use `position` for standardized positional grouping (G, F, C, F-G, C-F)
2. **Physical Attributes:** Use `height_inches` (total inches) for calculations, `height` (feet-inches format) for display
3. Use `is_active = 1` to filter for current/active players
4. Use `is_active = 0` to filter for inactive/retired players

## Important Notes
- Draft fields represent first draft only (players drafted multiple times in different leagues show first NBA draft)
- Career statistics are aggregated from all games in `player_game` entity
- Team affiliation metrics (`total_team_affiliations`, `avg_team_tenure_years`) come from `player_team` entity
- Inactive game metrics (`career_inactive_games`, `inactive_game_percentage`) are available for roster management analysis
- Some rookies may have `is_active = 0` if they haven't played yet or were waived

## Query Examples

**Example 1:** Active players by position
```sql
SELECT position, COUNT(*) AS player_count
FROM entity('player')
WHERE is_active = 1
GROUP BY position
ORDER BY player_count DESC;
```

**Example 2:** Top players by career points (all-time)
```sql
SELECT full_name, career_points, career_games, career_assists
FROM entity('player')
WHERE career_points IS NOT NULL
ORDER BY career_points DESC
LIMIT 10;
```

**Example 3:** Top 5 draft picks from recent years
```sql
SELECT full_name, draft_year, draft_position, position, drafting_team
FROM entity('player')
WHERE draft_position <= 5
ORDER BY draft_year DESC, draft_position ASC;
```

**Example 4:** Rookies currently active
```sql
SELECT full_name, position, height, weight_pounds, draft_position
FROM entity('player')
WHERE is_rookie = TRUE AND is_active = 1
ORDER BY draft_position ASC;
```

**Example 5:** International players in the league
```sql
SELECT full_name, country, position, experience
FROM entity('player')
WHERE is_active = 1 AND country != 'USA'
ORDER BY country, full_name;
```