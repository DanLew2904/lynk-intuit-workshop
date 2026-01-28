---
type: knowledge
domain: "default"
entity: "coaching_staff"
---

## Entity Summary
The `coaching_staff` entity is a fact table representing coaching staff assignments that link coaches to teams by season. Each record represents one coach's assignment to one specific team for one specific season, including their role classification (head coach vs assistant coach types). This entity serves as the bridge between coaches, teams, and seasons, enabling season-by-season analysis of coaching staff composition and coach career movement.

**Aliases:** `coaching_assignments`, `coach_assignments`, `team_coaches`, `staff_assignments`, `coach_team_season`, `coaching_roster`

## Important Notes

### Data Granularity & Structure
- **One Row Per Coach-Team-Season Assignment:** Each record represents one unique assignment of a coach to a team for a specific season (composite key: `team_id`, `season`, `coach_id`)
- **Assignment-Level Data:** This is NOT career-aggregated data; each row is a single season assignment
- **Multiple Assignments Per Coach:** A coach who works multiple seasons or for multiple teams will have multiple rows
- **Multiple Assignments Per Season:** A coach can have multiple rows in the same season if they:
  - Changed teams mid-season (traded or resigned and hired elsewhere)
  - Changed roles on the same team (e.g., promoted from assistant to head coach)
- **Time Period:** Season-level granularity (one assignment per season, not per game)

### When To Use This Entity
- **Season-by-Season Coaching Analysis:** Tracking which coaches worked for which teams in specific seasons
- **Team Coaching Staff Composition:** Analyzing the coaching staff makeup for a team in a given season
- **Coach Career Path Tracking:** Following a coach's career movements across teams and seasons
- **Coaching Role Analysis:** Distinguishing between head coaches and various assistant coach types
- **Coaching Tenure Analysis:** Calculating how long coaches stayed with specific teams (by counting consecutive seasons)
- **Staff Size Analysis:** Counting total coaching staff size by team and season
- **Coaching Turnover Analysis:** Identifying when teams changed coaches between seasons

### When NOT To Use This Entity
- **Career Win/Loss Statistics:** Use the `coach` entity instead - it contains pre-aggregated career totals including `total_wins`, `total_losses`, `total_games_coached`, and `win_percentage`
- **Game-by-Game Coaching Data:** This entity does NOT contain individual game records; it only tracks season-level assignments
- **In-Season Performance Metrics:** For game-level coaching performance, join to `team_game` entity via relationships
- **Player-Coach Relationships:** While you can join to players through relationships, use `player_team` entity for more direct player-team analysis

### Data Characteristics
- **Coach Type Classification:** The `coach_type` field contains five role types:
  - `Head Coach` - Primary head coach for the team
  - `Associate Head Coach` - Senior assistant coach position
  - `Lead Assistant Coach` - Lead assistant position
  - `Assistant Coach` - General assistant coach
  - `Assistant Coach for Player Development` - Specialized player development role
- **Boolean Helper Fields:** Use `is_head_coach`, `is_assistant_coach`, `is_associate_head_coach`, `is_lead_assistant_coach` formula fields for easier filtering instead of matching string values
- **Season Formats:** Two representations of season available:
  - `season` (string) - Display format like "2023-24"
  - `season_year` (number) - Ending year as number (e.g., 2024 for "2023-24"), use this for filtering and comparisons
- **Enriched with Dimension Data:** This fact table includes denormalized fields from related entities:
  - Coach info (`coach_name`, `first_name`, `last_name`) from `coach` entity
  - Team info (`team_full_name`, `team_city`, `team_nickname`) from `team` entity
  - Season info (`season_champion`) from `season` entity

### Important Distinctions

#### coaching_staff vs coach Entity
This is a critical distinction for correct entity selection:

**coaching_staff (THIS entity):**
- Assignment-level granularity (one row per coach-team-season)
- Contains NO performance metrics (no wins, losses, or win percentage)
- Use for: Season-specific questions, team staff composition, coach movement tracking
- Example queries: "Who was the Lakers coach in 2020?", "Which teams has Steve Kerr coached?", "Show me the Celtics coaching staff in 2023"

**coach entity:**
- Career-level granularity (one row per coach with lifetime totals)
- Contains pre-aggregated performance metrics (`total_wins`, `total_losses`, `win_percentage`)
- Use for: Career comparisons, all-time rankings, overall coaching success
- Example queries: "Which coach has the most wins?", "What is Gregg Popovich's career win percentage?", "Compare coaching records"

**Decision Rule:** If the question mentions "career", "all-time", "total wins/losses", or "win percentage" → use `coach` entity. If the question mentions a specific season, team, or "coaching staff" → use `coaching_staff` entity.

#### Head Coach vs Assistant Coach
- Use `is_head_coach = TRUE` to filter for head coaches only
- Use `is_assistant_coach = TRUE` to filter for any assistant coach type (includes all four assistant types)
- For specific assistant roles, use `coach_type` field or specific boolean fields
- IMPORTANT: Some teams may have multiple head coaches in one season (interim coaches, mid-season changes)

### Common Patterns
- **Current Coaching Staff:** Filter by the most recent `season_year` value in the dataset
- **Coach Career Span:** Group by `coach_id` and calculate `MIN(season_year)` to `MAX(season_year)`
- **Team Coaching History:** Filter by `team_id` and order by `season_year DESC`
- **Coach Movement:** Group by `coach_id` and list distinct teams with seasons
- **Staff Composition:** Filter by `team_id` and `season`, group by `coach_type` to see role distribution

### Common Pitfalls to Avoid

1. **DON'T use this entity for career wins/losses** - Use `coach` entity for performance statistics
2. **DON'T assume one head coach per team per season** - Teams can have interim coaches or mid-season changes
3. **DON'T use `season` (string) for filtering** - Use `season_year` (number) for comparisons and filtering
4. **DON'T forget `coach_type` can be NULL** - Filter with `WHERE coach_type IS NOT NULL` when analyzing roles
5. **DON'T confuse assignment count with coach count** - One coach can have multiple assignments (multiple seasons/teams)
6. **DON'T use for game-level coaching analysis** - This is season-level data only; join to `team_game` for game-by-game
7. **DON'T overlook assistant coach types** - There are four distinct assistant coach classifications beyond just "Assistant Coach"
