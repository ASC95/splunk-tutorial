# Examples
## Disable Splunk Web
### Files
#### $SPLUNK_HOME/etc/system/local/web.conf
```
[settings]
startwebserver = 0
```
- This configuration can be created with `$ ./splunk disable webserver`
