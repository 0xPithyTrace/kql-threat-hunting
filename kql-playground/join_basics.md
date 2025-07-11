# 🔗 KQL Playground: Join Basics

The `join` operator in KQL combines data from two tables based on a shared field — like `Account`, `IPAddress`, or `TimeGenerated`. It's a powerful way to correlate related activity or enrich logs with external data.

---

## 🧠 Common Use Cases for Joins

- Correlating failed and successful logons
- Enriching event data with alerts or threat intelligence
- Finding overlapping account activity across multiple tables

---

##  1. Inner Join — Correlate Failed + Successful Logons

```kql
let FailedLogons = SecurityEvent
| where EventID == 4625
| project FailTime = TimeGenerated, Account, Computer;

let SuccessLogons = SecurityEvent
| where EventID == 4624
| project SuccessTime = TimeGenerated, Account, Computer;

FailedLogons
| join kind=inner SuccessLogons on Account
| project Account, FailTime, SuccessTime, Computer
```
### 📝 Explanation:
Find accounts that had both failed and successful logons — a possible sign of brute-force or password spraying.

##  2. Left Outer Join — Admin Accounts with or without Alerts
```kql
let AdminAccounts = SecurityEvent
| where Account endswith "admin"
| summarize LastSeen = max(TimeGenerated) by Account;

let KnownAlerts = AlertInfo
| project AlertName, Account, TimeGenerated;

AdminAccounts
| join kind=leftouter KnownAlerts on Account
| project Account, LastSeen, AlertName, TimeGenerated
```
### 📝 Explanation:
Show all admin accounts, even if they have no alerts — useful for identifying unmonitored privileged accounts.

##  3. Right Outer Join — Alerts Without Logon Context
```kql
let RecentLogons = SecurityEvent
| where EventID == 4624
| summarize LastLogon = max(TimeGenerated) by Account;

let AlertedAccounts = AlertInfo
| project AlertName, Account, AlertTime = TimeGenerated;

RecentLogons
| join kind=rightouter AlertedAccounts on Account
| project Account, LastLogon, AlertName, AlertTime
| sort by AlertTime desc
| take 10
```
### 📝 Explanation:
Ensure all alert-related accounts are surfaced — even those without recent logon activity.

##  4. Full Outer Join — Merge and Compare Two Groups
```kql
let GroupA = datatable(User:string) ["alice", "bob", "carol"];
let GroupB = datatable(User:string) ["carol", "dan", "erin"];

GroupA
| join kind=fullouter GroupB on User
| project User
| sort by User asc
```
### 📝 Explanation:
Combine two user lists and retain all names — matched or unmatched. Useful for comparing group memberships.

## 🧠 Summary: Join Types in KQL
|Join Type|	Description|
|---------|------------|
|inner	| Only matching rows from both tables |
|leftouter	|  All rows from the left, and matches from the right |
|rightouter	| All rows from the right, and matches from the left |
|fullouter	| All rows from both tables, matched where possible |

## ✅ Tips for Using Joins in KQL
- Use let to define your subqueries first — it's cleaner and reusable
- Use project (or project-rename) to align fields before joining
- Joins are case-sensitive and require matching column names
