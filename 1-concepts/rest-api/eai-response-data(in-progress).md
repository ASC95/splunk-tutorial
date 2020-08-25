- https://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTprolog#EAI_response_data - explains what the eai element data means
# Examples
## EAI response data translation
### `<s:key name="can_write">1</s:key>`
- A value of 1 indicates that the current user can edit the data
  - E.g. ... ?
- It does not matter what the user's capabilities are. I removed all capabilities from a user and the value was still 1
### x
- x
# Purpose
- I was having trouble understanding what the values within the `<s:key name="eai:acl">` element meant, even after learning about permissions, but the
  source explains them
  - I still don't understand. This is some extremely obscure stuff that I'm going to have to come back to