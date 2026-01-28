---
type: knowledge
domain: "default"
entity: "team"
---

## Entity Summary
The `team` entity represents comprehensive NBA team information, combining current team identification, organizational details, arena information, and pre-aggregated historical performance metrics. Each record represents one current NBA team with enriched data including franchise history, roster management statistics, game performance aggregations, draft history, and coaching information. This is a dimension table (one row per team) that serves as a central hub for team-level analysis, pulling together metrics from multiple related entities to provide a complete team profile.

**Aliases:** `teams`, `nba_teams`, `franchises`, `nba_franchises`, `team_profiles`, `team_info`

## Important Notes

### Data Granularity & Structure
- **One Row Per Team:** Each record represents one current NBA team (single key: `id`)
- **Current Team Focus:** This entity contains data for active NBA teams, not historical franchise iterations (use `franchise` entity for historical team names/locations)
- **Joined Details:** Arena, ownership, and organizational data joined from team_details source

### When To Use This Entity
- **Team Identification:** Looking up team names, abbreviations, locations, and basic information
- **Team Comparison:** Comparing teams across various dimensions (arena size, franchise age, win-loss records)
- **High-Level Team Analysis:** Analyzing team-level aggregates like total wins, draft history, player tenure
- **Organizational Research:** Investigating team ownership, management, coaching staff, facilities
- **Team Demographics:** Analyzing geographic distribution of teams, arena capacities, founding years
- **Roster Management Analysis:** Studying player tenure patterns, roster turnover, loyalty metrics

### When NOT To Use This Entity
- **Game-by-Game Analysis:** Use other entities for individual game performance and results
- **Season-Specific Performance:** Use other entities with time filters for season-level analysis
- **Historical Franchise Iterations:** Use other entities for tracking team relocations and name changes
- **Player-Specific Stats:** Use other entities for player analysis
- **Current Roster:** This entity doesn't maintain current roster; use other entities with appropriate time filters

### Important Distinctions

#### Team vs Franchise
- **Team Entity:** Current NBA teams with current names and locations
- **Franchise Entity:** Historical iterations tracking relocations and name changes over time
- **Use Case:** Use `team` for current team analysis, `franchise` for historical franchise evolution

#### Aggregated vs Time-Filtered Metrics
- **Aggregated Metrics on Team:** All-time totals (e.g., `total_wins`, `total_team_dunks`)
- **Time-Filtered Analysis:** Query related entities directly (e.g., `team_game`) with date filters for specific periods
- **Important:** If you need season-specific or date-range performance, do NOT use the pre-aggregated team metrics

#### Home vs Away Performance
- **Home Stats:** `team_home_wins_count`, `team_home_losses_count`, `team_home_win_percentage`
- **Away Stats:** `team_away_wins_count`, `team_away_losses_count`, `team_away_win_percentage`

### Common Pitfalls to Avoid

1. **DON'T use pre-aggregated metrics for time-specific analysis** - Metrics like `total_wins` and `total_team_dunks` are all-time totals; for season-specific analysis, query the source entities directly with date filters
2. **DON'T confuse team with franchise** - Use `team` for current teams, `franchise` for historical franchise iterations and relocations
3. **DON'T use this entity for current roster** - This entity doesn't track current players; query `player_team` with appropriate time filters
4. **DON'T ignore the distinction between home and away metrics** - Always specify whether you need home, away, or combined statistics
5. **DON'T assume `year_founded` is original franchise founding** - For relocated teams, this may be the year of relocation, not original founding
