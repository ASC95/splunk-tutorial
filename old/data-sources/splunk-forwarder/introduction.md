- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2001/Forwarding/Aboutforwardingandreceivingdata 
- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2001/Forwarding/Typesofforwarders - nice chart that compares all forwarder types
# Introduction
- Splunk forwarders appear to be an optimization tool, not a critical piece of functionality
    - If my data volume becomes sufficiently large, then I'll need to learn more about deploying forwarders
## Splunk cloud platform deployment
- A universal forwarder is a separate piece of software that is downloaded and installed on machines within the corporate network. These forwarders
  are then configured to send data to the Splunk cloud platform
## Splunk Enterprise deployment
- I presume it's the same setup as above, except that the forwarders are configured to send data to the Splunk Enterpise instance (which is presumably
  installed on a particular computer, or group of computers)
# What is a Splunk forwarder?
- A Splunk forwarder is a literal Splunk instance that forwards data from itself to another Splunk instance or a non-Splunk system
- A Splunk instance that receives data from a forwarder is called a "receiver"
    - The receiver is often a Splunk indexer. It could also be another forwarder
- There are three types of forwarders:
    - Universal: only contains functionality to forward data
    - Heavy: a full Splunk Enterprise instance that can forward data, in addition to being able to index, search, and modify data
    - Light: depreciated in 6.0
## Universal forwarder
- A universal forwarder is preferred over a heavy forwarder when possible because it uses less resources than a heavy forwarder
### Disabled features
- Does not parse data
    - Thus, it cannot route data to different indexers based on the content of the data
- Does not included a bundled version of Python
- Does not search, index, or produce alerts with data
- Is downloaded as a separate piece of software
## Heavy forwarder
- A heavy forwarder is a full Splunk Enterprise instance with certain features disabled
    - A heavy forwarder cannot perform distributed searches (unlike an indexer)
    - Additional features can be disabled to reduce its footprint (e.g. Splunk Web)
- A heavy forwarder is necessary when the data need to be modified before being forwarded, or the data need to go to different destinations based on
  their contents
  - E.g. route data based on source or type of an event
  - E.g. parse the data before forwarding it
- A heavy forwarder has a smaller footprint than a full indexer
### Features
- Can index data locally before forwarding it
    - This feature must be explicitly enabled
# Why use a Splunk forwarder?
- A forwarder is a more robust source of data than a raw network feed because a forwarder offers:
    - Tagging of metadata (e.g. source, sourcetype, and host)
    - Configurable buffering
    - Data compression
    - SSL security
    - Use of any available network ports