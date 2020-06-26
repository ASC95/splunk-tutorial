- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2001/SearchReference/Rex
- https://docs.splunk.com/Documentation/Splunk/8.0.3/Knowledge/AboutSplunkregularexpressions - Splunk regex primer
- https://answers.splunk.com/answers/593992/is-it-possible-to-use-regex-on-a-token-value.html - regex and tokens
# Examples
## Extract new fields from existing fields
- `sourcetype="oms" | rex field="Bus Affected" "(?<first>\d)-(?<second>\d)"`
```
-------------------------------------------------------------------------------------------------------------------------------
| Event Datetime            | Restoration Datetime      | Line ID | Type         | Bus Affected | Amount _MW | first | second |
-------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 23:45:00+00:00 | 2019-01-02 00:00:00+00:00 | 5-6     | line_outage  | 5-6          | 1.0        | 5     | 6      |
-------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 19:15:00+00:00 | 2019-01-01 19:30:00+00:00 | 6-7     | loss_of_load | 6            | 1.0        | NULL  | NULL   |
-------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 13:45:00+00:00 | 2019-01-01 14:00:00+00:00 | 7-8     | loss_of_gen  | 7            | 1.0        | NULL  | NULL   |
-------------------------------------------------------------------------------------------------------------------------------
```
- For the three returned events, run a regular expression field extraction on the "Bus Affected" field. During the extraction, try to extract two
  named capture groups into two new, identically named fields: \<first> and \<second>. \<first> and \<second> both attempt to match exactly one digit.
  - The regex successfully extracts the named capture groups from only one event, but that's okay
- Return the three events with the new extracted fields
# Purpose
- Splunk regular expressions are PCRE
  - Yes they are! If greediness doesn't appear to be working remember that "." is not a digit!
- `rex` cannot be used in \<eval> markup with tokens (see source). Use `match` or `repalce` instead
# Arguments
## Required
- A regular expression enclosed in double quotes
## Optional
- A field to extract from
  - Defaults to _raw (that's why the argument is optional)