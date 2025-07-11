## 🔰 KQL Playground: Syntax Basics
This file introduces the fundamental structure of a KQL query.
It uses the SecurityEvent table as an example, but the concepts
apply to most tables across Microsoft Sentinel and Log Analytics.

## 📌 Basic structure of a KQL query:
```kql
TableName
| operator1
| operator2
| ...
```


## 1. View the most recent 10 events from the SecurityEvent table. 
> Useful for identifying column names without running a large query.
```kql
SecurityEvent
| take 10
```

## 2. Filter rows using the `where` operator
> (Example: Only events where the AccountName is "Administrator")
```kql
SecurityEvent
| where Account == "Administrator"
| take 5
```

## 3. Project specific columns (like SELECT in SQL). Returns only columns list after project
```kql
SecurityEvent
| where Account == "Administrator"
| project TimeGenerated, Account, EventID, Activity
```

## 4. Sort results (newest to oldest)
```kql
SecurityEvent
| where Account == "Administrator"
| sort by TimeGenerated desc
| take 5
```

## 5. Count total number of events in the table
```kql
SecurityEvent
| count
```

## 6. Group data using `summarize`
> (Example: Count number of events per Account)
```kql
SecurityEvent
| summarize EventCount = count() by Account
| sort by EventCount desc
| take 10
```

## 7. Add a calculated column using `extend`
> (Here, we'll label accounts as "admin" or "non-admin")
```kql
SecurityEvent
| extend AccountType = iff(Account == "Administrator", "admin", "non-admin")
| project TimeGenerated, Account, AccountType
```
---
## 💡 REMEMBER:
- KQL is case-sensitive (e.g., "Account" ≠ "account")
- The pipe (`|`) passes data to the next step (like UNIX shell)
- You can chain operators to filter, shape, and summarize data

## Practice by tweaking values, changing fields, or combining steps.
