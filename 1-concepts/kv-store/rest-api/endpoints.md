- https://dev.splunk.com/enterprise/docs/developapps/manageknowledge/kvstore/usetherestapitomanagekv/ - REST API endpoints summary
# storage/collections/data/{collection}/
## GET
- x
## POST
- x
## DELETE
- Delete all records from the specified collection
### Examples
```
curl -k -u austin:<password> -X DELETE \
    https://localhost:8089/servicesNS/nobody/kvstoretest/storage/collections/data/mycollection
```
