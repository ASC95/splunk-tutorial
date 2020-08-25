- https://docs.splunk.com/Documentation/Splunk/8.0.4/Security/Troubleshootyouforwardertoindexerauthentication
- https://community.splunk.com/t5/Getting-Data-In/What-are-the-basic-troubleshooting-steps-in-case-of-universal/td-p/456364
# Check that the universal forwarder is connected to the indexers
- `$ netstat -an | grep 9997`
  - The output of this command should not be blank. It should show that the universal forwarder is connected to the indexers specified in
    `outputs.conf`
# Ensure that the indexer is listening on the port that the forwarder is forwarding to
- Go to Settings > Forwarding and receiving > Configure receiving and ensure that the indexer is listening on the port (the port is probably 9997)
- Double check this by examining `/opt/splunk/etc/system/local/inputs.conf`
  - The type of listening could be many things
    - E.g. "splunktcp-ssl" or "splunktcp"
# Ping the indexer from the forwarder
- `$ ping `
  - The prompt should not hang! If it does, ping isn't working!


# Check 
- `openssl s_client -connect <server>:<port>`
  - This is not the problem. The output looks good