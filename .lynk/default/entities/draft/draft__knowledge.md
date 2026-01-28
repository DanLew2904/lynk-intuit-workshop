---
type: knowledge
domain: "default"
entity: "draft"
---

## Entity Summary
The `draft` entity represents NBA Draft Combine participants with comprehensive physical measurements, athletic testing results, shooting performance data, and draft history information. Each record represents one player's participation in a specific season's combine, containing pre-draft measurements and their eventual draft outcome. This is a fact table that combines combine statistics with historical draft data, where records represent combine-level data (one row per player per combine season).

**Aliases:** `draft_combine`, `nba_draft`, `combine_stats`, `draft_measurements`, `pre_draft_stats`, `combine_data`, `draft_prospects`

## Important Notes

### Data Granularity & Structure
- **One Row Per Player Per Season:** Each record represents one player's participation in one specific combine season (composite key: `player_id`, `season`)
- **Combine Participation Data:** Records exist only for players who participated in the NBA Draft Combine for a given season
- **Draft History Integration:** Draft outcome data (position, round, team) is joined from a separate draft history source

### When To Use This Entity
- **Pre-Draft Player Analysis:** Analyzing physical measurements and athletic testing results before players enter the NBA
- **Draft Prospect Comparison:** Comparing physical attributes and athletic performance across different draft classes
- **Combine Performance Trends:** Tracking how combine measurements and test results have changed over time
- **Draft Position Analysis:** Correlating combine performance with eventual draft position and outcomes
- **Physical Attribute Research:** Studying relationships between measurements (height, wingspan, vertical leap) and draft success
- **Shooting Performance Evaluation:** Analyzing pre-draft shooting ability from various distances and situations

### When NOT To Use This Entity
- **NBA Career Statistics:** Use other entities for actual NBA performance data
- **Draft-Only Information:** If you only need draft results without combine data, consider if draft history sources alone would be simpler
- **Current NBA Player Info:** Use other entities for active player information and career statistics

### Data Characteristics
- **Season Format:** `season` field uses numeric format (e.g., 2023 for the 2023 season)
- **Height Measurements:** Available in both inches (`height_wo_shoes`, `height_w_shoes`) and feet-inches format (`height_wo_shoes_ft_in`, `height_w_shoes_ft_in`)
- **Distance Measurements:** Wingspan, standing reach, hand measurements all stored in inches
- **Speed/Agility Times:** Lane agility and sprint times stored in seconds (lower is better)
- **Strength Testing:** `bench_press` represents number of repetitions at 185 pounds
- **Shooting Percentages:** Stored as string values from various spot locations and distances (15-foot, college distance, NBA distance)

### Important Distinctions

#### Physical Measurements
- **Height With vs Without Shoes:** Two separate measurements available (`height_wo_shoes` vs `height_w_shoes`) - typically differs by about 1 inch
- **Format Variations:** Most measurements available in both numeric inches and formatted feet-inches strings

#### Shooting Distance Categories
- **15-Foot Distance:** Mid-range shooting spots at 15 feet from the basket
- **College Distance:** Three-point shooting from college three-point line (shorter than NBA)
- **NBA Distance:** Three-point shooting from NBA three-point line (farther than college)
- **Spot Shooting vs Off-Dribble:** Separate measurements for catch-and-shoot (`spot_*`) and off-dribble (`off_drib_*`) scenarios
- **On-Move Shooting:** Dynamic shooting tests (`on_move_fifteen`, `on_move_college`)

#### Draft Fields
- **Draft Position:** `draft_position` is the overall pick number (1-60 typically)
- **Draft Round:** `draft_round` indicates which round (1 or 2)
- **First Round vs Lottery:** `is_first_round` (picks 1-30) vs `is_lottery_pick` (picks 1-14 specifically)
- **Pre-NBA Organization:** `pre_nba_organization` shows where player came from (College/University, High School, Other Team/Club)

### Common Pitfalls to Avoid

1. **DON'T assume all drafted players have combine data** - Not every drafted player participates in the combine; records only exist for combine participants
2. **DON'T confuse combine season with draft year** - While usually the same, `season` represents when combine occurred, `draft_year` represents when player was actually drafted
3. **DON'T treat shooting percentages as numeric without validation** - Shooting fields are stored as strings and may contain non-numeric values or nulls
4. **DON'T compare heights without specifying with/without shoes** - Always use consistent height measurement (either `height_wo_shoes` or `height_w_shoes`, not mixed)
5. **DON'T use this entity for NBA career analysis** - This entity contains pre-draft data only; use player performance entities for career stats
6. **DON'T assume lower is always better for all measurements** - For sprint/agility times, lower is better; for measurements like height, wingspan, vertical leap, higher is better
7. **DON'T forget the composite key** - Records are uniquely identified by both `player_id` AND `season` together
8. **DON'T ignore null values in athletic testing** - Not all players complete all tests; many fields may have null values requiring proper handling
9. **DON'T confuse draft position with draft value** - Lower draft position numbers (1, 2, 3) are higher value picks (selected earlier)
10. **DON'T use raw `organization_type` field** - Use `pre_nba_organization` which provides cleaned categorical values
