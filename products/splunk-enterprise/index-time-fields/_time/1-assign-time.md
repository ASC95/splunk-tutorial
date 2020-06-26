- https://docs.splunk.com/Documentation/Splunk/latest/Data/HowSplunkextractstimestamps - introduction to timestamp extraction section. Specifies the
  detailed assignment process
- https://answers.splunk.com/answers/218698/is-time-in-utc-or-local-time.html
# Examples
## _time is stored in UTC, but displayed and used in calculations according to the local timezone
### Search
```
index=electrical_utility_data sourcetype=realtime_oms | table _time, Event_Timestamp, Restoration_Timestamp
```
```
-------------------------------------------------------------------------------
| _time               | Event_Timestamp           | Restoration_Timestamp     |
-------------------------------------------------------------------------------
| 2020-06-01 05:45:00 | 2020-06-01 09:45:00+00:00 | 2020-06-01 10:00:00+00:00 |
-------------------------------------------------------------------------------
| 2020-06-01 06:45:00 | 2020-06-01 10:45:00+00:00 | 2020-06-01 11:00:00+00:00 |
-------------------------------------------------------------------------------
| 2020-06-01 18:45:00 | 2020-06-01 22:45:00+00:00 | 2020-06-01 23:00:00+00:00 |
-------------------------------------------------------------------------------
```
- On June 1st, 2020, I am in ET time (GMT - 4)
  - This is reflected in the displayed `_time` field
# Assignment process
- Splunk has a assignment process for deriving the value of the `_time` field for an event from the raw data 
  - The source details the assignment process, but there is more to it than is documented
- I won't restate the source. Instead, these notes offer additional, non-documented insight into the process
## Steps
- First, look in the stanza of the chosen `sourcetype` (defined in `props.conf`) for the `TIME_FORMAT` attribute. If the attribute exists, its regular
  expression value *will* be used to parse the raw data for each event's timestamp
  - For each single event, if the regex attribute value cannot successfully parse the raw data to get a timestamp, then Splunk will:
    - Go through the assignment process as detailed in the source
      - E.g. if the raw data of an event from a file input cannot be parsed, then Splunk could eventually assign the event's `_time` to be the
        modification time of the file
    - This is not detailed in the source, but Splunk will also *implicitly* parse each non-matching event and attempt to extract a timestamp on its
      own
      - This implicit step seems to happen right after Splunk attemps to use `TIME_FORMAT`, and before the rest less-than-accurate assignment steps
        are followed
      - I believe that the `[default]` stanza's definition of `DATETIME_CONFIG` might play a role in this
      - The net result is that even if the `sourcetype` of a data input is totally inadequate, any event that has a reasonably parsable raw timestamp
        string will probably get the correct value of its `_time` field