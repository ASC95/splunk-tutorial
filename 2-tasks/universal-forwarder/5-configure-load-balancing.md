- https://docs.splunk.com/Documentation/Forwarder/latest/Forwarder/Configureloadbalancing
# Examples
## Accidentally forward all data to all indexers
### Files
#### /opt/splunkforwarder/etc/system/local/outputs.conf
```
[tcpout]
disabled = false
defaultGroup = splkbdpidx1,splkbdpidx2

[tcpout:splkbdpidx1]
server = <IP_1>:<port_1>
disabled = 0
clientCert = /opt/splunkforwarder/etc/auth/bdp/splkforwarder_chain.pem
useClientSSLCompression = true
sslPassword = <password>
sslCommonNameToCheck = splunkbdpidx1.pnnl.gov
sslVerifyServerCert = true

[tcpout:splkbdpidx2]
server = <IP_2>:<port_2>
disabled = 0
clientCert = /opt/splunkforwarder/etc/auth/bdp/splkforwarder_chain.pem
useClientSSLCompression = true
sslPassword = <password>
sslCommonNameToCheck = splunkbdpidx2.pnnl.gov
sslVerifyServerCert = true
```
- If there is more than one target group stanza, the forwarder sends _all_ data to _each_ target group
- I thought that load balancing occurred _between_ target groups. I was incorrect. It only occurs _within_ target groups
## Correctly load balance between two indexers
### Files
#### /opt/splunkforwarder/etc/system/local/outputs.conf
```
[tcpout]
disabled = false
defaultGroup = bdp-indexers

[tcpout:bdp-indexers]
server = <IP_1>:<port_1>,<IP_2>:<port_2>
disabled = 0
autoLBVolume = 30

[tcpout-server://<IP_1>:<port_1>]
disabled = 0
clientCert = /opt/splunkforwarder/etc/auth/bdp/splkforwarder_chain.pem
useClientSSLCompression = true
sslPassword = <password>
sslCommonNameToCheck = splunkbdpidx1.pnnl.gov
sslVerifyServerCert = true

[tcpout-server://<IP_2>:<port_2>]
disabled = 0
clientCert = /opt/splunkforwarder/etc/auth/bdp/splkforwarder_chain.pem
useClientSSLCompression = true
sslPassword = <password>
sslCommonNameToCheck = splunkbdpidx2.pnnl.gov
sslVerifyServerCert = true
```
- Both servers are in the _same_ TCP target target group (i.e. "bdp-indexers"). Each server can still have separate configuration
  - In fact, all of the SSL configuration _needs_ to be set under _each_ individual single server configuration. When I tried to group the shared SSL
    configuration values under the bdp-indexers target group, the SSL verification failed
- `autoLBVolume = 30` tells Splunk to load balance every 30 bytes if it can
  - This didn't give me the load balancing behavior that I wanted since one index got all the data, but at least both indexers didn't get the same
    data!
    - There are many possible causes for this: 1) universal forwarders send data in blocks, which can be up to 64 kB in size 2) the output queue is
      500 kB by default (see `outputs.conf` documentation)
    - I'm sure I'd get expected behavior if I was forwarding a greater volume of data
# Purpose
- Load balancing behavior is configured by `outputs.conf`
- By default, a forwarder switches to a different indexer every 30 seconds
  - This behavior is defined by the `autoLBFrequency = 30` settings in `default/outputs.conf`
- Forwarders perform _automatic_ load balancing
  - The forwarder routes data to different indexers according to a _user-specified_ time or volume interval
- Using a third-party load balancer is not recommended
- A universal forwarder never parses or identifies event boundaries before forwarding data to an indexer