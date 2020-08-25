- https://docs.splunk.com/Documentation/Forwarder/latest/Forwarder/Enableareceiver
# Examples
## Enable receiver to listen for forwarders
### Files
#### inputs.conf
```
[splunktcp://9997]
```
- This enables a receiver to listen on all IP address on the specific port for any Splunk forwarders
- The conventional receiver input port is 9997
  - This is the port the receiver listens on for any forwarders
## Enable receiver to listen selectively for Splunk forwarders
### Files
#### inputs.conf
```
[splunktcp://127.0.0.1:9997]
```
- This restricts a receiver to listening on the specified remote server IP address + port for any Splunk forwarders
## Enable receiver to listen for third-party TCP requests
### Files
#### inputs.conf
```
[tcp://9998]
```
- This enables a receiver to listen on all IP addresses on the specific port for any TCP connections
- When I configure a receiver like this and get input from Cribl (which is in turn getting data from a Splunk forwarder), the events are 1) stored
  in the default index and 2) look like this:
  ```
  _linebreaker\x00\x00\x00\x00_done\x00\x00\x00\x00_done\x00\x00\x00\x00\x00\x00\x00\x00_raw\x00\x00\x00\xEC\x00\x00\x00\x00\x00\x00_raw\x00\x00\x00\x00V2020-04-07 11:00:00+00:00,8-9,67.74547167364612,0.9974573167554873,0.9669521814252354\x00\x00\x00\x00MetaData:Source\x00\x00\x00\x00source::call-python.sh\x00\x00\x00\x00MetaData:Host\x00\x00\x00\x00host::austin-Z390-UD\x00\x00\x00\x00MetaData:Sourcetype\x00\x00\x00\x00sourcetype::exec\x00\x00\x00\x00_time\x00\x00\x00\x001586257200\x00\x00\x00\x00_MetaData:Index\x00\x00\x00\x00local-test-index\x00\x00\x00\x00_meta\x00\x00\x00\x00>cribl_breaker::fallback foo::blah cribl_pipe::troubleshooting\x00\x00\x00\x00	interval\x00\x00\x00\x00-1\x00\x00\x00\x00__CRIBBLED\x00\x00\x00\x00bJxOU\x00\x00\x00\x00
  _linebreaker\x00\x00\x00\x00
  ```
- Thus, this is clearly not the solution to my problems...
## Enable a receiver to selectively listen for third-party TCP requests
### Files
#### inputs.conf
```
[tcp://<x>:9998]
```
- This restricts a receiver to listening on the specified remote server IP address + port for any TCP connections