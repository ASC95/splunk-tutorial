- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Outputlookup
# Examples
- See `splunk/2-tasks/kv-store/1-create/1-create.md` for the underlying KV Store collection used in these examples
## Clear existing KV Store collection
### Commands
```
makeresults | outputlookup buses_kvlookup
```
- Since `makeresults` generates 1 empty event, none of its fields match the fields defined for the underlying KV Store collection of "buses_kvlookup"
- As a result, the KV Store collection is cleared
- Concerning `outputlookup` parameters:
  - It does not matter if `create_empty` is false (the default) or set to true. The result is the same
  - Make sure that `append` is false (which it is by default). If it's true, nothing is cleared
## Create/update records with the `key_field` parameter
### Commands
#### Use existing values from another lookup
```
| inputlookup bus-locations | eval id=bus_id | outputlookup key_field=bus_id buses_kvlookup
```
- Create/update one or more records in the KV Store collection by looking up data from an existing lookup with `inputlookup`
  - The field names in the existing lookup must match the field names in the KV Store collection
    - Use `eval` and `rename` as needed
  - The field values must satisfy any type restrictions, if any
- The `key_field` parameter designates a field from the search results that should be used as the primary key for the KV Store collection records
  - Whichever field is designated will essentially be renamed to `_key`, so the original field will be lost
    - If needed, get around this by using `eval` to create a create a duplicate column
- When the `key_field` parameter is set, `outputlookup` _always_ either appends new records or updates existing records
  - If a matching record exists in the KV Store collection, it will be updated
  - If no record with a matching value for the `_key` field exists, it will be _created and appended_ to the KV Store collection
  - As a result:
    - `outputlookup` always appends nonexistent records, regardless of whether `append` true or false
    - There is no way to remove records
- If no `key_field` is designated, Splunk generates unique hashes and stores them in the `_key` field
#### Use custom values
```
| makeresults | eval bus_id=99, id=bus_id, longitude=5, latitude=5, name="Bus 99" | outputlookup key_field=bus_id buses_kvlookup
```
- Create/update one or more records in the KV Store collection by using `makeresults` and `eval` to generate matching fields and arbitrary values,
  then insert the records with `outputlookup`
- The `eval` fields must match the field names defined in `collections.conf` and the values of those fields must match the declared types, if any
  - When updating a record, if a single field value does not conform to a type restriction, the entire record will not be updated, even if there were
    other valid values
  - When adding a record, if a single field value does not conform to a type restriction, the entire record will not be created  
#### Use existing values from a record
```
| inputlookup buses_kvlookup | where id == 2 | eval name="yes" | outputlookup key_field=asvdas buses_kvlookup
```
- Note how I use "id" instead of "bus_id" in the filtering condition because I'm _getting_ results from the KV Store collection
- The strange thing about this example is that the value of `key_field` doesn't matter and the record is still updating properly
  - It's almost like Splunk says, "I can't find and match on field 'asvdas,' so I'll take the value stored in `_key` that I got from `inputlookup buses_kvlookup`
    and match on that anyway."
    - This behavior is so obscure and terrible that I only want to use the REST API to update the KV Store
      - I can't delete records unless I use the REST API anyway, so why bother obsessing over this
## Overwrite entire KV Store collection
### Commands
#### Use existing values from another lookup
```
| inputlookup bus-locations | rename bus_id as id | outputlookup buses_kvlookup
```
- Since `append=false` is the default for `outputlookup`, the search results from the `inputlookup` command completely overwrite the contents of the
  KV Store
  - This is because I did not use the `key_field` parameter as I did in the above examples
    - If I don't designate a primary key field, then Splunk generates unique hashes and stores them in a field called "_key"
- Only the columns (i.e. fields) that are defined in both the underlying "bus-locations" CSV and the underlying "buses_kvlookup" KV Store collection
  are written from the CSV file to the KV Store collection
  - E.g. the "bus_id" field in the underlying "bus-locations" CSV is called "id" in the underlying "buses_kvlookup" KV Store lookup, so it isn't
    written to the KV Store 
    - To fix this, I can just use the `rename` command 
## Append to KV Store collection with matching fields from CSV
### Commands
```
| inputlookup bus-locations | outputlookup append=true buses_kvlookup
```
- Since `append=true` is set for `outputlookup`, the search results of `inputlookup` do _not_ overwrite the contents of the KV Store. They merely
  append to the KV Store
  - The funny thing is that if I run the above example followed immediately by this example, these appended KV Store records will have NULL for their
    "id" field!
    - If I don't use `rename` in the above example and I do use `rename` in this example, then the appended records _will_ have the "id" field
      while the earlier records will have null in the "id" field. In other words, in principle the result is the same
# Purpose
- The `outputlookup` command is used to write search results to a CSV file or a KV Store collection
- TLDR: don't use the `outputlookup` command to update a KV Store collection
  - See the "Use existing values from a record" example