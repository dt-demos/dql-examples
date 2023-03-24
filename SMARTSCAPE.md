# smartscape

fetch dt.entity.host
| filter entityId == "HOST-29627D3DC19AD2DA"
| lookup [fetch events
            | filter event.kind == "DAVIS_PROBLEM" and in(affected_entity, "HOST-29627D3DC19AD2DA")
            | summarize count=count(), by:{dt.entity.host="HOST-29627D3DC19AD2DA"}
         ], sourceField:entityId, lookupField:dt.entity.host, prefix:"problems."
| lookup [fetch logs, from:-1h
           | summarize err=countIf(loglevel=="ERROR"), by:dt.entity.host
         ], sourceField:entityId, lookupField:dt.entity.host, prefix:"logs."