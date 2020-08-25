- https://community.splunk.com/t5/Archive/How-do-I-route-data-to-specific-index-based-on-a-field/td-p/77902
# Examples
## Perform basic script input data tagging
### Files
#### /opt/splunkforwarder/etc/system/local/inputs.conf
```
[default]
host = hahaha

[script://$SPLUNK_HOME/bin/scripts/call-python.sh]
disabled = 0
index = local-test-index
interval = -1
source = call-python.sh
```
- `source` defaults to the input file path if it is not set
  - Since it is set, all events on indexers will have the value "call-python.sh" for their `source` attribute
  - I _believe_ that the value of `source` is always set by the forwarder, since `source` will always get a default value from the forwarder
- `host` defaults to $decideOnStartup, which actually makes Splunk write a fixed string to `/opt/splunkforwarder/etc/system/local/inputs.conf` when
  Splunk is installed for the first time
  - Since it is set, all events on indexers will have the value "hahaha" for their `host` attribute
  - The value of `host` is _always_ set by the forwarder, never the indexer
- `index` defaults to whatever the default index is (e.g. "main")
  - The default index can be found be examining the `defaultDatabase` attribute in `indexes.conf`
- If not set, the indexer analyzes the data and chooses a value for `sourcetype`. There is no default value
  - In pratice, the forwarder appears to set the value of `sourcetype` to "exec" because that's what Cribl shows
    - When I erase the `sourcetype` key entirely in Cribl, the indexer assigns the `sourcetype` a value of "?"
# Purpose
- `inputs.conf` is used on a universal forwarder to configure what data the universal forwarder will send and what basic tagging it will perform on
  that data
- `host` and `source` are always defined at input time before indexing
  - This is evidenced by the fact that `props.conf` can _match_ on `host` and `source` values, but cannot set them
- Events _can_ be routed to different indexes using `props.conf` and `transforms.conf` in tandem on the indexer
- Events _can_ be assigned different sourcetypes using `props.conf` and `transforms.conf` in tandem on the indexer