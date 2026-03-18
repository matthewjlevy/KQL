```kql
// This query will simply list the devices pasted into the data table in the query results
// Use a query such as this to list devices you wish to "Take Action" on: https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-take-action?view=o365-worldwide
let devices = datatable(DeviceName:string)
[
    "desktop-nlrbega",
    "desktop-l68o7q9",
    "desktop-abc123",
    "desktop-xyz789"
];
DeviceInfo
| join kind=inner devices on DeviceName
| summarize arg_max(Timestamp, *) by DeviceId
| project DeviceName, DeviceId
| order by DeviceName asc
```
