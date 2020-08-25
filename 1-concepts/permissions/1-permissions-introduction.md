- https://docs.splunk.com/Documentation/Splunk/latest/Knowledge/Manageknowledgeobjectpermissions
# At a high level, there are three types of permissions
- These are role-based permissions, app-based role permissions, and individual-knowledge-object-based role permissions
  - There's actually a forth level of permissions via the `admin_all_objects` capability, but I'm going to pretend there are three
# Role-based permissions
- There are role-based permissions, which are composed of "capabilities," that give users with the assigned role the ability to perform some actions
# App-based role permissions
- There are _also_ app-based role permissions that give users with the assigned role the ability to perform _other_ actions within the context of an
  app
- Every Splunk role does not appear to be associated with any particular Splunk app, so I'm not sure what the documentation means by "make the
  knowledge object available to all roles associated with an app"
  - I guess it just means that every role _can_ be assigned app permissions, so those roles that I choose to grant "read" or "write" permissions for
    an app become "associated" with that app
# Individual-knowledge-object-based role permissions
- These are straightforward, but the interaction between them and app-based role permissions is complex
