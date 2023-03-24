# alias columns within summarize command

```
fetch logs
| filter isNotNull(bgp_as)
| summarize count=count(), by: {bgp_status=bgp_message_type, bgp_neighbors=bgp_as}
| fields bgp_neighbors, bgp_status, count 
| sort count desc
```

# group counts by formatted day

```
fetch logs, from:-6d
| filter vendor == "Gigamon"
| summarize count(), by:{Day=formatTimestamp(bin(timestamp, 1d), format:"MM-dd-YYYY")}, alias: logCount
| fields Day, logCount
| sort Day
```

## Last 24 hours total log lines
```
fetch logs, from: - 24h
| summarize count(), by:{Hour=formatTimestamp(bin(timestamp, 1h), format:"HH")}, alias: logCount
| fields Hour, logCount
| sort Hour
```

## Last 8 days total log lines
```
fetch logs, from:-6d
| summarize count(), by:{Day=formatTimestamp(bin(timestamp, 1d), format:"MM-dd-YYYY")}, alias: logCount
| fields Day, logCount
| sort Day
```