## get records types
```
fetch recordtypes
| summarize count(), by: {superType,name}
```

# alias columns within summarize command
```
fetch logs
| filter isNotNull(bgp_as)
| summarize count=count(), by: {bgp_status=bgp_message_type, bgp_neighbors=bgp_as}
| fields bgp_neighbors, bgp_status, count 
| sort count desc
```