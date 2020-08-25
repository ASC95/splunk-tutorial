- https://docs.splunk.com/Documentation/Splunk/latest/Indexer/Aboutindexesandindexers
- https://community.splunk.com/t5/Splunk-Search/What-exactly-is-a-tsdix-file/td-p/269709
- https://docs.splunk.com/Splexicon:Tsidxfile
- https://docs.splunk.com/Splexicon:Rawdatafile
- https://community.splunk.com/t5/Getting-Data-In/moving-indexes-and-dat-files/td-p/247774 - purpose of *.dat files (they store the next bucket ID to
  be created for the associated index)
# Introduction
- By default, indexes are stored under `$SPLUNK_HOME/var/lib/splunk/`
# Index contents
- At a high level, an index is a directory that contains many other files
  - Examine the `electrical_utility_data/` directory to see how many files there are!
- There are two main file types: index files (i.e. *.tsidx files) and rawdata files (i.e. journal.gz)
## Index files
- At a high level, an index file stores (1) a list of search keywords and (2) the locations of those keywords in the rawdata files
## Rawdata files
- At a high level, a rawdata file stores compressed event data and some metadata that is useful to the tsidx files
