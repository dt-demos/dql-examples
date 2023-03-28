
## Error rate
​
```
fetch logs
| fieldsAdd errors = toLong(loglevel == "ERROR")
| summarize errorRate = sum(errors)/count() * 100
```

## group counts by formatted day

```
fetch logs, from:-6d
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

## Parse our error descriptions and get a count for each
```
fetch logs
| filter k8s.namespace.name=="prod" and loglevel=="ERROR"
| filter k8s.deployment.name=="frontend-*"
| filter contains(content, "credit card", caseSensitive: FALSE)
| parse content, "LD'code = Code('INTEGER:errorCode')'"
| parse content, "LD'Code('LD'desc = 'LD:errorDesc'{'"
| fields timestamp, errorCode, errorDesc
| summarize Count = count(), by: {errorCode, errorDesc}
```