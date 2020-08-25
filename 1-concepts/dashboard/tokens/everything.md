- I hate Splunk tokens and this confusing SimpleXML so much so I'm just going to dump every corner case that I come across in this document

- The \<condition> elment can go within a \<change> element or a \<drilldown> element
  - \<condition> cannot go within a \<search> element
  - \<condition> cannot go within a \<table> element
- The \<condition> element can go within a search event handler element
  - More specifically, I'll probably only want to use the \<progress> and \<done> search event handler elements because they have access to job
    properties while \<cancelled>, \<error>, and \<fail> do not have access to job properties

- I finally solved my problem. Just use the \<error> event handler element to set the $graph_start_bound$ and $graph_end_bound$ tokens if there's ever
  a problem! Easy!

- This is how to use the \<eval> element
```
<eval token="graph_start_bound">if($graph_start_bound$="$start$", $time_range_value.earliest$, $start$)</eval>
```

- This is how to examine live token values
```
<title>SCADA Historian Log $graph_start_bound$ $graph_end_bound$</title>
```