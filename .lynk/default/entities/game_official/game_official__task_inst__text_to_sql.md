---
type: task-instructions
domain: "default"
entity: "game_official"
tasks: "text-to-sql"
---

# Game Official Entity Rules

## Always Do
- Display officials using `official_full_name` or combine `first_name` and `last_name` for identification
- Use both `game_id` and `official_id` together for unique assignment identification (composite key)

## Query Best Practices

1. **Jersey Number Display:** Include `jersey_num` when displaying official assignments for identification

## Query Examples

**Example 1:**
user: Officials assigned to a specific game
agent:
```sql
SELECT official_id,
       official_full_name,
       first_name,
       last_name,
       jersey_num
FROM entity('game_official')
WHERE game_id = 22300001
ORDER BY jersey_num;
```

**Example 2:**
user: Most active officials by assignment count
agent:
```sql
SELECT official_id,
       official_full_name,
       METRIC(count_assignments) as total_games_worked
FROM entity('game_official')
GROUP BY official_id, official_full_name
ORDER BY total_games_worked DESC
LIMIT 10;
```

**Example 3:**
user: Games worked by a specific official
agent:
```sql
SELECT game_id,
       official_full_name,
       jersey_num,
       METRIC(count_assignments) as games_count
FROM entity('game_official')
WHERE official_id = 201145
GROUP BY game_id, official_full_name, jersey_num
ORDER BY game_id DESC;
```

**Example 4:**
user: Average number of officials per game
agent:
```sql
SELECT METRIC(avg_officials_per_game) as avg_officials,
       METRIC(distinct_games) as total_games,
       METRIC(count_assignments) as total_assignments
FROM entity('game_official');
```

**Example 5:**
user: Games with incomplete officiating crews
agent:
```sql
SELECT game_id,
       COUNT(DISTINCT official_id) as official_count
FROM entity('game_official')
GROUP BY game_id
HAVING COUNT(DISTINCT official_id) < 3
ORDER BY game_id;
```
