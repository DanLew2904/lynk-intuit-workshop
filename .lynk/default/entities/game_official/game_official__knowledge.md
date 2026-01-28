---
type: knowledge
domain: "default"
entity: "game_official"
---

## Entity Summary
The `game_official` entity represents NBA game official and referee assignments, capturing which officials worked which games. Each record represents one official's assignment to one specific game, forming a many-to-many relationship between games and officials. This is a fact table (associative/bridge table) where each game typically has three records (one for each member of the officiating crew), and each official appears in multiple records across different games they've worked.

**Aliases:** `game_officials`, `game_referees`, `officiating_crew`, `referee_assignments`, `official_assignments`, `game_refs`

## Important Notes

### Data Granularity & Structure
- **One Row Per Official Per Game:** Each record represents one official's assignment to one specific game (composite key: `game_id`, `official_id`)
- **Multiple Records Per Game:** Each game has multiple records - typically 3, one for each official in the crew
- **Multiple Records Per Official:** Each official appears in many records across all games they've officiated
- **Bridge Table Pattern:** This entity creates a many-to-many relationship between games and officials

### When To Use This Entity
- **Officiating Crew Research:** Finding which officials worked specific games
- **Official Workload Analysis:** Analyzing how many games each official has worked
- **Crew Composition:** Understanding typical officiating crew sizes and compositions
- **Official Assignment Patterns:** Tracking which officials work together or appear in certain game types
- **Game-Level Official Data:** Joining official information to specific game analysis

### When NOT To Use This Entity
- **Official Career Statistics:** This entity doesn't contain performance metrics or career statistics for officials
- **Official Biographical Information:** This entity contains basic identification only (name, jersey number)

### Data Characteristics
- **Typical Crew Size:** NBA games typically have 3 officials per game
- **Jersey Numbers:** `jersey_num` field contains official's jersey/uniform number worn during games
- **Name Fields:** Separate `first_name` and `last_name` fields, plus calculated `official_full_name` formula
- **No Time Fields:** This entity doesn't have date/time fields directly; join to `game` entity for temporal analysis

### Important Distinctions

### Common Pitfalls to Avoid

1. **DON'T expect crew size to always be 3** - While typical, verify with data as some games may have different numbers of officials
2. **DON'T filter by official name without considering duplicates** - Multiple officials may share names; use `official_id` for precision
3. **DON'T analyze official performance from this entity** - This is assignment data only; performance and rating metrics are not included
4. **DON'T assume jersey numbers are unique** - Jersey numbers may be reused or shared across different officials
5. **DON'T use this for date/time filtering** - This entity has no temporal fields;
