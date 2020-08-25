- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Webconf
# Examples
## Use new management port
### Files
#### /opt/splunkforwarder/etc/system/local/web.conf
```
[settings]
mgmtHostPort = 127.0.0.1:8090
```
- This will only listen on the localhost IP on port 8090 for deployment server requests
- The default key-value pair is `mgmtHostPort=0.0.0.0:8089`
  - This will listen on all IP addresses on port 8089
# Purpose
- When any instance of Splunk is started, that instance has the ability to be managed by a deployment server
  - A deployment server is a different Splunk instance that can push configuration changes to the Splunk instances that it manages (a.k.a. "clients")
    - In reality, the clients query the deployment server. Nothing is "pushed" by the deployment server
- In order to enable a Splunk instance to be managed by a deployment server, that Splunk instance must be able to listen for the deployment server on
  a port
  - This port is called the "management port" and is typically 8089
- If port 8089 is not open on a machine when a Splunk instance is started, the Splunk instance must use a different port as its management port