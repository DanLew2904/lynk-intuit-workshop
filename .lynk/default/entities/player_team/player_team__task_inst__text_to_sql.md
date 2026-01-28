---
type: task-instructions
domain: "default"
entity: "player_team"
tasks: "text-to-sql"
---

# Player Team Entity Rules

## Always Do
- Display players using `player_full_name` field
- Display teams using `team_full_name` field
- Use `person_id` for grouping and tracking individual players
- Use `season_year` (number) for filtering, `season` (string) for display
- Use `end_date IS NULL` or `is_current = TRUE` to identify current team affiliations

## Query Best Practices

1. **Current Affiliations:** Use `end_date IS NULL` or `is_current = TRUE` to filter for current team memberships
2. **Player Grouping:** Always group by `person_id` (not name) for accurate player-level aggregations
3. **Tenure Analysis:** Use `tenure_years` for pre-calculated tenure duration; use `start_date` and `end_date` for custom date range filtering
4. **Team Changes:** Count `DISTINCT team_full_name` grouped by `person_id` to find number of teams a player has been with
5. **Temporal Ordering:** Order by `start_date` to show career progression chronologically

## Important Notes
- A player can have multiple records for the same team if they left and returned (e.g., LeBron James with Cleveland)
- `end_date IS NULL` indicates ongoing/current affiliation
- Some players may have overlapping records during trades within a season

## Query Examples

**Example 1:** Player's team history (career progression)
```sql
SELECT player_full_name, team_full_name, start_date, end_date, tenure_years
FROM entity('player_team')
WHERE person_id = '12345'
ORDER BY start_date DESC;
```

**Example 2:** Current team rosters (all active affiliations)
```sql
SELECT player_full_name, team_full_name, start_date, tenure_years
FROM entity('player_team')
WHERE is_current = TRUE
ORDER BY team_full_name, player_full_name;
```

**Example 3:** Players with most team changes
```sql
SELECT player_full_name, COUNT(DISTINCT team_full_name) AS team_count
FROM entity('player_team')
GROUP BY person_id, player_full_name
ORDER BY team_count DESC
LIMIT 10;
```

**Example 4:** Longest tenures with a single team
```sql
SELECT player_full_name, team_full_name, tenure_years, start_date, end_date
FROM entity('player_team')
WHERE tenure_years IS NOT NULL
ORDER BY tenure_years DESC
LIMIT 10;
```