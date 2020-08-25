- https://dev.splunk.com/enterprise/docs/developapps/webframework/binddatausingtokens/tokenmodels/ - Token models
- https://docs.splunk.com/Documentation/WebFramework - Splunk Web Framework
# Token models
## Default model
- For tokens in the "default" model, any changes made to the tokens are immediately propagated across the dashboard
  - E.g. let's say I want to update other dropdown list options immediately when a token is selected. This is a good situation for the "default" token
    model
  - If a token model is not explicitly specified, a token follows the "default" model
## Submitted model
- For tokens in the "submitted" model, clicking the search button first updates all of the token values in the "submitted" model, then runs the search
  with the updated token values
  - E.g. let's say I set heading text based on a token value and I don't want to change the heading until "after" the search is run (I say "after"
    because the update technically happens immediately before the search is run). This is a good situation for the "submitted" token model
- This model is specified with the `{tokenNamespace: "submitted"}` option in the constructors of various "classes" in the Splunk Web Framework
