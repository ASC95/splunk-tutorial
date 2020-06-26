- https://docs.splunk.com/Documentation/Splunk/8.0.3/SearchReference/ConditionalFunctions#match.28SUBJECT.2C_.22REGEX.22.29 - match documentation
- https://docs.splunk.com/Documentation/Splunk/8.0.3/Knowledge/AboutSplunkregularexpressions - Splunk regular expression syntax
- https://stackoverflow.com/questions/863125/regular-expression-to-count-number-of-commas-in-a-string - count commas
# Examples
## Find events whose _raw field contains 5 commas
### Search
```
index="electrical_utility_data" | where match(_raw, "^([^,]*,){5}[^,]*$")
```
- This will find all events that were OMS events and not SCADA events because OMS events have 5 commas while SCADA events have 4
# Purpose
- The `MATCH` command returns true if the regular expression matches the field or string literal
- It can be used in the search bar and in \<eval> markup!
  - However, it only returns true or false so it isn't very useful
# Syntax
- `MATCH (<field or text>, "<regex>")`