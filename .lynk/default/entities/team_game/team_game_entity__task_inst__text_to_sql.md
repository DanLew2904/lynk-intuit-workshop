---
type: task-instructions
domain: "default"
entity: "team_game"
tasks: "text-to-sql"
---

# Team Game Entity Rules

## Always Do
- Unless specified explicitly, filter `season_type NOT IN ('Pre Season', 'All-Star')`

## Query Best Practices

1. **Temporal Analysis:** Use `game_date` for chronological filtering and date range queries
2. **Season Type Filtering:** Filter by `season_type` when analyzing regular season vs playoff performance
3. **Home vs Away Display:** Use descriptive fields instead of raw boolean `home` field for better readability
4. **Win/Loss Analysis:** Use `win` field to filter for victories or losses in team performance queries
5. **Team Identification:** Include `team_name` or `team_abbreviation` for clear team context in results
