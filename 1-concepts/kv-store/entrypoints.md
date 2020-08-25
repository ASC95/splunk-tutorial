# Purpose
- The KV Store can be accessed in several different ways, but I need to understand the pros and cons of each approach
# Entrypoints to the KV Store
## Access KV Store via Splunk SDK for JavaScript
- Pros:
  - I don't have to access the REST API manually myself. I can just use the JS SDK
  - I don't have to write a custom search command
  - I don't have to use the crappy `outputlookup` built-in command
- Cons:
  - I have to convert my dashboard from Simple XML to HTML in order to use the JS SDK
    - See `splunk/1-concepts/dashboard/simple-xml/converting-between-html.md`
  - The Splunk JS SDK is very complex. It might be easier to write a custom command then to learn the SDK!
    - See `splunk/4-tutorials/access-kv-store-through-splunk-js/1-access-kv-store-through-splunk-js-introduction.md`
## Access KV Store via `inputlookup` and `outputlookup`
- Pros:
  - It's very easy to just read the contents of the KV Store using `inputlookup`
- Cons:
  - The `outputlookup` command has several problems:
    - It doesn't support the delete operation in CRUD, so it's insufficient by itself
    - It has weird behavior in general that makes me never want to use it
      - See `splunk/3-tools/spl-commands/lookup-related/outputlookup/kv-store-related.md`
## Access KV Store via REST API
  - Pros:
    - The REST API looks pretty clean
  - Cons:
    - I have to write a custom search command that will send requests to the REST API
      - This is not trivial to do. It's a lot of work
    