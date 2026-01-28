---
type: task-instructions
domain: "default"
entity: "coaching_staff"
tasks: "text-to-sql"
---

# Coaching Staff Entity Rules

## Always Do
- Display coaches using `coach_name` field
- Display teams using `team_full_name` field
- Use `season_year` (number) for filtering and `season` (string) for display
- Use `is_head_coach = TRUE` to filter for head coaches specifically
- Use `role` field for detailed position information (e.g., "Assistant Coach", "Defensive Coordinator")

## Query Best Practices

1. **Role Filtering:** Use `is_head_coach` boolean for head coach vs assistant distinction
2. **Detailed Roles:** Use `role` field when you need specific position titles beyond head/assistant
3. **Team Context:** Always include `season_year` or `season` for temporal context
4. **Sorting:** When displaying coaching staff, sort by `is_head_coach DESC` to show head coach first
5. **Multiple Assignments:** A coach may have multiple records in the same season (different teams or role changes)

## Important Notes
- Each record represents one coach-team-season assignment
- Use `coach_id` to join with `coach` entity for career-level information
- Use `team_id` to join with `team` entity for franchise-level information

## Query Examples

**Example 1:** Coaching staff for a specific team and season
```sql
SELECT coach_name, role, is_head_coach
FROM entity('coaching_staff')
WHERE team_full_name = 'Los Angeles Lakers' AND season_year = 2023
ORDER BY is_head_coach DESC, coach_name;
```

**Example 2:** Head coaches by season
```sql
SELECT season_year, team_full_name, coach_name
FROM entity('coaching_staff')
WHERE is_head_coach = TRUE
ORDER BY season_year DESC, team_full_name;
```

**Example 3:** Coach's career assignments
```sql
SELECT season_year, team_full_name, role
FROM entity('coaching_staff')
WHERE coach_id = 12345
ORDER BY season_year DESC;
```