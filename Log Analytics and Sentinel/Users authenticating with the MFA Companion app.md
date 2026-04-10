# KQL query to list users authenticating with the MFA Companion app

```kusto
// Lists all users authenticating with the Companion mobile app notification
SigninLogs
| where AuthenticationRequirement == "multiFactorAuthentication"
| where ResultType == 0
| project UserPrincipalName, AuthenticationDetails
| extend ['MFA Method'] = tostring(parse_json(AuthenticationDetails)[1].authenticationMethod)
| where ['MFA Method'] == "Companion mobile app notification"
| summarize SignInCount=count() by UserPrincipalName
| sort by SignInCount desc
```