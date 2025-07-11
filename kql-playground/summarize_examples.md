## 📊 KQL Playground: Summarize Examples

The `summarize` operator is used to group and aggregate data.
It's similar to SQL's GROUP BY.

Common uses:
- Count events by account, host, or IP
- Get the latest event per user
- Calculate averages, max, min, etc.
-----------------------------------------------------------


## 1. Count all events in the SecurityEvent table
```kql
SecurityEvent
| summarize TotalEvents = count()
```

## 2. Count events grouped by Account
```kql
SecurityEvent
| summarize EventCount = count() by Account
| sort by EventCount desc
| take 10
```

## 3. Count failed logon events (4625) by TargetUserName
```kql
SecurityEvent
| where EventID == 4625
| summarize FailedLogons = count() by Account
| sort by FailedLogons desc
| take 10
```

## 4. Show the most recent event per user
```kql
SecurityEvent
| summarize LastSeen = max(TimeGenerated) by Account
| sort by LastSeen desc
| take 10
```

## 5. Count logons by Account and Computer
```kql
SecurityEvent
| where EventID == 4624
| summarize LogonCount = count() by Account, Computer
| sort by LogonCount desc
| take 10
```

## 6. Use multiple aggregation functions
```kql
SecurityEvent
| where EventID == 4624
| summarize
    LogonCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by Account
| sort by LogonCount desc
| take 10
```
---
##🔧 COMMON AGGREGATES:
| Aggregate | Use |
|-----------|-----|
| count()   | number of events |
| max() / min() | latest or earliest time |
| avg(), sum() | math on numeric fields |

## Tips
- Always use `by` to define your grouping fields
-  Summarize is often combined with `sort` and `take`
