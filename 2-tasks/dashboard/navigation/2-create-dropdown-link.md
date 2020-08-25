- https://docs.splunk.com/Documentation/Splunk/6.4.6/AdvancedDev/BuildNavigation
# Examples
## Created nested dropdown menu
### Files
#### $SPLUNK_HOME/etc/apps/untopchan/default/data/ui/nav/default.xml
```
<nav search_view="search">
  <view name="unexpected_topology_changes_dashboard" default='true'/>
  <view name="search"/>
  <view name="datasets" />
  <view name="reports" />
  <view name="alerts" />
  <view name="dashboards" />
</nav>
```
- x
# Purpose
- x