- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Return - `return` documentation
- https://answers.splunk.com/answers/240798/how-to-return-a-single-value-from-a-subsearch-into.html - `return` usage example
# Examples
## Get the Line_IDs of all realtime_oms events that experienced a line_outage, then get all realtime_oms events with those Line_IDs
### Incorrect approach
```
index=electrical_utility_data sourcetype=realtime_oms 
    [ search index=electrical_utility_data sourcetype=realtime_oms Type=line_outage 
    | return Line_ID]
```
- At first glance, this search appears to do what I want. It appears to use a subsearch to find all realtime_oms events that experienced a line
  outage, and then return all of their Line_IDs to the outer search as "Line_ID=\<value>" boolean expressions 
  - E.g. if the set of Line_IDs created from realtime_oms events that experienced line outage events contains 4, 5, and 6, then the outer search
    should receive `(Line_ID="4") OR (Line_ID="5") OR (Line_ID="6")`
- However, this is not what happens. By default, `return` _only_ uses the first row found by the subsearch as a source of data. Since the first row
  just happens to have Line_ID="4-9", this search only returns `Line_ID="4-9"` to the outer search. It ignores the fact that other Line_IDs also
  experienced line_outage events
### Technically incorrect, but most performant approach
```
index=electrical_utility_data sourcetype=realtime_oms 
    [ search index=electrical_utility_data sourcetype=realtime_oms Type=line_outage 
    | return 100 Line_ID]
```
- This search does what I thought the first search did. It uses a subsearch to find all realtime_oms events that experienced a line_outage event.
  Instead of just returning the top row, it returns the first 100 rows. Since there are only 5 such realtime_oms events, all five of them are
  returned. The base search then finds _all_ realtime_oms events with those matching Line_IDs
  - I thought long and hard about a better way to do this, but I don't think there is one
### Technically correct, but inefficient approach
```
index=electrical_utility_data sourcetype=realtime_oms 
    [ search index=electrical_utility_data sourcetype=realtime_oms Type=line_outage 
    | return 
        [ search index=electrical_utility_data sourcetype=realtime_oms Type=line_outage
        | stats count 
        | return $count] Line_ID]
```
- This search is different from the last search because it uses another subsearch to determine how many realtime_oms events had a line outage event,
  then returns that number to the outer `return` command as an argument. The net result is that this outer subsearch looks like `... | return 5 Line_ID`
  instead of `... | return 100 Line_ID`
  - This is the best approach because it ensures I don't pick too low an integer to supply to the outer `return` command.
    - E.g. What if there were 1000 realtime_oms events, each with a unique Line_ID, that were line outages? In the last example, my arbitrary choice
      of 100 would have caused 900 of those Line_IDs to be missing
  - I bet this search is really slow!
# Purpose
- The `return` command is syntactic sugar to make it easier to return 1) field values and 2) \<field name>=\<field value> boolean expressions to outer
  searches
# Arguments
## Required
- None
## Optional
- A integer specifying how many rows `return` should use as a source of data
  - E.g. `... | return 5 <field>`
  - By default, this is 1, so `return` only uses the values in the very first event obtained by the subsearch
