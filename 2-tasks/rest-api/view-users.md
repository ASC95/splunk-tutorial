# Examples
## View users via REST API
### Web requests
#### Use browser
```
https://localhost:8089/services/authentication/users
```
- This returns a list of users and their capabilities (a.k.a. individual abilities) and roles (a.k.a. labels for groups of capabilities)
- This information can also be viewed at `http://localhost:8000/en-US/manager/launcher/authentication/users`
  - In this case, the Splunk Web endpoint and the REST API endpoint are similar, but I don't know if that's a conincidence