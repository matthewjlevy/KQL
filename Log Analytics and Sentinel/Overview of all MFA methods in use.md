# KQL query to get an overview of all MFA methods in use

```kusto
// Bar chart showing all MFA types used
SigninLogs
| where AuthenticationRequirement == "multiFactorAuthentication"
| where ResultType == 0
| project AuthenticationDetails
| extend ['MFA Method'] = tostring(parse_json(AuthenticationDetails)[1].authenticationMethod)
| summarize Count=count() by ['MFA Method']
| where ['MFA Method'] != "Previously satisfied" and isnotempty(['MFA Method'])
| sort by Count desc
| render barchart with (title="Types of MFA Methods used")
```