- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Inputlookup
# Examples
## Lookup from file
### Commands
```
| inputlookup bus-locations.csv
```
- Use the name of a CSV file to generate search results
## Lookup from CSV lookup name
### Commands
```
| inputlookup bus-locations
```
- Use the name of a lookup defined in `transforms.conf` to generate search results
## Lookup from KV Store lookup name
### Commands
```
| inputlookup buses_kvlookup | eval primary_key=_key
```
- This example is the same as the example above
  - "buses_kvlookup" is the name of a lookup just like "bus-locations" is the name of a lookup. It's the name of a lookup that matters, not whether
    that lookup is powered by a CSV file or the KV Store
- If I want to see the true primary keys of the KV Store collection records, I need to use `eval` to create a new column that copies the internal
  `_key` field
# Purpose
- Both a CSV (or csv.gz) file and a lookup table can be used to generate search results
  - Using a lookup name is better than a filename because I can read from both underlying CSVs and KV Store collections
