- https://community.splunk.com/t5/Security/How-did-forwarder-use-management-port/td-p/479248 - amazing post about the management port
- https://community.splunk.com/t5/Getting-Data-In/Can-I-change-the-default-management-port-8089-on-Splunk/td-p/229396 - another good post
- https://community.splunk.com/t5/Deployment-Architecture/Can-you-disable-the-management-port-8089-on-clients-via-the/td-p/174726 - details on
  disabling the management port. Although, I should never need to do this in a real deployment because I won't have multiple Splunk processes on the
  same machine
# What is the management port used for?
- Splunk deployment clients (e.g. universal and heavy forwarders) will send requests _to_ the management port of the deployment server to _ask_ to be
  sent new configuration changes
  - The deployment server does _not_ push changes to deployment clients; the clients _poll_ for changes
- Thus, I only care about port 8089 being open on a deployment server
  - Additionally, port 8089 is the _conventional_ deployment server management port. I can change the management port to be something else on the
    deployment server. If I do, then I must _also_ change the settings on the forwarders to use the new management port
- Even if the deployment server is disabled, this port is still bound when splunkd starts
# What about other ports?
- Port 9997 is the contentional _input_ port for data on Splunk indexers
  - Thus, I only care about port 9997 being open on indexers, _not_ forwarders