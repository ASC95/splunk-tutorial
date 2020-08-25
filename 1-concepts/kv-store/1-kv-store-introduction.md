- https://docs.splunk.com/Documentation/Splunk/latest/Admin/AboutKVstore
- https://dev.splunk.com/enterprise/docs/developapps/manageknowledge/kvstore/aboutkvstorecollections/

- KV Store files only reside on the search head (there's exceptions)
- A KV Store configuration must exist within the context of an app
- The KV Store listens on port 8191 
- A KV Store collection can be _used_ as a lookup, just like a CSV can be used a lookup
- It seems that the KV Store uses port 8191 for its own purposes, but that the _developer_ must use the management port (8089) to send REST API
  queries to interact with the KV Store (verify this)


- I think I'll do the nicknames things David suggested for the first KV Store CRUD demo, then I'll do something more fun like use OMF component JSON
  or something. He also wants the user to be able to add comments to the outage map or something