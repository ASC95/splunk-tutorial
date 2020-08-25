- https://docs.splunk.com/Documentation/Splunk/latest/RESTUM/RESTusing#Access_Control_List
- https://dev.splunk.com/enterprise/docs/developapps/manageknowledge/setpermissionsforobjects/
# Examples
## Examine the ACL properties of a saved search
### Web requests
#### Use browser
```
https://localhost:8089/servicesNS/austin/untopchan/saved/searches/My%20Unexpected%20Topology%20Change%20Clone/acl
```
- By appending `/acl` to the URL, I can examine the ACL properties of this specific search
  - This information is also available without the `/acl` URI, but it is easy to lose track of with all of the other information that is also provided
## Change permissions of a saved search
### Web requests
#### Use curl
```
curl -k -u <user>:<password> https://localhost:8089/servicesNS/austin/untopchan/saved/searches/My%20Unexpected%20Topology%20Change%20Clone/acl \
        -d owner=alice \
        -d perms.read=* \
        -d sharing=app
```
- Change the owner to "alice"

- Place additional read restrictions on the resource
  - Actually, since the value is "*", the resource can still be read by anyone

- Change the sharing of the resource from "user" (private) to "app" (app-wide)
  - E.g. `<s:key name="sharing">user</s:key>`

# Purpose
- Every resource has a set of access control list permissions that can be viewed and changed via the REST API
- The values of the ACL permissions that are returned are determined by the user-app namespace that was used in the URL
  - They are also determined by the \<user>:\<password> provided with a curl request
# Syntax
- The XML element `<s:key name="eai:acl">` contains the ACL properties that enforce ownership and permissions for a resource
- The XML element `<s:key name="perms"/>` normally contains no child elements, but it _can_ be modified to specify additional restrictions for a
  resource
  - E.g. ...
