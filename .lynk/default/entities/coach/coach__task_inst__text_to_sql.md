---
type: task-instructions
domain: "default"
entity: "coach"
tasks: "text-to-sql"
---

# Coach Entity Rules

## Always Do
- Display coaches using `coach_name` field (full name)

## Query Best Practices

- **Active vs Retired Coaches:** Use `last_year >= EXTRACT(YEAR FROM CURRENT_DATE)` for active coaches
- **Coach Type Distinction:** Head coach statistics use `total_head_coach_seasons`, `total_games_coached`; assistant statistics use `total_assistant_seasons`
- **Win/Loss Analysis:** `win_percentage` is pre-calculated; `career_wins_over_500` = wins - losses
- **Name Display:** Use `coach_name` for full name display; `first_name` and `last_name` available separately

## Query Examples

**Example 1:**
user: Currently active coaches with their career records
agent:
```sql
SELECT coach_name,
       first_year,
       last_year,
       total_wins,
       total_losses,
       win_percentage,
       career_wins_over_500
FROM entity('coach')
WHERE last_year >= EXTRACT(YEAR FROM CURRENT_DATE)
  AND total_games_coached > 0
ORDER BY win_percentage DESC;
```

**Example 2:**
user: Home vs away performance for a specific coach (career totals)
agent:
```sql
SELECT coach_name,
       total_home_wins,
       total_away_wins,
       total_games_coached,
       home_win_percentage
FROM entity('coach')
WHERE coach_name = 'Gregg Popovich';
```

**Example 3:**
user: Coaches who have been both head coach and assistant
agent:
```sql
SELECT coach_name,
       total_head_coach_seasons,
       total_assistant_seasons,
       total_wins,
       win_percentage
FROM entity('coach')
WHERE total_head_coach_seasons > 0
  AND total_assistant_seasons > 0
ORDER BY total_wins DESC;
```

**Example 4:**
user: What's the average win percentage across all NBA coaches by career length?
agent:
```sql
SELECT years_active,
       METRIC(count_coaches) count_coaches,
       METRIC(avg_win_percentage_all_coaches) as avg_win_percentage_all_coaches,
       METRIC(total_coaching_wins_all_coaches) as total_coaching_wins_all_coaches
FROM entity('coach')
GROUP BY years_active
ORDER BY years_active DESC;
```
