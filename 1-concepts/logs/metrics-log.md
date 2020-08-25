- https://docs.splunk.com/Documentation/Splunk/latest/Troubleshooting/Aboutmetricslog - understanding `$SPLUNK_HOME/var/log/splunk/metrics.log`
- https://conf.splunk.com/files/2017/slides/how-splunkd-works.pdf - presentation on Splunk processing details. These slides might be useful if I can
  find the accompanying presentation
- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Configurationparametersandthedatapipeline - data only goes through each phase of the
  data processing pipeline once
# Problem
- In summary, I have not discoverd how to search `metrics.log` to troubleshoot data pipeline errors because the search results generated from a direct
 Splunk forwarder connection and the results generated from a Cribl connection look the same: the `name` of the most commone pipeline is "parsing"  
# Examples?
## Analyze what pipeline incoming data is being sent through
```
index=_internal group=pipeline host=aifbdp-forwarder
```
- The contents of every file within `$SPLUNK_HOME/var/log/splunk/metrics.log`, including `metrics.log`, are monitored and stored within the
  `_internal` index
- When the data comes directly from a universal forwarder, the most common pipeline `name` is "parsing"
  - I think this means that data from the universal forwarder is going through the "parsing" pipeline?

- When the data comes from Cribl, the most common pipeline `name` is "x"


# Background
- Recall that incoming data may only pass through each phase of the data processing pipeline once, so it can be useful to understand which phase of
  the data processing pipeline that incoming data is being sent to
# Purpose
- `metrics.log` contains information about CPU usage and Splunk queue usage
  - Its contents are generated periodically, every 30 seconds or so, and it _only_ reports the top ten results for each `group`
    - This ...
- It can be used to debug the data processing pipeline
# Syntax
- x