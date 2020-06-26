- https://docs.splunk.com/Documentation/SplunkCloud/latest/SearchReference/Join
- https://answers.splunk.com/answers/568034/chart-over-multiple-fields.html
# Examples
## Join subsearches as line ratings
### Search
```
sourcetype="scada" 
| timechart span=15m first("Power _MVA") BY "Line ID"
| join _time 
    [ search sourcetype="scada" 
    | eval "Power _MVA"=150 
    | timechart span=15m first("Power _MVA") as "5-6,6-7 line rating"] 
| join _time 
    [ search sourcetype="scada" 
    | eval "Power _MVA"=300 
    | timechart span=15m first("Power _MVA") as "3-6 line rating"] 
| join _time
    [ search sourcetype="scada" 
    | eval "Power _MVA"=250 
    | timechart span=15m first("Power _MVA") as "1-4,4-5,7-8,2-8,8-9,4-9 line rating"]
```
- Each subsearch gets all 864 events and replaces the "Power _MVA" field in each event with a dummy value, such as 300. Then, the subsearch is joined
  to the main search
  - This is a horrible command in terms of performance
- The result is that the `timechart` command charts multiple time series at the same time. Neat!
  - Is there a more performant way to achieve the same result? Yes there is. Choose a single line ID. Every time that line ID appears in an event,
    that event delimits a logical Splunk transaction which consists of the measurements taken at the same instant from different buses. Make the
    "Power _MVA" field of every targeted event into a multivalued field, then split the multivalued field into new events. Finally, change the "Line
    ID" of the new events into a line rating name depending on the "Power _MVA" field
    - See `mvexpand.md` notes
  - Even better, learn how to use Splunk dashboard inputs to allow users to toggle lines and their ratings on and off
## Join on nonexistent column
### Search
```
index=electrical_utility_data sourcetype=realtime_scada
| where Power_MVA <= 0
| join blah
    [ search index=electrical_utility_data sourcetype=realtime_oms]
```
```
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| _time                 | Timestamp                 | Line_ID | Power_MVA | Origin_Bus_ID | Origin_Voltage_PU  | Destination_Bus_ID | Destination_Voltage_PU | Event_Timestamp           | Restoration_Timestamp     | Bus_Affected | Type         | Amount_MW |
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| 6/2/20 1:15:00.000 AM | 2020-06-02 05:15:00+00:00 | 4-5     | 0.0       | 8             | 0.9904921699138927 | 9                  | 0.9182110817256903     | 2020-06-02 20:30:00+00:00 | 2020-06-02 20:45:00+00:00 | 5            | loss_of_load | 1.0       |
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```
- Since "blah" is a nonexistent column for both the main search and the subsearch, I'm guessing that Splunk sees that it evaluates to NULL for both
  searches and happily joins on NULL
- The end result is very misleading. The base search had a realtime_scada event with a Line_ID of "8-9", but it was replaced by the value of Line_ID
  of the subsearch, which was "4-5". The end result doesn't even make sense. The only reason the loss_of_load realtime_oms event was joined with the
  base search is because it was the first event returned by the subsearch. If the correct realtime_oms event had been joined with the base search,
  "Type" would be "line_outage"
- When I try to join on a column that only the base search or subsearch has, _then_ I get 0 results as expected
# Purpose
- `join` is a centralized streaming command when a set of fields with which to join is provided
  - Otherwise, it is `dataset processing` command
- Yes, I *can* simply join multiple `timechart` results together, but using `BY` in the outer and inner searches does not work like I want
# Arguments
## Required
- A subsearch
## Optional
- A comma-separated list of fields upon which to join
- One or more \<join options>