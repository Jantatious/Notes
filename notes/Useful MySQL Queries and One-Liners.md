---
title: Useful MySQL Queries and One-Liners
created: '2024-04-16T00:16:59.423Z'
modified: '2024-04-16T00:25:39.417Z'
---

# Useful MySQL Queries and One-Liners

## MySQL System Management

### Get size of all databases (in MB)
- Add another `/ 1024` to get the size in GB
  - Or remove one if you're feeling spicy and want it in KB 
- Change the `1` to set the number of decimal places

```sql
SELECT table_schema "DB Name",
        ROUND(SUM(data_length + index_length) / 1024 / 1024, 1) "DB Size in MB" 
FROM information_schema.tables 
GROUP BY table_schema; 
```
