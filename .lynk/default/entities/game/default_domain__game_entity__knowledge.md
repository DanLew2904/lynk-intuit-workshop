---
type: knowledge
domain: "default"
entity: "game"
---

## Entity Summary
The `game` entity represents individual NBA basketball games with their detailed metadata, scheduling information, season classification, and game context. This dimension table serves as the central reference point for all game-related analysis, connecting teams, players, statistics, and events to specific games.

**Aliases:** `games`, `basketball_games`, `nba_games`, `game_dimension`, `dim_game`, `game_info`

## Important Notes
- **Central Dimension:** The `game` entity is the central dimension for connecting all basketball statistics and events to specific games
- **Game Summary Integration:** Additional game information (status, broadcast, live data) comes from `game_summary` source via `game_id_summary` join
- **Season Context:** Games are classified by `season_type` (Regular Season, Playoffs, All-Star, Pre Season) and `season_year`
- **Game Status:** Games can be in different states: Live (currently playing), Final (completed), or Scheduled (not yet started)
- **Broadcast Information:** National TV broadcast data available via `is_nationally_televised` and `natl_tv_broadcaster` fields
- **Team Structure:** Each game has a home team (`home_team_id`) and visitor/away team (`visitor_team_id`)