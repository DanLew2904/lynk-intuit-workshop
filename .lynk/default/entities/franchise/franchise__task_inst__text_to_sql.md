---
type: task-instructions
domain: "default"
entity: "franchise"
tasks: "text-to-sql"
---

# Franchise Entity Rules

## Always Do
- Each record represents a franchise iteration (city/name combination) for historical tracking
- Use `is_current = TRUE` to filter for currently active franchises, UNLESS explicitly asked about historical franchises

## Query Best Practices

1. **Franchise Identification:** Use `franchise_name` (calculated field combining city + nickname) or separate `city` and `nickname` fields
2. **Active vs Historical:** Filter by `is_current` for current franchises or `is_historical` for past iterations
3. **NULL Handling for year_active_till:** NULL values in `year_active_till` indicate the franchise is currently active

## Query Examples

**Example 1:**
user: All currently active franchises
agent:
```sql
SELECT team_id,
       franchise_name,
       city,
       nickname,
       year_founded,
       years_active
FROM entity('franchise')
WHERE is_current = TRUE
ORDER BY year_founded;
```

**Example 2:**
user: Franchise history for a specific team showing all relocations and name changes
agent:
```sql
SELECT team_id,
       city,
       nickname,
       franchise_name,
       year_founded,
       year_active_till,
       years_active,
       is_current
FROM entity('franchise')
WHERE team_id = 1610612747
ORDER BY year_founded;
```

**Example 3:**
user: Teams that have relocated to different cities
agent:
```sql
SELECT team_id,
       METRIC(distinct_cities) as city_count,
       METRIC(count_franchises) as franchise_iterations
FROM entity('franchise')
GROUP BY team_id
HAVING METRIC(distinct_cities) > 1
ORDER BY franchise_iterations DESC;
```

**Example 4:**
user: Longest-lasting franchise iterations
agent:
```sql
SELECT franchise_name,
       city,
       nickname,
       year_founded,
       year_active_till,
       years_active,
       is_historical
FROM entity('franchise')
ORDER BY years_active DESC
LIMIT 10;
```

**Example 5:**
user: Franchise iterations by decade
agent:
```sql
SELECT FLOOR(year_founded / 10) * 10 as decade,
       METRIC(count_franchises) as franchises_founded,
       METRIC(avg_franchise_duration) as avg_duration_years
FROM entity('franchise')
GROUP BY decade
ORDER BY decade;
```
