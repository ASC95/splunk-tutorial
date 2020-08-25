- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Indexesconf - `indexes.conf` documentation
# Examples
## Create a typical index
### Files
#### `$SPLUNK_HOME/etc/apps/untopchan2/local/indexes.conf`
```
[my_test_index]
coldPath = $SPLUNK_DB/my_test_index/colddb
enableDataIntegrityControl = 0
enableTsidxReduction = 0
homePath = $SPLUNK_DB/my_test_index/db
maxTotalDataSizeMB = 512000
thawedPath = $SPLUNK_DB/my_test_index/thaweddb
```
- This entry alone _does_ indeed create a new index on the Splunk Enterprise instance
- The following attributes are required:
  - `homePath`: a path to the hot and warm buckets of the index
    - Recommended value: `$SPLUNK_DB/<index name>/db`
      - E.g. `homePath = $SPLUNK_DB/electrical_utility_data/db`
  - `coldPath`: a path to the cold buckets for the index
    - Recommended value: `$SPLUNK_DB/<index name>/colddb`
      - E.g. `coldPath = $SPLUNK_DB/electrical_utility_data/colddb`
  - `thawedPath`: a path to the thawed buckets for an index
    - The indexer deletes frozen buckets, but those buckets can be archived before they are deleted. When an archived bucket is returned to the index,
      it is called a thawed bucket
    - Recommended value: `$SPLUNK_DB/<index name>/thaweddb`
      - E.g. `thawedPath = $SPLUNK_DB/electrical_utility_data/thaweddb`
- The value of `$SPLUNK_DB` is set in `splunk-launch.conf`
  - On macOS, `$SPLUNK_DB` expands to `/Applications/Splunk/var/lib/splunk/`
  - On Ubuntu, `$SPLUNK_DB` expands to `/opt/splunk/var/lib/splunk/`
# Purpose
- Every stanza in `indexes.conf` does the following:
  - Provides data to `$ ./splunk list index`
    - This means that if an index stanza is commented out, the CLI command won't report it even if the index exists!
    - By the same token, if an index stanza is written before starting Splunk, the CLI will list the nonexistent index!
  - Initializes the index within Splunk on start-up if the index doesn't exist
- If an index exists, but is not stored within this file, then Splunk Web WON'T show the index (verified)
  - Thus, the only way to remove an index that I deleted from this file without actually deleting it is to examine the `$SPLUNK_DB` folder
# Files
- An index is created entirely by a valid stanza in `indexes.conf`
- The default app partitioning command will give indexer _and_ search heads (but not forwarders) a copy of the `indexes.conf` defined in an app
  - There is no way to prevent this. I'll have to configure the search head as a forwarder and disable indexing on it if I don't want duplicate
    indexes (see `disable-search-head-indexing.md`)