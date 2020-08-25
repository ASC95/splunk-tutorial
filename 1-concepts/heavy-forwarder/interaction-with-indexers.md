- https://docs.splunk.com/Documentation/Splunk/latest/Forwarding/Typesofforwarders
- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Configurationparametersandthedatapipeline - data moves through each pipeline stage exactly
  once
- https://docs.splunk.com/Documentation/Splunk/latest/Deploy/Datapipeline - a higher-level, but less useful, description of the data pipeline
# Data only moves through each stage of the processing pipeline once
- "Data only goes through each phase once, so each configuration belongs on only one component, specifically, the first component in the deployment
  that handles that phase"
# Only a heavy forwarder can assign index-time fields to the forwarded data
- Only the heavy forwarder can set values for index-time fields (e.g. `host`, `source`, and `sourcetype`)
  - Thus, if an indexer receives data from a heavy forwarder, it cannot assign index-time fields!
    - Recall that once index-time fields are set, they _cannot_ be changed _ever_
  - The default queue in `inputs.conf` _is_ the `parsingQueue`, so changing that setting won't help
- This was confirmed by meticulous testing:
  - When data from a heavy forwarder is _not_ tagged with a sourcetype
    - And the indexer performs source-based sourcetype assignment within a `splunktcp` stanza in `inputs.conf`
      - The sourcetype of the events is "exec"
    - And the indexer performs source-based sourcetype assignment within a `source` stanza in `inputs.conf`
      - The sourcetype of the events is "exec"
    - And the indexer performs source-based sourcetype assignment within a `source` stanza in `props.conf`
      - The sourcetype of the events is "exec"
    - And the indexer does not perform any source-based sourcetype assignment
      - The sourcetype of the events is "exec"
    - And the indexer performs per-event sourcetype assignment within a `source` stanza in `props.conf`
      - The sourcetype of the events is "exec"
  - If I didn't set the index on the heavy forwarder, then the data always went to the `main` index on the indexer
  - If it _looks_ like the indexer is setting index-time fields on data from a heavy forwarder, it's probably because the indexer and heavy forwarder
    share some identical configuration that makes it hard to tell which machine is assigning the index-time fields
# Cribl behaves exactly like a heavy forwarder
- Justin was right!