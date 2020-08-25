- https://docs.splunk.com/Documentation/Splunk/latest/Deploy/Searchheadwithindexers - tutorial
- https://docs.splunk.com/Documentation/Splunk/latest/DistSearch/Configuredistributedsearch - add search peers _without_ using a search head cluster 
# Examples
## Add indexers as search peers
### Files
#### $SPLUNK_HOME/etc/system/local/distsearch.conf
```
[distributedSearch]
servers = https://192.168.1.1:8089,https://192.168.1.2:8089
```
- The host and port must be preceeded by a URI scheme of "http" or "https"
- The port must be the configured management port for a given host
## Copy the search head public key onto each search peer
### Files

- Communication between search peers and the search head is performed through public key authentication

  - It appears that anyone can query a search peer?
    - No, because ...

- The search peers (i.e. indexers) must store a copy of the search head's public key in a specific filesystem directory
  - 

