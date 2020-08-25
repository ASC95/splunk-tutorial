- https://docs.splunk.com/Documentation/Splunk/latest/Report/Managereportpermissions?ref=hk - report permissions
# Purpose
- The permissions for an individual knowledge object are pretty straight forward
  - If I choose to share my knowledge object with an app, I can then grant "read" or "write" access to my knowledge object to certain roles
    - By "certain roles" I really mean "every role that is actually assigned to a user," but that's an extremely minor distinction
- However, See `4-app-based-role-permissions.md` to understand the complex interaction _between_ app-based role permissions and individual-knowledge-object-based
  role permissions
# "Run as" option
- Some knowledge objects, like reports, have an additional "run as" option in addition to standard individual permissions
  - Setting the option to "owner" (default) runs the search with the permissions of the report owner
  - Setting the option to "user" runs the search with the permissions of the current report user
- I using "owner" in most cases makes sense because in order to get the most use out of a report, the user should be guaranteed to see everything that
  the owner wanted them to see
