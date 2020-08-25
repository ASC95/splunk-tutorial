- https://dev.splunk.com/enterprise/tutorials/tutorialusekv - tutorial
- https://dev.splunk.com/enterprise/docs/developapps/manageknowledge/kvstore/usetherestapitomanagekv/ - REST API endpoints summary
- https://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTkvstore - REST API endpoints documentation
# Converting to HTML isn't that bad
- I don't have to replace the Simple XML dashboard with the HTML one. I can have two separate versions of the dashboard. In fact, I could use Git to
  track both versions of a dashboard versions and gradually merge ongoing changes from the Simple XML version to the HTML version
# Using the JS SDK is quite complex
- I followed the tutorial as best I could. At the end, I still had these problems:
  - Clicking the "Delete Record" button with no input resulted in clearing the ENTIRE KV Store and I don't know why
    - This is a critical bug and I'll have to figure out how to fix the JS which could be a nightmare
      - Actually this is not a bug. This is the expected behavior of sending a DELETE request to 
  - Using the default and submitted token model for the SearchManager _both_ caused the search to run on page load and I don't know why
# Tutorials are useful
- Imagine if I had just started trying to learn the JS SDK right off the bat. I would have wasted so much time. Tutorials are wonderful because I _am_
  prototyping when I do a tutorial, and I should always prototype first
# Tutorials are hard
- Even the final example code for the tutorial is wrong because it's missing the closing \</form> tag and it has the \<br> elements