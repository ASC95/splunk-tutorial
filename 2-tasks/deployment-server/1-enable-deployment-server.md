# Examples
## Set up the deployment server
- Ensure the following are true on the deployment server instance:
  1) There is at least one directory within `$SPLUNK_HOME/etc/deployment-apps/`
      - If there is at least one directory within `deployment-apps/`, then within Splunk Web "Settings > Forwarder management" will show something,
        but "Settings > Server settings > Deployment client" won't show anything
        - What is this page even good for?
  2) The deployment server is not disabled via a `serverclass.conf` configuration
  3) The deployment server port (i.e. the management port) is what I think it is _on the deployment server_ via a `web.conf` configuration
      - The default management port is 8089
  4) Wait a few minutes before checking "Settings > Forwarder management" to see that the expected clients are "phoning home"
      - No additional client configuration is required beyond creating the `deploymentclient.conf` file on the client and making sure the IP and
        management port of the deployment server are specified there
  5) Configure the server classes and associated apps via "Settings > Forwarder management"
      - Upon creating a server class and adding an app and clients to it, the server class should immediately be reloaded
        - As a result, the deployment app should appear on the deployment client
        - Only if a _deployment app_ is configured to trigger a splunkd restart will the deployment client splunkd processes be restarted
  6) Follow the instructions for deploying a deployment app
# Purpose
- The deployment server is automatically enabled if it is not disabled
  - This is due to the configuration within `$SPLUNK_HOME/etc/system/default/serverclass.conf`
    - This configuration is what determines where Splunk searches for deployment apps on the deployment server and where is stores them on deployment
      clients!
- The AIFBDP deployment server follows the rules set in this directory of notes
