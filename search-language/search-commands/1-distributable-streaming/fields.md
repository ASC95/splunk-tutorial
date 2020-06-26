- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Fields - fields documentation
# Examples
## Restrict subsearch returned fields
### Search
```
index=electrical_utility_data sourcetype=realtime_scada
| where Power_MVA <= 0
| join type=left _time, Line_ID
    [ search index=electrical_utility_data sourcetype=realtime_oms | fields _time, Line_ID, Type]
```
# Purpose
- The `fields` command excludes or includes fields from the returned events
- It's great for limiting fields returned by a subsearch
- It cannot format tables for dashboards like the `table` command can. IDK why
# Arguments
## Required
- x
## Optional
- x