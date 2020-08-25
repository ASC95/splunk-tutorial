- https://dev.splunk.com/enterprise/tutorials/quickstart/changenavigation
# Examples
## Set default dashboard
### Files
#### $SPLUNK_HOME/etc/apps/untopchan/default/data/ui/nav/default.xml
```
<nav search_view="search">
  <view name="search"/>
  <view name="datasets" />
  <view name="reports" />
  <view name="alerts" />
  <view name="dashboards" />
  <view name="unexpected_topology_changes_dashboard" default='true'/>
</nav>
```
- Note how I removed the ".xml" extension from the dashboard

- WHAT DOES THE `search_view` attribute do????

# Purpose
- Usually, I don't want to display the boring default search page to the user when they initially enter my app
- I can determine which initial app dashboard should be shown to the user by modifying `/opt/splunk/etc/apps/<app>/default/data/ui/nav/default.xml`
