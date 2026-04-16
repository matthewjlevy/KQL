# Table results of MFA method used with UPN and timestamp detail
Use this to identify users still using SMS for MFA

```kusto
// A list of successfully completed MFA sign-ins, showing the MFA method used, the user (UPN), and the policy or reason that triggered MFA
SigninLogs
| where AuthenticationRequirement == "multiFactorAuthentication"
| where ResultType == 0
| extend ['MFA Method'] = tostring(parse_json(AuthenticationDetails)[1].authenticationMethod)
| extend ['MFA Requirement'] = tostring(parse_json(AuthenticationRequirementPolicies)[0].detail)
| where ['MFA Method'] != "Previously satisfied" and isnotempty(['MFA Method'])
| extend TimeGenerated
| project ['MFA Method'], UserPrincipalName, ['MFA Requirement'], TimeGenerated
