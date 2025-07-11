## 🧱 KQL Playground: Project & Rename
-----------------------------------------------------------
The `project` operator is used to SELECT which columns appear
in your results. It's similar to SQL's SELECT clause.

The `project-rename` operator renames columns in your result.

These are used to clean up and shape your output.
-----------------------------------------------------------

## 1. Show only a few columns
```kql
SecurityEvent
| take 5
| project TimeGenerated, EventID, Account, Computer
```

## 2. Rename columns using `project-rename`.
// Keeps all columns and renames the specified items
```kql
SecurityEvent
| take 5
| project-rename
    Time = TimeGenerated,
    User = Account,
    Host = Computer
```

## 3. Combine project and rename (same result, different approach)
```kql
SecurityEvent
| take 5
| project Time = TimeGenerated, User = Account, Host = Computer
```

## 4. Project only distinct values
```kql
SecurityEvent
| project Account
| distinct Account
| take 10
```

## 5. Rename columns in a summarized result
```kql
SecurityEvent
| summarize Total = count() by Account
| project Username = Account, TotalEvents = Total
| sort by TotalEvents desc
| take 10
```
---
## 💡 TIPS:
- `project` limits the number of columns in your output
- `project-rename` changes column names but keeps all columns
- `distinct` returns unique values (like SELECT DISTINCT in SQL)
- You can alias columns inline using: `NewName = OldName`
---
