---
type: knowledge
domain: "default"
entity: "player"
---

## Entity Summary
The `player` entity represents individual NBA basketball players with their personal information, career status, and demographic details. This dimension table serves as the central reference point for all player-related analysis, connecting player statistics, performance data, and career information across seasons and teams.

**Aliases:** `players`, `basketball_players`, `nba_players`, `player_dimension`, `dim_player`, `player_info`

## Important Notes
- **Central Dimension:** The `player` entity is the central dimension for all player-related queries
- **Active Status:** `is_active` distinguishes currently active (1) from inactive/retired (0) players
- **Name Structure:** Player names available as `full_name` (display), `first_name`, and `last_name` (flexible querying)
- **Position Standardization:** `position` contains standardized values (G, F, C, F-G, C-F); `position_raw` contains original values
- **Data Sources:** Player records are enriched with aggregated data from multiple entities:
  - **Draft information** (first draft only) from `draft` entity via `first_last` features
  - **Career statistics** aggregated from `player_game` entity via metric features
  - **Team affiliations** aggregated from `player_team` entity
  - **Coaching relationships** from `coaching_staff` entity
- **Undrafted Players:** Some players may have NULL draft fields (undrafted or international players)
- **Experience Levels:** `experience` (years) ranges from 0 (rookies) to 20+ for veterans; `is_rookie` identifies first-year players