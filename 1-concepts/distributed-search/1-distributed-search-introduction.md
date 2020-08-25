- https://docs.splunk.com/Documentation/Splunk/latest/InheritedDeployment/Deploymenttopology - descriptions of different types of deployment topologies
- https://docs.splunk.com/Documentation/Splunk/latest/Deploy/Deploymentcharacteristics#Primary_characteristics_of_deployments_at_representative_scaling_levels -
  Descriptions of distributed deployment sizes
# What is a deployment topology?
- A deployment topology is a label for a collective group of Splunk nodes
- There are specific types of deployment topologies that describe groups of nodes that have a special relationship to each other within the group
- These labels are conceptually helpful, but are not specific enough to be useful for implementing anything
# Deployment topology types
## Basic distributed search deployment topology
- This type of deployment topology is simply referred to as "basic distributed search"
  - It's a very vague term and shouldn't even be called a deployment topology imo
- It comprises a single search head with two to three non-clustered indexers
  - In a basic distributed search deployment, one search head can serve up to 100 people
## Indexer cluster deployment topology
- This type of deployment topology is simply referred to as an "indexer cluster"
- It comprises multiple indexers that replicate each other's data
## Search head cluster deployment topology
- This type of deployment topology is simply referred to as a "search head cluster"
- It comprises multiple search heads that collectively coordinate searching via a load balancer
## Combined indexer cluster and search head cluster deployment topology
- It comprises a search head cluster that works with an indexer cluster
# What is a distributed deployment?
- A distributed deployment is any deployment that separates searching and indexing onto at least two machines
  - This is a very vague definition, but it must be because Splunk calls everything a distributed deployment!
## What is the difference between a deployment topology and distributed deployment?
- A distributed deployment makes use of deployment topologies