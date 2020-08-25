- https://docs.splunk.com/Documentation/Splunk/latest/Knowledge/Manageknowledgeobjectpermissions - this is the official documentation, but most of
  these notes were created through trial and error
# Examples
## Interactions between app-based role permissions and individual knowledge object permissions
### Owner app read/write
- If the owner of an individual knowledge object has "read" and "write" access to an app, they can view, clone, edit, and delete the object
  - They can edit all properties of the object, including permissions
- It does not matter whether the owner has a role that has explicit access to the object or not
  - E.g. if the owner accidentally removed "read" + "write" access from _all_ roles that they had, they could still control their object
### Owner app read
- If the owner of an individual knowledge object only has "read" access to an app, they can only view, clone, and edit certain properties of the
  object
  - They _cannot_ delete the object, nor edit the _permissions_ of the object
- It does not matter whether the owner has a role that has explicit access to the object or not
  - E.g. if the owner accidentally removed "read" + "write" access from _all_ roles that they had, they could still control their object
### Non-owner app read + object read
- If a non-owner only has "read" access to an app and "read" access for an individual app-wide knowledge object, they can only view and clone
  the object
  - They _cannot_ edit the description nor delete the object
### Non-owner app read + object read/write
- If a non-owner has "read" access to an app, _and_ they have "read" and "write" permission to an individual app-wide knowledge object, they
  can view, clone, and edit the description of the object
  - They cannot delete the object
### Non-owner app read/write + object read
- If a non-owner has "read" and "write" access to an app, _but_ they only have "read" access to an individual app-wide knowledge object, they
  can view, clone, and delete the object
  - They cannot edit the description or other properties
### Non-owner app read/write + object read/write
- If a non-owner has "read" and "write" access to an app, _and_ "read" and "write" access to an individual app-wide knowledge object, they can
  view, clone, edit certain properties, and delete the object
  - They _cannot_ ever modify the permission of a knowledge object, even with read/write + read/write access
# Purpose
- An individual app can assign "read" and "write" permissions that affect itself to user roles 
- The examples demonstrate that the final permissions for an individual knowledge object are the result of a somewhat complex interaction between
  app-based role permissions, individual-knowledge-object-based role permissions, and owner permissions 
  - Don't forget about the `admin_all_objects` capability and associated permissions
    - The _only_ capability that lets a user share their knowledge object with _all_ apps (at once) is the `admin_all_objects` capability
      - "The ability to set permissions for knowledge objects is controlled at the app level. It is not connected to a role capability like other
        actions"
        - This is _true_, except for the `admin_all_objects` capability
    - It should also be noted that the ability to apply "read" and "write" permissions to a role at _all_ for _any_ app is controlled by the
      `admin_all_objects` capability
## Permission interaction summary
### Grant app-based "write" permission (app-based "read" permission is assumed)
- Gives owners and non-owners the ability to _delete_ individual knowledge objects, but only if those roles can _see_ the particular knowledge objects
  - Even if a non-owner has "read" and "write" access for an app, they cannot see an individual app-wide knowledge object _unless_ that knowledge
    object has granted "read" permission to a role associated with that user
  - Owners can always see their objects, so they can always delete them
- Gives owners the ability to edit permission properties of their objects
  - Gives owners the ability to share their private knowledge objects with the app
    - The _extent_ to which a _particular_ knowledge object actually is readable or writeable by non-owners is complicated (see examples). The app
      write permission just gives authorized users the _ability_ to share their private knowledge objects with the app
    - If a user does not elect to share their knowledge object with the app, then it remains private
    - If no role has "write" permission for an app, then _no_ user can make their private knowledge objects visible to all users of the app
      - Users can still create knowledge objects, and those knowlege objects will _appear_ to be within the context of the app, but they are not.
        These knowledge objects are _private_ and the app name context is just a convenience label for the owning user
### Grant app-based "read" permission
- Gives the owner of an individual knowledge object the ability to view, clone, and edit non-permission properties of the object
- Gives non-owners the abililty to view and clone the object
- If a role has "read" permission for an app, then all users with that role can 1) click on and see the app 2) access app-wide
  knowledge objects that have been shared with them
- If no role has "read" permission for an app, then _no_ user can click on or see the app at all, let alone see app-wide knowledge objects!
  - The exception is the `admin` role which has the `admin_all_objects` capability
# Bad app-based role permission configurations
- Splunk _does_ allow me to assign "write" permission to a role without also assigning "read" permission to the role
  - The result is exactly what I would expect
  - It's dumb so don't do it
# Invalid app-based role permission configurations
- Splunk does _not_ allow me to give neither "read" nor "write" permission to _any_ role (at least via the Splunk Web interface)
  - If I try to do this, Splunk will ignore when I click "Save" and will retain the prior configuration
  - This is good behavior, but it wouldn't be the end of the world since the admin could still read/write to the app via the `admin_all_objects`
    capability