---
type: task-instructions
domain: "default"
entity: "draft"
tasks: "text-to-sql"
---

# Draft Entity Rules

## Always Do
- Display players using `player_name` or combine `first_name` and `last_name` for identification

## Query Best Practices

1. **NULL Handling for Measurements:** Physical measurements and athletic tests may be NULL for players who didn't participate in all combine drills
2. **Physical Measurements:** Height is available in both inches (`height_wo_shoes`, `height_w_shoes`) and formatted strings (`height_wo_shoes_ft_in`, `height_w_shoes_ft_in`)

## Query Examples

**Example 1:**
user: Physical measurements for players in 2023 draft combine
agent:
```sql
SELECT player_name,
       position,
       height_wo_shoes_ft_in,
       weight,
       wingspan_ft_in,
       body_fat_pct
FROM entity('draft')
WHERE season = 2023
ORDER BY player_name;
```

**Example 2:**
user: Top athletic performers by vertical leap
agent:
```sql
SELECT player_name,
       position,
       max_vertical_leap,
       standing_vertical_leap,
       three_quarter_sprint,
       lane_agility_time
FROM entity('draft')
WHERE max_vertical_leap IS NOT NULL
ORDER BY max_vertical_leap DESC
LIMIT 10;
```

**Example 3:**
user: First round picks with their combine statistics
agent:
```sql
SELECT player_name,
       draft_year,
       draft_position,
       height_wo_shoes,
       wingspan,
       max_vertical_leap,
       bench_press
FROM entity('draft')
WHERE is_first_round = TRUE
ORDER BY draft_position;
```

**Example 4:**
user: Average physical measurements by draft round
agent:
```sql
SELECT draft_round,
       METRIC(avg_height_no_shoes) as avg_height,
       METRIC(avg_wingspan) as avg_wingspan,
       METRIC(avg_vertical_leap) as avg_vertical,
       METRIC(count_drafts) as player_count
FROM entity('draft')
WHERE draft_round IS NOT NULL
GROUP BY draft_round
ORDER BY draft_round;
```

**Example 5:**
user: Lottery picks from international backgrounds
agent:
```sql
SELECT player_name,
       draft_year,
       draft_position,
       pre_nba_organization,
       height_wo_shoes_ft_in,
       wingspan_ft_in
FROM entity('draft')
WHERE is_lottery_pick = TRUE
  AND UPPER(pre_nba_organization) != 'COLLEGE/UNIVERSITY'
ORDER BY draft_year DESC, draft_position;
```
