- https://docs.splunk.com/Documentation/Forwarder/latest/Forwarder/Configureforwardingwithoutputs.conf
- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Outputsconf
# Examples
## Forward data to local Splunk instance
### Files
#### /opt/splunkforwarder/etc/system/local/outputs.conf
```
[tcpout]
defaultGroup=local-splunk

[tcpout:local-splunk]
server=127.0.0.1:9997
```
- Yes, the global `[tcpout]` stanza and the `defaultGroup` attribute are required since I didn't use any `_TCP_ROUTING` attributes in an `inputs.conf`
# Background
## IP addresses
- "127.0.0.1" is the loopback address, a.k.a. my computer
  - This address does _not_ connect to the network that my computer is connected to. It is purely for local diagnostics
  - This address _can_ be used in `outputs.conf` to send data to Cribl or any other locally running server, provided that Cribl is also listening on
    localhost
- "192.168.x.x" is the network address created by my router where all of my networked devices communicate over LAN
# Purpose
- `outputs.conf` configures where and how a universal forwarder sends data that it collects
## Output processors
- There are two types of output processors: "tcpout" and "syslog"
  - Universal forwarders _only_ have the tcpout processor
# Syntax
- The global `[tcpout]` stanza is optional
  - However, certain attributes can _only_ be set in this global stanza
    - E.g. The forwarder sends data to all target groups defined by the _explicitly_ global `defaultGroup` attribute. These target groups must be
      defined later on in `[tcpout:<target_group>]` stanzas, _not_ `[tcpout-server://<ip address>:<port>]` stanzas
      - If I don't want to automatically forward data, don't set the `defaultGroup` attribute. If the `defaultGroup` attribute is not set, then the
        `_TCP_ROUTING` attribute must be used in `inputs.conf` in order to forward _any_ data
# Syntax
- `[tcpout]` is the format for a global configuration stanza for TCP output
- `[tcpout:<target group>]` is the format for a "target group" stanza for TCP output
  - Each target group can have one or more servers
  - A target group name cannot have spaces or colons. If it does, Splunk ignores the target group
- `[tcpout-server://<ip address>:<port>]` is the format for a single server configuration
  - Each server defined this way must _also_ be part of a target group
- In summary:
  - I probably always want a global `[tcpout]` stanza to override default values
  - I must always have at least one target group to send data
  - Single server configurations are optional to Splunk, but can be required by a situation if severs need individual configuration