- https://docs.splunk.com/Documentation/Splunk/latest/AddMcafeeCloud/InstallHWF
- https://community.splunk.com/t5/Archive/How-to-create-appserver-directory/td-p/266713 - some discussion of the purpose of the appserver port
# Examples
## Change conflicting ports on the heavy forwarder
### Files
#### $SPLUNK_HOME/etc/system/local/web.conf
```
[settings]
mgmtHostPort = 0.0.0.0:8090
httpport = 8001
appServerPorts = 8066
```
- Mandatory changes:
  - Change the management port from 8089 (default) to 8090 so it doesn't conflict with the search head management port
    - It's annoying that I have to do this even when I'm not using a search head as a deployment server, but whatever
    - The IP _and_ port of the management port must be specified
    - The default value is `mgmtHostPort = 0.0.0.0:8090`
      - I guess this means the deployment server, if it's active, will listen for _any_ IP that sends requests to its port 8089
- Optional changes if I also want to run Splunk Web on the heavy forwarder:
  - Change the Splunk Web HTTP port from 8000 (default) to 8001 so it doesn't conflict with the search head Splunk Web process
  - Change the Splunk Web appserver port from 8065 (default) to 8066 so it doesn't conflict with the search head Splunk Web process
    - Many Splunk apps have an `appserver` directory that holds static assets of the app, such as HTML pages, image files, and JS files
      - I suppose that Splunk needs a port to handle when these files are uploaded?
        - Regardless, disabling Splunk Web means that this port won't be used
#### $SPLUNK_HOME/etc/system/local/server.conf
```
[kvstore]
port = 8192
```
- Optional change if I want to run the kvstore process on the search head and heavy forwarder:
  - Change the kvstore port from 8191 (default) to 8192 so that it doesn't conflict with the search head
- Alternatively, I could disable the kvstore process entirely, since the kvstore process and Splunk Web process are separate
## Configure Splunk Enterprise to forward data as a heavy forwarder
### Files
#### $SPLUNK_HOME/etc/system/local/outputs.conf
```
[tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = localhost:9997

[tcpout-server://localhost:9997]
```
- Recall that the `defaultGroup` attribute is required under the `[tcpout]` stanza to enable forwarding for the selected target group
  - The value of the attribute _must_ be a target group, so a target group must be defined
  - See `universal-forwarder/1-configure-basic-forwarding.md` for details
- The `indexAndForward` attribute is false by default
  - This is what I want
- This result can also be created by going to Settings > Forwarding and receiving
# Purpose
- All that is required to make a Splunk Enterprise instance function as a heavy forwarder is to enable forwarding as shown in the second example
- If I want to run a heavy forwarder process (i.e. Splunk Enterprise) and a search head process (i.e. Splunk Enterprise) on the same machine, then I
  have to perform certain steps depending on what I want:
  - I must change the management port on the heavy forwarder because I can't disable it (for whatever reason. I don't even think the deployment-server
    process is running on the heavy forwarder)
  - I can disable the kvstore process on the heavy forwarder, or I must change the kvstore port on the heavy forwarder
  - I can disable the Splunk Web process on the heavy forwarder, or I must change the `httpport` and `appServerPorts` on the heavy forwarder
