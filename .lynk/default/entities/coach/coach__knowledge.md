---
type: knowledge
domain: "default"
entity: "coach"
---

## Entity Summary
The `coach` entity represents NBA basketball coaches with comprehensive career-level information, statistics, and performance metrics. Each record contains one coach's complete career summary including personal information (name, coaching span) and aggregated career totals (wins, losses, games coached, win percentage). This is a career-aggregated dimension table where all statistics represent lifetime totals across the coach's entire NBA career.

**Aliases:** `coaches`, `nba_coaches`, `coach_stats`, `coaching_records`, `head_coaches`, `coaching_careers`

## Important Notes
- **One Row Per Coach:** Each coach has exactly one record representing their entire NBA career
- **Career-Level Data:** ALL metrics in this entity represent CAREER TOTALS (entire coaching career), DO NOT use this entity for season-by-season analysis
- **Head Coach vs Assistant:** Win/loss statistics only count games as HEAD COACH; assistant coaching tenure tracked separately via `total_assistant_seasons` feature
- **Active Status:** Currently active coaches have `last_year >= current year`; retired coaches have `last_year < current year`
- **Season-Level Data:** For season-by-season coaching assignments, use the `coaching_staff` entity (linked via `coach_to_coaching_staff` relationship)
- **Pre-Aggregated Metrics:** Statistics like `total_wins`, `total_losses`, `total_games_coached`, and `win_percentage` are pre-calculated across entire career
- **Multiple Roles:** Coaches may have both `total_head_coach_seasons` and `total_assistant_seasons` representing different roles throughout career
- **Team Assignments:** A coach's team and season assignments are in `coaching_staff` entity; this entity only contains career-wide totals
- **game-by-game coaching analysis:** For game-by-game coaching analysis, use `team_game` entity with coaching filters (need to join via `coaching_staff` entity)
