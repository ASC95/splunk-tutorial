- https://docs.splunk.com/Documentation/Splunk/latest/Indexer/RemovedatafromSplunk - various ways to delete an index
# Examples
## Delete non-clustered index
### Commands
```
$ ./splunk remove index <index name>
```
- This command assumes that \<index name> is listed in `indexes.conf`
- The only other files in $SPLUNK_DB that appeared to be modified were in the fishbucket/ directory, which pretty well confirms that manual deletion
  of an index is a valid approach
- I can also manually edit `indexes.conf` and delete the associated index directory in the `$SPLUNK_DB` folder
# Sending data to a nonexistent index
- If this happens, Splunk doens't report any obvious errors in Splunk Web