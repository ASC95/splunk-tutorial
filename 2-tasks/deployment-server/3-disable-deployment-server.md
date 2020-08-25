# Examples
## Disable the deployment server
### Files
#### $SPLUNK_HOME/etc/system/local/serverclass.conf
```
[global]
disabled = true
```
- The deployment server is enabled by default
- Even if the deployment server is disabled, the management port is still bound when the splunkd proces starts
### Commands
```
$ ./splunk disable deploy-server
```
- This command generates the configuration shown in the first example 