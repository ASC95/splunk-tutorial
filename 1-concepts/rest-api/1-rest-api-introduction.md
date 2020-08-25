- https://docs.splunk.com/Documentation/Splunk/latest/RESTUM/RESTusing - General REST API introduction (all of these notes come from here)
- https://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTprolog - Full REST API reference
- https://docs.splunk.com/Documentation/Splunk/latest/RESTTUT/RESTTutorialIntro - General REST API tutorials

- The REST API lets me do two things: run searches and manage resources
- A resource is a single, named object stored by splunkd (e.g. a job, TCP raw input, saved search, etc.)
- There are two types of resources: object resources and configuration resources
- Resources are grouped into collections
- Collections can contain resources and other collections

- The REST API only supports three verbs: GET, POST, and DELETE

- Access the REST API on the management port (typically 8089) and use HTTPS
  - Disable the HTTPS requirement by setting `enableSplunkSSL = false` in `server.conf`

- Parameters passed in the URI must be URI-encoded
  - E.g. the "/" character when URI encoded is "%2F"
  - See my notes ...

- Authentication can be done via username + password or Splunk authentication tokens
  - Authentication tokens must be enabled separately
- Even if a user logs in successfully, they must also have role or capability-based authorization to use the REST API endpoints 
  - Most _individual_ endpoints in the full REST API reference specify the authorizations that are required to use them
    - The required authorizations for many, but not all, endpoints can be viewed by appending `/acl` to the URL
      - See `splunk/2-tasks/rest-api/view-access-controls-for-endpoint.md`
  
- XML is the default encoding scheme for most endpoints
- Alternate encoding schemes, like JSON, can be specified via the `output_mode` query parameter
- If an endpoint doesn't support an encoding scheme, I'll get an error

- Every resource must be accessed within a namespace
- A namespace is created by specifying a user and an app
  - E.g. Every user could have their own saved searches, but there could also be saved searches within an app that are available to all users
- See `...`

- There are three namespaces within which resources may be accessed: "\<user>/\<app>", 