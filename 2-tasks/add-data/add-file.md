- 

# Write custom regular expresions as file is uploaded
- First, I must add the file and pick/create a sourcetype
  - I _cannot_ perform search-time field extractions at this point in the workflow. That comes later
  - If I mess up during this process (e.g. I need to parse the file first before uploading it to Splunk because a header at the top of the file makes
    the events inconsistent), I can just go to Settings > Source types and delete what I just added
- Once the file is uploaded and sourcetype has been chosen/created, _then_ I can create field extractions by going to Settings > Fields > Field
  extractions > Add new

  schweitzer_engineering_laboratories_sel-487e_transformer_protection_relay