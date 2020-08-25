- https://docs.splunk.com/Documentation/Splunk/8.0.2/Search/Aboutsearch - two types of searches
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Search/Typesofcommands - six types of search commands
- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Commandsbytype - search commands grouped by category
# Introduction
- All of the notes in this directory are about *search* commands. Statistical commands like `count` are not search commands, so they are listed here.
  Therefore, putting documentation on statistical functions elsewhere is *not* redundant
- The Splunk boards are not that helpful. My personal note here are actually more comprehensive
# Search types
- At the highest logical level, there are two types of Splunk searches: raw event searches and transforming searches
  - This is a logical distinction
## Raw event searches
- This type of search returns zero, one, or more events
- This type of search does not usually include additional search commands
## Transforming searches
- Transforming searches perform statistical calculations against the raw events that are retrieved
# Search commands
- At a lower logical level, there are two broad types of search commands: streaming and non-streaming
- These two broad types are actually composed of six types
- A command may fall under multiple types
  - E.g. Many transforming commands are also non-streaming commands
## Streaming
- Streaming commands operate on single events. They get one event as input and output zero or one event
- Streaming commands fall into two types: distributable streaming and centralized streaming
## Non-streaming
- Non-streaming commands operate on all of the events returned by the search
- These commands force all data to the search head, so they cause a loss of parallelism and lots of data movement
- Non-streaming commands fall into four types: transforming, generating, orchestrating, and dataset processing

- These distinctions aren't terribly helpful to me