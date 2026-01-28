---
type: knowledge
domain: "default"
entity: "franchise"
---

## Entity Summary
The `franchise` entity represents historical NBA franchise information, tracking team relocations, name changes, and franchise evolution over time. Each record represents one specific iteration of a franchise (a unique city/nickname combination for a team), capturing when that iteration was active. This is a dimension table implementing a Type 2 Slowly Changing Dimension pattern, where a single team can have multiple records representing different periods of its history when it operated under different names or in different cities.

**Aliases:** `franchise_history`, `team_history`, `franchise_iterations`, `team_relocations`, `franchise_evolution`, `historical_franchises`

## Important Notes

### Data Granularity & Structure
- **One Row Per Franchise Iteration:** Each record represents one specific period of a team's history with a particular city and nickname combination (composite key: `team_id`, `year_founded`)
- **Multiple Records Per Team:** A single `team_id` can have multiple records if the franchise relocated or changed names
- **Historical Tracking:** Records capture both current and historical franchise iterations
- **Time-Based Dimension:** Each iteration has a founding year and optional end year, creating a temporal history

### When To Use This Entity
- **Franchise History Analysis:** Tracking how teams have evolved, relocated, or rebranded over time
- **Relocation Research:** Identifying which franchises have moved cities or changed names
- **Historical Team Names:** Finding what a team was called in a specific time period
- **Franchise Longevity Analysis:** Analyzing how long teams have existed in specific locations
- **Geographic Distribution:** Understanding where NBA franchises have been located throughout history
- **Team Identity Changes:** Researching when and why teams changed their nicknames or cities

### When NOT To Use This Entity
- **Current Team Information Only:** Use `team` entity for current, active team data without historical iterations

### Data Characteristics
- **Year Fields:** `year_founded` and `year_active_till` are numeric year values (e.g., 1995, 2008)
- **Active Status:** `year_active_till` is NULL for currently active franchise iterations
- **Calculated Fields:** `years_active`, `is_current`, `is_historical` are computed from founding and end years
- **Franchise Name:** `franchise_name` formula concatenates `city` and `nickname` (e.g., "Los Angeles Lakers")

### Important Distinctions

#### Team ID Consistency
- **Same Team ID Across Iterations:** A team keeps the same `team_id` even when relocating or changing names
- **Multiple Records Per Team ID:** Filter or aggregate carefully to avoid counting the same team multiple times

#### Active vs Historical Iterations
- **Current Iterations:** `year_active_till` IS NULL or `is_current` = TRUE
- **Historical Iterations:** `year_active_till` IS NOT NULL or `is_historical` = TRUE
- **Active Franchise:** The most recent iteration of a team that is currently in operation

#### Relocation vs Rebranding
- **Relocation:** Same `team_id` with different `city` values across iterations
- **Rebranding:** Same `team_id` and `city` but different `nickname` values
- **Both:** A team can relocate and rebrand simultaneously, creating a new iteration

### Common Pitfalls to Avoid

1. **DON'T count franchise records as unique teams** - Multiple records can represent the same team across different periods; use `team_id` to identify unique teams
2. **DON'T assume one record per team** - Teams with relocations or name changes will have multiple records; always filter or aggregate appropriately
3. **DON'T treat NULL `year_active_till` as missing data** - NULL indicates the franchise iteration is currently active, not that data is missing
4. **DON'T compare historical and current iterations without time filtering** - Use `is_current` or `year_active_till` IS NULL to filter for current franchises only
5. **DON'T calculate team counts without using DISTINCT `team_id`** - Counting records will give franchise iterations, not unique teams
6. **DON'T ignore the composite key** - Records are uniquely identified by both `team_id` AND `year_founded` together
7. **DON'T assume `years_active` represents total franchise age** - It represents the duration of that specific iteration, not the team's entire history
8. **DON'T forget temporal context** - When joining to other entities, ensure you're matching the correct time period using `year_founded` and `year_active_till`
9. **DON'T use this entity for team performance metrics** - This entity tracks identity and history, not statistics; use team performance entities for stats
10. **DON'T assume founding year means NBA entry** - `year_founded` represents when that iteration started, which may be a relocation date, not original franchise creation
