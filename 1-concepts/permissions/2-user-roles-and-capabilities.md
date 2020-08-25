- https://docs.splunk.com/Documentation/Splunk/latest/Admin/authorizeconf - describes all capabilities
# Purpose
- Users can _only_ be assigned roles; they cannot be assigned individual capabilities (thank goodness)
- Also, roles can inherit from other roles, but an inheriting roles can only _add_ capabilities, _not_ delete inherited capabilities (thank goodness)
- Thus, I need to understand individual capabilities that I care about, but I can always organize those capabilities underneath their owning roles
# Splunk default role capabilities
## admin
- The default "admin" role in Splunk comes bundled with these capabilities
### Capabilities
#### admin_all_objects
- This capability allows the user to bypass all typical a access control list restrictions so that they can read/write any object in the entire system
  - Based on reading the descriptions of all the capabilities, this is the _single_ capability that enables this behavior
- This is the most powerful capability in Splunk. _Only_ this capability grants a user with the assigned role the ability to do the following:
  - Modify app-based role permissions _at all_ (see `4-app-based-role-permissions.md`)
    - In other words, only users with this capability can assign app-based role permissions _full stop_ and click the save button
    - Apply app-based role permission to _all_ apps _at once_ (i.e. click the second radio button on an app's permission page)
  - View private knowledge objects
    - E.g. if a user creates a private knowledge object (e.g. a report), then an admin cannot see that report within the normal app "Reports" page.
      However, an admin _can_ see the report via the "Settings > All configurations" page
  - Share a private or app-wide knowledge object with _all_ apps _at once_ (i.e. select the "All apps" button when editing the permissions of an
    individual knowledge object)
    - _Every_ user without this role can _only_ share their knowledge object at most with a single app at a time
- This capability does _not_ enable the following behaviors, but it hardly matters:
  - Listing/searching all users, especially in the "All configurations" settings page
    - E.g. I made a new "weird-admin" role that inherited the user role but also had this capability. While the "weird-admin" couldn't search users in
      the "All configurations" page, they could still eventually find every private object
#### edit_user
- Allows the user to create, edit, or remove other users
  - E.g. enabling this capability on the "weird-admin" role enabled the weird admin to search by _all_ users in the "All configurations" page
    - However, without the `admin_all_objects` capability, the weird admin couldn't see any private objects
  - E.g. enabling this capability on the "weird-admin" role enabled the weird admin to change other user's passwords and assigned roles!
- The scope of this capability can be restricted by restricting the role to only be able to grant certain other roles by editing `authorize.conf`