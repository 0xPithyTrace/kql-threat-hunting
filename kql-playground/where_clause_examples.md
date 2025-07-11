## 🔍 KQL Playground: WHERE Clause Examples
The `where` operator filters rows that match a condition.
Think of it like a "WHERE" clause in SQL.

## 1. Filter for events by a specific user
```kql
SecurityEvent
| where Account == "Administrator"
| take 5
```

## 2. Filter for specific Event ID (e.g., 4625 = failed logon)
```kql
SecurityEvent
| where EventID == 4625
| take 5
```

## 3. Combine multiple conditions using AND/OR
```kql
SecurityEvent
| where EventID == 4625 and Account == "admin"
| take 5
```

## 4. Use the `in` operator to filter for multiple values
```kql
SecurityEvent
| where EventID in (4624, 4625, 4634)
| project TimeGenerated, EventID, Account
```

## 5. Filter events in the last 1 hour using `ago()`
```kql
SecurityEvent
| where TimeGenerated > ago(1h)
| take 5
```

## 6. Use string operators: contains, startswith, endswith
```kql
SecurityEvent
| where Account contains "adm"
| project Account, EventID
```

## 7. Use `!` (NOT) to exclude results
```kql
SecurityEvent
| where Account !contains "test"
| where EventID != 4624
| take 5
```

## 🔐 COMMON COMPARISON OPERATORS:
| symbol | meaning |
|--------|---------|
| == | equals |
| != | not equals |
| > < | greater than / less than (for time, numbers) |
| in () | matches any in list |
| contains, startswith, endswith | string filters |

### Use `ago(time)` for time-based filters (e.g., last 1h, 1d, 30m)
