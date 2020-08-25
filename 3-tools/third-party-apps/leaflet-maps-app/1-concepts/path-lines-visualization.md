- https://github.com/sghaskell/maps-plus#path-lines

- The documentation says that the "path identifier" option is used to determine which Splunk field contains the primary key IDs for each line on a map
  - In reality, this option is set like `<option name="leaflet_maps_app.maps-plus.pathIdentifier">track_id</option>` in a Simple XML dashboard
    - In this example, the "track_id" field has been _chosen_ to contain the primary keys of the path lines
    - I'll need to use the example dashboards _and_ the GitHub documentation to figure out how to correctly set options that I need

- 