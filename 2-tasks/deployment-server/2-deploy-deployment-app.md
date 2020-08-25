# Examples
## Exhaustive checklist
- If the build and version numbers of a deployment app are changed:
  - Directly on the deployment server instance:
    - This does _not_ cause the deployment server to _automatically_ reload associated server classes
      - I waited for 7 minutes to check this
  - Via checking out a Git branch:
    - This does _not_ cause the deployment server to _automatically_ reload associated server classes
      - I waited for 7 minutes to check this
  - In either of the above cases, doing a manual reload of the deployment server _afterwards_ via `$ ./splunk reload deploy-server` _does_ trigger the
    deployment server to reload the associated server classes and install the new deployment app on the deployment clients
    - This in turn _does_ restart the splunkd process of the deployment clients if the deployment app is configured to do so
- If I only update the version number of a deployment app and do a manual reload, this _does_ reload the server classes
- If I only update the build number of a deployment app and do a manual reload, this does _not_ reload the server classes
- If I don't update either the build or version number _and_ I do a manual reload of the deployment server, then the splunkd process on the client
  doesn't restart so presumably the server classes aren't reloaded and the deployment app is not redeployed
## How to deploy a deployment app
- Update the build _and_ version numbers, then do a manual reload of the deployment server