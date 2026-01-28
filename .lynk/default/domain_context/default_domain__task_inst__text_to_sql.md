---
type: task-instructions
domain: "*"
tasks: "text-to-sql"
---

# SQL Generation Rules

## Always Do

- Always use `season_year` field when available (ending year of season, e.g., 2018 for 2017-18 season)
- In case you have the `season_type` feature - unless specified explicitly, filter `season_type NOT IN ('Pre Season', 'All-Star')`

## Query Best Practices

- **Prefer combined entities** when querying multiple entity types (e.g., use `player_game_stats` instead of joining `players` + `games`)
- **Use season_year field** for season-based filtering when available
- **Use full names** (e.g., "Los Angeles Lakers", not "LAL")