- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Makeresults
# Examples
## Generate basic events
### Commands
```
| makeresults count=4
```
- This generates four events whose only field is the `_time` field
  - `count` defaults to 1
## Generate rich events
### Commands
```
| makeresults count=4 annotate=true
```
- This generates four events that have the following fields with the following values values:
  - _raw: NULL
  - _time: date and time that you run the makeresults command
  - host: NULL
  - source: NULL
  - sourcetype: NULL
  - splunk_server: the name of the server that the makeresults command is run on
  - splunk_server_group: NULL
# Purpose
- Use `makeresults` to generate empty events
- It must be the first search in a command