- https://docs.splunk.com/Documentation/Splunk/latest/RESTUM/RESTusing#Namespace - describes REST API namespaces
# Examples
## Examine saved searches in a specific user-app namespace
### Web requests
#### Use /servicesNS endpoint
```
https://localhost:8089/servicesNS/austin/untopchan/saved/searches
```
- This returns the configuration details of all saved searches that are visible to user "austin" within the "untopchan" app
- The returned saved searches include the app-wide "Unexpected Topology Change" alert and the private "My Unexpected Topology Change Clone" alert
  created by the user "austin"
  - The final configuration of the app-wide "Unexpected Topology Change" alert already includes the layered configuration from the app's `local/` and
    `default/` directories
```
curl -k -u <user>:<password> https://localhost:8089/servicesNS/austin/untopchan/saved/searches
```
- Returns the same as above, but I'm using curl so I can view the raw XML
#### Use /services endpoint
```
https://localhost:8089/services/saved/searches
```
- This returns the configuration details of all saved searches that are visible to the "active user-app namespace"
  - In other words, the user defaults to "austin" because that's who I'm logged in as and the app defaults to "search" because "search" is the default
    app
  - The returned saved searches are those that underlie the default reports that are bundled with the "search" app
- It is annoying that I cannot specify the actual user-app namespace with the `/services/` endpoint, so I should avoid using it when possible
## Examine saved searches available in the shared application resources of an app
### Web requests
#### Use the /servicesNS endpoint
```
https://localhost:8089/servicesNS/nobody/untopchan/saved/searches
```
- This returns the configuration details of all saved searches that are visible to any user within the "untopchan" app
  - "nobody" is a synonym for all users
    - It really means "show me app-wide resources that are available to all users"
- The returned saved searches include the app-wide "Unexpected Topology Change" alert, but not the private "My Unexpected Topology Change Clone" alert
  created by the user "austin"
## Examine indexes outside of any user-app namespace
### Web requests
#### Use /services endpoint
```
https://localhost:8089/services/data/indexes
```
- This returns the configuration details of all indexes
- There is no user-app namespace for indexes, so I don't need to specify one
#### User /servicesNS endpoint
```
https://localhost:8089/servicesNS/austin/untopchan/data/indexes
```
- Since I'm using the `/servicesNS` endpoint, I have to specify a user and an app
- However, since the `/data/indexes` endpoint explicitly is outside of any user-app namespace, the user-app that I specify are actually ignored
  and are irrelevent
  - This is bad design, but since different endpoints can require or not require a user-app namespace, it would be super annoying to create corner
    cases for those few endpoints that don't require a namespace
- This is an example of when I _should_ prefer the `/services` endpoint
# Purpose
- While the REST API endpoints specify how to access different types of resources, I still have to choose the namespace within which I want to access
  a specific type of resource
  - Thus, there are actually up to six different namespace URIs for each endpoint
- A namespace is composed of a username and Splunk app name (the directory name) 
- I think that most resources are within an user-app namespace, so the parameters in the `/servicesNS` endpoint are relevent
- However, some resources are explicitly outside of any user-app namespace, so the `/services` endpoint should be preferred
  - These resources include some introspection endpoints (I think)
    - E.g. `curl -k -u admin:pass https://localhost:8089/servicesNS/nobody/search/data/indexes -d name=Shadow` would create an index named "Shadow"
      within the "search" app, so a user-app namespace _is_ useful/required
    - E.g. `https://localhost:8089/services/data/indexes` would retrieve information about all indexes, so a user-app namespace would be misleading
      and confusing
# Syntax
- `/servicesNS/<user>/<app>/<resource>`
  - Interact with a specific resource within a specific user-app namespace
- `/servicesNS/nobody/<app>/<resource>`
  - Interact with a specific resource available to all users of an app
- `/servicesNS/-/<app>/<resource>`
  - Interact with a specific resource available to all users of an app
- `/servicesNS/<user>/-/<resource>`
  - Interact with a specific resource available to a specific user for all apps
- `/servicesNS/-/-/<resource>`
  - Interact with a specific resource available to all users of all apps
- `/services/...` 
  - Either:
    - Interact with a specific resource available to the currently logged in user and default app (not recommended)
    - Interact with a specific resource that is explicitly available outside of any user or app namespace