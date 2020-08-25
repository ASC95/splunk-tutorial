# Dashboard Title
- The title of a dashboard, as displayed on the dashboard page of an app, is read from the XML or HTML file that composes the dashboard
## Simple XML
- The title is read from the `<label></label>` element within the initial `<form>` element
## HTML
- The title is read from the `<title></title>` element within the initial `<head>` element
# Purpose
- Figuring out how Splunk manages dashboard metadata is really annoying, so I'm going to put my thoughts here
  - It gets even worse when I start dealing with Simple XML and HTML dashboards
# Dashboard file location
- There is nothing wrong with having an HTML dashboard in `default/html`. If a dashboard doesn't appear in the app "dashboards" page, it's because
  Splunk does not show the dashboard because it THINKS the HTML file is invalid
# HTML Dashboard syntax
- TLDR: if an HTML dashboard is not showing up in an app "dashboards" page, the best thing I can do is keep commenting things out until I find what
  HTML the Splunk parser is marking as an error. Does Splunk log this somewhere?
  - God forbid if there are multiple non-contigous syntax errors
  - The following cause the dashboard not to appear in the app "dashboards" page:
    - Missing a closing tag
    - Including a \<br> element
      - Maybe it's because \<br> elements don't have a closing element?