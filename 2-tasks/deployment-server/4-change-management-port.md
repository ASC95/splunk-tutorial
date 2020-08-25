# Examples
## Change management port on deployment server
### Files
#### $SPLUNK_HOME/etc/system/local/web.conf
```
[settings]
mgmtHostPort = 0.0.0.0:8090
```
- I have now changed the management host port to be 8090 instead of 8089
- Now forwarders will need to poll for changes on 8090 instead of 8089
## Change reference to the deployment server management port on a forwarder that is also a deployment client
### Files
#### $SPLUNK_HOME/etc/system/local/deploymentclient.conf
```
[deployment-client]
disabled = false # This is optional but is good for clarity
[target-broker:deploymentServer]
targetUri = localhost:8090
```
- There is no default `deploymentclient.conf` file. The documentation says there is, but there isn't!
- This is the _minimum_ required configuration for this file
  - If I don't include the empty `[deployment-client]` stanza, then the deployment client _won't_ phone home (at least not within a reasonable time
    interval that can be checked)!
- If I were using port 8089 instead, I would _still_ have to specify that port here because there is no default `deploymentclient.conf` file