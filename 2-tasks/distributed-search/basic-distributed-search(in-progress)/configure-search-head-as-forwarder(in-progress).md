- https://docs.splunk.com/Documentation/Splunk/latest/DistSearch/Forwardsearchheaddata - how to configure the search head as a forwarder
- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Outputsconf - `outputs.conf` documentation
# Examples
## Configure the search head to act as a forwarder with regard to its search peers
### Files
#### $SPLUNK_HOME/etc/system/local/outputs.conf
```
1  [indexAndForward]
2  index = false
3 
4  [tcpout]
5  defaultGroup = my_search_peers 
6  forwardedindex.filter.disable = true # Events for all indexes are now forwarded (not default behavior)
7  indexAndForward = false
8
9  [tcpout:my_search_peers]
10 server=10.10.10.1:9997,10.10.10.2:9997,10.10.10.3:9997
```
- Line 7 determines whether a Splunk instance indexes data in _addition_ to forwarding it _if_ that instance is configured as a forwarder
  - It is false by default. In other words, if a Splunk instance is configured as a forwarder, it does _not_ index the data in addition to forwarding
    it
    - This is evidenced by `system/default/outputs.conf`
- Line 2 determines whether indexing occurs at _all_ on a Splunk instance
  - When a Splunk instance is not configured as a forwarder, it defaults to true
  - When a Splunk instance _is_ configured as a forwarder, it default to false
  - The selection of the value of the setting is not discernable through the configuration files because there is no default `[indexAndForward]` stanza
  - If `indexAndForward` is set within the `[tcpout]` stanza, that value overries the default value of line 2
    - However, if `index` is explicitly set within the `[indexAndForward]` stanza, then _that_ value overrides any setting in the `[tcpout]` stanza
      - Confusing!
- The `outputs.conf` documentation states that the `indexAndForward` _setting_ (line 7) is only available for heavy forwarders, but since search
  heads have all of the capabilities of heavy forwarders that must be why this setting can be used as it is here
- In summary, line 2 and 7 _both_ appear to be redundant, but they add a lot of clarity so they are worth it
  - Line 13 is _not_ redundant because that setting defaults to false (i.e. any `forwardedindex.<n>.(whitelist|blacklist) = <regex>` filters are
    respected)
## Understand default index filtering rules
### Files
#### $SPLUNK_HOME/etc/system/default/outputs.conf
```
1 [tcpout]
2 maxQueueSize = auto
3 forwardedindex.0.whitelist = .*
4 forwardedindex.1.blacklist = _.*
5 forwardedindex.2.whitelist = (_audit|_internal|_introspection|_telemetry)
6 forwardedindex.filter.disable = false
7 indexAndForward = false
```
- The `outputs.conf` documentation doesn't say much about the interaction between whitelists and blacklists
  - I assume that the highest number `<n>` list gets precedence. Based on that assumption, these whitelist and blacklists _together_ do the following
    _because_ of their order:
    - Line 3: forward events that target any index
    - Line 4: ignore events that target indexes whose names start with "_"
    - Line 5: forward events that specifically target the _audit, _internal, _introspection, and _telemetry indexes
# Purpose
- It is considered best practice to forward all search head internal data to the search peers instead of storing the data on the search head itself
  - The search peers _must_ have the index definitions that the search peer previously used to store data
- So far, the Unexpected Topology Changes Splunk app has worked fine without the specific configurations shown above because 1) it wasn't ingesting
  any important data anyway 2) it has been forwarding _and_ not indexing most of its data and 3) the contents of any events targeted at any _.*
  blacklisted index (a.k.a. those events that weren't being forwarded or indexed) haven't mattered 

# I have not configured the search head yet because I want to check that my assumption about the default filtering rules in example 2 is correct
- This is honestly a low priority for me because this is just annoying configuration stuff that hasn't mattered