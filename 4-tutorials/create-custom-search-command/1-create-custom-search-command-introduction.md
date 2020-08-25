- https://dev.splunk.com/enterprise/docs/devtools/customsearchcommands/ - tutorial
- https://stackoverflow.com/questions/5128845/importerror-no-module-named-ssl - Splunk Python _ssl issue
- https://community.splunk.com/t5/Splunk-Search/Why-am-I-getting-error-quot-libssl-so-1-0-0-cannot-open-shared/td-p/267920 - Splunk Python _ssl issue
  (the solution was just to cd to the directory of the Splunk bundled Python)
- https://github.com/splunk/splunk-sdk-python/issues/165 - failed to parse transport header issue ()
# Lessons learned
- This is not a trivial endeavor
- Don't create a custom command to access the REST API. Use `inputlookup` and `outputlookup` instead
## Each custom search command should be its own app
- Custom commands are complex and are their own concept, so they should be defined in their own app
# Tutorial files
- `custom-command-tutorial/bin/app.py`: this is copied from `searchcommands_app/package/bin`
- `custom-command-tutorial/bin/duplicate.py`: this is the custom command that I tried to create by using `filter.py` as a template
- `/home/austin/programming/git-managed-tools/splunk-sdk-python/splunklib/searchcommands/eventing_command.py`: this is the base class that all
  filtering commands inherit from
- `custom-command-tutorial/bin/filter.py`: this is copied from `searchcommands_app/package/bin`
# Stages
## Part 1: just getting filter.py to work
- I did get `filter.py` to work with the search `index="electrical_utility_data" | filterexample predicate="sourcetype=='realtime_oms'"`
### Script modification
- I had to modify a view paths in my copied version of `filter.py`, but nothing beyond that
### Script invocation
- I couldn't successfully invoke `filter.py` as a standalone Python file
  - I got as far as running `export SPLUNK_HOME='/opt/splunk' && cd /opt/splunk/bin && python3 ${file}`
### Configuration file setup
- I don't think that the actual command that will be typed into the Splunk search bar can contain hyphens or underscores
## Part 2: defining my own filtering (i.e eventing) command
- x


# Defining my desired search command
## Access REST API command
- I want my command to:
  - Take field and value arguments for a new or existing KV Store record
    - Required arguments:
      - action: ( "GET" | "POST" | "DELETE" )
    - Optional arguments:
      - pk: the primary key of the record
        - Actually, "pk" would be required in the case of a "GET" or "DELETE" action, but not for a "POST" option
          - The complexity of deciding whether or not this is optional makes me think I shouldn't create a custom search command and that I should use
            `inputlookup` and `outputlookup` instead. I can have delete functionality by searching the entire KV Store with `inputlookup`, filtering
            out the event(s) I don't want to keep, then writing the remaining events back to the KV Store with `outputlookup`
            - This is horrible, but it works. It will be faster than trying to understand the JS SDK. It will be faster than trying to create a clone
              of these commands with my own custom search command
### I should not make a custom search command to access the REST API
## Duplicate events command
- I will make a stupid custom command that duplicates all found events 
- This type of command would be an "eventing command" because it applies a transformation to events as they are processed in the events pipeline
  - It's the opposite of `dedup`, which is also an eventing command