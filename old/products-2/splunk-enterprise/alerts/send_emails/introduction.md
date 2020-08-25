- https://docs.splunk.com/Documentation/Splunk/latest/Alert/Emailnotification#Configure_email_notification_settings - email alert configuration documentation
- https://answers.splunk.com/answers/399201/how-to-configure-my-email-settings-to-get-email-al.html - example email configuration
- https://answers.splunk.com/answers/231902/why-am-i-getting-error-connection-refused-while-se.html
- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Sendemail - `sendmail` command
# Example configuration
- Go to Settings > Server settings > Email settings
```
Mail host:  smtp.gmail.com:587
```
# Configuration
## Mail host
- Provide an STMP server and port
## Email security
- Check "Enable TLS"
## Username
- The username of the email address I want to use to send the Splunk email
## Password
- The password to my email account
# Other considerations
- If the email account has multifactor authentication, then it isn't suitable to be used as the sender email address
  - Google requires that "less secure app access" be enable to use an account as an SMTP forwarder
# sendemail command
## Example
### Search
```
index=electrical_utility_data sourcetype=realtime_scada 
| where Power_MVA <= 0 
| join type=left _time, Line_ID 
    [ search index=electrical_utility_data sourcetype=realtime_oms 
    | fields _time, Line_ID, Type] 
| where isnull(Type) 
| sendemail to="asc1995@gmail.com" message="hello from Splunk" server="smtp.gmail.com:587" use_tls=true from="blah@yahoo.com"
```
- The "to" field is required, but the "server" field is redundant since that is already set correctly in my Splunk email settings. All other fields
  besides "to" are optional because they have default values