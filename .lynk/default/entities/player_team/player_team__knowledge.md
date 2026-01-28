---
type: knowledge
domain: "default"
entity: "player_team"
---

## Entity Summary
The `player_team` entity represents player-team affiliation periods throughout a player's career. Each record contains information about a specific player's tenure with a specific team, including start and end dates, season information, and tenure duration.

**Aliases:** `player_team_stats`, `player_affiliations`, `team_affiliations`, `player_tenure`

## Important Notes
- **Record Granularity:** Each record represents one player's affiliation with one team for a specific period
- **Multiple Affiliations:** A player can have multiple records for different teams or different periods with the same team (e.g., traded away then returned)
- **Current Status:** `end_date IS NULL` indicates the affiliation is currently active
- **Tenure Calculation:** `tenure_years` is calculated from `start_date` and `end_date` (or current date if ongoing)
- **Season Format:** `season` contains string format (e.g., "2023-24"), `season_year` contains the ending year as a number (e.g., 2024)