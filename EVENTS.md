## List of distinct event kinds

```
fetch events
| summarize count(), by: {event.kind}
| fields event.kind
| sort event.kind
```

## Count of unique event types by event kind
```
fetch events
| summarize count=count(), by: {event.kind, event.type, event.group_label}
| sort event.kind, event.type, event.group_label
| fields event.kind, event.type, event.group_label, count
```

## Count of DAVIS event by event type
```
fetch events
| filter event.kind == "DAVIS_EVENT" //and event.type == "AVAILABILITY_EVENT" 
| summarize count=count(), by: {event.type}
| sort count desc
```

## Count of CUSTOM_INFO DAVIS events
```
fetch events
| filter event.kind == "DAVIS_EVENT" and event.type == "CUSTOM_INFO" 
| summarize count=count(), by: {event.name}
| fields event.name, count
| sort count desc
```

## Duration of open AVAILABILITY DAVIS events
```
fetch events
| filter event.kind == "DAVIS_EVENT" and event.type == "AVAILABILITY_EVENT" 
| fieldsAdd start=formatTimestamp(timestampFromUnixMillis(toLong(event.start)), format:"MM-dd-YYYY"), end=formatTimestamp(timestampFromUnixMillis(toLong(event.end)), format:"MM-dd-YYYY")
| fieldsAdd diff=duration(toLong(event.end)-toLong(event.start),unit:"h")
| fields event.group_label, impact_level, event.status, event.category, event.start, event.end, start, end, diff
```