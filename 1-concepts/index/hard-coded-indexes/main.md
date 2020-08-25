- https://community.splunk.com/t5/Archive/Is-all-indexed-data-stored-in-the-defaultdb-folder/td-p/84722 - `defaultdb` is where events are sent if no
  other index was specified for them
- https://community.splunk.com/t5/Archive/How-to-move-index-from-main-to-custom-index/td-p/303542 - move data from one index to another
# I haven't tested any of this
# What is the `$SPLUNK_HOME/var/lib/splunk/defaultdb/` folder?
- I don't understand how, but events that are sent to the `main` index are stored in this directory
- If the `defaultdb/` directory is removed or renamed, as shown in the second source, Splunk will recreate this directory the next time it starts
  - This folder must be hard-coded by Splunk
- If events are not assigned to a different index, they are stored in the `main` index
- The fact that `$SPLUNK_HOME/etc/system/default/indexes.conf` lists `main` as the default index is a separate configuration detail
  - If the default index were changed, Splunk will still be hard-coded to create the `defaultdb/` index, but it just wouldn't use it?